---
title: Vault Server Overview
menu:
  docs_v2022.12.28:
    identifier: overview-vault-server
    name: Overview
    parent: vault-server-guides
    weight: 10
menu_name: docs_v2022.12.28
section_menu_id: guides
info:
  cli: v0.13.0
  installer: v2022.12.28
  operator: v0.13.0
  unsealer: v0.13.0
  version: v2022.12.28
---

> New to KubeVault? Please start [here](/docs/v2022.12.28/concepts/README).

# Overview

The KubeVault operator makes it easy to deploy, maintain and manage Vault servers in Kubernetes clusters. It covers automatic initialization and unsealing and also stores unseal keys and root token in a secure way. The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. It has the following features:

- **Vault Policy Management**: Provides a Kubernetes native way to manage Vault policies and bind those policies to the users or the auth method roles.

  - [Vault Policy](/docs/v2022.12.28/guides/policy-management/overview#vaultpolicy)
  - [Vault Policy Binding](/docs/v2022.12.28/guides/policy-management/overview#vaultpolicybinding)

- **Vault Secret Engine Management**: Provides a Kubernetes native way to manage Vault secret engines.

  - [GCP Secret Engine](/docs/v2022.12.28/guides/secret-engines/gcp/overview)
  - [AWS Secret Engine](/docs/v2022.12.28/guides/secret-engines/aws/overview)
  - [Azure Secret Engine](/docs/v2022.12.28/guides/secret-engines/azure/overview)
  - Database Secret Engine
    - [MongoDB Secret Engine](/docs/v2022.12.28/guides/secret-engines/mongodb/overview)
    - [MySQL Secret Engine](/docs/v2022.12.28/guides/secret-engines/mysql/overview)
    - [PostgreSQL Secret Engine](/docs/v2022.12.28/guides/secret-engines/postgres/overview)

## Setup Vault Server

![Overview](/docs/v2022.12.28/images/guides/vault-server/overview_vault_server_guide.svg)

Deploy Vault server using the KubeVault operator:

- [Deploy Vault Server](/docs/v2022.12.28/guides/vault-server/vault-server)
- [Enable Vault CLI](/docs/v2022.12.28/guides/vault-server/vault-server#enable-vault-cli)

 Configure external Vault server so that the  KubeVault operator can communicate with it:

- [Configure Cluster and External Vault Server](/docs/v2022.12.28/guides/vault-server/external-vault-sever)
