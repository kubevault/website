---
title: Vault Server Overview
menu:
  docs_v2026.11.15:
    identifier: overview-vault-server
    name: Overview
    parent: vault-server-guides
    weight: 10
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

# Overview

The KubeVault operator makes it easy to deploy, maintain and manage Vault servers in Kubernetes clusters. It covers automatic initialization and unsealing and also stores unseal keys and root token in a secure way. The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. It has the following features:

- **Vault Policy Management**: Provides a Kubernetes native way to manage Vault policies and bind those policies to the users or the auth method roles.

  - [Vault Policy](/docs/v2026.11.15/guides/policy-management/overview#vaultpolicy)
  - [Vault Policy Binding](/docs/v2026.11.15/guides/policy-management/overview#vaultpolicybinding)

- **Vault Secret Engine Management**: Provides a Kubernetes native way to manage Vault secret engines.

  - [AWS Secret Engine](/docs/v2026.11.15/guides/secret-engines/aws/overview)
  - [Azure Secret Engine](/docs/v2026.11.15/guides/secret-engines/azure/overview)
  - Database Secret Engine
    - [Druid Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/druid/overview)
    - [Elasticsearch Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/elasticsearch/overview)
    - [etcd Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/etcd/overview)
    - [Ignite Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/ignite/overview)
    - [Kafka Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/kafka/overview)
    - [MariaDB Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/mariadb/overview)
    - [Microsoft SQL Server Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/mssqlserver/overview)
    - [Milvus Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/milvus/overview)
    - [MongoDB Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/mongodb/overview)
    - [MySQL Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/mysql/overview)
    - [Neo4j Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/neo4j/overview)
    - [Oracle Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/oracle/overview)
    - [PostgreSQL Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/postgres/overview)
    - [Qdrant Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/qdrant/overview)
    - [RabbitMQ Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/rabbitmq/overview)
    - [Redis Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/redis/overview)
    - [SAP HanaDB Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/hanadb/overview)
    - [Solr Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/solr/overview)
    - [Weaviate Secret Engine](/docs/v2026.11.15/guides/secret-engines/database/weaviate/overview)
  - [GCP Secret Engine](/docs/v2026.11.15/guides/secret-engines/gcp/overview)
  - [Key/Value Secret Engine](/docs/v2026.11.15/guides/secret-engines/kv/overview)
  - [PKI Secret Engine](/docs/v2026.11.15/guides/secret-engines/pki/overview)

## Setup Vault Server

![Overview](/docs/v2026.11.15/images/guides/vault-server/overview_vault_server_guide.svg)

Deploy Vault server using the KubeVault operator:

- [Deploy Vault Server](/docs/v2026.11.15/guides/vault-server/vault-server)
- [Enable Vault CLI](/docs/v2026.11.15/guides/vault-server/vault-server#enable-vault-cli)

 Configure external Vault server so that the  KubeVault operator can communicate with it:

- [Configure Cluster and External Vault Server](/docs/v2026.11.15/guides/vault-server/external-vault-sever)
