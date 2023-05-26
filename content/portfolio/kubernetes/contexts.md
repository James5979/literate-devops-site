+++
title = "Contexts"
draft = false
weight = 54
+++

Configure access to multiple clusters. --- [1]

Get all Kubernetes contexts:

```shell
kubectl config get-contexts
```

| CURRENT | NAME             | CLUSTER          | AUTHINFO         | NAMESPACE |
|---------|------------------|------------------|------------------|-----------|
| \*      | kind-lab-cluster | kind-lab-cluster | kind-lab-cluster | default   |

Switch context:

```shell
kubectl config use-context $CLUSTER
```

Switch namespace within the current context:

```shell
kubectl config set-context --current --namespace=$NAME
```


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)
