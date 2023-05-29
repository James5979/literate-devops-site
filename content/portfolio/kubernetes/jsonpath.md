+++
title = "JSONPath"
description = "JSONPath expressions."
draft = false
weight = 72
+++

Filter fields using JSONPath.


## Examples {#examples}

**1.** CPU capacity.

**a)** Show the total CPU capacity per node:

```shell
kubectl get nodes --output=jsonpath='{.items[*].metadata.name }{"\n"}{ .items[*].status.capacity.cpu}'
```

| lab-cluster-control-plane | lab-cluster-worker | lab-cluster-worker2 |
|---------------------------|--------------------|---------------------|
| 12                        | 12                 | 12                  |

**b)** Show the total CPU capacity per node, with titles:

```shell
kubectl get nodes --output=jsonpath='{"NAME"}{"\t"}{"CPU"}{"\n"}{.items[*].metadata.name}{"\t"}{.items[*].status.capacity.cpu}'
```

| NAME                                                             | CPU      |
|------------------------------------------------------------------|----------|
| lab-cluster-control-plane lab-cluster-worker lab-cluster-worker2 | 12 12 12 |

**c)** Show the total CPU capacity per node, using a titled loop:

```shell
kubectl get nodes --output=jsonpath='{"NAME"}{"\t"}{"CPU"}{"\n"}{range.items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\n"}{end}'
```

| NAME                      | CPU |
|---------------------------|-----|
| lab-cluster-control-plane | 12  |
| lab-cluster-worker        | 12  |
| lab-cluster-worker2       | 12  |

**2.** Show all container ports in a pod.

**Prerequisite**: create a multi-container pod:

```yaml { linenos=inline }
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
    ports:
    - name: port1
      containerPort: 80
  - name: busybox1
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "sleep"
    - "3600"
    ports:
    - name: port2
      containerPort: 1234
  - name: busybox2
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "sleep"
    - "3600"
    ports:
    - name: port3
      containerPort: 2345
```

List all container ports in a pod:

```shell
kubectl get pods/$NAME --namespace=default --output=jsonpath='{"CONTAINER"} {"PORT"}{"\n"}{range.spec.containers[*]}{.name} {.ports[*].containerPort}{"\n"}{end}'
```

| CONTAINER | PORT |
|-----------|------|
| nginx     | 80   |
| busybox1  | 1234 |
| busybox2  | 2345 |

**3.** Show the names of users defined in the kubeconfig:

```shell
echo -n "Output: " && kubectl config view --kubeconfig=$HOME/.kube/config --output=jsonpath='{.users[*].name}'
```

Output: kind-lab-cluster

**4.** Filter predicates.

**a)** Show the `ready` status for the `nginx` container:

```shell
echo -n "Ready: " && kubectl get pods/$NAME --namespace=default --output=jsonpath='{.status.containerStatuses[?(@.image == "docker.io/library/nginx:1.24.0")].ready}'
```

Ready: true

**b)** Show the `restart count` for the `busybox` container:

```shell
kubectl get pods/$NAME --namespace=default --output=jsonpath='{"NAME"}{"\t"}{"RESTARTS"}{"\n"}{range.status.containerStatuses[?(@.image == "docker.io/library/busybox:1.36.0")]}{.name}{"\t"}{.restartCount}{"\n"}{end}'
```

| NAME     | RESTARTS |
|----------|----------|
| busybox1 | 1        |
| busybox2 | 1        |

**c)** Show all contexts defined for the `kind-lab-cluster` user:

```shell
kubectl config view --kubeconfig=$HOME/.kube/config --output=jsonpath='{.contexts[?(@.context.user == "kind-lab-cluster")]}' | jq .
```

{
  "context": {
    "cluster": "kind-lab-cluster",
    "user": "kind-lab-cluster"
  },
  "name": "kind-lab-cluster"
}
