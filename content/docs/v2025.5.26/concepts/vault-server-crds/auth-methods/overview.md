---
title: Managing Externally Provisioned Vault Servers
menu:
  docs_v2025.5.26:
    identifier: overview-auth-methods
    name: External Vault
    parent: auth-methods-vault-server-crds
    weight: 5
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

# Managing Externally Provisioned Vault Servers

The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. These Vault servers can be running outside a Kubernetes cluster or running inside a Kubernetes cluster but provisioned using a Helm chart.

The KubeVault operator uses an [AppBinding](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/appbinding) to connect to an externally provisioned Vault server. Following authentication methods are currently supported by the KubeVault operator:

- [AWS IAM Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/aws-iam)
- [Kubernetes Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/kubernetes)
- [TLS Certificates Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/tls)
- [Token Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/token)
- [Userpass Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/userpass)
- [GCP IAM Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/gcp-iam)
- [Azure Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/azure)
- [JWT/OIDC Auth Method](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/jwt-oidc)
