+++
title = "Ingress"
description = "Ingress rules."
draft = false
weight = 71
+++

An `Ingress` is an API object that manages external access to the services within the cluster. Ingress usually provides load balancing, SSL termination and name-based virtual hosting. --- [1]


## Create ingress rules {#create-ingress-rules}

Imperative command:

```shell
kubectl create ingress $NAME --annotation=nginx.ingress.kubernetes.io/rewrite-target=/ --class=$CLASS --rule="$URL=$SERVICE:$PORT"
```

**Note**: if you use a wildcard within your path (i.e. when specifying the `--rule` option), then the Kubernetes ingress object will default to using `Prefix` value as its `pathType`.

| pathType | Host/path         |
|----------|-------------------|
| Exact    | foo.bar.com/baz   |
| Prefix   | foo.bar.com/baz\* |

Example manifest (fanout model):

```yaml { linenos=inline }
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-fanout
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
              number: 80
      - path: /bar
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 8080
```

Example manifest (name-based virtual hosts):

```yaml { linenos=inline }
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-vhosts
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: "/"
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
  - host: bar.foo.com
    http:
      paths:
      - path: "/"
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
```

Example manifest (wildcards):

```yaml { linenos=inline }
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wildcards
spec:
  rules:
  - host: "foo.bar.com"
    http:
      paths:
      - path: "/bar"
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
  - host: "*.foo.com"
    http:
      paths:
      - path: "/foo"
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
```


## References {#references}

1.  [kubernetes.io](https://kubernetes.io/docs/concepts/services-networking/ingress/)
