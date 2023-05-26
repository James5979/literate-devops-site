+++
title = "Environment variables"
draft = false
weight = 41
+++

Set environment variables within a container.


## Create a pod and specify an environment variable {#create-a-pod-and-specify-an-environment-variable}

Imperative command:

```shell
kubectl run $NAME --dry-run=client --env=$ENV --image=$IMAGE --labels=$LABELS --namespace=default --output=yaml -- sleep 3600
```

Use the `env` field within the pod spec to pass environment variables through to the container.

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: default
  labels:
    app: busybox
spec:
  containers:
  - name: busybox
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "sleep"
    - "3600"
    env:
    - name: TEST
      value: success
```

Test that the environment variable exists within the container:

```shell
kubectl exec pods/busybox --stdin --tty -- /bin/env | grep TEST
```
