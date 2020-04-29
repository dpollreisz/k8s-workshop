# Bosh Deployments

Now let’s deploy the Nginx release. Go to the [Cloud Foundry Nginx Github](https://github.com/cloudfoundry-community/nginx-release) page. We’ll be needing the manifest to deploy it. Navigate to manifests/nginx-lite.yml. Copy the contents of this file to nginx.yml on your jumpbox.

Once you have it try to deploy it.
```
bosh -e aws -d nginx deploy ./nginx.yml
```

You should see the following error
```
Error: Instance group 'nginx' references an unknown disk type 'default'
```

The reason for the error is because the deployment is referencing an invalid disk type. If you pull up your cloud config, you’ll notice you don’t have a default disk. You’ll need to edit your nginx.yml file to make it correspond to the cloud config.

Try to deploy it once again.

```
bosh -e aws -d nginx deploy ./nginx.yml
```
This should be the output

```
Task 11 | 01:47:29 | Preparing package compilation: Finding packages to compile (00:00:00)
Task 11 | 01:47:29 | Compiling packages: nginx/66c3f00b386cb6166d60748308b27fa31f4285ad2eac86024e7166b29837bf0d (00:04:39)
Task 11 | 01:52:42 | Creating missing vms: nginx/05eeb5fd-687b-4f84-8f09-9af220e314d0 (0)
Task 11 | 01:52:42 | Creating missing vms: nginx/85fc0e69-3a2c-4e77-b845-f1f2810d84a1 (1) (00:02:24)
Task 11 | 01:55:10 | Creating missing vms: nginx/05eeb5fd-687b-4f84-8f09-9af220e314d0 (0) (00:02:28)
Task 11 | 01:55:11 | Updating instance nginx: nginx/05eeb5fd-687b-4f84-8f09-9af220e314d0 (0) (canary) (00:00:39)
Task 11 | 01:55:50 | Updating instance nginx: nginx/85fc0e69-3a2c-4e77-b845-f1f2810d84a1 (1) (00:00:38)

Task 11 Started  Tue Apr 28 01:47:29 UTC 2020
Task 11 Finished Tue Apr 28 01:56:28 UTC 2020
Task 11 Duration 00:08:59
Task 11 done
```

Once it deploys check the VM’s that it created.
```
bosh -e aws -d nginx vms
```

The output should be
```
Instance                                    Process State  AZ  IPs        VM CID               VM Type  Active  Stemcell
nginx/05eeb5fd-687b-4f84-8f09-9af220e314d0  running        z1  10.0.0.21  i-0217a03c4c1eaa006  small    true    bosh-aws-xen-hvm-ubuntu-xenial-go_agent/621.69
```

Now confirm nginx is working properly
```
curl 10.0.0.21
```

## Scaling

Let’s scale our nginx.yml deployment from 1 to 2 instances. Once the file is updated try to redeploy with bosh -e aws -d nginx deploy nginx.yml. You should see the delta.

```
Using environment '10.0.0.6' as client 'admin'

Using deployment 'nginx'

  instance_groups:
  - name: nginx
-   instances: 1
+   instances: 2

Continue? [yN]:
```

Once the operation completes get the VMs again:
```
bosh -e aws -d nginx vms
```

You should now see two nginx VMs
```
Using environment '10.0.0.6' as client 'admin'

Task 13. Done

Deployment 'nginx'

Instance                                    Process State  AZ  IPs        VM CID               VM Type  Active  Stemcell
nginx/05eeb5fd-687b-4f84-8f09-9af220e314d0  running        z1  10.0.0.21  i-0217a03c4c1eaa006  small    true    bosh-aws-xen-hvm-ubuntu-xenial-go_agent/621.69
nginx/85fc0e69-3a2c-4e77-b845-f1f2810d84a1  running        z1  10.0.0.22  i-0a3dac299b5c44696  small    true    bosh-aws-xen-hvm-ubuntu-xenial-go_agent/621.69

2 vms

Succeeded
```

## Upgrade Stemcell

Now over time stemcells will be released with patched CVE’s or other updates. It’s very easy to update to the latest version.

First let’s upload a newer stemcell:
```
bosh -e aws upload-stemcell https://bosh.io/d/stemcells/bosh-aws-xen-hvm-ubuntu-xenial-go_agent?v=621.71
```

Check the stemcells loaded in the environment

```
bosh -e aws stemcells
```

Output

```
Using environment '10.0.0.6' as client 'admin'

Name                                     Version  OS             CPI  CID
bosh-aws-xen-hvm-ubuntu-xenial-go_agent  621.71   ubuntu-xenial  -    ami-0b1e748464b9b7470 light
~                                        621.69*  ubuntu-xenial  -    ami-02ff958a8af2e0d0c light

(*) Currently deployed

2 stemcells

Succeeded
```

The act of uploading a new stemcell does not update any active deployments. You’ll need to do that separately.

{{< hint info >}}
**Note**

Active stemcells will have an * next to their version numbers. You cannot delete a stemcell that’s currently being used by a deployment.
Let’s update our nginx deployment to use the latest stemcell. In this case we stated we wanted to use the latest version of ubuntu-xenial so we don’t need to make a manifest change. To apply the change just run this command
{{< /hint >}}

```
bosh -e aws -d nginx deploy nginx.yml
```

Output
```
Using environment '10.0.0.6' as client 'admin'

Using deployment 'nginx'

  stemcells:
+ - alias: ubuntu
+   os: ubuntu-xenial
+   version: '621.71'
- - alias: ubuntu
-   os: ubuntu-xenial
-   version: '621.69'

Continue? [yN]:
```

After this completes check the stemcells again
```
bosh -e aws stemcells
```

Output

```
bosh-aws-xen-hvm-ubuntu-xenial-go_agent  621.71*   ubuntu-xenial  -    ami-0b1e748464b9b7470 light
~                                        621.69  ubuntu-xenial  -    ami-02ff958a8af2e0d0c light

(*) Currently deployed

2 stemcells

Succeeded
```