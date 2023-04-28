+++
title = "Port types"
draft = false
weight = 7
+++

Types of port found within a Kubernetes service:

<style>.table-nocaption table { width: 70%;  }</style>

<div class="ox-hugo-table table-nocaption">

| Field      | Description         |
|------------|---------------------|
| targetPort | Port on the pod     |
| port       | Port on the service |
| nodePort   | Port on the node    |

</div>

The ports are networked together like so:

```text
LAN <--> nodePort|service|port <--> targetPort|pod
```
