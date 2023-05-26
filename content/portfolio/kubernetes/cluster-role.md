+++
title = "ClusterRole"
draft = false
weight = 60
+++

An RBAC `ClusterRole` contain rules that represent a set of permissions in a cluster. --- [1]


## Create an admin ClusterRole for cluster-scoped access to all resources {#create-an-admin-clusterrole-for-cluster-scoped-access-to-all-resources}

Imperative command:

```shell
kubectl create clusterrole $NAME --dry-run=client --output=yaml --resource=*.* --verb=*
```

Example manifest:

```yaml { linenos=inline }
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin
rules:
- apiGroups:
  - '*'
  resources:
  - '*'
  verbs:
  - '*'
```


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole)
