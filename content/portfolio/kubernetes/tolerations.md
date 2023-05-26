+++
title = "Tolerations"
draft = false
weight = 24
+++

A toleration allows a pod to be scheduled on any node with a matching taint.


## Add tolerations to a pod {#add-tolerations-to-a-pod}

Example manifest:

```yaml { linenos=inline, hl_lines=["9-16"] }
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "nginx"
    effect: "NoSchedule"
  - key: "gpu"
    operator: "Exists"
    effect: "NoSchedule"
  containers:
  - name: nginx
    image: docker.io/library/nginx:1.24.0
    imagePullPolicy: IfNotPresent
```

Toleration operators:

<style>.table-nocaption table { width: 70%;  }</style>

<div class="ox-hugo-table table-nocaption">

| Operator | Description                        |
|----------|------------------------------------|
| Equal    | Only keys must match               |
| Exists   | Must have matching key-value pairs |

</div>

**Pitfall**: tolerations do **not** ensure that a pod will be scheduled on a node with a particular taint, it only permits that a pod can be scheduled on such a node.

**CKA Exam Tip**: tolerations require that you reference the [kubernetes.io](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) documentation since no imperative command exists. You can always use the output from the corresponding taint to infer what a toleration should look like, albeit without the toleration operator be present.

**Example**

```shell
kubectl get nodes/$NAME --output=jsonpath='{.spec.taints}'
```
