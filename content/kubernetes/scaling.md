+++
title = "Scaling"
tags = ["command"]
draft = false
weight = 6
+++

To scale a replication controller, replicaset, statefulset, or deployment, use the `kubectl scale` command:

```shell
kubectl scale $RESOURCE/$NAME --filename=$FILE --namespace=default --replicas=$NUMBER
```

**Pitfall**: this will not update the value for the replicas within the manifest.


## Scale a deployment {#scale-a-deployment}

Imperative command:

```shell
kubectl scale deployments.apps/$NAME --replicas=$NUMBER deployment/$NAME --namespace=default
```

Alternatively, update the number of replicas in the manifest and apply the changes using `kubectl apply`:

```shell
kubectl apply --filename=$FILE
```
