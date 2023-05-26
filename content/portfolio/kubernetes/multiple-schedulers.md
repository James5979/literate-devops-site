+++
title = "Multiple schedulers"
draft = false
weight = 34
+++

Create a custom scheduler.


## Create a service account for the custom scheduler {#create-a-service-account-for-the-custom-scheduler}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: ServiceAccount
metadata:
  name: custom-scheduler
  namespace: kube-system
  labels:
    scheduler: custom
```


## Create a pair of cluster role bindings {#create-a-pair-of-cluster-role-bindings}

Create the first cluster role binding for the service account, specifying the value `system:kube-scheduler` as the cluster role.

Example manifest:

```yaml { linenos=inline }
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: custom-scheduler
  labels:
    scheduler: custom
roleRef:
  kind: ClusterRole
  name: system:kube-scheduler
  apiGroup: rbac.authorization.k8s.io
subjects:
- kind: ServiceAccount
  name: custom-scheduler
  namespace: kube-system
```

Create the second cluster role binding for the service account, specifying the value `system:volume-scheduler` as the cluster role.

Example manifest:

```yaml { linenos=inline }
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: custom-volume-scheduler
  labels:
    scheduler: custom
roleRef:
  kind: ClusterRole
  name: system:volume-scheduler
  apiGroup: rbac.authorization.k8s.io
subjects:
- kind: ServiceAccount
  name: custom-scheduler
  namespace: kube-system
```


## Create a configmap containing the configutaion for the custom-scheduler {#create-a-configmap-containing-the-configutaion-for-the-custom-scheduler}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: ConfigMap
metadata:
  name: custom-scheduler
  namespace: kube-system
  labels:
    scheduler: custom
data:
  custom-scheduler-config.yaml: |
    apiVersion: kubescheduler.config.k8s.io/v1beta2
    kind: KubeSchedulerConfiguration
    profiles:
    - schedulerName: custom-scheduler
    leaderElection:
      leaderElect: false
```


## Create the custom scheduler {#create-the-custom-scheduler}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Pod
metadata:
  name: custom-scheduler
  namespace: kube-system
  labels:
    scheduler: custom
spec:
  containers:
  - name: custom-scheduler
    image: registry.k8s.io/kube-scheduler:v1.26.0
    imagePullPolicy: IfNotPresent
    command:
    - /usr/local/bin/kube-scheduler
    - --config=/etc/kubernetes/custom-scheduler/custom-scheduler-config.yaml
    securityContext:
      privileged: false
    resources:
      requests:
        cpu: "0.1"
    volumeMounts:
      - name: custom-scheduler
        mountPath: /etc/kubernetes/custom-scheduler
    livenessProbe:
      httpGet:
        path: /healthz
        port: 10259
        scheme: HTTPS
      initialDelaySeconds: 15
    readinessProbe:
      httpGet:
        path: /healthz
        port: 10259
        scheme: HTTPS
  nodeName: lab-cluster-control-plane
  serviceAccountName: custom-scheduler
  volumes:
    - name: custom-scheduler
      configMap:
        name: custom-scheduler
  hostNetwork: false
  hostPID: false
```


## List all custom scheduler objects using a selector {#list-all-custom-scheduler-objects-using-a-selector}

Resource output for the `custom-scheduler`:

```shell
kubectl get sa,clusterrolebinding,cm,po --namespace=kube-system --output=wide --selector=scheduler=custom
```


## Test the custom scheduler {#test-the-custom-scheduler}

Example manifest:

```yaml { linenos=inline }
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
    scheduler: custom
spec:
  containers:
  - name: nginx
    image: docker.io/library/nginx:1.24.0
    imagePullPolicy: IfNotPresent
  schedulerName: custom-scheduler
```

Verify that the pod was scheduled using the `custom-scheduler`:

```shell
kubectl describe pods/$NAME --namespace=$NAMESPACE | grep --extended-regexp "Message|custom-scheduler"
```

Show all recent events for the pod:

```shell
kubectl get events --field-selector=involvedObject.name=$NAME --namespace=default --output=wide
```
