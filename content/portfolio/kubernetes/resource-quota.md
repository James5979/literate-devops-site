+++
title = "ResourceQuota"
draft = false
weight = 28
+++

Limit aggregate resource consumption per namespace. --- [1]


## Create a namespace-scoped resource quota {#create-a-namespace-scoped-resource-quota}

Imperative command:

```shell
kubectl create quota $NAME --hard=persistentvolumeclaims=10,pods=10,replicationcontrollers=2,resourcequotas=2,secrets=10,services=2,limits.cpu=500m,limits.memory=2048Mi,requests.cpu=250m,requests.memory=1024Mi --namespace=$NAMESPACE
```

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    persistentvolumeclaims: "10"
    pods: "10"
    replicationcontrollers: "2"
    resourcequotas: "2"
    secrets: "10"
    services: "2"
    limits.cpu: 500m
    limits.memory: 2Gi
    requests.cpu: 250m
    requests.memory: 1Gi
```

**CKA Exam Tip**: resource quotas require that you reference the [kubernetes.io](https://kubernetes.io/docs/concepts/policy/resource-quotas/) documentation, since no imperative command exists. You can always use the `kubectl explain` command to infer what fields you should use.

**Pitfall**: creating a resource quota for CPU and/or memory means that each pod must have its own resource requirements defined. If no requirements have been set, and no defaults exist, then the pod will not be scheduled.

Resource output for a quota:

```shell
kubectl get resourcequotas
```

Describe a resource quota:

```shell
kubectl describe resourcequotas/$NAME --namespace=$NAMESPACE
```


## Define the resource requirements for a pod {#define-the-resource-requirements-for-a-pod}

Example manifest:

```yaml { linenos=inline, hl_lines=["20-26"] }
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: dev
  labels:
    app: busybox
spec:
  containers:
  - name: busybox
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "while true ; do sleep 3600 ; done"
    resources:
      limits:
        cpu: 500m
        memory: 256Mi
      requests:
        cpu: 250m
        memory: 128Mi
```


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/policy/resource-quotas/)
