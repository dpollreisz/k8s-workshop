---
title : "Config Maps"
weight: 70
---

# Config Maps

A ConfigMap is an API object used to store **non-confidential** data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

In our example, we'll be exposing some values via environment variables. 

First, let's create the ConfigMap object
```
kind: ConfigMap 
apiVersion: v1 
metadata:
  name: example-
   
data:
  # Configuration values can be set as key-value properties
  database: mongodb
  database_uri: mongodb://localhost:27017

  MESSAGE: "A NEW MESSAGE!"
  # Or set as complete file contents (even JSON!)
  keys: | 
    image.public.key=771 
    rsa.public.key=42

```

Now add the config map
```
kubectl apply -f 
.yml
```

The nice thing is that you can completely decouple your configuration from your application. 

Now let's leverage these values in our deployment. Your spec portion of the deployment.yml should look like this:
```
    spec:
      containers:
      - image: nicksterling/hello-kubernetes:2
        name: hello-kubernetes
        ports:
        - containerPort: 8080
        envFrom:
          - configMapRef:
              name: example-
              
        resources: {}

```

Apply the change
```
kubectl apply -f deployment.yml
```