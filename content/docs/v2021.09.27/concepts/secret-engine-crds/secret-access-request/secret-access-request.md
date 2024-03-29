---
title: SecretAccessRequest | Vault Secret Engine
menu:
  docs_v2021.09.27:
    identifier: secretaccessrequest-crds
    name: SecretAccessRequest
    parent: database-crds-concepts
    weight: 100
menu_name: docs_v2021.09.27
section_menu_id: concepts
info:
  cli: v0.5.0
  installer: v2021.09.27
  operator: v0.5.0
  unsealer: v0.5.0
  version: v2021.09.27
---

> New to KubeVault? Please start [here](/docs/v2021.09.27/concepts/README).

# SecretAccessRequest

## What is SecretAccessRequest

A `SecretAccessRequest` is a Kubernetes `CustomResourceDefinition` (CRD) which allows a user to request a Vault server for credentials in a Kubernetes native way. If `SecretAccessRequest` is approved, then the KubeVault operator will issue credentials and create Kubernetes secret containing credentials. The secret name will be specified in `status.secret.name` field.

![SecretAccessRequest CRD](/docs/v2021.09.27/images/concepts/database_accesskey_request.svg)

KubeVault operator performs the following operations when a DatabaseAccessRequest CRD is created:

- Checks whether `status.conditions[].type` is `Approved` or not
- If Approved, requests the Vault server for credentials
- Creates a Kubernetes Secret which contains the credentials
- Sets the name of the k8s secret to SecretAccessRequest's `status.secret.name`
- Assigns read permissions on that Kubernetes secret to specified subjects or user identities

## SecretAccessRequest CRD Specification

Like any official Kubernetes resource, a `SecretAccessRequest` object has `TypeMeta`, `ObjectMeta`, `Spec` and `Status` sections.

A sample `SecretAccessRequest` object is shown below:

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretAccessRequest
metadata:
  name: aws-cred-req
  namespace: dev
spec:
  roleRef:
    kind: AWSRole
    name: aws-role
  subjects:
    - kind: ServiceAccount
      name: test-user-account
      namespace: test
```

Here, we are going to describe the various sections of the `SecretAccessRequest` crd.

### SecretAccessRequest Spec

SecretAccessRequest `spec` contains information about database role and subject.

```yaml
spec:
  roleRef:
    apiGroup: <role-apiGroup>
    kind: <role-kind>
    name: <role-name>
    namespace: <role-namespace>
  subjects:
    - kind: <subject-kind>
      apiGroup: <subject-apiGroup>
      name: <subject-name>
      namespace: <subject-namespace>
  ttl: <ttl-for-leases>
```
