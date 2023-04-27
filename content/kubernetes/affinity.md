+++
title = "affinity"
tags = ["tip"]
draft = true
weight = 27
+++

Node affinity affects which node a pod can be placed on during scheduling, based on what labels a node has and what logic is applied to such labels within the pod spec.

```yaml
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
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: compute
            operator: In
            values:
            - large
            - medium
          - key: compute
            operator: NotIn
            values:
            - small
          - key: gpu
            operator: Exists
          - key: quantum
            operator: DoesNotExist
```

**Tip**: the `affinity` field will require you to utilise the [kubernetes.io](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) documentation as a reference for looking up the spec.

To remove the labels created on the nodes earlier:

```shell
kubectl label --overwrite nodes/$NODE1 ${LABEL_KEY1}- ${LABEL_KEY3}-
kubectl label --overwrite nodes/$NODE2 ${LABEL_KEY2}- ${LABEL_KEY3}-
```
