---
title: Vault Unseal-Key
menu:
  docs_v2022.02.22:
    identifier: vault-unseal-key
    name: Vault Unseal-Key
    parent: reference-cli
menu_name: docs_v2022.02.22
section_menu_id: reference
info:
  cli: v0.7.0
  installer: v2022.02.22
  operator: v0.7.0
  unsealer: v0.7.0
  version: v2022.02.22
---

## vault unseal-key

get, set, delete, list and sync unseal-key

### Synopsis


$ kubectl vault unseal-key [command] [flags] to get, set, delete, list or sync vault unseal-keys

Examples:
 $ kubectl vault unseal-key get [flags]
 $ kubectl vault unseal-key set [flags]
 $ kubectl vault unseal-key delete [flags]
 $ kubectl vault unseal-key list [flags]
 $ kubectl vault unseal-key sync [flags]


```
vault unseal-key [flags]
```

### Options

```
  -h, --help   help for unseal-key
```

### Options inherited from parent commands

```
      --alsologtostderr                  log to standard error as well as files
      --analytics                        Send analytical events to Google Analytics (default true)
      --as string                        Username to impersonate for the operation
      --as-group stringArray             Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --cache-dir string                 Default cache directory (default "/home/runner/.kube/cache")
      --certificate-authority string     Path to a cert file for the certificate authority
      --client-certificate string        Path to a client certificate file for TLS
      --client-key string                Path to a client key file for TLS
      --cluster string                   The name of the kubeconfig cluster to use
      --context string                   The name of the kubeconfig context to use
      --insecure-skip-tls-verify         If true, the server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kubeconfig string                Path to the kubeconfig file to use for CLI requests.
      --log-backtrace-at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log-dir string                   If non-empty, write log files in this directory
      --log-flush-frequency duration     Maximum number of seconds between log flushes (default 5s)
      --logtostderr                      log to standard error instead of files
      --match-server-version             Require server version to match client version
  -n, --namespace string                 If present, the namespace scope for this CLI request
      --request-timeout string           The length of time to wait before giving up on a single server request. Non-zero values should contain a corresponding time unit (e.g. 1s, 2m, 3h). A value of zero means don't timeout requests. (default "0")
  -s, --server string                    The address and port of the Kubernetes API server
      --stderrthreshold severity         logs at or above this threshold go to stderr (default 0)
      --tls-server-name string           Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                     Bearer token for authentication to the API server
      --user string                      The name of the kubeconfig user to use
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging
```

### SEE ALSO

* [vault](/docs/v2022.02.22/reference/cli/vault)	 - KubeVault cli by AppsCode
* [vault unseal-key delete](/docs/v2022.02.22/reference/cli/vault_unseal-key_delete)	 - delete vault unseal-key
* [vault unseal-key get](/docs/v2022.02.22/reference/cli/vault_unseal-key_get)	 - get vault unseal-key
* [vault unseal-key list](/docs/v2022.02.22/reference/cli/vault_unseal-key_list)	 - list vault unseal-key
* [vault unseal-key set](/docs/v2022.02.22/reference/cli/vault_unseal-key_set)	 - set vault unseal-key
* [vault unseal-key sync](/docs/v2022.02.22/reference/cli/vault_unseal-key_sync)	 - sync vault unseal-key

