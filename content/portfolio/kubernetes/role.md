+++
title = "Role"
draft = false
weight = 57
+++

An RBAC `Role` contain rules that represent a set of permissions in a namespace. --- [1]


## Create an admin Role for a particular namespace {#create-an-admin-role-for-a-particular-namespace}

Imperative command:

```shell
kubectl create role $NAME --dry-run=client --namespace=$NAMESPACE --output=yaml --resource=configmaps,deployments.apps,pods,pods/log,secrets,services --verb=create,delete,get,list,patch,update,watch
```

Example manifest:

```yaml { linenos=inline }
# Create the admin Role for the dev namespace:
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: dev-admin
  namespace: dev
rules:
- apiGroups:
  - ""
  resources:
  - configmaps
  - pods
  - pods/log
  - secrets
  - services
  verbs:
  - create
  - delete
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - apps
  resources:
  - deployments
  verbs:
  - create
  - delete
  - get
  - list
  - patch
  - update
  - watch
```


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole)
