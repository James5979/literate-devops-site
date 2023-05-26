+++
title = "Scaling"
draft = false
weight = 6
+++

To scale the number replicas used by Kubernetes resources, use the `kubectl scale` command:

```shell
kubectl scale --replicas=$NUMBER --filename=$FILE --namespace=default
```

**Note**: this command will not update the manifest file specified within the `--filename` option (shown above).


## Scale a deployment {#scale-a-deployment}

Imperative command:

```shell
kubectl scale --replicas=$NUMBER deployments.apps/$NAME --namespace=default
```

Alternatively, update the number of replicas within the manifest and apply the changes:

```shell
kubectl apply --filename=$FILE
```
