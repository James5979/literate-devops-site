+++
title = "Rollouts"
draft = false
weight = 38
+++

When upgrading pods in a deployment, rolling updates are used as the default deployment strategy, ensuring no downtime for the application.

Use the following spec to configure the `RollingUpdate` strategy:

```yaml
spec:
  strategy:
    rollingUpdate:
      maxSurge: 0
      maxUnavailable: 1
    type: RollingUpdate
```

**CKA Exam Tip**: use the `kubectl explain Deployment.spec.strategy` command to access the local documentation relating to the `stategy` field.

To create a rollout, first update the image used for the pods within the template section. Then apply the rollout by running the `kubectl apply --filename $FILE` command.

There is also a `kubectl set image` command that can be used to set the image and perform the rollout, but this comes at the expense of not having the changes reflected within the manifest.

```shell
kubectl set image deployments.apps/$NAME $CONTAINER=$IMAGE --namespace=default
```

You can also update the image by supplying the `--filename` option (in-lieu of using an object reference), but this also has the same drawbacks as mentioned above.

```shell
kubectl set image --filename=$FILE $CONTAINER=$IMAGE --namespace=default
```

Get the status output for a rollout:

```shell
kubectl rollout status deployments.apps/$NAME --namespace=default
```

It is worth remembering that there also exists the `Recreate` strategy, but using this will cause downtime for your application, as all of the pods are scaled down simultaneously.

To rollback a deployment:

```shell
kubectl rollout undo deployments.apps/nginx --namespace=default
```

The `kubectl rollout undo` command uses replicasets to undo each rollout. A rollout is essentially an additional replicaset added to the deployment, with the previous replicaset being scaled down to zero pods.

To view the rollout history:

```shell
kubectl rollout history deployments.apps/$NAME
```
