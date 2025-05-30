---
title: Vault Root-Token
menu:
  docs_v2025.5.30:
    identifier: vault-root-token
    name: Vault Root-Token
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

## vault root-token

get, set, delete, sync, generate, and rotate root-token

### Synopsis


$ kubectl vault root-token [command] [flags] to get, set, delete, sync, generate, and rotate vault root-token

Examples:
 $ kubectl vault root-token get [flags]
 $ kubectl vault root-token set [flags]
 $ kubectl vault root-token delete [flags]
 $ kubectl vault root-token sync [flags]
 $ kubectl vault root-token generate [flags]
 $ kubectl vault root-token rotate [flags]


```
vault root-token [flags]
```

### Options

```
  -h, --help   help for root-token
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
* [vault root-token delete](/docs/v2025.5.30/reference/cli/vault_root-token_delete)	 - delete vault root-token
* [vault root-token generate](/docs/v2025.5.30/reference/cli/vault_root-token_generate)	 - generate vault root-token
* [vault root-token get](/docs/v2025.5.30/reference/cli/vault_root-token_get)	 - get vault root-token
* [vault root-token rotate](/docs/v2025.5.30/reference/cli/vault_root-token_rotate)	 - rotate vault root-token
* [vault root-token set](/docs/v2025.5.30/reference/cli/vault_root-token_set)	 - set vault root-token
* [vault root-token sync](/docs/v2025.5.30/reference/cli/vault_root-token_sync)	 - sync vault root-token

