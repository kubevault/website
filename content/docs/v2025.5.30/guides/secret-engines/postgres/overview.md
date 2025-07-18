---
title: Manage PostgreSQL credentials using the KubeVault operator
menu:
  docs_v2025.5.30:
    identifier: overview-postgresql
    name: Overview
    parent: postgres-secret-engines
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

# Manage PostgreSQL credentials using the KubeVault operator

PostgreSQL is one of the supported plugins for the database secrets engine. This plugin generates database credentials dynamically based on configured roles for the PostgreSQL database. You can easily manage [PostgreSQL secret engine](https://www.vaultproject.io/docs/secrets/databases/postgresql.html) using the KubeVault operator.

![PostgreSQL secret engine](/docs/v2025.5.30/images/guides/secret-engines/postgresql/postgres_secret_engine_guide.svg)

You need to be familiar with the following CRDs:

- [AppBinding](/docs/v2025.5.30/concepts/vault-server-crds/auth-methods/appbinding)
- [SecretEngine](/docs/v2025.5.30/concepts/secret-engine-crds/secretengine)
- [PostgresRole](/docs/v2025.5.30/concepts/secret-engine-crds/database-secret-engine/postgresrole)

## Before you begin

- Install KubeVault operator in your cluster from [here](/docs/v2025.5.30/setup/README).

To keep things isolated, we are going to use a separate namespace called `demo` throughout this tutorial.

```bash
$ kubectl create ns demo
namespace/demo created
```

In this tutorial, we are going to create a [role](https://www.vaultproject.io/docs/secrets/databases/postgresql#setup) using PostgreSQL and issue credential using SecretAccessRequest.

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

## Enable and Configure PostgreSQL Secret Engine

When a [SecretEngine](/docs/v2025.5.30/concepts/secret-engine-crds/secretengine) crd object is created, the KubeVault operator will enable a secret engine on specified path and configure the secret engine with given configurations.

A sample SecretEngine object for the PostgreSQL secret engine:

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretEngine
metadata:
  name: postgres-engine
  namespace: demo
spec:
  vaultRef:
    name: vault
  postgres:
    databaseRef:
      name: postgres
      namespace: demo
    pluginName: "postgresql-database-plugin"
```

Let's deploy SecretEngine:

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/postgres/secretengine.yaml
secretengine.engine.kubevault.com/postgres-engine created
```

Wait till the status become `Success`:

```bash
$ kubectl get secretengines -n demo
NAME           STATUS    AGE
postgres-engine   Success   10s
```

Since the status is `Success`, the PostgreSQL secret engine is enabled and successfully configured. You can use `kubectl describe secretengine -n <namepsace> <name>` to check for error events, if any.

## Create PostgreSQL Role

By using [PostgresRole](/docs/v2025.5.30/concepts/secret-engine-crds/database-secret-engine/postgresrole), you can create a [role](https://www.vaultproject.io/docs/secrets/databases/postgresql#setup) on the Vault server in Kubernetes native way.

A sample PostgresRole object is given below:

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: PostgresRole
metadata:
  name: postgres-superuser-role
  namespace: demo
spec:
  secretEngineRef:
    name: vault
  creationStatements:
    - "CREATE ROLE \"{{name}}\" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}';"
    - "GRANT SELECT ON ALL TABLES IN SCHEMA public TO \"{{name}}\";"
  defaultTTL: 1h
  maxTTL: 24h

```

Let's deploy PostgresRole:

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/postgres/secretenginerole.yaml
postgresrole.engine.kubevault.com/postgres-superuser-role created

$ kubectl get postgresrole -n demo
NAME                      STATUS    AGE
postgres-superuser-role   Success   34m
```

You can also check from Vault that the role is created.
To resolve the naming conflict, name of the role in Vault will follow this format: `k8s.{clusterName}.{metadata.namespace}.{metadata.name}`.

> Don't have Vault CLI? Download and configure it as described [here](/docs/v2025.5.30/guides/vault-server/vault-server#enable-vault-cli)

```bash
$ vault list your-database-path/roles
Keys
----
k8s.-.demo.postgres-superuser-role

$ vault read your-database-path/roles/k8s.-.demo.postgres-superuser-role
Key                      Value
---                      -----
creation_statements      [CREATE ROLE "{{name}}" WITH LOGIN PASSWORD '{{password}}' VALID UNTIL '{{expiration}}'; GRANT SELECT ON ALL TABLES IN SCHEMA public TO "{{name}}";]
db_name                  k8s.-.db.postgres
default_ttl              1h
max_ttl                  24h
renew_statements         []
revocation_statements    []
rollback_statements      []
```

If we delete the PostgreSQL, then the respective role will be deleted from the Vault.

```bash
$ kubectl delete postgresrole -n demo postgresrole-superuser-role
postgresrolerole.engine.kubevault.com "postgres-superuser-role" deleted
```

Check from Vault whether the role exists:

```bash
$ vault read your-database-path/roles/k8s.-.demo.postgres-superuser-role
No value found at your-database-path/roles/k8s.-.demo.postgres-superuser-role

$ vault list your-database-path/roles
No value found at your-database-path/roles/
```

## Generate PostgreSQL credentials

Here, we are going to make a request to Vault for PostgreSQL credentials by creating `postgres-cred-rqst` SecretAccessRequest in `demo` namespace.

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretAccessRequest
metadata:
  name: postgres-cred-rqst
  namespace: demo
spec:
  roleRef:
    kind: PostgresRole
    name: postgres-superuser-role
  subjects:
    - kind: ServiceAccount
      name: demo-sa
      namespace: demo
```

Here, `spec.roleRef` is the reference of Postgres against which credentials will be issued. `spec.subjects` is the reference to the object or user identities a role binding applies to it will have read access of the credential secret.

Now, we are going to create SecretAccessRequest.

```bash
$ kubectl apply -f docs/examples/guides/secret-engines/postgres/mysqlaccessrequest.yaml
secretaccessrequest.engine.kubevault.com/postgres-cred-rqst created

$ kubectl get secretaccessrequest -n demo
NAME              AGE
postgres-cred-rqst   72m
```

Database credentials will not be issued until it is approved. The KubeVault operator will watch for the approval in the `status.conditions[].type` field of the request object. You can use [KubeVault CLI](https://github.com/kubevault/cli), a [kubectl plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/), to approve or deny SecretAccessRequest.

```bash
# using KubeVault CLI as kubectl plugin to approve request
$ kubectl vault approve secretaccessrequest postgres-cred-rqst -n demo
approved

$ kubectl get secretaccessrequest -n demo postgres-cred-rqst -o yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretAccessRequest
metadata:
  name: postgres-cred-rqst
  namespace: demo
spec:
  roleRef:
    kind: PostgresRole
    name: postgres-superuser-role
    namespace: demo
  subjects:
  - kind: ServiceAccount
    name: demo-sa
    namespace: demo
status:
  conditions:
  - lastUpdateTime: "2020-11-18T06:41:57Z"
    message: This was approved by kubectl vault approve secretaccessrequest
    reason: KubectlApprove
    type: Approved
  lease:
    duration: 1h0m0s
    id: your-database-path/creds/k8s.-.demo.postgres-superuser-role/ni3TCo2HkSwCUb8kmQuvIDdx
    renewable: true
  secret:
    name: postgres-cred-rqst-gy66wq
```

Once SecretAccessRequest is approved, the KubeVault operator will issue credentials from Vault and create a secret containing the credential. It will also create a role and rolebinding so that `spec.subjects` can access secret. You can view the information in the `status` field.

```bash
$ kubectl get secretaccessrequest postgres-cred-rqst -n demo -o json | jq '.status'
{
  "conditions": [
    {
      "lastUpdateTime": "2019-11-18T06:41:57Z",
      "message": "This was approved by kubectl vault approve secretaccessrequest",
      "reason": "KubectlApprove",
      "type": "Approved"
    }
  ],
  "lease": {
    "duration": "1h0m0s",
    "id": "your-database-path/creds/k8s.-.demo.postgres-superuser-role/ni3TCo2HkSwCUb8kmQuvIDdx",
    "renewable": true
  },
  "secret": {
    "name": "postgres-cred-rqst-gy66wq"
  }
}

$ kubectl get secret -n demo postgres-cred-rqst-gy66wq -o yaml
apiVersion: v1
data:
  password: QTFhLVBkZGlsZFFxa0o1cnlvR20=
  username: di1rdWJlcm5ldGVzLWRlbW8TE1NzQwNTkzMTc=
kind: Secret
metadata:
  name: postgres-cred-rqst-gy66wq
  namespace: demo
  ownerReferences:
  - apiVersion: engine.kubevault.com/v1alpha1
    controller: true
    kind: SecretAccessRequest
    name: postgres-cred-rqst
    uid: 54ce63ca-d0e7-4b97-9085-b52eb3cb334f
type: Opaque
```

If SecretAccessRequest is deleted, then credential lease (if any) will be revoked.

```bash
$ kubectl delete secretaccessrequest -n demo postgres-cred-rqst
secretaccessrequest.engine.kubevault.com "postgres-cred-rqst" deleted
```

If SecretAccessRequest is `Denied`, then the KubeVault operator will not issue any credential.

```bash
$ kubectl vault deny secretaccessrequest postgres-cred-rqst -n demo
  Denied
```

> Note: Once SecretAccessRequest is `Approved`, you cannot change `spec.roleRef` and `spec.subjects` field.
