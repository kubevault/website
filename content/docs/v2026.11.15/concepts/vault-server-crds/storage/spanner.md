---
title: Google Cloud Spanner | Vault Server Storage
menu:
  docs_v2026.11.15:
    identifier: spanner-storage
    name: Google Cloud Spanner
    parent: storage-vault-server-crds
    weight: 120
menu_name: docs_v2026.11.15
section_menu_id: concepts
info:
  cli: v0.27.0
  installer: v2026.11.15
  operator: v0.27.0
  unsealer: v0.27.0
  version: v2026.11.15
---

> New to KubeVault? Please start [here](/docs/v2026.11.15/concepts/README).

# Google Cloud Spanner

In the Spanner storage backend, Vault data will be stored in [Google Cloud Spanner](https://cloud.google.com/spanner). Vault documentation for the Spanner storage backend can be found [here](https://developer.hashicorp.com/vault/docs/configuration/storage/google-cloud-spanner).

> `spec.backend.spanner` is only available on the `kubevault.com/v1alpha2` `VaultServer` API.

```yaml
apiVersion: kubevault.com/v1alpha2
kind: VaultServer
metadata:
  name: vault-with-spanner
  namespace: demo
spec:
  version: "2.0.0"
  replicas: 1
  backend:
    spanner:
      database: "projects/my-project/instances/my-instance/databases/vault"
      credentialSecretRef:
        name: spanner-credential
```

## spec.backend.spanner

To use Spanner as backend storage in Vault, specify `spec.backend.spanner` in the [VaultServer](/docs/v2026.11.15/concepts/vault-server-crds/vaultserver) CRD.

```yaml
spec:
  backend:
    spanner:
      database: <spanner_database_path>
      table: <table_name>
      haEnabled: <true/false>
      haTable: <ha_table_name>
      maxParallel: <max_parallel>
      credentialSecretRef:
        name: <secret_name>
```

Here, we are going to describe the various attributes of the `spec.backend.spanner` field.

### spanner.database

`spanner.database` is a required field that specifies the full name of the Spanner database, in the form `projects/<project>/instances/<instance>/databases/<database>`.

```yaml
spec:
  backend:
    spanner:
      database: "projects/my-project/instances/my-instance/databases/vault"
```

### spanner.table

`spanner.table` is an optional field that specifies the name of the table to use for Vault data. Defaults to `Vault`. Vault does not create the table; it must already exist, with the schema below:

```sql
CREATE TABLE Vault (
  Key   STRING(MAX) NOT NULL,
  Value BYTES(MAX),
) PRIMARY KEY (Key);
```

```yaml
spec:
  backend:
    spanner:
      table: "Vault"
```

### spanner.haEnabled

`spanner.haEnabled` is an optional field that specifies whether this backend should be used to run Vault in high availability mode. This field accepts a boolean value. The default value is `false`.

```yaml
spec:
  backend:
    spanner:
      haEnabled: true
```

### spanner.haTable

`spanner.haTable` is an optional field that specifies the name of the table used for HA leader election. Defaults to the value of `table` suffixed with `HA`. When `haEnabled` is `true`, this table must also already exist, with the schema below:

```sql
CREATE TABLE VaultHA (
  Key       STRING(MAX) NOT NULL,
  Value     STRING(MAX),
  Identity  STRING(MAX),
  Timestamp TIMESTAMP,
) PRIMARY KEY (Key);
```

```yaml
spec:
  backend:
    spanner:
      haTable: "VaultHA"
```

### spanner.maxParallel

`spanner.maxParallel` is an optional field that specifies the maximum number of concurrent requests to Spanner.

```yaml
spec:
  backend:
    spanner:
      maxParallel: 128
```

### spanner.credentialSecretRef

`spanner.credentialSecretRef` is an optional field that specifies the name of the `Secret`, in the same namespace as the `VaultServer`, containing the Google application credential used to reach Spanner. The secret contains the following key:

- `sa.json`

```yaml
spec:
  backend:
    spanner:
      credentialSecretRef:
        name: spanner-credential
```
