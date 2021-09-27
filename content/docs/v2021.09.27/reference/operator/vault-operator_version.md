---
title: Vault-Operator Version
menu:
  docs_v2021.09.27:
    identifier: vault-operator-version
    name: Vault-Operator Version
    parent: reference-operator
menu_name: docs_v2021.09.27
section_menu_id: reference
info:
  cli: v0.5.0
  installer: v2021.09.27
  operator: v0.5.0
  unsealer: v0.5.0
  version: v2021.09.27
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
      --enable-analytics                 Send analytical events to Google Analytics (default true)
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-operator](/docs/v2021.09.27/reference/operator/vault-operator)	 - Vault Operator by AppsCode - HashiCorp Vault Operator for Kubernetes

