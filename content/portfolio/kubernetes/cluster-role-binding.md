+++
title = "ClusterRoleBinding"
draft = false
weight = 61
+++

A `ClusterRoleBinding` grants all the permissions defined in a `ClusterRole` to a user, set of users, or group.


## Grant a user admin access to all resources in the cluster {#grant-a-user-admin-access-to-all-resources-in-the-cluster}

Imperative command:

```shell
kubectl create clusterrolebinding $NAME --clusterrole=$CLUSTERROLE --dry-run=client --output=yaml --user=$USER
```

Example manifest:

```yaml { linenos=inline }
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
subjects:
- kind: User
  name: dillinger
  apiGroup: rbac.authorization.k8s.io
```
