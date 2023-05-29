+++
title = "ClusterConfiguration"
draft = false
weight = 47
+++

Cluster configuration for kubeadm deployed clusters.


## Output the cluster config {#output-the-cluster-config}

Display the configmap containing the configuration of the cluster:

```shell
kubectl get configmaps/kubeadm-config --namespace=kube-system --output=yaml
```

Dump the current cluster state to STDOUT:

```shell
kubectl cluster-info dump --all-namespaces --output=yaml
```
