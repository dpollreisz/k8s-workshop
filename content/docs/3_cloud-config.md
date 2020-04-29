# Cloud Config

A Cloud Config will act as the interface between your IaaS and your deployments. It allows you to establish T-Shirt sizes for disk and compute resources.

To see what cloud configurations are in your environment:

```
bosh -e aws cloud-config
```

You should see this:

```
Using environment '10.0.0.6' as client 'admin'

No cloud config

Exit code 1
```

Let’s create a file named **cloud-config.yml**

```
azs:
- name: z1
  cloud_properties: {availability_zone: us-east-1a}

vm_types:
- name: small
  cloud_properties:
    instance_type: t2.micro
    ephemeral_disk: {size: 3000, type: gp2}
- name: medium
  cloud_properties:
    instance_type: m3.medium
    ephemeral_disk: {size: 30000, type: gp2}

disk_types:
- name: small
  disk_size: 3000
  cloud_properties: {type: gp2}
- name: large
  disk_size: 50_000
  cloud_properties: {type: gp2}

networks:
- name: public
  type: manual
  subnets:
  - range: 10.0.0.0/24
    gateway: 10.0.0.1
    az: z1
    reserved: [10.0.0.1-10.0.0.20, 10.0.0.45]
    dns: [8.8.8.8]
    cloud_properties: {subnet: <SUBNET ID>}
- name: vip
  type: vip

compilation:
  workers: 5
  reuse_compilation_vms: true
  az: z1
  vm_type: medium
  network: public
```

Now let’s apply the new cloud config

```
bosh -e aws update-cloud-config cloud-config.yml
```

If all goes well you’ll see this output:

```
Succeeded
```