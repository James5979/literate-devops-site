+++
title = "kubeconfig"
draft = false
weight = 55
+++

Create a cluster configuration file (i.e. if one does not exist):

```yaml { linenos=inline }
apiVersion: kubeadm.k8s.io/v1beta3
kind: ClusterConfiguration
clusterName: "kind-lab-cluster"
controlPlaneEndpoint: "127.0.0.1:42553"
certificatesDir: "/etc/kubernetes/pki"
```

Generate a new kubeconfig (for your user) in order to enforce RBAC rules. Unlike the admin user, which will simply bypass RBAC altogether:

```shell
kubeadm kubeconfig user --config $HOME/manifests/yaml/examples/admin.yaml --client-name dillinger > dillinger-kubeconfig.yaml
```
