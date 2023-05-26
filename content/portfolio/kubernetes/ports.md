+++
title = "Ports"
draft = false
weight = 7
+++

Kubernetes service ports:

<style>.table-nocaption table { width: 70%;  }</style>

<div class="ox-hugo-table table-nocaption">

| Port       | Description         |
|------------|---------------------|
| targetPort | Port on the pod     |
| port       | Port on the service |
| nodePort   | Port on the node    |

</div>

The ports in the table (above) are networked together as follows:

```text
LAN <--> nodePort|service|port <--> targetPort|pod
```
