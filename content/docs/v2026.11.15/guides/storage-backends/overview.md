---
title: Storage Backends Overview
menu:
  docs_v2026.11.15:
    identifier: storage-backends-overview
    name: Overview
    parent: storage-backends-guides
    weight: 1
menu_name: docs_v2026.11.15
section_menu_id: guides
info:
  cli: v0.27.0
  installer: v2026.11.15
  operator: v0.27.0
  unsealer: v0.27.0
  version: v2026.11.15
---

> New to KubeVault? Please start [here](/docs/v2026.11.15/concepts/README).

# Storage Backends

A `VaultServer` keeps the data that Vault encrypts and stores in the storage backend set under `spec.backend`. Exactly one backend can be configured. The guides in this section deploy a `VaultServer` with each of the supported backends, in the same way as the [Raft](/docs/v2026.11.15/guides/storage-backends/raft) and [PostgreSQL](/docs/v2026.11.15/guides/storage-backends/postgresql) guides: prepare the storage, create the credentials, deploy the `VaultServer`, then write a secret to see that it works.

To learn what each field of a backend does, read the [storage concepts](/docs/v2026.11.15/concepts/vault-server-crds/storage/overview).

## Supported Storage Backends

| Backend | High availability | What you need |
| --- | --- | --- |
| [Aerospike](/docs/v2026.11.15/guides/storage-backends/aerospike) | No | An Aerospike server with an existing namespace |
| [Alibaba Cloud OSS](/docs/v2026.11.15/guides/storage-backends/alicloudoss) | No | An OSS bucket and an access key pair |
| [Amazon DynamoDB](/docs/v2026.11.15/guides/storage-backends/dynamodb) | Yes (`haEnabled`) | AWS credentials (the table is created by Vault) |
| [Amazon S3](/docs/v2026.11.15/guides/storage-backends/s3) | No | An S3 bucket and an access key pair |
| [Azure Blob Storage](/docs/v2026.11.15/guides/storage-backends/azure) | No | An Azure storage account and its access key |
| [Cassandra](/docs/v2026.11.15/guides/storage-backends/cassandra) | No | Cassandra with a keyspace and a table (KubeDB `Cassandra`) |
| [CockroachDB](/docs/v2026.11.15/guides/storage-backends/cockroachdb) | Yes (`haEnabled`) | A CockroachDB cluster and a user |
| [Consul](/docs/v2026.11.15/guides/storage-backends/consul) | Yes | A Consul server |
| [CouchDB](/docs/v2026.11.15/guides/storage-backends/couchdb) | No | A CouchDB server with a database |
| [etcd](/docs/v2026.11.15/guides/storage-backends/etcd) | Yes (`haEnabled`) | An etcd v3 cluster (KubeDB `Etcd`) |
| [Filesystem](/docs/v2026.11.15/guides/storage-backends/filesystem) | No | A `PersistentVolumeClaim` (created from `volumeClaimTemplate`) |
| [Google Cloud Spanner](/docs/v2026.11.15/guides/storage-backends/spanner) | Yes (`haEnabled`) | A Spanner database with the `Vault` tables and a service account key |
| [Google Cloud Storage](/docs/v2026.11.15/guides/storage-backends/gcs) | Yes (`haEnabled`) | A GCS bucket and a service account key |
| [In-Memory](/docs/v2026.11.15/guides/storage-backends/inmem) | No | Nothing. Data is lost on restart |
| [Microsoft SQL Server](/docs/v2026.11.15/guides/storage-backends/mssql) | No | A SQL Server (KubeDB `MSSQLServer`) |
| [MySQL](/docs/v2026.11.15/guides/storage-backends/mysql) | Yes (`haEnabled`) | A MySQL server (KubeDB `MySQL`) |
| [OCI Object Storage](/docs/v2026.11.15/guides/storage-backends/oci) | Yes (`haEnabled`) | Two OCI buckets and an API signing key |
| [OpenStack Swift](/docs/v2026.11.15/guides/storage-backends/swift) | No | A Swift container and OpenStack credentials |
| [PostgreSQL](/docs/v2026.11.15/guides/storage-backends/postgresql) | Yes (`haEnabled`) | A PostgreSQL server (KubeDB `Postgres`) |
| [Raft](/docs/v2026.11.15/guides/storage-backends/raft) | Yes | A `StorageClass` for the integrated storage volumes |
| [ZooKeeper](/docs/v2026.11.15/guides/storage-backends/zookeeper) | Yes | A ZooKeeper ensemble (KubeDB `ZooKeeper`) |

## Things to Know

- **Namespace and names.** Every guide uses the `demo` namespace and a `VaultServer` named `vault`. Follow the `Cleanup` section of a guide before you start another one in the same namespace.
- **High availability.** Backends without high availability have no leader election, so run them with a single replica. Backends marked `haEnabled` run in HA mode only when that field is set. Raft, Consul and ZooKeeper are always HA-capable.
- **Credentials.** Credentials are read from a `Secret` in the namespace of the `VaultServer`, referenced by `credentialSecretRef`. For the databases that KubeDB manages, `databaseRef` points to the `AppBinding` of the database instead, and the operator takes the address, the credentials and the TLS settings from it.
- **Cleanup order.** With `terminationPolicy: WipeOut`, the operator connects to the storage backend to delete the data Vault stored there. Always delete the `VaultServer` before the storage backend it uses.
