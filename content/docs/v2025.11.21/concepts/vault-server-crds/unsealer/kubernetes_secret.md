---
title: Kubernetes Secret | Vault Unsealer
menu:
  docs_v2025.11.21:
    identifier: kubernetes-secret-unsealer
    name: Kubernetes Secret
    parent: unsealer-vault-server-crds
    weight: 1
menu_name: docs_v2025.11.21
section_menu_id: concepts
info:
  cli: v0.23.0
  installer: v2025.11.21
  operator: v0.23.0
  unsealer: v0.23.0
  version: v2025.11.21
---

> New to KubeVault? Please start [here](/docs/v2025.11.21/concepts/README).

# mode.kubernetesSecret

To use **kubernetesSecret** mode specify `mode.kubernetesSecret`. In this mode, unseal keys and root token will be stored in a Kubernetes secret.

```yaml
spec:
  unsealer:
    mode:
      kubernetesSecret:
        secretName: <secret_name>
```

`mode.kubernetesSecret` has the following fields:

## kubernetesSecret.secretName

`kubernetesSecret.secretName` is a required field that specifies the name of the Kubernetes secret. If this secret does not exist, then Unsealer will create it. The secret will be created in the same namespace of [VaultServer](/docs/v2025.11.21/concepts/vault-server-crds/vaultserver).

```yaml
spec:
  unsealer:
    mode:
      kubernetesSecret:
        secretName: "vault-keys"
```
