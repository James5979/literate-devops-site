+++
title = "Persistent volume claims"
draft = false
weight = 66
+++

A `PersistentVolumeClaim` is a request for storage by the user. --- [1]


## Create a persistent volume claim and bind it to a pod {#create-a-persistent-volume-claim-and-bind-it-to-a-pod}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

**CKA Exam Tip**: persistent volumes require that you reference the [kubernetes.io](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims/) documentation, since no imperative command exists. You can always use the `kubectl explain` command to infer what fields you should use.

Use a persistent volume within a pod:

```yaml { linenos=inline, hl_lines=["16-22"] }
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
    - name: my-pv
      mountPath: /opt/data
  volumes:
  - name: my-pv
    persistentVolumeClaim:
      claimName: my-claim
```

Delete and re-create the pod to verify that the volume is persistent across all subsequent pod lifecycles.


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#introduction/)
