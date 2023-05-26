+++
title = "Sorting"
draft = false
weight = 74
+++

Sort Kubernetes output.

Sort pod names, alphabetically:

```shell
kubectl get pods --namespace=kube-system --output=custom-columns=NAME:.metadata.name --sort-by=.metadata.name
```

Sort pods based on creation time:

```shell
kubectl get pods --output=custom-columns=NAME:.metadata.name,CREATED-AT:.metadata.creationTimestamp --sort-by='{.metadata.creationTimestamp}'
```

Sort persistent volumes based on storage capacity:

```shell
kubectl get persistentvolumes --output=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage --sort-by=.spec.capacity.storage
```

**Pitfall**: both the `custom-columns` output and the `--sort-by` option implicitly specify the `.items[*]` array within all their queries, so you should **never** need to specify it yourself. You can also leave off the single quotes and curly braces (conventional to the `--output=jsonpath` syntax).
