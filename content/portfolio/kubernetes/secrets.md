+++
title = "Secrets"
draft = false
weight = 44
+++

A `Secret` is an object that contains a small amount of sensitive data. --- [1]

Imperative command:

```shell
kubectl create secret generic $NAME --dry-run=client --from-literal=$SECRET1 --from-literal=$SECRET2 --from-literal=$SECRET3 --output=yaml
```

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
data:
  GOLDENEYE_CODES: aWFtaW52aW5jaWJsZQ==
  PASSWORD: dDBwLTUzY3IzdCE=
  PRIMARY_MISSION: dGVh
type: Opaque
```

Resource output for a secret:

```shell
kubectl get secrets/$NAME --namespace=default
```

Describe a secret:

```shell
kubectl describe secrets/$NAME --namespace=default
```

To get the value for a secret, you must output the secret in YAML format, and then decode the value:

```shell
echo -n "Secret value: " && kubectl get secrets/$NAME --namespace=default --output=yaml | awk '/PASSWORD/ { print $2 }' | base64 --decode
```

Secret value: t0p-53cr3t!


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/configuration/secret/)
