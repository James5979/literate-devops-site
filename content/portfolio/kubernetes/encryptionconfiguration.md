+++
title = "EncryptionConfiguration"
description = "Encryption at rest."
draft = false
weight = 48
+++

Setup resource encryption in Kubernetes.


## The concern {#the-concern}

Confidential data is not encryptyed at rest in Kubernetes, and can be viewed if you have access to the ETCD database:

```shell
echo -n "Unencrypted output: " && kubectl exec pods/etcd-$CONTROLPLANE --namespace=kube-system -- /usr/local/bin/etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key get /registry/secrets/default/my-secret | tail --lines=3 | head --lines=1
```

Unencrypted output: GOLDENEYE_CODESiaminvincible

The worry is that an ETCD compromise could lead an attacker accessing the data stored within the Kubernetes secrets, potentially allowing further compromises.

To rectify this concern, we should encrypt Kubernetes secrets at rest.


## Create a secret key {#create-a-secret-key}

Generate a 32 byte random key and base64 encode it:

```shell
head --bytes=32 /dev/urandom | base64
```


## Create a new encryption configuration file {#create-a-new-encryption-configuration-file}

Example manifest:

```yaml { linenos=inline }
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - secretbox:
      keys:
      - name: primary-key
        secret: QSVowJEw4tL3t94z5fqP7h54d6U8ji/LBiKkoQvPkYY=
  - identity: {}
```

**Note**: the secret key above is intended for display purposes only, my actual key is not the same :wink:.

Copy the encryption configuration file to the controlplane node, and save it as `/etc/kubernetes/enc/enc.yaml`.

Update the static pod manifest for the Kubernetes API server, and make sure that it contains the following fields:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kube-apiserver
  namespace: kube-system
  ...
spec:
  containers:
  - ...
    command:
    - kube-apiserver
    ...
    - --encryption-provider-config=/etc/kubernetes/enc/enc.yaml
    volumeMounts:
    ...
    - name: enc
      mountPath: /etc/kubernetes/enc
      readonly: true
    ...
  volumes:
  ...
  - name: enc
    hostPath:
      path: /etc/kubernetes/enc
      type: DirectoryOrCreate
  ...
```

Restart the Kubernetes API server (to pick up the changes):

```shell
kubectl delete pods/kube-apiserver-$CONTROLPLANE --namespace=kube-system
```

Since encryption is performed during Kubernetes write operations, you must recreate all your Kubernetes secrets, i.e. to encrypt them:

```shell
kubectl get secrets --all-namespaces --output=json | kubectl replace --filename -
```

Confidential Kubernetes data (i.e. secrets) are now encryptyed at rest, no longer can they be viewed in plain text (using an ETCD database query).

Check that you can still access your Kubernetes secrets (the usual way):

```shell
echo -n "Secret: " && kubectl get secrets/my-secret --output=jsonpath='{.data.GOLDENEYE_CODES}' | base64 --decode
```

Secret: iaminvincible

{{< figure src="/images/i-am-invincible.gif" width="1200px" >}}

Remember, this will only protect your secrets from an ETCD database compromise.

It is very important to restrict the permissions of the user for the kube-apiserver to deny access to the encrytpion configuration file, otherwise an attacker could just as well access the encrytpion configuration file and read the secret key! Also, this does not protect you from a compromised controlplane host, since the attacker would once agian have access to the encrytpion configuration file and be able to read the secret key, see [KMS](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider) to help protect confidential information against host compromises.

See the full Kubernetes [documentation](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/) on this topic, and make sure that you are aware of all the security considerations, some of which are not outlined above.


## References {#references}

-   [kubernetes.io](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)
-   [reddit.com](https://www.reddit.com/r/reactiongifs/comments/3i7elu/mrw_i_build_a_computer_and_it_boots_up_perfectly/)
-   [imgur.com](https://i.imgur.com/whKhQGz.gif)
