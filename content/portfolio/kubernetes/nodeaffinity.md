+++
title = "nodeAffinity"
draft = false
weight = 27
+++

Node affinity affects what nodes a pod can be scheduled on, by utilising a mixture of node labels and operator logic.

Example manifest:

```yaml { linenos=inline, hl_lines=["13-30"] }
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
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: compute
            operator: In
            values:
            - medium
            - small
          - key: gpu
            operator: Exists
          - key: gpu
            operator: NotIn
            values:
            - amd
          - key: quantum
            operator: DoesNotExist
```

**CKA Exam Tip**: node affinity requires that you reference the [kubernetes.io](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) documentation, since no imperative command exists. You can always use the `kubectl explain` command to infer what fields you should use.

**Tip**: use a combination of taints and tolerations with node affinity to ensure that specific workloads can only be scheduled on certain nodes.
