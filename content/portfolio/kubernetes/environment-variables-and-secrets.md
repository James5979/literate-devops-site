+++
title = "Environment variables and Secrets"
draft = false
weight = 45
+++

Pass through environment variables to a container in a [pod](/portfolio/kubernetes/pod/) using a [secret](/portfolio/kubernetes/secrets/).

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
    - secretRef:
        name: my-secret
```

Test that the environment variables exist within the container:

```shell
kubectl exec pods/busybox --stdin --tty -- /bin/env | grep --extended-regexp "GOLDENEYE_CODES|PASSWORD|PRIMARY_MISSION"
```

Populate a single environment variable using a secret:

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
    - name: PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: PASSWORD
```

**CKA Exam Tip**: use the `kubectl explain Pod.spec.containers.env.valueFrom` command to access the local documentation relating to the `valueFrom` field.

Test that the environment variable exists within the container:

```shell
kubectl exec pods/busybox --stdin --tty -- /bin/env | grep PASSWORD
```
