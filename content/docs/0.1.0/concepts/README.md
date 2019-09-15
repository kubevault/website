---
title: KubeVault Concepts
menu:
  docs_0.1.0:
    identifier: concepts-readme
    name: Overview
    parent: concepts
    weight: 10
menu_name: docs_0.1.0
section_menu_id: concepts
url: /docs/0.1.0/concepts/
aliases:
- /docs/0.1.0/concepts/README/
info:
  version: 0.1.0
---

# Concepts

Concepts help you learn about the different parts of the KubeVault and the abstractions it uses.

<ul class="nav nav-tabs" id="conceptsTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="operator-tab" data-toggle="tab" href="#operator" role="tab" aria-controls="operator" aria-selected="true">Vault Operator</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="csi-driver-tab" data-toggle="tab" href="#csi-driver" role="tab" aria-controls="csi-driver" aria-selected="false">Secret Engines</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="policy-mgr-tab" data-toggle="tab" href="#policy-mgr" role="tab" aria-controls="policy-mgr" aria-selected="false">Policy Management</a>
  </li>
</ul>
<div class="tab-content" id="conceptsTabContent">
  <div class="tab-pane fade show active" id="operator" role="tabpanel" aria-labelledby="operator-tab">

- What is KubeVault?
  - [Overview](/docs/0.1.0/concepts/what-is-kubevault). Provides a conceptual introduction to KubeVault operator, including the problems it solves and its high-level architecture.
- Custom Resource Definitions
  - [Vault Server](/docs/0.1.0/concepts/vault-server-crds/vaultserver). Introduces the concept of `VaultServer` for configuring a HashiCorp Vault server in a Kubernetes native way.
  - [Vault Server Version](/docs/0.1.0/concepts/vault-server-crds/vaultserverversion). Introduces the concept of `VaultServerVersion` to specify the docker images of HashiCorp Vault, Unsealer and Exporter.
- Vault Unsealer Options
  - [AWS KMS and SSM](/docs/0.1.0/concepts/vault-server-crds/unsealer/aws_kms_ssm)
  - [Azure Key Vault](/docs/0.1.0/concepts/vault-server-crds/unsealer/azure_key_vault)
  - [Google KMS GCS](/docs/0.1.0/concepts/vault-server-crds/unsealer/google_kms_gcs)
  - [Kubernetes Secret](/docs/0.1.0/concepts/vault-server-crds/unsealer/kubernetes_secret)
- Vault Server Storage
  - [Azure](/docs/0.1.0/concepts/vault-server-crds/storage/azure)
  - [DynamoDB](/docs/0.1.0/concepts/vault-server-crds/storage/dynamodb)
  - [Etcd](/docs/0.1.0/concepts/vault-server-crds/storage/etcd)
  - [GCS](/docs/0.1.0/concepts/vault-server-crds/storage/gcs)
  - [In Memory](/docs/0.1.0/concepts/vault-server-crds/storage/inmem)
  - [MySQL](/docs/0.1.0/concepts/vault-server-crds/storage/mysql)
  - [PosgreSQL](/docs/0.1.0/concepts/vault-server-crds/storage/postgresql)
  - [AWS S3](/docs/0.1.0/concepts/vault-server-crds/storage/s3)
  - [Swift](/docs/0.1.0/concepts/vault-server-crds/storage/swift)
- Authentication Methods for Vault Server
  - [AWS IAM Auth Method](/docs/0.1.0/concepts/vault-server-crds/auth-methods/aws-iam)
  - [Kubernetes Auth Method](/docs/0.1.0/concepts/vault-server-crds/auth-methods/kubernetes)
  - [TLS Certificates Auth Method](/docs/0.1.0/concepts/vault-server-crds/auth-methods/tls)
  - [Token Auth Method](/docs/0.1.0/concepts/vault-server-crds/auth-methods/token)
  - [Userpass Auth Method](/docs/0.1.0/concepts/vault-server-crds/auth-methods/userpass)

</div>
<div class="tab-pane fade" id="csi-driver" role="tabpanel" aria-labelledby="csi-driver-tab">

- AWS IAM Secret Engines
  - [AWSRole](/docs/0.1.0/concepts/secret-engine-crds/awsrole)
  - [AWSAccessKeyRequest](/docs/0.1.0/concepts/secret-engine-crds/awsaccesskeyrequest)
- Database Secret Engines
  - [MongoDBRole](/docs/0.1.0/concepts/database-crds/mongodb)
  - [MySQLRole](/docs/0.1.0/concepts/database-crds/mysql)
  - [PostgresRole](/docs/0.1.0/concepts/database-crds/postgresrole)
  - [DatabaseAccessRequest](/docs/0.1.0/concepts/database-crds/databaseaccessrequest)

</div>
<div class="tab-pane fade" id="policy-mgr" role="tabpanel" aria-labelledby="policy-mgr-tab">

- Vault Policy Management
  - [VaultPolicy](/docs/0.1.0/concepts/policy-crds/vaultpolicy)
  - [VaultPolicyBinding](/docs/0.1.0/concepts/policy-crds/vaultpolicybinding)

</div>
</div>
