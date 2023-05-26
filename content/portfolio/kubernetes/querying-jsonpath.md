+++
title = "Querying JSONPath"
draft = true
weight = 72
+++

Query a resource using JSONPath:

```shell
kubectl get nodes --namespace=default --output=jsonpath='{.items[*].metadata.name }{"\n"}{ .items[*].status.capacity.cpu}'
```

Query an object using JSONPath:

```shell
kubectl get pods/$NAME --namespace=default --output=jsonpath='{.spec.containers[*].name}{"\t"}{.spec.containers[*].ports[*].containerPort}'
```

**Pitfall**: notice when querying a specific object from a resource that the `items` array is missing.

**Tip**: the two queries above is all that is required for the  exam.

Add titles to the queried output:

```shell
kubectl get nodes --namespace=default --output=jsonpath='{"NAME"}{"\t"}{"CPU"}{"\n"}{.items[*].metadata.name}{"\t"}{.items[*].status.capacity.cpu}'
```

This does not look very pretty, use a loop to make the output more tidy:

```shell
kubectl get nodes --namespace=default --output=jsonpath='{"NAME"}{"\t"}{"CPU"}{"\n"}{range.items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'
```

Much better!

Create a multi-container pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: docker.io/library/nginx:1.24.0
    imagePullPolicy: IfNotPresent
  - name: busybox
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "sleep"
    - "3600"
```

Filter the output using a filter predicate:

```shell
kubectl get pods/$NAME --output=jsonpath='{.status.containerStatuses[?(@.image == "docker.io/library/nginx:latest")].ready}'
```

```shell
kubectl get pods/$NAME --output=jsonpath='{.status.containerStatuses[?(@.image == "docker.io/library/busybox:latest")].restartCount}'
```

Query the kubeconfig file:

```shell
kubectl config view --kubeconfig=$HOME/.kube/config --output=jsonpath='{.users[*].name}'
```

Filter the kubeconfig file using a filter predicate:

```shell
kubectl config view --kubeconfig=$HOME/.kube/config --output=jsonpath='{.contexts[?(@.context.user == "kind-lab-cluster")]}'
```
