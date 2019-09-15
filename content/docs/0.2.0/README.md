---
title: Welcome | KubeVault
description: Welcome to KubeVault
menu:
  docs_0.2.0:
    identifier: readme-kubevault
    name: Readme
    parent: welcome
    weight: -1
menu_name: docs_0.2.0
section_menu_id: welcome
url: /docs/0.2.0/welcome/
aliases:
- /docs/0.2.0/
- /docs/0.2.0/README/
info:
  version: 0.2.0
---

# KubeVault

KubeVault by AppsCode is a collection of tools for running HashiCorp [Vault](https://www.vaultproject.io/) on Kubernetes. You can deploy and mange Vault using Vault operator. Using Vault operator, you can deploy Vault for following storage backends:

- [Azure Storage](/docs/0.2.0/concepts/vault-server-crds/storage/azure)
- [DynamoDB](/docs/0.2.0/concepts/vault-server-crds/storage/dynamodb)
- [Etcd](/docs/0.2.0/concepts/vault-server-crds/storage/etcd)
- [GCS](/docs/0.2.0/concepts/vault-server-crds/storage/gcs)
- [In Memory](/docs/0.2.0/concepts/vault-server-crds/storage/inmem)
- [MySQL](/docs/0.2.0/concepts/vault-server-crds/storage/mysql)
- [PosgreSQL](/docs/0.2.0/concepts/vault-server-crds/storage/postgresql)
- [AWS S3](/docs/0.2.0/concepts/vault-server-crds/storage/s3)
- [Swift](/docs/0.2.0/concepts/vault-server-crds/storage/swift)

From here you can learn all about Vault operator's architecture and how to deploy and use Vault operator.

- [Concepts](/docs/0.2.0/concepts/). Concepts explain the CRDs (CustomResourceDefinition) used by Vault operator.

- [Setup](/docs/0.2.0/setup/). Setup contains instructions for installing
  the Vault operator in various cloud providers.

- [Monitoring](/docs/0.2.0/guides/monitoring). Monitoring contains instructions for setup prometheus with Vault server

- [Guides](/docs/0.2.0/guides/). Guides show you how to perform tasks with Vault operator.

- [Reference](/docs/0.2.0/reference/). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/kubevault/project/issues/new) if you see some problem. Or better yet, submit your own [contributions](/docs/0.2.0/CONTRIBUTING) to help
make our docs better.

---

**KubeVault binaries collect anonymous usage statistics to help us learn how the software is being used and how we can improve it. To disable stats collection, run the operator with the flag** `--enable-analytics=false`.

---
