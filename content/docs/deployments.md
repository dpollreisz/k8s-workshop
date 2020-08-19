---
title : "Deployments"
weight: 40
---

# Deployments

If you delete a pod, it's gone forever. That's not very resilient. Deployments allow us to create pod definitions and manage the lifecycle of that pod. Let's create a **deployment.yml** file:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: hello-k8s
  name: hello-k8s
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-k8s
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: hello-k8s
    spec:
      containers:
      - image: nicksterling/hello-kubernetes:1
        name: hello-kubernetes
        ports:
        - containerPort: 8080
        resources: {}
status: {}

```

And apply it: 
```
kubectl apply -f deployment.yml
```

Now let's create a **loadbalancer.yml** service to point to our new Deployment
```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: hello-k8s
  name: hello-k8s
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: hello-k8s
  type: LoadBalancer
status:
  loadBalancer: {}
```

And apply it
```
kubectl apply -f loadbalancer.yml
```

## Scale The Pods
One thing that you can do is scale the number of pods across your cluster for high availability. In your **deployment.yml** file look for replicas and increase it to **4**

Once you've done that apply the change
```
kubectl apply -f deployment.yml
```

Now watch the change
```
kubectl get all 
```

In the next section we'll focus on updating your app to a newer version with a rolling update. 