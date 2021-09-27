---
title: KubeVault Concepts
menu:
  docs_v2021.09.27:
    identifier: concepts-readme
    name: Concepts
    parent: concepts
    weight: 10
menu_name: docs_v2021.09.27
section_menu_id: concepts
url: /docs/v2021.09.27/concepts/
aliases:
- /docs/v2021.09.27/concepts/README/
info:
  cli: v0.5.0
  installer: v2021.09.27
  operator: v0.5.0
  unsealer: v0.5.0
  version: v2021.09.27
---

# Concepts

Concepts help you learn about the different parts of KubeVault and the abstractions it uses.

- What is KubeVault?
  - [Overview](/docs/v2021.09.27/concepts/overview). Provides an introduction to KubeVault operator, including the problems it solves and its use cases.
  - [Operator architecture](/docs/v2021.09.27/concepts/architecture). Provides a high-level illustration of the architecture of the KubeVault operator.

<ul class="nav nav-tabs" id="conceptsTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="vault-server-tab" data-toggle="tab" href="#vault-server" role="tab" aria-controls="vault-server" aria-selected="true">Vault Server</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="secret-engine-tab" data-toggle="tab" href="#secret-engine" role="tab" aria-controls="secret-engine" aria-selected="false">Secret Engines</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="vault-policy-tab" data-toggle="tab" href="#vault-policy" role="tab" aria-controls="vault-policy" aria-selected="false">Vault Policies</a>
  </li>
</ul>
<div class="tab-content" id="conceptsTabContent">
  <div class="tab-pane fade show active" id="vault-server" role="tabpanel" aria-labelledby="vault-server-tab">

## AppBinding

Introduces a way to specify connection information, credential, and parameters that are necessary for communicating with an app or service.

- [AppBinding](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/appbinding)

## Vault Server Version

Introduces the concept of `VaultServerVersion` to specify the docker images of HashiCorp Vault, Unsealer, and Exporter.

- [VaultServerVersion](/docs/v2021.09.27/concepts/vault-server-crds/vaultserverversion)

## Vault Server

Introduces the concept of `VaultServer` for configuring a HashiCorp Vault server in a Kubernetes native way.

- [VaultServer](/docs/v2021.09.27/concepts/vault-server-crds/vaultserver)

  - Vault Unsealer Options
    - [AWS KMS and SSM](/docs/v2021.09.27/concepts/vault-server-crds/unsealer/aws_kms_ssm)
    - [Azure Key Vault](/docs/v2021.09.27/concepts/vault-server-crds/unsealer/azure_key_vault)
    - [Google KMS GCS](/docs/v2021.09.27/concepts/vault-server-crds/unsealer/google_kms_gcs)
    - [Kubernetes Secret](/docs/v2021.09.27/concepts/vault-server-crds/unsealer/kubernetes_secret)

  - Vault Server Storage
    - [Azure](/docs/v2021.09.27/concepts/vault-server-crds/storage/azure)
    - [DynamoDB](/docs/v2021.09.27/concepts/vault-server-crds/storage/dynamodb)
    - [Etcd](/docs/v2021.09.27/concepts/vault-server-crds/storage/etcd)
    - [GCS](/docs/v2021.09.27/concepts/vault-server-crds/storage/gcs)
    - [In Memory](/docs/v2021.09.27/concepts/vault-server-crds/storage/inmem)
    - [MySQL](/docs/v2021.09.27/concepts/vault-server-crds/storage/mysql)
    - [PosgreSQL](/docs/v2021.09.27/concepts/vault-server-crds/storage/postgresql)
    - [AWS S3](/docs/v2021.09.27/concepts/vault-server-crds/storage/s3)
    - [Swift](/docs/v2021.09.27/concepts/vault-server-crds/storage/swift)
    - [Consul](/docs/v2021.09.27/concepts/vault-server-crds/storage/consul)
    - [Raft](/docs/v2021.09.27/concepts/vault-server-crds/storage/raft)

  - Authentication Methods for Vault Server
    - [AWS IAM Auth Method](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/aws-iam)
    - [Kubernetes Auth Method](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/kubernetes)
    - [TLS Certificates Auth Method](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/tls)
    - [Token Auth Method](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/token)
    - [Userpass Auth Method](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/userpass)
    - [GCP IAM Auth Method](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/gcp-iam)
    - [Azure Auth Method](/docs/v2021.09.27/concepts/vault-server-crds/auth-methods/azure)

</div>
<div class="tab-pane fade" id="secret-engine" role="tabpanel" aria-labelledby="secret-engine-tab">

## Secret Engine

`SecretEngine` is a Kubernetes `Custom Resource Definition`(CRD). It provides a way to enable and configure a Vault secret engine.

- [Secret Engine](/docs/v2021.09.27/concepts/secret-engine-crds/secretengine)

  - AWS IAM Secret Engine
    - [AWSRole](/docs/v2021.09.27/concepts/secret-engine-crds/aws-secret-engine/awsrole)

  - GCP Secret Engine
    - [GCPRole](/docs/v2021.09.27/concepts/secret-engine-crds/gcp-secret-engine/gcprole)

  - Azure Secret Engine
    - [AzureRole](/docs/v2021.09.27/concepts/secret-engine-crds/azure-secret-engine/azurerole)

  - Database Secret Engines
    - [MongoDBRole](/docs/v2021.09.27/concepts/secret-engine-crds/database-secret-engine/mongodb)
    - [MySQLRole](/docs/v2021.09.27/concepts/secret-engine-crds/database-secret-engine/mysql)
    - [PostgresRole](/docs/v2021.09.27/concepts/secret-engine-crds/database-secret-engine/postgresrole)
    - [ElasticsearchRole](/docs/v2021.09.27/concepts/secret-engine-crds/database-secret-engine/elasticsearch)

</div>
<div class="tab-pane fade" id="vault-policy" role="tabpanel" aria-labelledby="vault-policy-tab">

## Vault Policy

Everything in the Vault is path-based, and policies are no exception. Policies provide a declarative way to grant or forbid access to certain operations in Vault. Policies are `deny` by default, so an empty policy grants no permission in the system.

- [VaultPolicy](/docs/v2021.09.27/concepts/policy-crds/vaultpolicy): is used to create, update or delete Vault policies.
- [VaultPolicyBinding](/docs/v2021.09.27/concepts/policy-crds/vaultpolicybinding): is used to create Vault auth roles associated with an authentication type/entity and a set of Vault policies.

</div>
</div>
