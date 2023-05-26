+++
title = "Ingress"
draft = true
weight = 71
+++

Choose a `Host/path` syntax when specifying the `--rule` option within the `kubectl create ingress` command in order to specify the pathType required:

| pathType | Host/path         |
|----------|-------------------|
| Exact    | foo.bar.com/baz   |
| Prefix   | foo.bar.com/baz\* |

Use an imperative command to generate the manifest for a Kubernetes `Ingress`:

```shell
kubectl create ingress $NAME --annotation=nginx.ingress.kubernetes.io/rewrite-target=/ \
--class=$CLASS \
--dry-run=client \
--rule="$HOST=$SERVICE:$PORT" \
--output=yaml
```

**Examples**

Ingress using a simple fanout model (single loadbalancer):

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-fanout
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /foo
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 4200
      - path: /bar
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 8080
```

Ingress using wildcards:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wildcards
spec:
  rules:
  - host: "foo.bar.com"
    http:
      paths:
      - pathType: Prefix
        path: "/bar"
        backend:
          service:
            name: service1
            port:
              number: 80
  - host: "*.foo.com"
    http:
      paths:
      - pathType: Prefix
        path: "/foo"
        backend:
          service:
            name: service2
            port:
              number: 80
```

Ingress using name-based virtual hosts:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: name-based-virtual-hosts
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: service1
            port:
              number: 80
  - host: bar.foo.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: service2
            port:
              number: 80
```
