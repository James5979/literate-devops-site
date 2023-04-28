+++
title = "Edit command"
tags = ["command"]
draft = false
weight = 17
+++

To imperatively edit a Kubernetes object:

```shell
kubectl edit $RESOURCE/$NAME --namespace=$NAMESPACE
```

The **edit** command is not advisable, since there exists no easy way to keep track of the changes made to a Kubernetes object (unless you are using annotations). But even with the existence of annotations, there still exists the issue of not having the changes reviewed (and approved). To fix this, use a revision control system, such as [git](<man git>), and make the changes there (in a manifest file that is under revision control).

**Tip**: if you are studying for the CKA exam, you are encouraged to use the `kubectl edit` command - if only for the sake of speed.
