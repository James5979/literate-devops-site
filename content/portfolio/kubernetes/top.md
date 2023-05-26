+++
title = "Top"
draft = false
weight = 37
+++

Display Resource (CPU/Memory) usage.

List resources by node:

```shell
kubectl top nodes
```

| NAME                      | CPU(cores) | CPU% | MEMORY(bytes) | MEMORY% |
|---------------------------|------------|------|---------------|---------|
| lab-cluster-control-plane | 145m       | 1%   | 1043Mi        | 7%      |
| lab-cluster-worker        | 19m        | 0%   | 361Mi         | 2%      |
| lab-cluster-worker2       | 27m        | 0%   | 427Mi         | 2%      |

List resources by pod (in the `kube-system` namespace):

```shell
kubectl top pods --namespace=kube-system
```

| NAME                                              | CPU(cores) | MEMORY(bytes) |
|---------------------------------------------------|------------|---------------|
| coredns-787d4945fb-542g4                          | 3m         | 59Mi          |
| coredns-787d4945fb-p8647                          | 3m         | 13Mi          |
| etcd-lab-cluster-control-plane                    | 27m        | 68Mi          |
| kindnet-4s4tm                                     | 1m         | 41Mi          |
| kindnet-88gvr                                     | 1m         | 41Mi          |
| kindnet-qzsnj                                     | 1m         | 41Mi          |
| kube-apiserver-lab-cluster-control-plane          | 47m        | 346Mi         |
| kube-controller-manager-lab-cluster-control-plane | 18m        | 105Mi         |
| kube-proxy-459df                                  | 1m         | 59Mi          |
| kube-proxy-hpzbn                                  | 1m         | 57Mi          |
| kube-proxy-pvpfg                                  | 1m         | 57Mi          |
| kube-scheduler-lab-cluster-control-plane          | 4m         | 68Mi          |
| metrics-server-67ccc48c4-6dh8l                    | 5m         | 17Mi          |

List resources by container:

```shell
kubectl top pods --containers --namespace=default
```

| POD        | NAME  | CPU(cores) | MEMORY(bytes) |
|------------|-------|------------|---------------|
| chatterbox | noisy | 0m         | 0Mi           |
| chatterbox | quiet | 0m         | 0Mi           |

**Pitfall**: the `--containers` option does not appear in `kubectl top --help` output.
