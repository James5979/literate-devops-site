+++
title = "Ingress Controller"
draft = true
weight = 70
+++

The Kubernetes `NGINX ingress controller`.


## Installation {#installation}

Create the directory structure:

```shell
mkdir --parents $HOME/manifests/yaml/ingress
```

Download the manifest file for the Kubernetes NGINX ingress controller:

```shell
wget --output-document $HOME/manifests/yaml/ingress/nginx-ingress-controller.yaml https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.0/deploy/static/provider/cloud/deploy.yaml
```

Apply the manifest:

```shell
kubectl apply --filename=$HOME/manifests/yaml/ingress/nginx-ingress-controller.yaml
```
