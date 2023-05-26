+++
title = "Kubernetes upgrades"
draft = false
weight = 52
+++

Upgrade a Kubernetes cluster.


## Controlplane Upgrade {#controlplane-upgrade}

Drain the controlplane node (only if it is being used to run workloads):

```shell
kubectl drain nodes/$NAME --force --ignore-daemonsets
```

Resource output for the controlplane:

```shell
kubectl get nodes/$NAME
```

Run the following commands directly from the controlplane.

Check the version information for the `kubeadm` package:

```shell
apt-cache policy kubeadm | grep --extended-regexp 'Candidate|Installed'
```

Upgrade kubeadm:

```shell
apt-get update && apt-get --allow-change-held-packages --assume-yes install kubeadm=$VERSION
```

Check the kubeadm version (after performing the upgrade):

```shell
kubeadm version
```

Print the upgrade plan:

```shell
kubeadm upgrade plan
```

Apply the upgrade to the controlplane components:

```shell
kubeadm upgrade apply $VERSION
```

Check the package version of the `kubelet`:

```shell
apt-cache policy kubelet | grep --extended-regexp 'Candidate|Installed'
```

Upgrade the kublet on the controlplane:

```shell
apt-get --allow-change-held-packages --assume-yes install kubelet=1.20.5-00
```

Restart the kubelet:

```shell
systemctl daemon-reload
systemctl restart kubelet.service
```

Uncordon the controlplane (if necessary):

```shell
kubectl uncordon nodes/$NAME
```

Check that the kubelet has been upgraded to the correct version:

```shell
kubectl get nodes/$NAME
```


## Worker Upgrade {#worker-upgrade}

Drain the worker node:

```shell
kubectl drain nodes/$NAME --force --ignore-daemonsets
```

Run the following commands directly from the worker node.

Check the version information for the `kubeadm` package:

```shell
apt-cache policy kubeadm | grep --extended-regexp 'Candidate|Installed'
```

Upgrade kubeadm:

```shell
apt-get update && apt-get --allow-change-held-packages --assume-yes install kubeadm=$VERSION
```

Check the kubeadm version (after performing the upgrade):

```shell
kubeadm version
```

Apply the upgrade to the worker node:

```shell
kubeadm upgrade node config --kubelet-version $VERSION
```

Check the package version of the `kubelet`:

```shell
apt-cache policy kubelet | grep --extended-regexp 'Candidate|Installed'
```

Upgrade the kublet on the worker node:

```shell
apt-get --allow-change-held-packages --assume-yes install kubelet=$VERSION
```

Restart the kubelet:

```shell
systemctl daemon-reload
systemctl restart kubelet.service
```

Uncordon the worker:

```shell
kubectl uncordon nodes/$NAME
```

Check that the kubelet has been upgraded to the correct version:

```shell
kubectl get nodes/$NAME
```
