---
title : "Services"
weight: 30
---

# Service Overview

A service is a way to expose an application running on a pod (or set of pods) as a network service. By default Kubernetes gives pods their own IP addresses and DNS entries but they are not addressable to the outside world. 

There are three major types of Services
1) _ClusterIP_: The default type of Service in K8s. It will give you a service that other apps inside your cluster can access. **There is no external access**
2) _NodePort_: This is most primitive way of getting external traffic to your app. This will open up a port on all the nodes in your cluster and traffic received on this port will forward to the application. 
3) _LoadBalancer_: It leverages your IaaS' native LB to expose a cluster resource to an IP address given by the IaaS' LB. 
4) _Ingress_: Ok... it's technically not a service. It's a _resource_ but I would be remiss if I didn't mention it. This is another way to allow traffic into your cluster. It's the most powerful way but also the most complex. It's implemented through a third party proxy like Nginx or HAProxy and are managed via Ingress Controllers. It's essentially a sophisticated router.  

# Deploy Service
Now that we have some definitions down let's deploy a service to our pod so we can see it

Let's deploy it imperatively first
```
kubectl expose pod hello-k8s --type=LoadBalancer --port 8080
```

Let's see what it deployed
```
kubectl get svc
```

You should now see your _hello-k8s_ LoadBalancer service. 
```
OUTPUT
```

Now let's navigate to the External IP at port 8080!

You should see this
**IMAGE HERE**

Again, we shouldn't deploy anything in a one-off manner. Let's delete this service.
```
kubectl delete svc hello-k8s
```



And while we're at it... delete the pod too. 
```
kubectl delete po hello-k8s
```
We have a better way of deploying pods... With Deployments!