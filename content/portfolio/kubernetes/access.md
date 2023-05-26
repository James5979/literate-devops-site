+++
title = "Access"
draft = false
weight = 63
+++

Check that your current user has the necessary access to the perform an action:

```shell
# An ACTION could be any command, such as: create deployments, get pods, etc.
kubectl auth can-i $ACTION --namespace=$NAMESPACE
```

Check that a particular user has the necessary access to the perform an action:

```shell
kubectl auth can-i $ACTION --as $USER --namespace=$NAMESPACE
```
