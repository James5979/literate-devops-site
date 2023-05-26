+++
title = "Default resource limits and requests"
draft = false
weight = 29
+++

When a pod exceeds its CPU limit, it gets throttled. When a pod exceeds its memory limit, it is not immediately terminated. Only if a pod continuously exceeds its memory limit is it terminated.

The default limits and requests for a pod are used when no resource requirements have been defined within the spec. This can useful when you have a resource quota applied to the same namespace as the pod. In circumstances when no limits have been set, and a cpu and memory resouce quota is in effect, the pod will fail to run, and you will receive an error. By setting the default limits and requests, you can mitigate this issue, and stop such circumstances from occuring. Instead, a pod will receive the default values (added to its spec) when it is missing a cpu or memory resource requirement.

**Note**: whenever a cpu or memory request is left undefined, but the corresponding limits have been set, the resource requests are implicitly set to the same value as that of the resource limits.


## Create the default (CPU and memory) limits and requests {#create-the-default--cpu-and-memory--limits-and-requests}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
---
apiVersion: v1
kind: LimitRange
metadata:
  name: memory-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

**CKA Exam Tip**: limit ranges require that you reference the [kubernetes.io](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/) documentation, since no imperative command exists. You can always use the `kubectl explain` command to infer what fields you should use.

Resource output for a limit range:

```shell
kubectl get limitranges/$NAME --namespace=$NAMESPACE
```

Describe a limit range:

```shell
kubectl describe limitranges/$NAME --namespace=$NAMESPACE
```
