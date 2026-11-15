---
title: Overview
menu:
  docs_v2026.11.15:
    identifier: storage-overview
    name: Overview
    parent: storage-vault-server-crds
    weight: 1
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

- [Azure](/docs/v2026.11.15/concepts/vault-server-crds/storage/azure)
- [Consul](/docs/v2026.11.15/concepts/vault-server-crds/storage/consul)
- [DynamoDB](/docs/v2026.11.15/concepts/vault-server-crds/storage/dynamodb)
- [Etcd](/docs/v2026.11.15/concepts/vault-server-crds/storage/etcd)
- [Filesystem](/docs/v2026.11.15/concepts/vault-server-crds/storage/filesystem)
- [GCS](/docs/v2026.11.15/concepts/vault-server-crds/storage/gcs)
- [Inmem](/docs/v2026.11.15/concepts/vault-server-crds/storage/inmem)
- [MySQL](/docs/v2026.11.15/concepts/vault-server-crds/storage/mysql)
- [PostgreSQL](/docs/v2026.11.15/concepts/vault-server-crds/storage/postgresql)
- [Raft](/docs/v2026.11.15/concepts/vault-server-crds/storage/raft)
- [S3](/docs/v2026.11.15/concepts/vault-server-crds/storage/s3)
- [Swift](/docs/v2026.11.15/concepts/vault-server-crds/storage/swift)
