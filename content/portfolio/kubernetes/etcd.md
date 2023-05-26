+++
title = "ETCD"
draft = false
weight = 53
+++

ETCD is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or cluster of machines. --- [1]


## Client installation {#client-installation}

Install the `etcdctl` binary on the controlplane:

```shell
apt --assume-yes install etcd-client
```

Check the API version used by the `etcdctl` binary:

```shell
etcdctl --version 2> /dev/null || etcdctl version
```

**Pitfall**: if you find that the API version is set to version **2**, you must export the environment variable `ETCDCTL_API=3`.


## Usage {#usage}

Find the relevant certificate information and endpoint values used by the ETCD cluster:

```shell
kubectl describe pods/etcd-$CONTROLPLANE --namespace=kube-system
```

**Tip**: you can always obtain the certificate information by reviewing the command array for the `etcd` container.


## Querying {#querying}

Query an ETCD database:

```shell
ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --endpoints=$ETCD_SERVER_IP:2379 --key=/etc/kubernetes/pki/etcd/server.key get / --keys-only --limit=10000 --prefix
```

You can also specify the certificate information using environment variables:

```shell
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key
```

**CKA Exam Tip**: if you are using the `etcdctl` binary on the controlplane node, remember that the certificate information is accessible from both the pod and the controlplane itself, since all the directories are just `hostPath` [volumes](/portfolio/kubernetes/volumes/).


## Snapshot {#snapshot}

Snapshot an ETCD cluster:

```shell
ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --endpoints=$ETCD_SERVER_IP:2379 --key=/etc/kubernetes/pki/etcd/server.key snapshot save etcd-snapshot-$DATE.db
```

**Warning**: checking the status of a snapshot can sometimes change the hash value. This may cause issues later when you attempt to restore.

To view the status of a snapshot:

```shell
ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --endpoints=$ETCD_SERVER_IP:2379 --key=/etc/kubernetes/pki/etcd/server.key snapshot status etcd-snapshot-$DATE.db --write-out=table
```


## ETCD restore {#etcd-restore}

To restore an ETCD cluster using a snapshot:

```shell
ETCDCTL_API=3 etcdctl snapshot restore etcd-snapshot-$DATE.db --data-dir /var/lib/etcd-from-backup
```

**Note**: the restore process does not require any communication with the ETCD cluster, so no certificate information is required to run the restore process.

Update the ETCD manifest, pointing the cluster to the restored data:

```yaml
[...]
spec:
  containers:
  - command:
    - etcd
    - [...]
    - --data-dir /var/lib/etcd
    - [...]
    [...]
    volumeMounts:
    - mountPath: /var/lib/etcd
      name: etcd-data
    - [...]
  [...]
  volumes:
  - [...]
  - hostPath:
      path: /var/lib/etcd-from-backup
      type: DirectoryOrCreate
    name: etcd-data
[...]
```

After editing the manifest, the ETCD pod should restart automatically. For brevity, just delete the ETCD pod so that it can be re-created quickly:

```shell
kubectl delete pods/etcd-$CONTROLPLANE --namespace=kube-system
```

If you have any issues, you may have to restart the entire controlplane:

```shell
systemctl restart kubelet.service
```


## References {#references}

1.  [etcd.io](https://etcd.io/)
