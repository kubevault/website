---
title: Mount Key/Value Secrets using CSI Driver
menu:
  docs_v2025.5.26:
    identifier: csi-driver-kv
    name: CSI Driver
    parent: kv-secret-engines
    weight: 15
menu_name: docs_v2025.5.26
section_menu_id: guides
info:
  cli: v0.21.0
  installer: v2025.5.26
  operator: v0.21.0
  unsealer: v0.21.0
  version: v2025.5.26
---

# Mount Key/Value Secrets using CSI Driver

## Kubernetes Secrets Store CSI Driver

Secrets Store CSI driver for Kubernetes secrets - Integrates secrets stores with Kubernetes via a [Container Storage Interface (CSI)](https://kubernetes-csi.github.io/docs/) volume.

The Secrets Store CSI driver `secrets-store.csi.k8s.io` allows Kubernetes to mount multiple secrets, keys, and certs stored in enterprise-grade external secrets stores into their pods as a volume. Once the Volume is attached, the data in it is mounted into the container’s file system.

![Secrets-store CSI architecture](/docs/v2025.5.26/guides/secret-engines/csi_architecture.svg)

When the `Pod` is created through the K8s API, it’s scheduled on to a node. The `kubelet` process on the node looks at the pod spec & see if there's any `volumeMount` request. The `kubelet` issues an `RPC` to the `CSI driver` to mount the volume. The `CSI driver` creates & mounts `tmpfs` into the pod. Then the `CSI driver` issues a request to the `Provider`. The provider talks to the external secrets store to fetch the secrets & write them to the pod volume as files. At this point, volume is successfully mounted & the pod starts running.

You can read more about the Kubernetes Secrets Store CSI Driver [here](https://secrets-store-csi-driver.sigs.k8s.io/).

## Consuming Secrets

At first, you need to have a Kubernetes 1.16 or later cluster, and the kubectl command-line tool must be configured to communicate with your cluster. If you do not already have a cluster, you can create one by using [kind](https://kind.sigs.k8s.io/docs/user/quick-start/). To check the version of your cluster, run:

```bash
$ kubectl version --short
Client Version: v1.21.2
Server Version: v1.21.1
```

Before you begin:

- Install KubeVault operator in your cluster from [here](/docs/v2025.5.26/setup/README).
- Install Secrets Store CSI driver for Kubernetes secrets in your cluster from [here](https://secrets-store-csi-driver.sigs.k8s.io/getting-started/installation.html).
- Install Vault Specific CSI provider from [here](https://github.com/hashicorp/vault-csi-provider)

To keep things isolated, we are going to use a separate namespace called `demo` throughout this tutorial.

```bash
$ kubectl create ns demo
namespace/demo created
```

> Note: YAML files used in this tutorial stored in [examples](/docs/v2025.5.26/examples/guides/secret-engines/kv) folder in GitHub repository [KubeVault/docs](https://github.com/kubevault/kubevault)

## Vault Server

If you don't have a Vault Server, you can deploy it by using the KubeVault operator.

- [Deploy Vault Server](/docs/v2025.5.26/guides/vault-server/vault-server)

The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. You need to configure both the Vault server and the cluster so that the KubeVault operator can communicate with your Vault server.

- [Configure cluster and Vault server](/docs/v2025.5.26/guides/vault-server/external-vault-sever#configuration)

Now, we have the [AppBinding](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/appbinding) that contains connection and authentication information about the Vault server. And we also have the service account that the Vault server can authenticate.

```bash
$ kubectl get appbinding -n demo
NAME    AGE
vault   50m

$ kubectl get appbinding -n demo vault -o yaml
apiVersion: appcatalog.appscode.com/v1alpha1
kind: AppBinding
metadata:
  creationTimestamp: "2021-08-16T08:23:38Z"
  generation: 1
  labels:
    app.kubernetes.io/instance: vault
    app.kubernetes.io/managed-by: kubevault.com
    app.kubernetes.io/name: vaultservers.kubevault.com
  name: vault
  namespace: demo
  ownerReferences:
  - apiVersion: kubevault.com/v1alpha1
    blockOwnerDeletion: true
    controller: true
    kind: VaultServer
    name: vault
    uid: 6b405147-93da-41ff-aad3-29ae9f415d0a
  resourceVersion: "602898"
  uid: b54873fd-0f34-42f7-bdf3-4e667edb4659
spec:
  clientConfig:
    service:
      name: vault
      port: 8200
      scheme: http
  parameters:
    apiVersion: config.kubevault.com/v1alpha1
    kind: VaultServerConfiguration
    kubernetes:
      serviceAccountName: vault
      tokenReviewerServiceAccountName: vault-k8s-token-reviewer
      usePodServiceAccountForCSIDriver: true
    path: kubernetes
    vaultRole: vault-policy-controller
```

## Enable and Configure KV Secret Engine

We will use the [Vault CLI](https://www.vaultproject.io/docs/commands/#vault-commands-cli-) throughout the tutorial to [enable and configure](https://www.vaultproject.io/docs/secrets/kv/kv-v1.html#setup) the KV secret engine.

> Don't have Vault CLI? Download and configure it as described [here](/docs/v2025.5.26/guides/vault-server/vault-server#enable-vault-cli)

### Enable KV Secret Engine

Enable the KV secret engine:

```bash
$ vault secrets enable -path=secret kv
Success! Enabled the kv secrets engine at: secret/
```

### Write KV Secret

Write arbitrary key-value pairs:

```bash
$ vault kv put secret/db-pass password="db-secret-password"
Success! Data written to: secret/db-pass
```

### Read KV Secret

Read a specific key-value pair:

```bash
$ vault kv get secret/db-pass
====== Data ======
Key         Value
---         -----
password    db-secret-password
```

Let's say pod's service account name is `pod-sa` located in `demo` namespace. We need to create a [VaultPolicy](/docs/v2025.5.26/concepts/policy-crds/vaultpolicy) and a [VaultPolicyBinding](/docs/v2025.5.26/concepts/policy-crds/vaultpolicybinding) so that the pod has access to read secrets from the Vault server.

### Create Service Account for Pod

Let's create the service account `pod-sa` which will be used in VaultPolicyBinding.
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: pod-sa
  namespace: demo
```

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/kv/serviceaccount.yaml
serviceaccount/pod-sa created

$ kubectl get serviceaccount -n demo
NAME                SECRETS   AGE
pod-sa              1         4h10m
```

### Create VaultPolicy and VaultPolicyBinding for Pod's Service Account

When a VaultPolicyBinding object is created, the KubeVault operator create an auth role in the Vault server. The role name is generated by the following naming format: `k8s.(clusterName or -).namespace.name`. Here, it is `k8s.-.demo.kv-se-role`.

```yaml
apiVersion: policy.kubevault.com/v1alpha1
kind: VaultPolicy
metadata:
  name: kv-se-policy
  namespace: demo
spec:
  vaultRef:
    name: vault
  policyDocument: |
    path "secret/db-pass" {
      capabilities = ["read"]
    }
---
apiVersion: policy.kubevault.com/v1alpha1
kind: VaultPolicyBinding
metadata:
  name: kv-se-role
  namespace: demo
spec:
  vaultRef:
    name: vault
  policies:
  - ref: kv-se-policy
  subjectRef:
    kubernetes:
      serviceAccountNames:
      - "pod-sa"
      serviceAccountNamespaces:
      - "trial"
```

Let's create VaultPolicy and VaultPolicyBinding:

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/kv/policy.yaml
vaultpolicy.policy.kubevault.com/kv-se-policy created

$ kubectl apply -f docs/examples/guides/secret-engines/kv/policybinding.yaml
vaultpolicybinding.policy.kubevault.com/kv-se-role created
```

Check if the VaultPolicy and the VaultPolicyBinding are successfully registered to the Vault server:

```bash
$ kubectl get vaultpolicy -n demo
NAME                           STATUS    AGE
kv-se-policy                  Success   8s

$ kubectl get vaultpolicybinding -n demo
NAME                           STATUS    AGE
kv-se-role                    Success   10s
```

## Mount secrets into a Kubernetes pod

So, we can create `SecretProviderClass` now. You can read more about `SecretProviderClass` [here](https://secrets-store-csi-driver.sigs.k8s.io/concepts.html#secretproviderclass).

### Create SecretProviderClass

Create `SecretProviderClass` object with the following content:

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
  name: vault-database
  namespace: demo
spec:
  provider: vault
  parameters:
    vaultAddress: "http://vault.demo:8200"
    roleName: "k8s.-.demo.kv-se-role"
    objects: |
      - objectName: "db-password"
        secretPath: "secret/db-pass"
        secretKey: "password"
```

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/kv/secretproviderclass.yaml
secretproviderclass.secrets-store.csi.x-k8s.io/vault-database created
```
NOTE: The `SecretProviderClass` needs to be created in the same namespace as the pod.

### Create Pod

Now we can create a `Pod` to consume the `KV` secrets. When the `Pod` is created, the `Provider` fetches the secret and writes them to Pod's volume as files. At this point, the volume is successfully mounted and the `Pod` starts running.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  namespace: demo
spec:
  serviceAccountName: pod-sa
  containers:
    - image: jweissig/app:0.0.1
      name: test-app
      volumeMounts:
        - name: secrets-store-inline
          mountPath: "/secrets-store/test"
          readOnly: true
  volumes:
    - name: secrets-store-inline
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: "vault-database"
```

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/kv/pod.yaml
pod/mypod created
```
## Test & Verify

Check if the Pod is running successfully, by running:

```bash
$ kubectl get pods -n demo
NAME                    READY   STATUS    RESTARTS   AGE
mypod                   1/1     Running   0          11s
```

### Verify Secret

If the Pod is running successfully, then check inside the app container by running

```bash
$ kubectl exec -it -n demo  mypod sh
/ # ls /secrets-store/test
db-password

/ # cat /secrets-store/test/db-password
db-secret-password

/ # exit
```

So, we can see that the secret `db-password` is mounted into the pod, where the secret key is mounted as file and value is the content of that file.

## Cleaning up

To clean up the Kubernetes resources created by this tutorial, run:

```bash
$ kubectl delete ns demo
namespace "demo" deleted
```
