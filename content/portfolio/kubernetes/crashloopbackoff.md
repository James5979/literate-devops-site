+++
title = "CrashLoopBackOff"
draft = false
weight = 30
+++

When troubleshooting pods, always investigate why a container is in a `CrashLoopBackOff` state.

**Example**

Using the `kubectl describe` command, view the container `state` and the reasons for its state:

```text
containers:
  nginx:
  ...
  state:          Terminated
    Reason:       OOMKilled
```

Notice that the container has been terminated since it has exceeded its memory limits.
