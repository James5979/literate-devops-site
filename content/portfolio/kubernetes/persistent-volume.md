+++
title = "PersistentVolume"
draft = false
weight = 65
+++

A `PersistentVolume` is a piece of storage within the cluster that has been provisioned by an administrator (or dynamically provisioned using a [storage class](/portfolio/kubernetes/storage-class/)). --- [1]


## Create a persistent volume {#create-a-persistent-volume}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-small
  labels:
    size: small
spec:
  accessModes:
  - ReadWriteOnce
  capacity:
    storage: 1Gi
  persistentVolumeReclaimPolicy: Delete
  hostPath:
    path: /srv/kubernetes/hostpath2
```

**CKA Exam Tip**: persistent volumes require that you reference the [kubernetes.io](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolume) documentation, since no imperative command exists. You can always use the `kubectl explain` command to infer what fields you should use.


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
