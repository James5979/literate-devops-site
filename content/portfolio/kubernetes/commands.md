+++
title = "Commands"
draft = false
weight = 40
+++

In a Kubernetes container, the `command` field is analogous to the `ENTRYPOINT` within a docker image (i.e. the binary to run). Whereas, the `args` field is analogous to the `CMD` field in a docker image (i.e. the arguments to pass to the binary). You can override the `ENTRYPOINT` and the `CMD` of the container image, if you define the respective `command` and `args` fields within the container section of the spec of the kubernetes object.

To set the `args` field within a pod spec, specify the `--` argument (when running the `kubectl run` command), followed by the arguments to pass through to the container.

Imperative command:

```shell
kubectl run $NAME --image=$IMAGE --labels=$LABELS --namespace=default -- sleep 3600
```

**Tip**: `--` must always be the last argument in the `kubectl run` command and come prior to any container arguments.

To set the `command` field within a pod spec, specify the `--command` and `--` arguments (when running the `kubectl run` command), followed by any command and optional command arguments to pass through to the container.

```shell
kubectl run $NAME --command --image=$IMAGE --labels=$LABELS --namespace=default -- /bin/sh -c 'sleep 3600'
```

**Note**: the `command` field will override the `ENTRYPOINT` baked into the container image.

**Tip**: `--` must always be the last argument in the `kubectl run` command and come prior to any container command and arguments.
