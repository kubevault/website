---
title: Vault-Operator
menu:
  docs_v2025.5.30:
    identifier: vault-operator
    name: Vault-Operator
    parent: reference-operator
    weight: 0
menu_name: docs_v2025.5.30
section_menu_id: reference
url: /docs/v2025.5.30/reference/operator/
aliases:
- /docs/v2025.5.30/reference/operator/vault-operator/
info:
  cli: v0.22.0
  installer: v2025.5.30
  operator: v0.22.0
  unsealer: v0.22.0
  version: v2025.5.30
---

## vault-operator

Vault Operator by AppsCode - HashiCorp Vault Operator for Kubernetes

### Options

```
      --bypass-validating-webhook-xray        if true, bypasses validating webhook xray checks
      --default-seccomp-profile-type string   Default seccomp profile
  -h, --help                                  help for vault-operator
      --use-kubeapiserver-fqdn-for-aks        if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-operator operator](/docs/v2025.5.30/reference/operator/vault-operator_operator)	 - Launch Vault operator
* [vault-operator run](/docs/v2025.5.30/reference/operator/vault-operator_run)	 - Launch KubeVault Webhook Server
* [vault-operator version](/docs/v2025.5.30/reference/operator/vault-operator_version)	 - Prints binary version number.

