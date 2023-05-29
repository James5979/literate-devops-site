+++
title = "Ingress Controller"
description = "Kubernetes ingress controller."
draft = false
weight = 70
+++

All ingress controllers provide access to the Kubernetes `Ingress` resource. The NGINX ingress controller is just one of the many ingress controllers to choose from.


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
