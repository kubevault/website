---
title: Welcome | KubeVault
description: Welcome to KubeVault
menu:
  docs_v2022.09.22:
    identifier: readme-kubevault
    name: Readme
    parent: welcome
    weight: -1
menu_name: docs_v2022.09.22
section_menu_id: welcome
url: /docs/v2022.09.22/welcome/
aliases:
- /docs/v2022.09.22/
- /docs/v2022.09.22/README/
info:
  cli: v0.10.0
  installer: v2022.09.22
  operator: v0.10.0
  unsealer: v0.10.0
  version: v2022.09.22
---

![KubeVault Overview](/docs/v2022.09.22/images/kubevault-overview.svg)

# KubeVault

KubeVault by AppsCode is a collection of tools for running HashiCorp [Vault](https://www.vaultproject.io/) on Kubernetes. 

## Operator
You can deploy and manage Vault on Kubernetes clusters using KubeVault operator. From here you can learn all about Vault operator's architecture and how to deploy and use Vault operator.

- [Concepts](/docs/v2022.09.22/concepts/). Concepts explain the CRDs (CustomResourceDefinition) used by Vault operator.

- [Setup](/docs/v2022.09.22/setup/). Setup contains instructions for installing
  the Vault operator in various cloud providers.

- [Monitoring](/docs/v2022.09.22/guides/monitoring/overview). Monitoring contains instructions for setup prometheus with Vault server

- [Guides](/docs/v2022.09.22/guides/). Guides show you how to perform tasks with Vault operator.

- [Reference](/docs/v2022.09.22/reference/operator). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

## CLI

[Command line interface](https://github.com/kubevault/cli) for KubeVault. This is intended to be used as a [kubectl plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/).

- [Reference](/docs/v2022.09.22/reference/cli). Detailed exhaustive lists of command-line options, configuration options, API definitions, and procedures.
- [Installation](/docs/v2022.09.22/setup/install/kubectl_plugin) Install Kubectl Vault Plugin.

## Unsealer

[Unsealer](https://github.com/kubevault/unsealer) automates the process of [initializing](https://www.vaultproject.io/docs/commands/operator/init.html) and [unsealing](https://www.vaultproject.io/docs/concepts/seal.html#unsealing) HashiCorp Vault instances running.

- [Reference](/docs/v2022.09.22/reference/unsealer). Detailed exhaustive lists of command-line options, configuration options, API definitions, and procedures.

## CSI Driver

KubeVault works seamlessly with [Secrets Store CSI driver for Kubernetes secrets](https://github.com/kubernetes-sigs/secrets-store-csi-driver).

- [Reference](https://secrets-store-csi-driver.sigs.k8s.io/) Kubernetes Secrets Store CSI Driver.


> We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/kubevault/project/issues/new) if you see some problem. Or better yet, submit your own [contributions](/docs/v2022.09.22/CONTRIBUTING) to help
make our docs better.
