+++
title = "Granular access"
draft = false
weight = 58
+++

For a Kubernetes `Role`, rules can be assigned on a per object basis.

Imperative command:

```shell
kubectl create role $NAME --dry-run=client --namespace=default --output=yaml --resource=pods,pods/log --resource-name=$OBJECT1 --resource-name=$OBJECT2 --verb=get,list,watch
```

Example manifest:

```yaml { linenos=inline }
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
- apiGroups:
  - ""
  resources:
  - pods
  - pods/log
  resourceNames:
  - busybox
  - nginx
  verbs:
  - get
  - list
  - watch
```
