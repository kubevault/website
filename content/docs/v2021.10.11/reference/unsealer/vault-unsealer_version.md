---
title: Vault-Unsealer Version
menu:
  docs_v2021.10.11:
    identifier: vault-unsealer-version
    name: Vault-Unsealer Version
    parent: reference-unsealer
menu_name: docs_v2021.10.11
section_menu_id: reference
info:
  cli: v0.5.1
  installer: v2021.10.11
  operator: v0.5.1
  unsealer: v0.5.1
  version: v2021.10.11
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
      --enable-analytics                 Send analytical events to Google Analytics (default true)
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-unsealer](/docs/v2021.10.11/reference/unsealer/vault-unsealer)	 - Automates initialisation and unsealing of Hashicorp Vault

