+++
title = "Custom columns"
draft = false
weight = 21
+++

Custom columns allows you to customise table output.

Example usage of custom columns output (using a JSONPath syntax):

```shell
kubectl get pods --namespace=default --output=custom-columns=NAME:metadata.name,IP:status.podIP,NODE:spec.nodeName
```

| NAME                   | IP          | NODE                |
|------------------------|-------------|---------------------|
| nginx-5ccbbcf759-28tlk | 10.244.1.18 | lab-cluster-worker2 |
| nginx-5ccbbcf759-29dz5 | 10.244.2.11 | lab-cluster-worker  |
| nginx-5ccbbcf759-w5k65 | 10.244.2.12 | lab-cluster-worker  |
