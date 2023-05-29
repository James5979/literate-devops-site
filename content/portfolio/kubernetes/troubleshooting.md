+++
title = "Troubleshooting"
description = "Troubleshooting."
draft = false
weight = 75
+++

**Applications**

When troubleshooting a failed Kubernetes application, walk through all objects in the stack, and verify that there is no misconfiguration present.

**Controlplane**

Troubleshoot a failed controlplane component.

-   Investigate system pods by using the `kubectl describe` command.
-   Check logs and events for failures.
-   Correct any misconfiguration found within the respecitive manifest file for a controlplane component.

**Node**

Troubleshoot a Kubernetes node that shows a `NotReady` status.

-   Check logs and events.
-   Check CPU, memory and disk usage.
-   Check that the kubelet service is running on the node.
-   Check the host networking for issues on the node.
-   Check for expired certificates used by the kublet:

<!--listend-->

```shell
openssl x509 -in /var/lib/kubelet/$NAME.crt -text
```

**Scheduler**

If the status of a pod is in the `Pending` state, then it may be because the pod could not be schedulled. You must always look for reasons why a pod could not be schedulled:

-   Does the pod show a `Node` value of `<none>` in the `kubectl describe` output?
-   Are there any issues with the scheduler?
-   Could there be an issue with taints or tollerations?
