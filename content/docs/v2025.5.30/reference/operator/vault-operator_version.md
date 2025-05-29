---
title: Vault-Operator Version
menu:
  docs_v2025.5.30:
    identifier: vault-operator-version
    name: Vault-Operator Version
    parent: reference-operator
menu_name: docs_v2025.5.30
section_menu_id: reference
info:
  cli: v0.22.0
  installer: v2025.5.30
  operator: v0.22.0
  unsealer: v0.22.0
  version: v2025.5.30
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
      --bypass-validating-webhook-xray        if true, bypasses validating webhook xray checks
      --default-seccomp-profile-type string   Default seccomp profile
      --use-kubeapiserver-fqdn-for-aks        if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-operator](/docs/v2025.5.30/reference/operator/vault-operator)	 - Vault Operator by AppsCode - HashiCorp Vault Operator for Kubernetes

