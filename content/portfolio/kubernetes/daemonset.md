+++
title = "DaemonSet"
draft = false
weight = 31
+++

A DaemonSet ensure that all nodes in a cluster are running a copy of a [Pod](/portfolio/kubernetes/pod/).


## Create a daemonset {#create-a-daemonset}

Example manifest:

```yaml { linenos=inline }
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: mydaemon
  namespace: default
  labels:
    daemon: mydaemon
spec:
  selector:
    matchLabels:
      daemon: mydaemon
  template:
    metadata:
      labels:
        daemon: mydaemon
    spec:
      containers:
      - name: mydaemon
        image: busybox:1.36.0
        imagePullPolicy: IfNotPresent
        command:
        - "/bin/sh"
        args:
        - "-c"
        - "while true ; do echo I am a daemon doing daemonic things && sleep 3600 ; done"
```

Resource output for a daemonset:

```shell
kubectl get daemonset/mydaemon --namespace=default
```

Show all objects created by a daemonset (via selectors):

```shell
kubectl get all --namespace=default --selector=daemon=mydaemon
```
