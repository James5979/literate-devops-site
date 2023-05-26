+++
title = "RoleBinding"
draft = false
weight = 59
+++

A `RoleBinding` grants all the permissions defined in a `Role` to a user, set of users, or group.


## Grant a user access to resources in a namespace: {#grant-a-user-access-to-resources-in-a-namespace}

Imperative command:

```shell
kubectl create rolebinding $NAME --dry-run=client --namespace=$NAMESPACE --output=yaml --role=$ROLE --user=$USER
```

Example manifest:

```yaml { linenos=inline }
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dev-admin
  namespace: dev
roleRef:
  kind: Role
  name: dev-admin
  apiGroup: rbac.authorization.k8s.io
subjects:
- kind: User
  name: dillinger
  apiGroup: rbac.authorization.k8s.io
```
