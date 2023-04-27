+++
title = "DNS"
draft = false
weight = 14
+++

You can get the FQDN for a service by connecting to one of its endpoints and checking the contents of `resolv.conf`:

```shell
kubectl exec pods/$NAME --namespace=$NAMESPACE -- /bin/cat /etc/resolv.conf
```

The FQDN for Kubernetes services always has the form:

```text
<my-service>.<namespace>.svc.<cluster-name>
```

To connect to a service in a different namespace, use the FQDN, or the shorthand `<my-service>.<namespace>`.

To test connectiviy to a pod in a different namespace, you must always use the FQDN `<my-pod>.<namespace>.pod.<cluster-name>`.

**Note**: always use a service to communicate with a pod, since both their resolvable names and their IP addresses are transient (by design). For troubleshooting purposes though, it is okay to test the connectivity to a pod.

You can find the name of the cluster by running the following command:

```shell
kubectl get configmaps/kubeadm-config --namespace=kube-system --output=jsonpath='{.data.ClusterConfiguration}' | grep dnsDomain
```

Output:

```text
dnsDomain: cluster.local
```

Example of FQDNs for both a pod and its service:

<style>.table-nocaption table { width: 70%;  }</style>

<div class="ox-hugo-table table-nocaption">

| Resource | Fully Qualified Domain Name          |
|----------|--------------------------------------|
| Pod      | 10-96-0-10.default.pod.cluster.local |
| Service  | nginx.default.svc.cluster.local      |

</div>
