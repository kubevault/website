---
title: PostgreSQL | Vault Server Storage
menu:
  docs_v2025.5.30:
    identifier: postgresql-storage
    name: PostgreSQL
    parent: storage-vault-server-crds
    weight: 40
menu_name: docs_v2025.5.30
section_menu_id: concepts
info:
  cli: v0.22.0
  installer: v2025.5.30
  operator: v0.22.0
  unsealer: v0.22.0
  version: v2025.5.30
---

> New to KubeVault? Please start [here](/docs/v2025.5.30/concepts/README).

# PostgreSQL

In PostgreSQL storage backend, Vault data will be stored in [PostgreSQL](https://www.postgresql.org/). Vault documentation for PostgreSQL storage can be found in [here](https://www.vaultproject.io/docs/configuration/storage/postgresql.html).

```yaml
apiVersion: kubevault.com/v1alpha1
kind: VaultServer
metadata:
  name: vault-with-postgresql
  namespace: demo
spec:
  replicas: 1
  version: "1.2.0"
  backend:
    postgresql:
      connectionURLSecret: "my-postgres-conn"
```

## spec.backend.postgresql

To use PostgreSQL as backend storage in Vault specify `spec.backend.postgresql` in [VaultServer](/docs/v2025.5.30/concepts/vault-server-crds/vaultserver) CRD.

```yaml
spec:
  backend:
    postgresql:
      connectionURLSecret: <secret_name>
      table: <table_name>
      maxParallel: <max_parallel>
```

Here, we are going to describe the various attributes of the `spec.backend.postgresql` field.

### postgresql.connectionURLSecret

`postgresql.connectionURLSecret` is a required field that specifies the name of the secret containing the connection string to use to authenticate and connect to PostgreSQL. The secret contains the following key:

- `connection_url`

```yaml
spec:
  backend:
    postgresql:
      connectionURLSecret: "my-postgres-conn"
```

### postgresql.table

`postgresql.table` is an optional field that specifies the name of the table in which to write Vault data. If it is not specified, then Vault will set the value `vault_kv_store`. Vault will not create the table, so this table must exist in the database.

```yaml
spec:
  backend:
    postgresql:
      table: "vault_data"
```

### postgresql.maxParallel

`maxParallel` is an optional field that specifies the maximum number of parallel operations to take place. This field accepts integer value. If this field is not specified, then Vault will set value to `128`.

```yaml
spec:
  backend:
    postgresql:
      maxParallel: 124
```
