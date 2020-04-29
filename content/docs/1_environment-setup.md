# Environment Setup

Once you have logged into the AWS Management Console, you’ll need to do the following:

1. Configure AWS
1. Deploy a Jumpbox in EC2
1. Install the BOSH CLI

## Configure AWS

Now we need to configure the AWS environment. Go to the BOSH AWS Configuration page for instructions.

{{< hint info >}}
**Note**
You will need to modify the region from us-east-1 to your GovCloud region! I also recommend everyone create their own VPC if possible. If not then we’ll need to keep track of subnets/ranges.
{{< /hint >}}

## Deploy Jumpbox

Now we’ll need a Jumpbox up in AWS to perform the commands. (You can run it locally but for the sake of simplicity we’ll run everything in the cloud.)

- Go to EC2
- Click on Instances
- Click on Launch Instance

You’ll have a number of options to choose from. (More than likely.)

![Create Instance](/docs/img/create-instance.png)

Click on Select on the “Amazon Linux 2 AMI” row.

![Choose Instance Type](/docs/img/choose-instance-type.png)

Now I recommend choosing a VM size of M5.Large or M5.XLarge. The extra cores and memory will help with compiling times.

Once you have it selected choose **Next**.

- Make sure you select your BOSH VPC and choose your subnet.
- You only need one instance so keep it at the default
- Change Auto Assign Public IP to **Enable** 
![Configure](/docs/img/configure_instance_1.png)
- Scroll to the bottom to Network Interfaces. In the Primary IP field enter: 10.0.0.5 
  ![Configure](/docs/img/configure_instance_2.png)

Click **Next**

Change the storage size from 8 GB to something more reasonable. I recommend 32 GB.

![Storage](/docs/img/storage.png)

Click **Next**

No need to add tags at this time. It’s up to you!

Click **Next**

Choose Select an existing security group and choose your BOSH security group! ![Security](/docs/img/security-group.png)

Click **Review and Launch**

Review the details then **Launch**

You will need to choose a key pair. Make sure you select your BOSH keypair.

![Key Pair](/docs/img/keypair.png)

Click **Launch Instances** and wait for your Jumpbox to launch.

{{< hint info >}}
**Note**

Be sure your instance has a Private IP of 10.0.0.5 and it has a Public DNS entry!
{{< /hint >}}

## Jumpbox Config

Now that we have a jumpbox we need to configure it!

Ensure your PEM file has the correct permissions (If on Linux and MacOS)

```
chmod 600 bosh.pem
```

Transfer your PEM file to the Jumpbox

```
scp -i ./bosh.pem ./bosh.pem ec2-user@<IP ADDRESS>:~
```

Log into your Jumpbox:

```
ssh -i ./bosh.pem ec2-user@<IP ADDRESS>
```

Once on the server you’ll need to install some packages to make everything function properly.
```
sudo yum install gcc gcc-c++ ruby ruby-devel mysql-devel postgresql-devel postgresql-libs sqlite-devel libxslt-devel libxml2-devel patch openssl git
```
## Install BOSH CLI

Download the latest version of the BOSH CLI
```
curl -JL -o bosh https://github.com/cloudfoundry/bosh-cli/releases/download/v6.2.1/bosh-cli-6.2.1-linux-amd64
```

Make the bosh binary executable and move the binary to your PATH:
```
chmod +x ./bosh
sudo mv ./bosh /usr/local/bin/bosh
```

You should now be able to use bosh. Verify by querying the CLI for its version:
```
bosh -v
# version 5.3.1-8366c6fd-2018-09-25T18:25:51Z
# Succeeded
```
