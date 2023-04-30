+++
title = "Pod"
draft = false
weight = 1
+++

A Pod is the smallest unit of compute within a Kubernetes cluster.


## Create a pod {#create-a-pod}

Imperative command:

```shell
kubectl run $NAME --image=$IMAGE --labels=$LABELS --namespace=default
```

Example manifest:

```yaml { linenos=inline }
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
    imagePullPolicy: Never
```

Resource output for a pod:

```shell
kubectl get pods/$NAME --namespace=default
```
