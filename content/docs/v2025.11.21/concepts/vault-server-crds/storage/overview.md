---
title: Overview
menu:
  docs_v2025.11.21:
    identifier: storage-overview
    name: Overview
    parent: storage-vault-server-crds
    weight: 1
menu_name: docs_v2025.11.21
section_menu_id: concepts
info:
  cli: v0.23.0
  installer: v2025.11.21
  operator: v0.23.0
  unsealer: v0.23.0
  version: v2025.11.21
---

> New to KubeVault? Please start [here](/docs/v2025.11.21/concepts/README).

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

- [Azure](/docs/v2025.11.21/concepts/vault-server-crds/storage/azure)
- [Consul](/docs/v2025.11.21/concepts/vault-server-crds/storage/consul)
- [DynamoDB](/docs/v2025.11.21/concepts/vault-server-crds/storage/dynamodb)
- [Etcd](/docs/v2025.11.21/concepts/vault-server-crds/storage/etcd)
- [Filesystem](/docs/v2025.11.21/concepts/vault-server-crds/storage/filesystem)
- [GCS](/docs/v2025.11.21/concepts/vault-server-crds/storage/gcs)
- [Inmem](/docs/v2025.11.21/concepts/vault-server-crds/storage/inmem)
- [MySQL](/docs/v2025.11.21/concepts/vault-server-crds/storage/mysql)
- [PostgreSQL](/docs/v2025.11.21/concepts/vault-server-crds/storage/postgresql)
- [Raft](/docs/v2025.11.21/concepts/vault-server-crds/storage/raft)
- [S3](/docs/v2025.11.21/concepts/vault-server-crds/storage/s3)
- [Swift](/docs/v2025.11.21/concepts/vault-server-crds/storage/swift)
