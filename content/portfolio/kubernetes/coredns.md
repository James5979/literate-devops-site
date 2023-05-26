+++
title = "CoreDNS"
draft = true
weight = 69
+++

You can resolve the name of a service by using just its name, when you are in the same namespace with the service.

All services within the cluster are resolvable (from within all namespaces) using the shorthand `<service>.<namespace>` for lookups. Pods outside of a namespace are only resolvable using their FQDN, i.e. =&lt;pod&gt;.&lt;namespace&gt;.pod.&lt;cluster-name&gt;.

CoreDNS is the default name resolution service deployed within Kubernetes clusters. CoreDNS comprises of a both a deployment and a service, with the configuration being stored within a configmap. Pods point towards CoreDNS for name resolution using entries stored within the `/etc/resolv.conf` for a pod. It is the job of the kubelet to ensure that the configuring of `/etc/resolv.conf` points to CoreDNS.

If you require an application to point to a service within the cluster, e.g. a database that is reachable on a port, and if that service exists within a different namespace relative to that application, then all references to that service must be written using the `<service>.<namespace>` notation. This way, CoreDNS can resolve any IP address for a service that sits outside of the namespace for the application. So, whenever an application requires a reference to a service outside of its namespace, you will have to use this notation. This is relevant to configmaps, environment varibles, etc, where the service needs to be referenced as an endpoint.

For example, you can check that port `80` is open for the `nginx` service, running in the `dev` namespace, from the output of a command using a pod that is in the `default` namespace:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
  namespace: default
  labels:
    app: test-pod
spec:
  containers:
  - name: busybox
    image: busybox
    args: ["nc", "-v", "-w", "5", "-z", "nginx.dev", "80"]
---
apiVersion: v1
kind: Namespace
metadata:
  name: dev
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
  - port: 80
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
    image: nginx
    ports:
    - containerPort: 80
```

```shell
kubectl logs pods/test-pod
```

From the output, we can see that both port `80` is open and that the name `nginx.dev` was resolvable.

**Note**: if you replace the netcat command with `nc -v -w 5 -z nginx 80`, then you will notice that the command will fail, since the `nginx` service is nolonger resolvable.
