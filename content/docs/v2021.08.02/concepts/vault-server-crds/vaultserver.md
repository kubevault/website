---
title: Vault Server | KubeVault Concepts
menu:
  docs_v2021.08.02:
    identifier: vaultserver-vault-server-crds
    name: Vault Server
    parent: vault-server-crds-concepts
    weight: 10
menu_name: docs_v2021.08.02
section_menu_id: concepts
info:
  cli: v0.4.0
  installer: v2021.08.02
  operator: v0.4.0
  unsealer: v0.4.0
  version: v2021.08.02
---

> New to KubeVault? Please start [here](/docs/v2021.08.02/concepts/README).

# VaultServer

## What is VaultServer

A `VaultServer` is a Kubernetes `CustomResourceDefinition` (CRD) which is used to deploy a HashiCorp Vault server on Kubernetes clusters in a Kubernetes native way.

When a `VaultServer` is created, the KubeVault operator will deploy a Vault server and create necessary Kubernetes resources required for the Vault server.

![VaultServer CRD](/docs/v2021.08.02/images/concepts/vault_server.svg)

## VaultServer CRD Specification

Like any official Kubernetes resource, a `VaultServer` object has `TypeMeta`, `ObjectMeta`, `Spec` and `Status` sections.

A sample `VaultServer` object is shown below:

```yaml
apiVersion: kubevault.com/v1alpha1
kind: VaultServer
metadata:
  name: example
  namespace: default
spec:
  replicas: 1
  version: "1.2.0"
  backend:
    inmem: {}
  tls:
    certificates:
      - alias: ca
  unsealer:
    secretShares: 4
    secretThreshold: 2
    mode:
      kubernetesSecret:
        secretName: vault-keys
status:
  authMethodStatus:
  - path: kubernetes
    status: EnableSucceeded
    type: kubernetes
  clientPort: 8200
  initialized: true
  observedGeneration: 2
  phase: Running
  serviceName: vault
  updatedNodes:
  - vault-745564bddb-4pt98
  vaultStatus:
    active: vault-745564bddb-4pt98
    unsealed:
    - vault-745564bddb-4pt98
```

Here, we are going to describe the various sections of the `VaultServer` crd.

### VaultServer Spec

VaultServer Spec contains the configuration about how to deploy Vault in the Kubernetes cluster. It also covers automate unsealing of Vault.

The `spec` section has following parts:

#### spec.replicas

`spec.replicas` specifies the number of Vault nodes to deploy. It has to be a positive number.

```yaml
spec:
  replicas: 3 # 3 vault server will be deployed in Kubernetes cluster
```

#### spec.version

Specifies the name of the `VaultServerVersion` CRD. This CRD holds the image name and version of the Vault, Unsealer, and Exporter. To know more information about `VaultServerVersion` CRD see [here](/docs/v2021.08.02/concepts/vault-server-crds/vaultserverversion).

```yaml
spec:
  version: "1.2.0"
```

#### spec.tls

`spec.tls` is an optional field that specifies the TLS policy of Vault nodes. If this is not specified, the KubeVault operator will auto-generate TLS assets and secrets.

```yaml
spec:
  tls:
    tlsSecret: <tls_assets_secret_name> # name of the secret containing TLS assets
    caBundle: <pem_coded_ca>
```

- **`tls.tlsSecret`**: Specifies the name of the secret containing TLS assets. The secret must contain following keys:
  - `tls.crt`
  - `tls.key`

  The server certificate must allow the following wildcard domains:
  - `localhost`
  - `*.<namespace>.pod`
  - `<vaultServer-name>.<namespace>.svc`

  The server certificate must allow the following IP:
  - `127.0.0.1`

- **`tls.caBundle`**: Specifies the PEM encoded CA bundle which will be used to validate the serving certificate.

#### spec.configSource

