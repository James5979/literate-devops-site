+++
title = "DNS"
draft = false
weight = 14
+++

Kubernetes uses a built-in DNS server, so that applications are able to connect to each other using domain names.

To connect to an application running within a pod, first expose an application using a Kubernetes service, then connect to that application using the domain name created for that service.

To get the Fully Qualified Domain Name (FQDN) for the Kubernetes service, simply connect to one of the endpoints of that service, and check the contents of `/etc/resolv.conf`.

You should be able to find the FQDN for the service by comparing all the IP addresses within `/etc/resolv.conf` to the IP address for the Kubernetes service, and cross-reference the FQDN.

```shell
kubectl exec pods/$NAME --namespace=$NAMESPACE -- /bin/cat /etc/resolv.conf
```

If this appears to be a little arduous, then you can always construct the FQDN yourself, using the canonical form:

```text
<my-service>.<namespace>.svc.<cluster-name>
```

To connect to a service within the same namespace, the domain name is exactly the same as the name given to the Kubernetes service.

To connect to a service within a different namespace, you can either use the FQDN, or you can use the shorthand `<my-service>.<namespace>`.

To connect to a pod in a different namespace, you must always use the FQDN of the pod:

```text
<my-pod>.<namespace>.pod.<cluster-name>.
```

**Note**: always use a service to communicate with a pod, i.e. do not a FQDN, since the domain name for the pod is transient.

To find the name of the cluster (kubeadm-based):

```shell
kubectl get configmaps/kubeadm-config --namespace=kube-system --output=jsonpath='{.data.ClusterConfiguration}' | grep dnsDomain
```

Output:

```text
dnsDomain: cluster.local
```

Example FQDNs (for a pod and a service):

<style>.table-nocaption table { width: 70%;  }</style>

<div class="ox-hugo-table table-nocaption">

| Resource | Fully Qualified Domain Name          |
|----------|--------------------------------------|
| Pod      | 10-96-0-10.default.pod.cluster.local |
| Service  | nginx.default.svc.cluster.local      |

</div>
