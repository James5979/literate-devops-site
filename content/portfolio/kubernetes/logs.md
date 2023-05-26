+++
title = "Logs"
draft = false
weight = 36
+++

Kubernetes captures logs from each container running in a [pod](/portfolio/kubernetes/pod/). --- [1]


## Create a pod {#create-a-pod}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Pod
metadata:
  name: chatterbox
  namespace: default
  labels:
    app: chatterbox
spec:
  containers:
  - name: quiet
    image: busybox:1.36.0
    imagePullPolicy: IfNotPresent
    command:
    - "/bin/sh"
    args:
    - "-c"
    - "while true ; do echo 'I am the quiet application' && sleep 60 ; done"
  - name: noisy
    image: busybox:1.36.0
    imagePullPolicy: IfNotPresent
    command:
    - "/bin/sh"
    args:
    - "-c"
    - "while true ; do echo 'I am the noisy application' && sleep 45 ; done"
```

To show the logs for a pod:

```shell
kubectl logs pods/$NAME --all-containers --namespace=default
```

For pods with multiple containers, you should specify which container you want to display logs for:

```shell
kubectl logs pods/$NAME --container=$CONTAINER --namespace=default
```

**Tip**: remember that you can stream logs using the `--follow` option and tail logs using the `--tail` option.

**Pitfall**: before checking logs, always inspect the number of containers running in a pod, and identify the container name(s) using the `kubectl describe` command.


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/cluster-administration/logging/#basic-logging-in-kubernetes)
