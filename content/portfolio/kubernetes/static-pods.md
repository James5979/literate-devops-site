+++
title = "Static pods"
draft = false
weight = 32
+++

Static pods are pods created directly by the kubelet, independent of the Kubernetes API server. They are created from manifest files placed within a specific directory on the host.

To find the location of where the static pod manifests are stored, you must first find the value of the `staticPodPath`, found using the configuration file of the kubelet. To find where this configuration file is stored, view the command field for the running kubelet process:

```shell
echo -n "Kubelet configuration file: " && ps -ef | grep --extended-regexp '/usr/bin/kubelet' | grep --only-matching "\-\-config=.*.yaml" | sed --expression "s/--config=//"
```

```text
Kubelet configuration file: /var/lib/kubelet/config.yaml
```

Now find the value for the `staticPodPath`:

```shell
echo -n "Static pods are stored here: " && awk '/staticPodPath/ { print $2 }' /var/lib/kubelet/config.yaml
```

```text
Static pods are stored here: /etc/kubernetes/manifests
```

**Pitfall**: sometimes the static pod path is defined using the command line argument `--pod-manifest-path`. You should check whether this option exists within the command created by the running kubelet process.

**Note**: even though the Kubernetes API server is aware of static pods, it does not manage them in any way. When viewing a static pod object using the Kubernetes API server (e.g. when using `kubectl`), the pod will appear with the name of the host node appended to the end of its name. You can use this information to guess whether the object is most likely a static pod.

To determine (for certain) whether a pod is a static pod, use the `kubectl describe` command and check the value of the `kind` field (under `ownerReferences`) and if the value is equal to `Node`, then the object has to be a static pod:

```shell
kubectl get pods/$NAME --namespace=kube-system --output=yaml | grep --after-context=5 ownerReferences | grep 'kind:'
```
