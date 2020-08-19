---
title : "Environment Setup"
weight: 10
---
# Environment Setup
You should be able to do everything locally from your computer without need of a Jumpbox. We will need some dependencies installed:

## Install CLIs

- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Helm 3](https://helm.sh/docs/intro/install)


## Log In to Cluster
You will be given instructions in Zoom on how to login...

## Create Namespace
Now that you've logged in, you'll need your own namespace to work out of. 

First, let's look at all of the namespaces currently in the system:
```
kubectl get namespaces
```

You should see something like this: 
```
NAME      STATUS    AGE
default   Active    13m
```

But let's create your namespace
```
kubectl create namespace YOUR_NAME_HERE
```

Now verify the namespace was created
```
kubectl get namespaces
```

By default kubectl commands will use the _default_ namespace. Let's change that to use your namespace
```
kubectl config set-context --current --namespace=YOUR_NAME_HERE
```