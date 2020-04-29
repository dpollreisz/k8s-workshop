# Deploy Director

First we need to download the BOSH Deployment scripts
```
git clone https://github.com/cloudfoundry/bosh-deployment
```

Next we need to create a file called deploy.sh to make the process of deploying a BOSH director easier. Copy the code below and make modifications were necessary.

```
#!/bin/bash

bosh create-env bosh-deployment/bosh.yml \
 --state=state.json \
 --vars-store=creds.yml \
 -o bosh-deployment/aws/cpi.yml \
 -o bosh-deployment/jumpbox-user.yml \
 -v director_name=aws \
 -v internal_cidr=10.0.0.0/24 \
 -v internal_gw=10.0.0.1 \
 -v internal_ip=10.0.0.6 \
 -v access_key_id=<ACCESS KEY> \
 -v secret_access_key=<SECRET KEY> \
 -v region=us-east-1 \
 -v az=us-east-1a \
 -v default_key_name=bosh \
 -v default_security_groups=[bosh] \
 --var-file private_key=~/bosh.pem \
 -v subnet_id=<SUBNET ID> 
```
Make the script executable

```
chmod +x deploy.sh
```

## Run It!

```
./deploy.sh
```
You should see output simillar to this
```
Deployment manifest: '/home/ec2-user/bosh-deployment/bosh.yml'
Deployment state: 'state.json'

Started validating
  Downloading release 'bosh'... Skipped [Found in local cache] (00:00:00)
  Validating release 'bosh'... Finished (00:00:01)
  Downloading release 'bpm'... Skipped [Found in local cache] (00:00:00)
  Validating release 'bpm'... Finished (00:00:00)
  Downloading release 'bosh-aws-cpi'... Skipped [Found in local cache] (00:00:00)
  Validating release 'bosh-aws-cpi'... Finished (00:00:00)
  Downloading release 'os-conf'... Skipped [Found in local cache] (00:00:00)
  Validating release 'os-conf'... Finished (00:00:00)
  Validating cpi release... Finished (00:00:00)
  Validating deployment manifest... Finished (00:00:00)
  Downloading stemcell... Skipped [Found in local cache] (00:00:00)
  Validating stemcell... Finished (00:00:00)
Finished validating (00:00:02)

Started installing CPI
  Compiling package 'ruby-2.6.5-r0.29.0/269dc54d5306119b0e4f89be04f6c470b4876f552753815586fd1ab8ebeaa70d'... Finished (00:01:49)
  Compiling package 'bosh_aws_cpi/dcda0118570e8de60bac99ba59aecb4c5f10b19b8a37771ba758cdfaaa1a80b2'... Finished (00:00:02)
  Installing packages... Finished (00:00:00)
  Rendering job templates... Finished (00:00:00)
  Installing job 'aws_cpi'... Finished (00:00:00)
Finished installing CPI (00:01:52)

Uploading stemcell 'bosh-aws-xen-hvm-ubuntu-xenial-go_agent/621.71'... Finished (00:00:03)

Started deploying
  Creating VM for instance 'bosh/0' from stemcell 'ami-0b1e748464b9b7470 light'... Finished (00:00:23)
  Waiting for the agent on VM 'i-074fee967757f0f1f' to be ready... Finished (00:01:51)
  Creating disk... Finished (00:00:09)
  Attaching disk 'vol-071ff280862d0caf7' to VM 'i-074fee967757f0f1f'... Finished (00:00:15)
  Rendering job templates... Finished (00:00:05)
  Compiling package 'golang/e84bb0a48f00f92e3a1086feb199436af258b568'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'ruby-2.6.5-r0.29.0/269dc54d5306119b0e4f89be04f6c470b4876f552753815586fd1ab8ebeaa70d'... Finished (00:01:48)
  Compiling package 'ruby-2.6.5-r0.29.0/269dc54d5306119b0e4f89be04f6c470b4876f552753815586fd1ab8ebeaa70d'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'mysql/440cfa0993fc387154f20775119925d74440a8247fc9c58976c9ee99c1bdcba7'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'libpq/a83e0662fb3d483552de8e02df46809b6c6755e6f64f83f1cac244db61b5c342'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'tini/3d7b02f3eeb480b9581bec4a0096dab9ebdfa4bc'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'bpm-runc/796202c2568ebf561fc62617e8e2909986b60a7e'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'verify_multidigest/64d1958934e10a0eccc05ddf0d7ba0c8215e6f6d4c227cb93998087335378fa8'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'postgres-9.4/601f3635b43d0e7ba3ae866e3bd69425cdf33f7fb34a7f1bb21cc26818fb598e'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'bosh_aws_cpi/dcda0118570e8de60bac99ba59aecb4c5f10b19b8a37771ba758cdfaaa1a80b2'... Finished (00:00:03)
  Compiling package 'davcli/58f558960854f58c55e3d506d3906019178dbc189fbbed1616b8b3c7c02142ea'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'director/7da7966c7452459aec32b4b60654ae77e4fb34f9888b70861ecb84a82d342b57'... Skipped [Package already compiled] (00:00:00)
  Compiling package 's3cli/7e752dee192da026f6a0cdf2653b855cc6efbe6b041564660f8520c39ddd5a78'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'health_monitor/5b655029a0794bd4bda04196accf9c9fb2b9a6217aef9833242a9f26d119c2f7'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'bosh-gcscli/52223432539bbd0607db053f542440869688b4404dd65f2ddf33c2d195b1b891'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'nginx/4cf27fedc69b8b74c26b1e45df7ea715dc9eb427654aab00ef652182b7d81bb5'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'postgres-10/42474d2a099578fd2c4632dca3e90161e2a9e112cc8495ac39be56c7a1792e10'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'bpm/8366da1860667621bdbce0891ffac62597e673ff'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'registry/14b4ba14f98eb9c07ae31f119358f07e4db08c581ab37979035f31bec3795141'... Skipped [Package already compiled] (00:00:00)
  Compiling package 'gonats/f58980bd4b0436ff65f588627116dfff63f346f4d13175b7ba47380ab89e08a6'... Skipped [Package already compiled] (00:00:00)
  Updating instance 'bosh/0'... Finished (00:00:10)
  Waiting for instance 'bosh/0' to be running... Finished (00:00:35)
  Running the post-start scripts 'bosh/0'... Finished (00:00:01)
Finished deploying (00:05:24)

Cleaning up rendered CPI jobs... Finished (00:00:00)

Succeeded
```

## Elastic IP

The last thing we need to do is associate the Elastic IP with the director we just created.

- In EC2 go to Elastic IPs
- Find the IP you allocated earlier and right click it.
- Go to Associate Address 

![Elastic](/docs/img/elastic.png)
- Choose your Director in the Instance field. (HINT: It should be called bosh/0)
- Choose your Private IP. (It should be 10.0.0.6) 
![Elastic](/docs/img/elastic2.png)

## Connect to Director
```
# Configure local alias
bosh alias-env aws -e 10.0.0.6 --ca-cert <(bosh int ./creds.yml --path /director_ssl/ca)

# Log in to the Director
export BOSH_CLIENT=admin
export BOSH_CLIENT_SECRET=`bosh int ./creds.yml --path /admin_password`

# Query the Director for more info
bosh -e aws env
```