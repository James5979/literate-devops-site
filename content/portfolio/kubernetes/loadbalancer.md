+++
title = "LoadBalancer"
draft = false
weight = 11
+++

The `LoadBalancer` service is similar to a `ClusterIP` service, except that it is predominantly used in conjunction with public cloud providers. A provider will offer an external load balancer whenever a loadbalancer service is created.

For instance, here is an [example](https://www.civo.com/learn/managing-external-load-balancers-on-civo) of just one type of loadbalancer service, offered by the [Civo](https://civo.com/) public cloud provider.

**Note**: a loadbalancer service is not to be mistaken for the internal load balancer that Kubernetes provides (automatically) between [pods](/portfolio/kubernetes/pod/) within a [replicaset](/portfolio/kubernetes/replicaset/).


## Create a loadbalancer service {#create-a-loadbalancer-service}

Imperative command:

```shell
kubectl create service loadbalancer $NAME --namespace=default --tcp=$PORT:$TARGETPORT
```

**Pitfall**: the label selector on the service will have to be updated manually, since this command does not directly expose any application. Use the `kubectl expose` [command](/portfolio/kubernetes/exposing/) if you want to have the labels set to the correct values automatically.

Example manifest:

```yaml { linenos=inline, hl_lines=["8-9","18"] }
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
  annotations:
    #kubernetes.civo.com/loadbalancer-algorithm: round_robin
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
  externalTrafficPolicy: Cluster
```

A Kubernetes loadbalancer service makes extensive use of metadata annotations, so that the external load balancer can be configured using the service.

For instance, here is an [example](https://www.civo.com/docs/kubernetes/load-balancers) of the annotations used by the Civo public provider.

**Note**: each public cloud provider will use its own set of annotations, to configure their own external load balancers.

Resource output for a loadbalancer service:

```shell
kubectl get services/$NAME --namespace=default
```
