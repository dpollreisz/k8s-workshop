---
title : "Pods"
weight: 20
---

# Pods

A pod is the smallest unit of deployment in Kubernetes. It consists of one or more related containers that will deploy together on the same worker node. Pods will share networking and storage across all the grouped containers inside the pod.

{{< hint info >}}
**NOTE**  
A pod typically only has one container... but it can contain more than one. Examples would be a sidecar container that does some data initialization or a APM sidecar that pulls application metrics to name just a few. 
{{< /hint >}}

The simplest way to deploy a pod is declaratively. Run this command:
```
kubectl run hello-k8s --restart=Never --image=nicksterling/hello-kubernetes:1 --port=8080
```

Now let's see the running pod
```
kubectl get pods
```

You should see output like the following
```
NAME        READY   STATUS    RESTARTS   AGE
hello-k8s   1/1     Running   0          13s
```

Let's dive into the pod a little bit more and get more details
```
kubectl describe pod hello-k8s
```

You should see a lot of output
```
Name:         hello-k8s
Namespace:    default
Priority:     0
Node:         kind-worker2/172.19.0.2
Start Time:   Mon, 22 Jun 2020 21:32:05 -0600
Labels:       run=hello-k8s
Annotations:  <none>
Status:       Running
IP:           10.244.1.4
IPs:
  IP:  10.244.1.4
Containers:
  hello-k8s:
    Container ID:   containerd://1a18e5a6c08ca26e5f2f15576dd7b0f00ef1fca62c46fde42e86eb34305c3f17
    Image:          nicksterling/hello-kubernetes:1
    Image ID:       docker.io/nicksterling/hello-kubernetes@sha256:e91f805764b59b877e1d1ac70a678bbb097db1a7613eb8ba59f03e7b5c12e5c1
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 22 Jun 2020 21:32:05 -0600
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-bhwkz (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-bhwkz:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-bhwkz
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From                   Message
  ----    ------     ----  ----                   -------
  Normal  Scheduled  87s   default-scheduler      Successfully assigned default/hello-k8s to kind-worker2
  Normal  Pulled     87s   kubelet, kind-worker2  Container image "nicksterling/hello-kubernetes:1" already present on machine
  Normal  Created    87s   kubelet, kind-worker2  Created container hello-k8s
  Normal  Started    87s   kubelet, kind-worker2  Started container hello-k8s
```

## Declarative vs Imperative
Up to this point we've done everything imperatively. We've issued commands into the CLI and started up a pod. This is **not** the recommended way to work with Kubernetes. We recommend telling Kubernetes __declaratively__ using a manifest file the state of what we want and let Kubernetes figure out how to do it.

Let's remove the pod we've imperatively created. 
```
kubectl delete pod hello-k8s
```

Now let's create a manifest file called **pod.yml** and put this as the contents
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: hello-k8s
  name: hello-k8s
spec:
  containers:
  - image: nicksterling/hello-kubernetes:1
    name: hello-k8s
    ports:
    - containerPort: 8080
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

Now let's apply that file 
```
kubectl apply -f pod.yml
```

We have a pod again! Now up to this point we don't have an easy way to view this in a web browser. We need to expose that as a **service**. Let's go over that next. 