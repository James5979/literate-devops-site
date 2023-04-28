+++
title = "LoadBalancer"
draft = false
weight = 11
+++

The `LoadBalancer` service is similar to a `NodePort` service, except that it is predominantly used with public cloud providers. The provider will be able to offer you an external load balancer that will go in front of this service.

Example load balancer configuration: offered by the [Civo](https://www.civo.com/learn/managing-external-load-balancers-on-civo) public cloud provider.

**Note**: Kubernetes already provides an internal load balancer when communicating with pods that are replicas.


## Create a loadbalancer service {#create-a-loadbalancer-service}

Imperative command:

```shell
kubectl create service loadbalancer $NAME --namespace=default --tcp=$PORT:$TARGETPORT
```

**Pitfall**: the label selector on the service will have to be updated manually, since this command does not directly expose any application. Use the `kubectl expose` command if you want to have the labels set to the correct values automatically.

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  type: LoadBalancer
```

Resource output for a loadbalancer service:

```shell
kubectl get services/$NAME --namespace=default
```
