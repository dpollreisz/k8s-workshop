---
title : "Secrets"
weight: 80
---

# Secrets

If ConfigMaps are used for non-sensitive data, then Secrets are used for sensitive data. Examples include:

- SSH Keys
- API Keys
- Database Passwords
- OAuth Tokens

{{< hint danger >}}
**Secrets are not encrypted**  
Before we go further, secrets are Base64 encoded... but are NOT encrypted.

There are ways to encrypt secrets. We won't cover that in this workshop but one way is by using [Vault](https://www.hashicorp.com/resources/vault-secret-management-kubernetes/)
{{< /hint >}}

Now let's create a **secret.yml** file and insert this into it:
```
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

Now let's consume it in the deployment.yml file. This should now be your spec section:
```
    spec:
      containers:
      - image: nicksterling/hello-kubernetes:2
        name: hello-kubernetes
        ports:
        - containerPort: 8080
        env:
          - name: SECRET_USERNAME
            valueFrom:
              secretKeyRef:
                name: mysecret
                key: username

          - name: SECRET_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysecret
                key: password
       
        envFrom:
          - configMapRef:
              name: example-configmap
        resources: {}

```

Apply the change
```
kubectl apply -f deployment.yml
```

You should see the new environment variables in the UI