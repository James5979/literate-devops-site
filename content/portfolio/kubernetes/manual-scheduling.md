+++
title = "Manual scheduling"
draft = false
weight = 20
+++

To ignore a scheduler completely and manually assign a pod to a node, use the `nodeName` field.

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
    image: nginx
  nodeName: lab-cluster-worker2
```
