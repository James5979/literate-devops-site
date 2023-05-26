+++
title = "JSONPath for custom-columns"
draft = false
weight = 73
+++

Customise column output using JSONPath:

```shell
kubectl get pods --output=custom-columns=POD:.metadata.name,CONTAINER:.spec.containers[*].name,PORT:.spec.containers[*].ports[0].containerPort
```

| POD    | CONTAINER | PORT |
|--------|-----------|------|
| nginx  | nginx     | 80   |
| webapp | nginx     | 8080 |
