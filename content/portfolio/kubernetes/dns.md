+++
title = "DNS"
description = "Kubernetes DNS."
draft = false
weight = 14
+++

Kubernetes uses a built-in DNS server called CoreDNS, allowing applications to connect to each other using domain names.

To use the Fully Qualified Domain Name (FQDN) for a Kubernetes [service](/portfolio/kubernetes/clusterip/), specify it using the form:

```text
<my-service>.<namespace>.svc.<cluster-name>
```

To connect to a Kubernetes service in a different namespace, you must use the FQDN (see above), or the shorthand:

```text
<my-service>.<namespace>
```

To connect to a [pod](/portfolio/kubernetes/pod/) in a different namespace, you must always use the Fully Qualified Domain Name of the pod:

```text
<my-pod>.<namespace>.pod.<cluster-name>.
```

**Note**: always use a Kubernetes service to connect to a pod and not the FQDN, since domain names for pods are transient by design.

To find the cluster name (for a kubeadm deployed cluster):

```shell
echo -n "Cluster name: " && kubectl get configmaps/kubeadm-config --namespace=kube-system --output=jsonpath='{.data.ClusterConfiguration}' | awk '/dnsDomain/ { print $2}'
```

Cluster name: cluster.local


## Examples {#examples}

<style>.table-nocaption table { width: 70%;  }</style>

<div class="ox-hugo-table table-nocaption">

| Resource Object | Fully Qualified Domain Name          |
|-----------------|--------------------------------------|
| Pod             | 10-96-0-10.default.pod.cluster.local |
| Service         | nginx.default.svc.cluster.local      |

</div>


## Testing Kubernetes DNS {#testing-kubernetes-dns}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: default
  labels:
    app: busybox
spec:
  containers:
  - name: busybox
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "nc"
    - "-v"
    - "-w"
    - "5"
    - "-z"
    - "nginx.dev"
    - "80"
---
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: dev
  labels:
    app: nginx
spec:
  selector:
    app: nginx
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: dev
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: docker.io/library/nginx:1.24.0
    imagePullPolicy: IfNotPresent
    ports:
    - name: http
      containerPort: 80
```

View the logs for the busybox pod and see whether the domain `nginx.dev` was resolvable:

```shell
echo -n "Output: " && kubectl logs pods/busybox --namespace=default
```

Output: nginx.dev (10.43.16.241:80) open

The output shows that `nginx.dev` was resolvable.
