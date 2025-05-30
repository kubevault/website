---
title: Vault Generate
menu:
  docs_v2025.5.30:
    identifier: vault-generate
    name: Vault Generate
    parent: reference-cli
menu_name: docs_v2025.5.30
section_menu_id: reference
info:
  cli: v0.22.0
  installer: v2025.5.30
  operator: v0.22.0
  unsealer: v0.22.0
  version: v2025.5.30
---

## vault generate

Generate secretproviderclass

### Synopsis

Generate secretproviderclass from secretrolebinding. Provide flags secretrolebinding, role and keys to mount.

See more about Secrets-Store-CSI-Driver and the usage of SecretProviderClass:
	Link: https://secrets-store-csi-driver.sigs.k8s.io/concepts.html#secretproviderclass

SecretRoleBinding needs to be created and successful beforehand. Provided roles must be in the SecretRoleBinding and provided keys must be available for the RoleKind.

Output format can be yaml or json, defaults to yaml.

Examples:
 # Generate secretproviderclass with name <name1> and namespace <ns1>
 # secretrolebinding with namespace <ns2> and name <name2>
 # vaultrole kind MongoDBRole and name <name3>
 # keys to mount <secretKey> and it's mapping name <objectName>

 $ kubectl vault generate secretproviderclass <name1> -n <ns1> \
 --secretrolebinding=<ns2>/<name2> \
 --vaultrole=MongoDBRole/<name3> \
 --keys <secretKey>=<objectName> -o yaml

 # Generate secretproviderclass for the MongoDB username and password

 $ kubectl vault generate secretproviderclass mongo-secret-provider -n test \
 --secretrolebinding=dev/secret-r-binding \
 --vaultrole=MongoDBRole/mongo-role \
 --keys username=mongo-user --keys password=mongo-pass -o yaml


```
vault generate [flags]
```

### Options

```
  -f, --filename strings            Filename, directory, or URL to files identifying the resource to update
  -h, --help                        help for generate
      --keys stringToString         Key/Value map used to store the keys to read and their mapping keys. secretKey=objectName (default [])
  -k, --kustomize string            Process the kustomization directory. This flag can't be used together with -f or -R.
  -o, --output string               output format yaml/json. default to yaml
  -R, --recursive                   Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
  -b, --secretrolebinding string    secret role binding. namespace/name
  -p, --vault-ca-cert-path string   vault CA cert path in secret provider, default to Insecure mode.
  -r, --vaultrole string            vault role. RoleKind/name
```

### Options inherited from parent commands

```
      --as string                             Username to impersonate for the operation. User could be a regular user or a service account in a namespace.
      --as-group stringArray                  Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --as-uid string                         UID to impersonate for the operation.
      --cache-dir string                      Default cache directory (default "/home/runner/.kube/cache")
      --certificate-authority string          Path to a cert file for the certificate authority
      --client-certificate string             Path to a client certificate file for TLS
      --client-key string                     Path to a client key file for TLS
      --cluster string                        The name of the kubeconfig cluster to use
      --context string                        The name of the kubeconfig context to use
      --default-seccomp-profile-type string   Default seccomp profile
      --disable-compression                   If true, opt-out of response compression for all requests to the server
      --insecure-skip-tls-verify              If true, the server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kubeconfig string                     Path to the kubeconfig file to use for CLI requests.
      --match-server-version                  Require server version to match client version
  -n, --namespace string                      If present, the namespace scope for this CLI request
      --request-timeout string                The length of time to wait before giving up on a single server request. Non-zero values should contain a corresponding time unit (e.g. 1s, 2m, 3h). A value of zero means don't timeout requests. (default "0")
  -s, --server string                         The address and port of the Kubernetes API server
      --tls-server-name string                Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                          Bearer token for authentication to the API server
      --user string                           The name of the kubeconfig user to use
```

### SEE ALSO

* [vault](/docs/v2025.5.30/reference/cli/vault)	 - KubeVault cli by AppsCode