`spec.configSource` is an optional field that allows the user to provide extra configuration for Vault. This field accepts a [VolumeSource](https://github.com/kubernetes/api/blob/release-1.11/core/v1/types.go#L47). You can use any Kubernetes supported volume source such as configMap, secret, azureDisk, etc.

> Please note that the config file name must be `vault.hcl` to work.

```yaml
spec:
  configSource:
    <type of volume>: # for example `configMap`
      name: <name of volume>
```

### spec.dataSources

`spec.dataSources` is an `optional` field that allows the user to provide a list of [VolumeSources](https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes) (i.e. secrets, configmaps, etc.) which will be mounted into the VaultServer pods. These volumes will be mounted into `/etc/vault/data/<name>` directory. The first data will be named as `data-0`, the second one will be named as `data-1` and so on.

```yaml
spec:
  dataSources:
  - secret:  # mounted on /etc/vault/data/data-0
      secretName: custom-cert
  - configMap: # mounted on /etc/vault/data/data-1
      name: special-config
```

### spec.backend

`spec.backend` is a required field that specifies the Vault backend storage configuration. KubeVault operator generates storage configuration according to this `spec.backend`.

```yaml
spec:
  backend:
    ...
```

List of supported backends:

- [Azure](/docs/v2021.08.02/concepts/vault-server-crds/storage/azure)
- [Consul](/docs/v2021.08.02/concepts/vault-server-crds/storage/consul)
- [DynamoDB](/docs/v2021.08.02/concepts/vault-server-crds/storage/dynamodb)
- [Etcd](/docs/v2021.08.02/concepts/vault-server-crds/storage/etcd)
- [GCS](/docs/v2021.08.02/concepts/vault-server-crds/storage/gcs)
- [In Memory](/docs/v2021.08.02/concepts/vault-server-crds/storage/inmem)
- [MySQL](/docs/v2021.08.02/concepts/vault-server-crds/storage/mysql)
- [PosgreSQL](/docs/v2021.08.02/concepts/vault-server-crds/storage/postgresql)
- [AWS S3](/docs/v2021.08.02/concepts/vault-server-crds/storage/s3)
- [Swift](/docs/v2021.08.02/concepts/vault-server-crds/storage/swift)
- [Filesystem](/docs/v2021.08.02/concepts/vault-server-crds/storage/filesystem)

#### spec.unsealer

`spec.unsealer` is an optional field that specifies [Unsealer](https://github.com/kubevault/unsealer) configuration. Unsealer handles automatic initializing and unsealing of Vault. See [here](/docs/v2021.08.02/concepts/vault-server-crds/unsealer/unsealer) for Unsealer documentation.

```yaml
spec:
  unsealer:
    secretShares: <num_of_secret_shares>
    secretThresold: <num_of_secret_threshold>
    retryPeriodSeconds: <retry_period>
    overwriteExisting: <true/false>
    mode:
      ...
```

#### spec.serviceTemplate

You can also provide a template for the services created by KubeVault operator for VaultServer through `spec.serviceTemplate`. This will allow you to set the type and other properties of the services. `spec.serviceTemplate` is an optional field.

```yaml
spec:
  serviceTemplate:
    spec:
      type: NodePort
```

VaultServer allows following fields to be set in `spec.serviceTemplate`:

- metadata:
  - annotations (set as annotations on Vault service)
- spec:
  - type
  - ports
  - clusterIP
  - externalIPs
  - loadBalancerIP
  - loadBalancerSourceRanges
  - externalTrafficPolicy
  - healthCheckNodePort
  - sessionAffinityConfig

#### spec.podTemplate

VaultServer allows providing a template for Vault pod through `spec.podTemplate`. KubeVault operator will pass the information provided in `spec.podTemplate` to the Deployment created for Vault. `spec.podTemplate` is an optional field.

```yaml
spec:
  podTemplate:
    spec:
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
```

VaultServer accepts the following fields to set in `spec.podTemplate:`

- metadata:
  - annotations (set as annotations on Vault pods)
- controller:
  - annotations (set as annotations on Vault statefulset)
- spec:
  - resources
  - imagePullSecrets
  - nodeSelector
  - affinity
  - schedulerName
  - tolerations
  - priorityClassName
  - priority
  - securityContext

You can find the full list of fields [here](https://github.com/kmodules/offshoot-api/blob/kubernetes-1.16.3/api/v1/types.go). Some of the fields of `spec.podTemplate` are described below:

##### spec.podTemplate.spec.imagePullSecret

`spec.podTemplate.spec.imagePullSecrets` is an optional field that points to secrets to be used for pulling docker images if you are using a private docker registry.

##### spec.podTemplate.spec.nodeSelector

`spec.podTemplate.spec.nodeSelector` is an optional field that specifies a map of key-value pairs. For the pod to be eligible to run on a node, the node must have each of the indicated key-value pairs as labels (it can have additional labels as well). To learn more, see [here](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#nodeselector) .

##### spec.podTemplate.spec.resources

`spec.podTemplate.spec.resources` is an optional field. This can be used to request compute resources required by Vault pods. To learn more, visit [here](http://kubernetes.io/docs/user-guide/compute-resources/).

#### spec.authMethods

`spec.authMethods` is an optional field that specifies the list of auth methods to enable in Vault.

```yaml
spec:
  authMethods:
    - type: kubernetes
      path: k8s
    - type: aws
      path: aws
```

`spec.authMethods` has following fields:

##### spec.authMethods[].type

`spec.authMethods[].type` is a required field that specifies the name of the authentication method type.

##### spec.authMethods[].path

`spec.authMethods[].path` is a required field that specifies the path where to enable the auth method.

##### spec.authMethods[].description

`spec.authMethods[].description` is an optional field that specifies a human-friendly description of the auth method.

##### spec.authMethods[].pluginName

`spec.authMethods[].pluginName` is an optional field that specifies the name of the auth plugin to use based on the name in the plugin catalog.

##### spec.authMethods[].local

`spec.authMethods[].local` is an optional field that specifies if the auth method is local only. Local auth methods are not replicated nor (if a secondary) removed by replication.

##### spec.authMethods[].config

`spec.authMethods[].config` is an optional field that specifies configuration options for auth method.

`spec.authMethods[].config` has following fields:

- `defaultLeaseTTL` : `Optional`. Specifies the default lease duration.

- `maxLeaseTTL` : `Optional`. Specifies the maximum lease duration.

- `pluginName` : `Optional`. Specifies the name of the plugin in the plugin catalog to use.

- `auditNonHMACRequestKeys` : `Optional`. Specifies the list of keys that will not be HMAC'd by audit devices in the request data object.

- `auditNonHMACResponseKeys`: `Optional`. Specifies the list of keys that will not be HMAC'd by audit devices in the response data object.

- `listingVisibility`: `Optional`.  Specifies whether to show this is mount in the UI-specific listing endpoint.

- `passthroughRequestHeaders`: `Optional`. Specifies a list of headers to whitelist and pass from the request to the backend.

### VaultServer Status

VaultServer Status shows the status of a Vault deployment. The status of the Vault is monitored and updated by the KubeVault operator.

```yaml
status:
  phase: <phase>
  initialized: <true/false>
  serviceName: <service_name>
  clientPort: <client_port>
  vaultStatus:
    active: <active_vault_pod_name>
    standby: <names_of_the_standby_vault_pod>
    sealed: <names_of_the_sealed_vault_pod>
    unsealed: <names_of_the_unsealed_vault_pod>
```

- `phase`: Indicates the phase Vault is currently in.

- `initialized`: Indicates whether Vault is initialized or not.

- `serviceName`: Name of the service by which Vault can be accessed.

- `clientPort`: Indicates the port client will use to communicate with Vault.

- `vaultStatus`: Indicates the status of Vault pods. It has the following fields:

  - `active`: Name of the active vault pod.

  - `standby`: Names of the standby vault pods.

  - `sealed`: Names of the sealed vault pods.

  - `unsealed`: Names of the unsealed vault pods.

- `authMethodStatus` : Indicates the status of the auth methods specified in `spec.authMethods`. It has the following fields:

  - `type`: Specifies the name of the authentication method type

  - `path`: Specifies the path in which the auth method is enabled.

  - `status`: Specifies whether the auth method is enabled or not.

  - `reason`: Specifies the reason why failed to enable the auth method.
