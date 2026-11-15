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

- [Aerospike](/docs/v2026.11.15/concepts/vault-server-crds/storage/aerospike)
- [Alibaba Cloud OSS](/docs/v2026.11.15/concepts/vault-server-crds/storage/alicloudoss)
- [Amazon DynamoDB](/docs/v2026.11.15/concepts/vault-server-crds/storage/dynamodb)
- [Amazon S3](/docs/v2026.11.15/concepts/vault-server-crds/storage/s3)
- [Azure Blob Storage](/docs/v2026.11.15/concepts/vault-server-crds/storage/azure)
- [Cassandra](/docs/v2026.11.15/concepts/vault-server-crds/storage/cassandra)
- [CockroachDB](/docs/v2026.11.15/concepts/vault-server-crds/storage/cockroachdb)
- [Consul](/docs/v2026.11.15/concepts/vault-server-crds/storage/consul)
- [CouchDB](/docs/v2026.11.15/concepts/vault-server-crds/storage/couchdb)
- [etcd](/docs/v2026.11.15/concepts/vault-server-crds/storage/etcd)
- [Filesystem](/docs/v2026.11.15/concepts/vault-server-crds/storage/filesystem)
- [Google Cloud Spanner](/docs/v2026.11.15/concepts/vault-server-crds/storage/spanner)
- [Google Cloud Storage](/docs/v2026.11.15/concepts/vault-server-crds/storage/gcs)
- [In-Memory](/docs/v2026.11.15/concepts/vault-server-crds/storage/inmem)
- [Microsoft SQL Server](/docs/v2026.11.15/concepts/vault-server-crds/storage/mssql)
- [MySQL](/docs/v2026.11.15/concepts/vault-server-crds/storage/mysql)
- [OCI Object Storage](/docs/v2026.11.15/concepts/vault-server-crds/storage/oci)
- [OpenStack Swift](/docs/v2026.11.15/concepts/vault-server-crds/storage/swift)
- [PostgreSQL](/docs/v2026.11.15/concepts/vault-server-crds/storage/postgresql)
- [Raft](/docs/v2026.11.15/concepts/vault-server-crds/storage/raft)
- [ZooKeeper](/docs/v2026.11.15/concepts/vault-server-crds/storage/zookeeper)
