+++
title = "NodePort"
tags = ["tip"]
draft = false
weight = 8
+++

A `NodePort` service provides external access to Kubernetes applications by creating a listening port on each node in the cluster. You can connect to the service by simply using the port and IP address from one of the nodes.

**Pitfall**: nodeport can only use ports in the range of `30000` to `32767`.


## Create a nodeport service {#create-a-nodeport-service}

Imperative command:

```shell
kubectl create service nodeport $NAME --namespace=default --node-port=$NODEPORT --tcp=$PORT:$TARGETPORT
```

**Pitfall**: the label selector on the service will have to be updated manually, since this command does not directly expose any application. Use the `kubectl expose` [command](/portfolio/kubernetes/exposing/) if you want to have the labels set to the correct values automatically.

Example manifest:

```yaml { linenos=inline, hl_lines=["13","17"] }
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
    nodePort: 30080
    port: 80
    protocol: TCP
    targetPort: 80
  type: NodePort
```

Resource output for a nodeport service:

```shell
kubectl get services/$NAME --namespace=default
```
