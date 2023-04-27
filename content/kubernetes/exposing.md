+++
title = "Exposing"
tags = ["command", "tip"]
draft = false
weight = 10
+++

Expose a Kubernetes object to a service.

Imperative command:

```shell
kubectl expose $RESOURCE/$NAME --name=$SERVICE_NAME --namespace=default --port=$PORT --protocol=TCP --selector=$LABELS --target-port=$PORT --type=$SERVICE_TYPE
```

**Pitfall**: the `kubectl expose` command will **not** generate the `nodePort` field for you automatically, whereas `kubectl create service nodeport` will. Another difference between these two commands is that the `expose` command will fail if the resource does not exist beforehand, whereas the `create` command will not.

You can also use the `--expose` option with the `kubectl run` command, when you only need to expose a pod:

```shell
kubectl run $NAME --expose --image=$IMAGE --labels=$LABELS --namespace=default --port=$PORT
```
