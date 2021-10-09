---
title: Vault Server Overview
menu:
  docs_v2021.10.11:
    identifier: overview-vault-server
    name: Overview
    parent: vault-server-guides
    weight: 10
menu_name: docs_v2021.10.11
section_menu_id: guides
info:
  cli: v0.5.1
  installer: v2021.10.11
  operator: v0.5.1
  unsealer: v0.5.1
  version: v2021.10.11
---

> New to KubeVault? Please start [here](/docs/v2021.10.11/concepts/README).

# Overview

The KubeVault operator makes it easy to deploy, maintain and manage Vault servers in Kubernetes clusters. It covers automatic initialization and unsealing and also stores unseal keys and root token in a secure way. The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. It has the following features:

- **Vault Policy Management**: Provides a Kubernetes native way to manage Vault policies and bind those policies to the users or the auth method roles.

  - [Vault Policy](/docs/v2021.10.11/guides/policy-management/overview#vaultpolicy)
  - [Vault Policy Binding](/docs/v2021.10.11/guides/policy-management/overview#vaultpolicybinding)

- **Vault Secret Engine Management**: Provides a Kubernetes native way to manage Vault secret engines.

  - [GCP Secret Engine](/docs/v2021.10.11/guides/secret-engines/gcp/overview)
  - [AWS Secret Engine](/docs/v2021.10.11/guides/secret-engines/aws/overview)
  - [Azure Secret Engine](/docs/v2021.10.11/guides/secret-engines/azure/overview)
  - Database Secret Engine
    - [MongoDB Secret Engine](/docs/v2021.10.11/guides/secret-engines/mongodb/overview)
    - [MySQL Secret Engine](/docs/v2021.10.11/guides/secret-engines/mysql/overview)
    - [PostgreSQL Secret Engine](/docs/v2021.10.11/guides/secret-engines/postgres/overview)

## Setup Vault Server

![Overview](/docs/v2021.10.11/images/guides/vault-server/overview_vault_server_guide.svg)

Deploy Vault server using the KubeVault operator:

- [Deploy Vault Server](/docs/v2021.10.11/guides/vault-server/vault-server)
- [Enable Vault CLI](/docs/v2021.10.11/guides/vault-server/vault-server#enable-vault-cli)

 Configure external Vault server so that the  KubeVault operator can communicate with it:

- [Configure Cluster and External Vault Server](/docs/v2021.10.11/guides/vault-server/external-vault-sever)
