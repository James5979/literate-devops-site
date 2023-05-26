+++
title = "Edit"
draft = false
weight = 17
+++

Imperative command to edit an existing Kubernetes object:

```shell
kubectl edit $RESOURCE/$NAME --namespace=$NAMESPACE
```

It is not advisable to use the `kubectl edit` command in a production environment, since there exists no easy way to keep track of the changes made using this method. Changes should always be reviewed and approved, therefore it is good practice to first make changes to a Kubernetes manifest file, and then apply those changes, meanwhile keeping track of all changes using a revision control system, such as [git](<man git>).

**Tip**: if you are studying for the [CKA](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/) exam, you are encouraged to use the `kubectl edit` command - if only for the sake of speed and brevity.
