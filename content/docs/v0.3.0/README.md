---
title: Welcome | KubeVault
description: Welcome to KubeVault
menu:
  docs_v0.3.0:
    identifier: readme-kubevault
    name: Readme
    parent: welcome
    weight: -1
menu_name: docs_v0.3.0
section_menu_id: welcome
url: /docs/v0.3.0/welcome/
aliases:
- /docs/v0.3.0/
- /docs/v0.3.0/README/
info:
  version: v0.3.0
---

![KubeVault Overview](/docs/v0.3.0/images/kubevault-overview.svg)

# KubeVault

KubeVault by AppsCode is a collection of tools for running HashiCorp [Vault](https://www.vaultproject.io/) on Kubernetes. 

## Operator
You can deploy and manage Vault on Kubernetes clusters using [KubeVault operator](https://github.com/kubevault/operator). Using Vault operator, you can deploy Vault for following storage backends:

- [Azure Storage](/docs/v0.3.0/concepts/vault-server-crds/storage/azure)
- [DynamoDB](/docs/v0.3.0/concepts/vault-server-crds/storage/dynamodb)
- [Etcd](/docs/v0.3.0/concepts/vault-server-crds/storage/etcd)
- [GCS](/docs/v0.3.0/concepts/vault-server-crds/storage/gcs)
- [In Memory](/docs/v0.3.0/concepts/vault-server-crds/storage/inmem)
- [MySQL](/docs/v0.3.0/concepts/vault-server-crds/storage/mysql)
- [PosgreSQL](/docs/v0.3.0/concepts/vault-server-crds/storage/postgresql)
- [AWS S3](/docs/v0.3.0/concepts/vault-server-crds/storage/s3)
- [Swift](/docs/v0.3.0/concepts/vault-server-crds/storage/swift)
- [Consul](/docs/v0.3.0/concepts/vault-server-crds/storage/consul)

From here you can learn all about Vault operator's architecture and how to deploy and use Vault operator.

- [Concepts](/docs/v0.3.0/concepts/). Concepts explain the CRDs (CustomResourceDefinition) used by Vault operator.

- [Setup](/docs/v0.3.0/setup/). Setup contains instructions for installing
  the Vault operator in various cloud providers.

- [Monitoring](/docs/v0.3.0/guides/monitoring). Monitoring contains instructions for setup prometheus with Vault server

- [Guides](/docs/v0.3.0/guides/). Guides show you how to perform tasks with Vault operator.

- [Reference](/docs/v0.3.0/reference/). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

## CLI

[Command line interface](https://github.com/kubevault/cli) for KubeVault. This is intended to be used as a [kubectl plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/).

## Unsealer

[Unsealer](https://github.com/kubevault/unsealer) automates the process of [initializing](https://www.vaultproject.io/docs/commands/operator/init.html) and [unsealing](https://www.vaultproject.io/docs/concepts/seal.html#unsealing) HashiCorp Vault instances running.

## CSI Driver

[CSI Driver](https://github.com/kubevault/csi-driver) project implements a Kubernetes CSI driver for HashiCorp Vault servers.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/kubevault/project/issues/new) if you see some problem. Or better yet, submit your own [contributions](/docs/v0.3.0/CONTRIBUTING) to help
make our docs better.

---

**KubeVault binaries collect anonymous usage statistics to help us learn how the software is being used and how we can improve it. To disable stats collection, run the operator with the flag** `--enable-analytics=false`.

---
