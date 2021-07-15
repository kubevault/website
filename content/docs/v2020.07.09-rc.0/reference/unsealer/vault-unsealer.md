---
title: Vault-Unsealer
menu:
  docs_v2020.07.09-rc.0:
    identifier: vault-unsealer
    name: Vault-Unsealer
    parent: reference-unsealer
    weight: 0
menu_name: docs_v2020.07.09-rc.0
section_menu_id: reference
url: /docs/v2020.07.09-rc.0/reference/unsealer/
aliases:
- /docs/v2020.07.09-rc.0/reference/unsealer/vault-unsealer/
info:
  cli: v0.4.0-rc.0
  csi: v0.4.0-rc.0
  installer: v2021.07.14-rc.0
  operator: v0.4.0-rc.0
  unsealer: v0.4.0-rc.0
  version: v2020.07.09-rc.0
---

## vault-unsealer

Automates initialisation and unsealing of Hashicorp Vault

### Options

```
      --enable-analytics                 Send analytical events to Google Analytics (default true)
  -h, --help                             help for vault-unsealer
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [vault-unsealer run](/docs/v2020.07.09-rc.0/reference/unsealer/vault-unsealer_run)	 - Launch Vault unsealer
* [vault-unsealer version](/docs/v2020.07.09-rc.0/reference/unsealer/vault-unsealer_version)	 - Prints binary version number.

