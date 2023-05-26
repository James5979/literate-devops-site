+++
title = "Sidecars"
description = "Sidecar containers."
draft = false
weight = 49
+++

{{< figure src="/images/sidecar.jpg" width="1200px" >}}


## What is a sidecar? {#what-is-a-sidecar}

A sidecar is a secondary container within a [pod](/portfolio/kubernetes/pod), analogous to the sidecar on a motorcyle.

It is considered good practise to use only a single container per pod, avoiding multi-container pods altogether, unless two or more applications require tightly-coupled resources, such as sharing a network namespace, or using the same storage [volumes](/portfolio/kubernetes/volumes/).

The `kubectl logs` [command](/portfolio/kubernetes/logs) requires that applications write to STDOUT. Sometimes, legacy application ported to Kubernetes will still send messages to logs instead of STDOUT. In these situations, you can use a sidecar to parse the log files and redirect their contents to STDOUT. Another example, is using a sidecar to clone artifacts that can be used as content and served using a web server, such as [git-sync](https://github.com/kubernetes/git-sync) with NGINX.


## Share a volume with a sidecar {#share-a-volume-with-a-sidecar}

Example manifest:

```yaml { linenos=inline, hl_lines=["21-31"] }
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  volumes:
  - name: shared-data
    emptyDir: {}
  containers:
  - name: nginx
    image: docker.io/library/nginx:1.24.0
    imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 80
    volumeMounts:
    - name: shared-data
      mountPath:  /usr/share/nginx/html
  - name: content
    image: docker.io/library/busybox:1.36.0
    imagePullPolicy: IfNotPresent
    command:
    - "/bin/sh"
    args:
    - "-c"
    - "echo 'Hello from the sidecar container!' > /nginx-data/index.html && sleep 3600"
    volumeMounts:
    - name: shared-data
      mountPath: /nginx-data
```

Test that the sidecar is able to pass content through to the `nginx` container:

```shell
echo -n "Output: " && kubectl exec pods/nginx --container=nginx -- /bin/cat  /usr/share/nginx/html/index.html
```

Output: Hello from the sidecar container!

You can also create a [nodeport](/portfolio/kubernetes/nodeport) service for the `nginx` pod, and access the NGINX application using the `curl` command:

```yaml { linenos=inline }
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

Test output:

```shell
echo -n "Output: " && curl 127.0.0.1:80
```

Output: Hello from the sidecar container!


## References {#references}

-   [Unsplash](https://unsplash.com/photos/38FLdKhz_rM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
