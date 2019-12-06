---
title: Managing Externally Provisioned Vault Servers
menu:
  docs_v0.3.0:
    identifier: overview-auth-methods
    name: External Vault
    parent: auth-methods-vault-server-crds
    weight: 5
menu_name: docs_v0.3.0
section_menu_id: concepts
info:
  version: v0.3.0
---

> New to KubeVault? Please start [here](/docs/v0.3.0/concepts/README).

# Managing Externally Provisioned Vault Servers

The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. These Vault servers can be running outside a Kubernetes cluster or running inside a Kubernetes cluster but provisioned using a Helm chart.

The KubeVault operator uses an [AppBinding](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/appbinding) to connect to an externally provisioned Vault server. Following authentication methods are currently supported by the KubeVault operator:

- [AWS IAM Auth Method](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/aws-iam)
- [Kubernetes Auth Method](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/kubernetes)
- [TLS Certificates Auth Method](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/tls)
- [Token Auth Method](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/token)
- [Userpass Auth Method](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/userpass)
- [GCP IAM Auth Method](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/gcp-iam)
- [Azure Auth Method](/docs/v0.3.0/concepts/vault-server-crds/auth-methods/azure)
