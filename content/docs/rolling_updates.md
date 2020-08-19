---
title : "Rolling Updates"
weight: 50
---

# Rolling Updates

Now let's update our app to the latest version. Open up the **deployment.yml** file and look for the image. Replace the image with this one: ``nicksterling/hello-kubernetes:2``

Once you've made the change apply it:
```
kubectl apply -f deployment.yml
```

This will go through and roll out the new image. Go to your app to view the change.

You can roll back a deployment to an earlier version if something went wrong. We won't cover that in this workshop but you can find more details here: [Rolling Back a Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment)
