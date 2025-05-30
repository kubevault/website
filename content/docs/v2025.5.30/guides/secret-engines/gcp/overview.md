---
title: Manage GCP IAM Secrets using the KubeVault operator
menu:
  docs_v2025.5.30:
    identifier: overview-gcp
    name: Overview
    parent: gcp-secret-engines
    weight: 10
menu_name: docs_v2025.5.30
section_menu_id: guides
info:
  cli: v0.22.0
  installer: v2025.5.30
  operator: v0.22.0
  unsealer: v0.22.0
  version: v2025.5.30
---

> New to KubeVault? Please start [here](/docs/v2025.5.30/concepts/README).

# Manage GCP IAM Secrets using the KubeVault operator

The Google Cloud Vault secrets engine dynamically generates Google Cloud service account keys and OAuth tokens based on IAM policies. This enables users to gain access to Google Cloud resources without needing to create or manage a dedicated service account. You can easily manage [GCP secret engine](https://www.vaultproject.io/docs/secrets/gcp/index.html) using the KubeVault operator.

![GCP secret engine](/docs/v2025.5.30/images/guides/secret-engines/gcp/gcp_guide.svg)

You need to be familiar with the following CRDs:

- [AppBinding](/docs/v2025.5.30/concepts/vault-server-crds/auth-methods/appbinding)
- [SecretEngine](/docs/v2025.5.30/concepts/secret-engine-crds/secretengine)
- [GCPRole](/docs/v2025.5.30/concepts/secret-engine-crds/gcp-secret-engine/gcprole)

## Before you begin

- Install KubeVault operator in your cluster from [here](/docs/v2025.5.30/setup/README).

To keep things isolated, we are going to use a separate namespace called `demo` throughout this tutorial.

```bash
$ kubectl create ns demo
namespace/demo created
```

In this tutorial, we are going to create a [role](https://www.vaultproject.io/api/secret/gcp/index.html#create-update-roleset) using GCPRole and issue credential using SecretAccessRequest.

## Vault Server

If you don't have a Vault Server, you can deploy it by using the KubeVault operator.

- [Deploy Vault Server](/docs/v2025.5.30/guides/vault-server/vault-server)

The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. You need to configure both the Vault server and the cluster so that the KubeVault operator can communicate with your Vault server.

- [Configure cluster and Vault server](/docs/v2025.5.30/guides/vault-server/external-vault-sever#configuration)

Now, we have the [AppBinding](/docs/v2025.5.30/concepts/vault-server-crds/auth-methods/appbinding) that contains connection and authentication information about the Vault server.

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

## Enable and Configure GCP Secret Engine

When a [SecretEngine](/docs/v2025.5.30/concepts/secret-engine-crds/secretengine) crd object is created, the KubeVault operator will enable a secret engine on specified path and configure the secret engine with given configurations.

A sample SecretEngine object for GCP secret engine:

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretEngine
metadata:
  name: gcp-engine
  namespace: demo
spec:
  vaultRef:
    name: vault
  gcp:
    credentialSecret: gcp-cred
```

To configure the GCP secret engine, you need to provide google service account credentials through a Kubernetes secret.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: gcp-cred
  namespace: demo
data:
  sa.json: eyJtc2ciOiJleGFtcGxlIn0= ## base64 encoded google service account credential
```

Let's deploy SecretEngine:

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/gcp/secret.yaml
secret/gcp-cred created

$ kubectl apply -f docs/examples/guides/secret-engines/gcp/secretengine.yaml
secretengine.engine.kubevault.com/gcp-engine created
```

Wait till the status become `Success`:

```bash
$ kubectl get secretengines -n demo
NAME         STATUS
gcp-engine   Success
```

Since the status is `Success`, the GCP secret engine is enabled and successfully configured. You can use `kubectl describe secretengine -n <namepsace> <name>` to check for error events, if any.

## Create GCP Roleset

By using [GCPRole](/docs/v2025.5.30/concepts/secret-engine-crds/gcp-secret-engine/gcprole), you can [create gcp roleset](https://www.vaultproject.io/api/secret/gcp/index.html#create-update-roleset) on the Vault server in Kubernetes native way.

A sample GCPRole object is given below:

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: GCPRole
metadata:
  name: gcp-role
  namespace: demo
spec:
  secretEngineRef:
    name: gcp-secret-engine
  secretType: "access_token"
  project: appscode-testing
  bindings: |
    resource "//cloudresourcemanager.googleapis.com/projects/appscode-testing" {
      roles = ["roles/viewer"]
    }
  tokenScopes:
    - https://www.googleapis.com/auth/cloud-platform
```

Let's deploy GCPRole:

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/gcp/secretenginerole.yaml
gcprole.engine.kubevault.com/gcp-role created

$ kubectl get gcprole -n demo
NAME       STATUS
gcp-role   Success
```

You can also check from Vault that the roleset is created.
To resolve the naming conflict, name of the roleset in Vault will follow this format: `k8s.{clusterName}.{metadata.namespace}.{metadata.name}`.

> Don't have Vault CLI? Download and configure it as described [here](/docs/v2025.5.30/guides/vault-server/vault-server#enable-vault-cli)

```bash
$ vault list your-gcp-path/roleset
Keys
----
k8s.-.demo.gcp-role

$ vault read your-gcp-path/roleset/k8s.-.demo.gcp-role
Key                      Value
---                      -----
bindings                 map[//cloudresourcemanager.googleapis.com/projects/appscode-testing:[roles/viewer]]
project                  appscode-testing
secret_type              access_token
service_account_email    vaultk8s---demo-gcp-1629380381@appscode-testing.iam.gserviceaccount.com
token_scopes             [https://www.googleapis.com/auth/cloud-platform]
```

If we delete the GCPRole, then the respective role will be deleted from the Vault.

```bash
$ kubectl delete -f docs/examples/guides/secret-engines/gcp/secretenginerole.yaml
  gcprole.engine.kubevault.com "gcp-role" deleted
```

Check from Vault whether the roleset exists:

```bash
$ vault read your-gcp-path/roleset/k8s.-.demo.gcp-role
  No value found at your-gcp-path/roleset/k8s.-.demo.gcp-role

$ vault list your-gcp-path/roleset
  No value found at your-gcp-path/roleset/
```

## Generate GCP credentials


Here, we are going to make a request to Vault for GCP credential by creating `gcp-cred-req` SecretAccessRequest in `demo` namespace.

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretAccessRequest
metadata:
  name: gcp-cred-req
  namespace: demo
spec:
  roleRef:
    kind: GCPRole
    name: gcp-role
  subjects:
    - kind: ServiceAccount
      name: demo-sa
      namespace: demo
```

Here, `spec.roleRef` is the reference of GCPRole against which credentials will be issued. `spec.subjects` is the reference to the object or user identities a role binding applies to and it will have read access of the credential secret.

Now, we are going to create SecretAccessRequest.

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/gcp/secretaccessrequest.yaml
secretaccessrequest.engine.kubevault.com/gcp-cred-req created

$ kubectl get secretaccessrequests -n demo
NAME          AGE
gcp-cred-req  3s
```

GCP credentials will not be issued until it is approved. The KubeVault operator will watch for the approval in the `status.conditions[].type` field of the request object. You can use [KubeVault CLI](https://github.com/kubevault/cli), a [kubectl plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/), to approve or deny SecretAccessRequest.

```bash
# using KubeVault CLI as kubectl plugin to approve request
$ kubectl vault approve secretaccessrequest gcp-cred-req -n demo
  approved

$ kubectl get secretaccessrequest -n demo gcp-cred-req -o yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretAccessRequest
metadata:
  name: gcp-cred-req
  namespace: demo
spec:
  roleRef:
    name: gcp-role
    namespace: demo
  subjects:
  - kind: ServiceAccount
    name: demo-sa
    namespace: demo
status:
  conditions:
  - lastUpdateTime: "2019-11-12T10:40:30Z"
    message: This was approved by kubectl vault approve secretaccessrequest
    reason: KubectlApprove
    type: Approved
  lease:
    duration: 0s
  secret:
    name: gcp-cred-req-unyzu6

```

Once SecretAccessRequest is approved, the KubeVault operator will issue credentials from Vault and create a secret containing the credential. It will also create a role and rolebinding so that `spec.subjects` can access secret. You can view the information in the `status` field.

```bash
$ kubectl get secretaccessrequest gcp-cred-req -n demo -o json | jq '.status'
{
  "conditions": [
    {
      "lastUpdateTime": "2019-11-12T10:40:30Z",
      "message": "This was approved by kubectl vault approve secretaccessrequest",
      "reason": "KubectlApprove",
      "type": "Approved"
    }
  ],
  "lease": {
    "duration": "0s"
  },
  "secret": {
    "name": "gcp-cred-req-unyzu6"
  }
}

$ kubectl get secret -n demo gcp-cred-req-unyzu6 -o yaml
apiVersion: v1
data:
  expires_at_seconds: MTU3MzU1ODgzMA==
  token: eWEyOS5jLktsMndCLTR5=
  token_ttl: MzU5OQ==
kind: Secret
metadata:
  name: gcp-cred-req-unyzu6
  namespace: demo
type: Opaque

```

If SecretAccessRequest is deleted, then credential lease (if any) will be revoked.

```bash
$ kubectl delete secretaccessrequest -n demo gcp-cred-req
  secretaccessrequest.engine.kubevault.com "gcp-cred-req" deleted
```

If SecretAccessRequest is `Denied`, then the KubeVault operator will not issue any credential.

```bash
$ kubectl vault deny secretaccessrequest gcp-cred-req -n demo
  Denied
```

> Note: Once SecretAccessRequest is `Approved` or `Denied`, you cannot change `spec.roleRef` and `spec.subjects` field.
