---
title: KubeVault Concepts
menu:
  docs_v2025.5.26:
    identifier: concepts-architecture
    name: Architecture
    parent: concepts
    weight: 20
menu_name: docs_v2025.5.26
section_menu_id: concepts
info:
  cli: v0.21.0
  installer: v2025.5.26
  operator: v0.21.0
  unsealer: v0.21.0
  version: v2025.5.26
---

> New to KubeVault? Please start [here](/docs/v2025.5.26/concepts/README).


![KubeVault operator architecture](/docs/v2025.5.26/images/concepts/architecture.svg)

# Architecture

KubeVault operator is composed of the following controllers:

- A **Vault Server controller** that deploys Vault in Kubernetes clusters. It also injects unsealer and stastd exporter as sidecars to perform unsealing and monitoring respectively.

- An **Auth controller** that enables auth methods in Vault.

- A **Policy controller** that manages Vault policies and also binds Vault policies with Kubernetes service accounts.

- A **Secret Engine controller** that enables and configures Vault [secret engines](https://www.vaultproject.io/docs/secrets/index.html) based on the given configuration.

- A set of **Role controllers** that configure secret engine roles that are used to generate credentials.

- A set of **AccessKeyRequest controllers** that generate and issue credentials to the user for various secret engine roles.
