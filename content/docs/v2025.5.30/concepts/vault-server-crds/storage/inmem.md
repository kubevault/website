---
title: In Memory | Vault Server Storage
menu:
  docs_v2025.5.30:
    identifier: inmem-storage
    name: In Memory
    parent: storage-vault-server-crds
    weight: 30
menu_name: docs_v2025.5.30
section_menu_id: concepts
info:
  cli: v0.22.0
  installer: v2025.5.30
  operator: v0.22.0
  unsealer: v0.22.0
  version: v2025.5.30
---

> New to KubeVault? Please start [here](/docs/v2025.5.30/concepts/README).

# In-Memory

In `In-Memory` backend storage, Vault data will be kept in memory. If the Kubernetes pod on which Vault is running is restarted, then all data will be lost. This is useful for development and experimentation, but the use of this backend is highly discouraged in production. Vault documentation for In-Memory storage can be found in [here](https://www.vaultproject.io/docs/configuration/storage/in-memory.html).

```yaml
apiVersion: kubevault.com/v1alpha1
kind: VaultServer
metadata:
  name: vault-with-inmem
  namespace: demo
spec:
  replicas: 1
  version: "1.2.0"
  backend:
    inmem: {}
```

## spec.backend.inmem

To use In-Memory as storage backend in Vault specify `spec.backend.inmem` in [VaultServer](/docs/v2025.5.30/concepts/vault-server-crds/vaultserver) CRD.

```yaml
spec:
  backend:
    inmem: {}
```
