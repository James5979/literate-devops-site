+++
title = "Metrics Server"
draft = false
weight = 35
+++

The Kubernetes `Metrics Server` is a scalable, efficient source of container resource metrics. --- [1]


## Installation {#installation}

Create the directory structure:

```shell
mkdir --parents $HOME/git/manifests/yaml/metrics
```

Download the manifest file for the Kubernetes Metrics Server:

```shell
wget --output-document=$HOME/manifests/yaml/metrics/metrics-server.yaml https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Modify the container arguments for the Kubernetes Metrics Server:

```yaml
...
spec:
      containers:
      - args:
        - ...
        - --kubelet-insecure-tls=true
...
```

Apply the manifest:

```shell
kubectl apply --filename=$HOME/manifests/yaml/metrics/metrics-server.yaml
```

Wait a minute (or two) for the Kubernetes metrics to populate.

Check that the Metrics Server is operational by simply running a few `kubectl top` [commands](/portfolio/kubernetes/top/).

**Tip**: you will not have to setup the Kubernetes Metrics Server in an exam context for the [CKA](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/).


## References {#references}

1.  [github.com](https://github.com/kubernetes-sigs/metrics-server#kubernetes-metrics-server)
