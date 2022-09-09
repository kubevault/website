---
title: Vault-Unsealer
menu:
  docs_v2022.09.09:
    identifier: vault-unsealer
    name: Vault-Unsealer
    parent: reference-unsealer
    weight: 0
menu_name: docs_v2022.09.09
section_menu_id: reference
url: /docs/v2022.09.09/reference/unsealer/
aliases:
- /docs/v2022.09.09/reference/unsealer/vault-unsealer/
info:
  cli: v0.9.0
  installer: v2022.09.09
  operator: v0.9.0
  unsealer: v0.9.0
  version: v2022.09.09
---

## vault-unsealer

Automates initialisation and unsealing of Hashicorp Vault

### Options

```
  -h, --help                             help for vault-unsealer
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-unsealer run](/docs/v2022.09.09/reference/unsealer/vault-unsealer_run)	 - Launch Vault unsealer
* [vault-unsealer version](/docs/v2022.09.09/reference/unsealer/vault-unsealer_version)	 - Prints binary version number.

