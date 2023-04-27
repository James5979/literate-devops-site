+++
title = "nodeSelector"
draft = false
weight = 26
+++

Schedule workloads on specific nodes.

**Prerequisite**: label nodes with special workloads, e.g. requires special hardware, such as a GPU.

Schedule a pod on a particular node (or set of nodes) using a `nodeSelector`.

Example manifest:

```yaml { linenos=inline, hl_lines=["12-13"] }
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
    image: nginx
  nodeSelector:
    compute: large
```
