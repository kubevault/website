---
title: Overview
menu:
  docs_v2022.06.16:
    identifier: storage-overview
    name: Overview
    parent: storage-vault-server-crds
    weight: 1
menu_name: docs_v2022.06.16
section_menu_id: concepts
info:
  cli: v0.8.0
  installer: v2022.06.16
  operator: v0.8.0
  unsealer: v0.8.0
  version: v2022.06.16
---

> New to KubeVault? Please start [here](/docs/v2022.06.16/concepts/README).

# Storage Backend

## Configuring Storage Backend

```yaml
spec:
  backend:
    <backend-type>:
      ...
```

Here, we are going to describe the various attributes of the `spec.backend` field.

List of supported modes:

- [Azure](/docs/v2022.06.16/concepts/vault-server-crds/storage/azure)
- [Consul](/docs/v2022.06.16/concepts/vault-server-crds/storage/consul)
- [DynamoDB](/docs/v2022.06.16/concepts/vault-server-crds/storage/dynamodb)
- [Etcd](/docs/v2022.06.16/concepts/vault-server-crds/storage/etcd)
- [Filesystem](/docs/v2022.06.16/concepts/vault-server-crds/storage/filesystem)
- [GCS](/docs/v2022.06.16/concepts/vault-server-crds/storage/gcs)
- [Inmem](/docs/v2022.06.16/concepts/vault-server-crds/storage/inmem)
- [MySQL](/docs/v2022.06.16/concepts/vault-server-crds/storage/mysql)
- [PostgreSQL](/docs/v2022.06.16/concepts/vault-server-crds/storage/postgresql)
- [Raft](/docs/v2022.06.16/concepts/vault-server-crds/storage/raft)
- [S3](/docs/v2022.06.16/concepts/vault-server-crds/storage/s3)
- [Swift](/docs/v2022.06.16/concepts/vault-server-crds/storage/swift)
