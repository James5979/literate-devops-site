+++
title = "Volumes"
draft = false
weight = 64
+++

Mount a host directory within a pod using a volume.

Example manifest:

```yaml { linenos=inline, hl_lines=["16-24"] }
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
    volumeMounts:
    - name: my-volume
      mountPath: /mnt
      readOnly: false
  volumes:
  - name: my-volume
    hostPath:
      path: /srv/kubernetes/hostpath1
      type: Directory
```

**CKA Exam Tip**: use the `kubectl explain Pod.spec.containers.volumeMounts` command and the `kubectl explain Pod.spec.volumes` command to access the local documentation relating to the `volumeMounts` and `volumes` fields, respectively.
