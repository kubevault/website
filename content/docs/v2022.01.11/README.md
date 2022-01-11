---
title: Welcome | KubeVault
description: Welcome to KubeVault
menu:
  docs_v2022.01.11:
    identifier: readme-kubevault
    name: Readme
    parent: welcome
    weight: -1
menu_name: docs_v2022.01.11
section_menu_id: welcome
url: /docs/v2022.01.11/welcome/
aliases:
- /docs/v2022.01.11/
- /docs/v2022.01.11/README/
info:
  cli: v0.6.0
  installer: v2022.01.11
  operator: v0.6.0
  unsealer: v0.6.0
  version: v2022.01.11
---

![KubeVault Overview](/docs/v2022.01.11/images/kubevault-overview.svg)

# KubeVault

KubeVault by AppsCode is a collection of tools for running HashiCorp [Vault](https://www.vaultproject.io/) on Kubernetes. 

## Operator
You can deploy and manage Vault on Kubernetes clusters using KubeVault operator. Using KubeVault operator, you can deploy Vault for following storage backends:

- [Azure Storage](/docs/v2022.01.11/concepts/vault-server-crds/storage/azure)
- [DynamoDB](/docs/v2022.01.11/concepts/vault-server-crds/storage/dynamodb)
- [Etcd](/docs/v2022.01.11/concepts/vault-server-crds/storage/etcd)
- [GCS](/docs/v2022.01.11/concepts/vault-server-crds/storage/gcs)
- [In Memory](/docs/v2022.01.11/concepts/vault-server-crds/storage/inmem)
- [MySQL](/docs/v2022.01.11/concepts/vault-server-crds/storage/mysql)
- [PosgreSQL](/docs/v2022.01.11/concepts/vault-server-crds/storage/postgresql)
- [AWS S3](/docs/v2022.01.11/concepts/vault-server-crds/storage/s3)
- [Swift](/docs/v2022.01.11/concepts/vault-server-crds/storage/swift)
- [Consul](/docs/v2022.01.11/concepts/vault-server-crds/storage/consul)
- [Raft](/docs/v2022.01.11/concepts/vault-server-crds/storage/raft)

From here you can learn all about Vault operator's architecture and how to deploy and use Vault operator.

- [Concepts](/docs/v2022.01.11/concepts/). Concepts explain the CRDs (CustomResourceDefinition) used by Vault operator.

- [Setup](/docs/v2022.01.11/setup/). Setup contains instructions for installing
  the Vault operator in various cloud providers.

- [Monitoring](/docs/v2022.01.11/guides/monitoring). Monitoring contains instructions for setup prometheus with Vault server

- [Guides](/docs/v2022.01.11/guides/). Guides show you how to perform tasks with Vault operator.

- [Reference](/docs/v2022.01.11/reference/). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

## CLI

[Command line interface](https://github.com/kubevault/cli) for KubeVault. This is intended to be used as a [kubectl plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/).

## Unsealer

[Unsealer](https://github.com/kubevault/unsealer) automates the process of [initializing](https://www.vaultproject.io/docs/commands/operator/init.html) and [unsealing](https://www.vaultproject.io/docs/concepts/seal.html#unsealing) HashiCorp Vault instances running.

## CSI Driver

KubeVault works seamlessly with [Secrets Store CSI driver for Kubernetes secrets](https://github.com/kubernetes-sigs/secrets-store-csi-driver).

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/kubevault/kubevault/issues/new) if you see some problem. Or better yet, submit your own [contributions](/docs/v2022.01.11/CONTRIBUTING) to help
make our docs better.
