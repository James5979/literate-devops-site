+++
title = "initContainers"
description = "Init containers."
draft = false
weight = 50
+++

An `initContainer` is identical to any other container in Kubernetes, albeit it is short-lived and is intended to perform application bootstrap at creation time within a pod.


## Create a pod containing an init container {#create-a-pod-containing-an-init-container}

In this example, an init container will clone a remote git repository (to a volume) and then consume the repository using the nginx container within the pod.

Example manifest:

```yaml { linenos=inline, hl_lines=["16-32"] }
kind: Pod
apiVersion: v1
metadata:
  name: hugo
  namespace: default
  labels:
    app: hugo
spec:
  imagePullSecrets:
  - name: regcred
  securityContext:
    seccompProfile:
      type: RuntimeDefault
    fsGroup: 101
    fsGroupChangePolicy: "OnRootMismatch"
  initContainers:
  - name: git-sync
    image: registry.k8s.io/git-sync/git-sync:v3.6.6
    imagePullPolicy: IfNotPresent
    env:
    - name: GIT_SYNC_BRANCH
      value: "main"
    - name: GIT_SYNC_DEPTH
      value: "1"
    - name: GIT_SYNC_ONE_TIME
      value: "true"
    - name: GIT_SYNC_PERMISSIONS
      value: "0750"
    - name: GIT_SYNC_REPO
      value: "https://github.com/James5979/literate-devops-site/"
    - name: GIT_SYNC_ROOT
      value: "/usr/local/src"
    securityContext:
      runAsUser: 65533
      runAsGroup: 101
      allowPrivilegeEscalation: false
    volumeMounts:
    - name: markup
      mountPath: /usr/local/src
    resources:
      requests:
        cpu: "100m"
        memory: "128Mi"
        ephemeral-storage: "2Gi"
      limits:
        cpu: "200m"
        memory: "256Mi"
        ephemeral-storage: "4Gi"
  containers:
  - name: hugo
    image: ghcr.io/james5979/hugo:0.112.2
    imagePullPolicy: IfNotPresent
    ports:
    - name: http
      containerPort: 1313
    args:
    - "server"
    - "--bind=0.0.0.0"
    - "--config=/usr/local/src/literate-devops-site/config.yaml"
    - "--contentDir=/usr/local/src/literate-devops-site/content"
    - "--environment=production"
    - "--layoutDir=/usr/local/src/literate-devops-site/layouts"
    - "--port=1313"
    - "--themesDir=/usr/local/src/literate-devops-site/themes"
    securityContext:
      runAsUser: 101
      runAsGroup: 101
      allowPrivilegeEscalation: false
    volumeMounts:
    - name: markup
      mountPath: /usr/local/src
      readOnly: true
    resources:
      requests:
        cpu: "100m"
        memory: "128Mi"
        ephemeral-storage: "2Gi"
      limits:
        cpu: "200m"
        memory: "256Mi"
        ephemeral-storage: "4Gi"
  volumes:
  - name: markup
    emptyDir: {}
```
