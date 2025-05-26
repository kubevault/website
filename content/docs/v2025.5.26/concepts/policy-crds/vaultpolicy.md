---
title: VaultPolicy | KubeVault Concepts
menu:
  docs_v2025.5.26:
    identifier: vaultpolicy-policy-crds
    name: VaultPolicy
    parent: policy-crds-concepts
    weight: 10
menu_name: docs_v2025.5.26
section_menu_id: concepts
info:
  cli: v0.21.0
  installer: v2025.5.26
  operator: v0.21.0
  unsealer: v0.21.0
  version: v2025.5.26
---

> New to KubeVault? Please start [here](/docs/v2025.5.26/concepts/README).

# VaultPolicy

## What is VaultPolicy

A `VaultPolicy` is a Kubernetes `CustomResourceDefinition` (CRD) which represents Vault server [policies](https://www.vaultproject.io/docs/concepts/policies.html) in a Kubernetes native way.

When a `VaultPolicy` is created, the KubeVault operator will create a policy in the associated Vault server according to specification. If the `VaultPolicy` CRD is deleted, the respective policy will also be deleted from the Vault server.

![Vault Policy CRD](/docs/v2025.5.26/images/concepts/vault_policy.svg)

## VaultPolicy CRD Specification

Like any official Kubernetes resource, a `VaultPolicy` object has `TypeMeta`, `ObjectMeta`, `Spec` and `Status` sections.

A sample `VaultPolicy` object is shown below:

```yaml
apiVersion: policy.kubevault.com/v1alpha1
kind: VaultPolicy
metadata:
  name: secret-admin
  namespace: default
spec:
  vaultRef:
    name: vault
  policyDocument: |
    path "secret/*" {
      capabilities = ["create", "read", "update", "delete", "list"]
    }
status:
  observedGeneration: 1
  phase: Success
```

Here, we are going to describe the various sections of the `VaultPolicy` crd.

### VaultPolicy Spec

VaultPolicy `spec` contains policy and vault information necessary to create a [Vault policy](https://www.vaultproject.io/docs/concepts/policies.html). `VaultPolicy` CRD has the following fields in the `.spec` section.

#### spec.vaultRef

`spec.vaultRef` is a `required` field that specifies the name of an [AppBinding](/docs/v2025.5.26/concepts/vault-server-crds/auth-methods/appbinding) reference which is used to connect with a Vault server. AppBinding must be on the same namespace with VaultPolicy object.

```yaml
spec:
  vaultRef:
    name: vault-app
```

#### spec.vaultPolicyName

To resolve the naming conflict, KubeVault operator will generate policy names in Vault server in this format: `k8s.{clusterName}.{metadata.namespace}.{metadata.name}`. `spec.vaultPolicyName` is an `optional` field. If set, it will overwrite the generated policy name in Vault server.

```yaml
spec:
  vaultPolicyName: my-custom-policy
```

#### spec.policyDocument

`spec.policyDocument` is an `optional` field that specifies the vault policy in `hcl` format. Both `spec.policyDocument` and `spec.policy` cannot be empty at once.

```yaml
spec:
  policyDocument: |
      path "secret/*" {
        capabilities = ["create", "read", "update", "delete", "list"]
      }
      path "abc/*" {
        capabilities = ["read"]
      }
```

#### spec.policy

Vault uses [HCL](https://github.com/hashicorp/hcl) as its configuration language. HCL is also fully JSON compatible. That is, JSON can be used as a completely valid input to a system expecting HCL. This helps to make systems interoperable with other systems.

`spec.policy` is an `optional` field that accepts the vault policy in `YAML` format. This can be more convenient since Kubernetes uses YAML as its native configuration language.

```yaml
spec:
  policy:
    path:
      secret/*:
        capabilities:
        - create
        - read
        - update
        - delete
        - list
      abc/*:
        capabilities:
        - read
```

### VaultPolicy Status

VaultPolicy `status` shows the status of a Vault Policy. It is managed by the KubeVault operator. It contains the following fields:

- `observedGeneration`: Specifies the most recent generation observed for this resource. It corresponds to the resource's generation, which is updated on mutation by the API Server.

- `phase`: Indicates whether the policy successfully applied to Vault or failed.

- `conditions` : Represents the latest available observations of a VaultPolicy's current state.
