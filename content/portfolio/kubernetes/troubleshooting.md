+++
title = "Troubleshooting"
draft = true
weight = 75
+++

**Applications**

When troubleshooting application stack, walk through all endpoints, port values, labels, selectors, environment variables, volumes, volume mounts, configmaps and secrets for the complete stack. Do this from ingress all the way back to the backend applications.

**Controlplane**

Check controlplane components using the `kubectl describe` command. Check logs and events for any failed components. Edit the manifest file to correct any misconfigutaion.

**Nodes**

Get the resource output for a node and then describe a node that shows as `NotReady`. Check the logs and events for the node. Check CPU, memory and disk usage on the node. Check the kubelet service on the node. Check networking to the node. Check for any expired certificates for the kublet.

To check is a certificate has expired for the kubelet:

```shell
openssl x509 -in /var/lib/kubelet/$NAME.crt -text
```

**Scheduler**

If the status of a pod is in the `Pending` state, then it may be because the pod could not be schedulled. You must look for reasons why the pod could not be schedulled. Does the pod show a `Node` value of `<none>` in the describe output? Is there issues with the scheduler? Could there be an issue with taints or tollerations?
