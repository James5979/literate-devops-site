+++
title = "DNS"
draft = false
weight = 14
+++

Kubernetes uses a built-in DNS server, allowing applications to connect to each other using domain names.

To connect to a Kubernetes application, expose the application using a Kubernetes [service](/portfolio/kubernetes/service/), then use the domain name for that service as the endpoint in any subsequent application configuration.

To use the Fully Qualified Domain Name (FQDN) for a Kubernetes service, specify it using the form:

```text
<my-service>.<namespace>.svc.<cluster-name>
```

To connect to a Kubernetes service in a different namespace, you must either use the FQDN (see above), or using this shorthand:

```text
<my-service>.<namespace>
```

To connect to a [pod](/portfolio/kubernetes/pod/) in a different namespace, you must always use the Fully Qualified Domain Name of the pod:

```text
<my-pod>.<namespace>.pod.<cluster-name>.
```

**Note**: always use a Kubernetes service to connect to another pod, i.e. do not use the FQDN for a pod within a Kubernetes application, since pod domain names are transient.

To find out the cluster name in a kubeadm deployed Kubernetes cluster:

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
