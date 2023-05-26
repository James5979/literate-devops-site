+++
title = "nodeSelector"
draft = false
weight = 26
+++

Schedule workloads on specific nodes.

**Prerequisite**: label any node that contains special hardware for running particular workloads, such as having access to a GPU, etc.

Schedule a pod on a particular node (or set of nodes) using a `nodeSelector`.

Example manifest:

```yaml { linenos=inline, hl_lines=["13-14"] }
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
    image: docker.io/library/nginx:1.24.0
    imagePullPolicy: IfNotPresent
  nodeSelector:
    compute: small
```
