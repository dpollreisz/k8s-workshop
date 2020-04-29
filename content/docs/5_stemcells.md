# Stemcells

Now before we can deploy Nginx, we need an Operating System image. This is the stemcell.

Let’s check the stemcells loaded into the system
```
bosh -e aws stemcells
```

There shouldn’t be any loaded. Let’s load a stemcell:
```
bosh -e aws upload-stemcell https://bosh.io/d/stemcells/bosh-aws-xen-hvm-ubuntu-xenial-go_agent?v=621.69
```

Now you can check to see if it uploaded successfully
```
bosh -e aws stemcells
```

