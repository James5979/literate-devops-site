+++
title = "ServiceAccount"
draft = false
weight = 62
+++

A `ServiceAccount` is used by a container process to authenticate with the Kubernetes API from a [pod](/portfolio/kubernetes/pod/).


## Create a service account {#create-a-service-account}

Imperative command:

```shell
kubectl create serviceaccount $NAME --dry-run=client --namespace=default --output=yaml
```

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: ServiceAccount
metadata:
  name: pod-reader
  namespace: default
```

Describe a service account:

```shell
kubectl describe serviceaccounts/$NAME --namespace=default
```


## Associate a service account with a rolebinding {#associate-a-service-account-with-a-rolebinding}

You can use roles and role bindings, along with service accounts, to control what a pod has access to.

Imperative command:

```shell
kubectl create rolebinding $NAME --dry-run=client --namespace=$NAMESPACE --output=yaml --role=$ROLE --serviceaccount=$NAMESPACE:$SERVICE_ACCOUNT
```

Example manifest:

```yaml { linenos=inline }
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader
  namespace: default
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
subjects:
- kind: ServiceAccount
  name: pod-reader
  namespace: default
```


## Associate a pod with a service account {#associate-a-pod-with-a-service-account}

Example manifest:

```yaml { linenos=inline, hl_lines=["15"] }
apiVersion: v1
kind: Pod
metadata:
  name: pod-reader
  labels:
    app: pod-reader
spec:
  containers:
  - name: busybox
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "sleep"
    - "3600"
  serviceAccountName: pod-reader
```

**CKA Exam Tip**: use the `kubectl explain Pod.spec` command to access the local documentation relating to the `serviceAccountName` field.

Check that the `TokenRequest API` has allocated a token to the pod (containing the service account):

```shell
kubectl exec pods/pod-reader --stdin --tty --  /bin/cat /var/run/secrets/kubernetes.io/serviceaccount/token
```
