+++
title = "Environment variables and ConfigMaps"
draft = false
weight = 43
+++

Pass through environment variables to a [Pod](/portfolio/kubernetes/pod/) using a [ConfigMap](/portfolio/kubernetes/configmap/).

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
    envFrom:
    - configMapRef:
        name: my-configmap
```

Test that the environment variables exist within the container:

```shell
kubectl exec pods/busybox --stdin --tty -- /bin/env | grep --extended-regexp "APP|TEST"
```

Populate a single environment variable using a configmap:

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
      valueFrom:
        configMapKeyRef:
          name: my-configmap
          key: "TEST"
```

**CKA Exam Tip**: use the `kubectl explain Pod.spec.containers.env.valueFrom` command to access the local documentation relating to the `valueFrom` field.

Test that the environment variable exists within the container:

```shell
kubectl exec pods/busybox --stdin --tty -- /bin/env | grep TEST
```
