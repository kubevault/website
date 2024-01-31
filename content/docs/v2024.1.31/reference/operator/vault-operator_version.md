---
title: Vault-Operator Version
menu:
  docs_v2024.1.31:
    identifier: vault-operator-version
    name: Vault-Operator Version
    parent: reference-operator
menu_name: docs_v2024.1.31
section_menu_id: reference
info:
  cli: v0.17.0
  installer: v2024.1.31
  operator: v0.17.0
  unsealer: v0.17.0
  version: v2024.1.31
---

## vault-operator version

Prints binary version number.

```
vault-operator version [flags]
```

### Options

```
      --check string   Check version constraint
  -h, --help           help for version
      --short          Print just the version number.
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-operator](/docs/v2024.1.31/reference/operator/vault-operator)	 - Vault Operator by AppsCode - HashiCorp Vault Operator for Kubernetes

