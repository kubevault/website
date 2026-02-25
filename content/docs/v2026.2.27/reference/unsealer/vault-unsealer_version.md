---
title: Vault-Unsealer Version
menu:
  docs_v2026.2.27:
    identifier: vault-unsealer-version
    name: Vault-Unsealer Version
    parent: reference-unsealer
menu_name: docs_v2026.2.27
section_menu_id: reference
info:
  cli: v0.24.0
  installer: v2026.2.27
  operator: v0.24.0
  unsealer: v0.24.0
  version: v2026.2.27
---

## vault-unsealer version

Prints binary version number.

```
vault-unsealer version [flags]
```

### Options

```
      --check string   Check version constraint
  -h, --help           help for version
      --short          Print just the version number.
```

### Options inherited from parent commands

```
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-unsealer](/docs/v2026.2.27/reference/unsealer/vault-unsealer)	 - Automates initialisation and unsealing of Hashicorp Vault

