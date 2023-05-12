+++
title = "ClusterIP"
draft = false
weight = 9
+++

Since IP addresses for [pods](/portfolio/kubernetes/pod/) are transient, `ClusterIP` allows pods to communicate internally by providing a fixed IP address.


## Create a clusterip service {#create-a-clusterip-service}

Imperative command:

```shell
kubectl create service clusterip $NAME --namespace=default --tcp=$PORT:$TARGETPORT
```

**Pitfall**: the label selector on the service will have to be updated manually, since this command does not directly expose any application. Use the `kubectl expose` command if you want to have the labels set to the correct values automatically.

Example manifest:

```yaml { linenos=inline, hl_lines=["16"] }
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
  - name: http
    port: 80
    protocol: TCP
    targetPort: 80
  type: ClusterIP
```

Resource output for a clusterip service:

```shell
kubectl get services/$NAME --namespace=default
```
