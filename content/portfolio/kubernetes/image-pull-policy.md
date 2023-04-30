+++
title = "Image pull policy"
draft = false
weight = 2
+++

If you wish to deploy a Kubernetes application using images cached on the nodes, ensure that the `imagePullPolicy` is set to either `Never` or `IfNotPresent`.

Example manifest:

```yaml { linenos=inline, hl_lines=["12"] }
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: docker.io/library/nginx:latest
    imagePullPolicy: Never
```
