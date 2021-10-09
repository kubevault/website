---
title: Azure | Vault Server Storage
menu:
  docs_v2021.10.11:
    identifier: azure-storage
    name: Azure
    parent: storage-vault-server-crds
    weight: 10
menu_name: docs_v2021.10.11
section_menu_id: concepts
info:
  cli: v0.5.1
  installer: v2021.10.11
  operator: v0.5.1
  unsealer: v0.5.1
  version: v2021.10.11
---

> New to KubeVault? Please start [here](/docs/v2021.10.11/concepts/README).

# Azure

In Azure storage backend, Vault data will be stored in [Azure Storage Container](https://azure.microsoft.com/en-us/services/storage/). Vault documentation for azure storage can be found in [here](https://www.vaultproject.io/docs/configuration/storage/azure.html).

```yaml
apiVersion: kubevault.com/v1alpha1
kind: VaultServer
metadata:
  name: vault-with-azure
  namespace: demo
spec:
  replicas: 1
  version: "1.2.0"
  backend:
    azure:
      accountName: "vault-ac"
      accountKeySecret: "azure-cred"
      container: "my-vault-storage"
```

## spec.backend.azure

To use Azure as backend storage in Vault specify `spec.backend.azure` in [VaultServer](/docs/v2021.10.11/concepts/vault-server-crds/vaultserver) CRD.

```yaml
spec:
  backend:
    azure:
      accountName: <storage_account_name>
      accountKeySecret: <storage_account_key_secret_name>
      container: <container_name>
      maxParallel: <max_parallel>
```

Here, we are going to describe the various attributes of the `spec.backend.azure` field.

### azure.accountName

`azure.accountName` is a required field that specifies the Azure Storage account name.

```yaml
spec:
  backend:
    azure:
      accountName: "my-vault-storage"
```

### azure.accountKeySecret

`azure.accountKeySecret` is a required field that specifies the name of the secret containing Azure Storage account key. The secret contains the following key:

- `account_key`

```yaml
spec:
  backend:
    azure:
      accountKeySecret: "azure-storage-key"
```

### azure.container

`azure.container` is a required field that specifies the Azure Storage Blob container name.

```yaml
spec:
  backend:
    azure:
      container: "my-vault-storage"
```

### azure.maxParallel

`maxParallel` is an optional field that specifies the maximum number of parallel operations to take place. This field accepts integer value. If this field is not specified, then Vault will set value to `128`.

```yaml
spec:
  backend:
    azure:
      maxParallel: 124
```
