+++
title = "ConfigMaps and Secrets as Volumes"
draft = false
weight = 46
+++

Mount volumes to a container (in a [pod](/portfolio/kubernetes/pod/)) using both a [configmap](/portfolio/kubernetes/configmaps/) and a [secret](/portfolio/kubernetes/secrets/) respectively.

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: default
  labels:
    app: busybox
spec:
  containers:
  - name: busybox
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    args:
    - "sleep"
    - "3600"
    volumeMounts:
    - name: configs
      mountPath: /etc/my-app
    - name: sensitive-configs
      mountPath: /etc/my-other-app
  volumes:
  - name: configs
    configMap:
      name: my-configmap
  - name: sensitive-configs
    secret:
      secretName: my-secret
```

Test that the key-value pairs for both the configmap and secret all exist as individual files within the container:

```shell
for i in my-app my-other-app ; do kubectl exec pods/busybox -- /bin/ls /etc/$i ; done
```
