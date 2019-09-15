---
title: Vault Server Authentication Methods
menu:
  docs_0.2.0:
    identifier: overview-auth-methods
    name: AppBinding
    parent: auth-methods-vault-server-crds
    weight: 5
menu_name: docs_0.2.0
section_menu_id: concepts
info:
  version: 0.2.0
---

> New to KubeVault? Please start [here](/docs/0.2.0/concepts/README).

# Vault Server Authentication Methods

In Vault operator, usually Vault connection information are handled by [AppBinding](/docs/0.2.0/concepts/vault-server-crds/auth-methods/appbinding). Following authentication methods are currently supported by Vault operator using AppBinding:

- [AWS IAM Auth Method](/docs/0.2.0/concepts/vault-server-crds/auth-methods/aws-iam)
- [Kubernetes Auth Method](/docs/0.2.0/concepts/vault-server-crds/auth-methods/kubernetes)
- [TLS Certificates Auth Method](/docs/0.2.0/concepts/vault-server-crds/auth-methods/tls)
- [Token Auth Method](/docs/0.2.0/concepts/vault-server-crds/auth-methods/token)
- [Userpass Auth Method](/docs/0.2.0/concepts/vault-server-crds/auth-methods/userpass)
