+++
title = "LoadBalancer"
draft = false
weight = 11
+++

The `LoadBalancer` service is similar to a `ClusterIP` service, except that it is predominantly used in conjunction with public cloud providers. A provider will offer an external load balancer whenever a loadbalancer service is created.

For instance, here is an [example](https://www.civo.com/learn/managing-external-load-balancers-on-civo) of just one type of loadbalancer service, offered by the [Civo](https://civo.com/) public cloud provider.

**Note**: a loadbalancer service should **not** be mistaken for the internal load balancer that Kubernetes provides between [pods](/portfolio/kubernetes/pod/) (in a [replicaset](/portfolio/kubernetes/replicaset/)).


## Create a loadbalancer service {#create-a-loadbalancer-service}

Imperative command:

```shell
kubectl create service loadbalancer $NAME --namespace=default --tcp=$PORT:$TARGETPORT
```

**Pitfall**: the selector for the service will have to be updated manually, since this command does not directly expose an application. Use the `kubectl expose` [command](/portfolio/kubernetes/exposing/) if you wish to have the selector labels set to their correct values automatically.

Example manifest:

```yaml { linenos=inline, hl_lines=["9-10","19-20"] }
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
  annotations:
    kubernetes.civo.com/loadbalancer-algorithm: least_connections
    kubernetes.civo.com/firewall-id: 9d3d230e-c2c4-44fc-834f-ce36ce83419f
spec:
  selector:
    app: nginx
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 80
  type: LoadBalancer
  externalTrafficPolicy: Local
```

The Kubernetes loadbalancer service uses metadata annotations to provide extra configuration options for external load balancers, e.g. see the annotations used to configure a [Civo](https://www.civo.com/docs/kubernetes/load-balancers) load balancer.

Resource output for a loadbalancer service:

```shell
kubectl get services/$NAME --namespace=default
```
