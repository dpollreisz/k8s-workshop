# Bosh Releases

Software is packaged in the form of **releases**. We will deploy it later via a **deployment** with a custom manifest file.

Check out [Bosh.io’s Releases](https://bosh.io/releases/) page and find “cloudfoundry-incubator/envoy-nginx-release”

We will be deploying this release. Run this command:
```
bosh -e aws upload-release --sha1 13cf87b2394c7d3924f9d66836c56302fb46a90d https://bosh.io/d/github.com/cloudfoundry-community/nginx-release?v=1.17.0
```

Now verify you can see the nginx release:

```
bosh -e aws releases
```