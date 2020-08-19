---
title : "Logging"
weight: 60
---

# Logging

Logging is best done when the app exports its logs to STDOUT and STDERR. Kubernetes can pick these up and can stream them to something like Elastic or Splunk.

To see the logs for our current deployment, run this: 
```
kubectl logs -f deployment/hello-k8s --all-containers=true
```