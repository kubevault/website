---
title: Manage MySQL/MariaDB credentials using the KubeVault operator
menu:
  docs_v2021.08.02:
    identifier: overview-mysql
    name: Overview
    parent: mysql-secret-engines
    weight: 10
menu_name: docs_v2021.08.02
section_menu_id: guides
info:
  cli: v0.4.0
  installer: v2021.08.02
  operator: v0.4.0
  unsealer: v0.4.0
  version: v2021.08.02
---

> New to KubeVault? Please start [here](/docs/v2021.08.02/concepts/README).

# Manage MySQL/MariaDB credentials using the KubeVault operator

MySQL is one of the supported plugins for the database secrets engine. This plugin generates database credentials dynamically based on configured roles for the MySQL database, and also supports Static Roles. You can easily manage [MySQL Database secret engine](https://www.vaultproject.io/docs/secrets/databases/mysql-maria.html) using the KubeVault operator.

![MySQL secret engine](/docs/v2021.08.02/images/guides/secret-engines/mysql/mysql_secret_engine_guide.svg)

You need to be familiar with the following CRDs:

- [AppBinding](/docs/v2021.08.02/concepts/vault-server-crds/auth-methods/appbinding)
- [SecretEngine](/docs/v2021.08.02/concepts/secret-engine-crds/secretengine)
- [MySQLRole](/docs/v2021.08.02/concepts/secret-engine-crds/database-secret-engine/mysql)
- [DatabaseAccessRequest](/docs/v2021.08.02/concepts/secret-engine-crds/database-secret-engine/databaseaccessrequest)

## Before you begin

- Install KubeVault operator in your cluster from [here](/docs/v2021.08.02/setup/README).

To keep things isolated, we are going to use a separate namespace called `demo` throughout this tutorial.

```console
$ kubectl create ns demo
namespace/demo created
```

In this tutorial, we are going to create a [role](https://www.vaultproject.io/docs/secrets/databases/mysql-maria#setup) using MySQLRole and issue credential using DatabaseAccessRequest.

## Vault Server

If you don't have a Vault Server, you can deploy it by using the KubeVault operator.

- [Deploy Vault Server](/docs/v2021.08.02/guides/vault-server/vault-server)

The KubeVault operator can manage policies and secret engines of Vault servers which are not provisioned by the KubeVault operator. You need to configure both the Vault server and the cluster so that the KubeVault operator can communicate with your Vault server.

- [Configure cluster and Vault server](/docs/v2021.08.02/guides/vault-server/external-vault-sever#configuration)

Now, we have the [AppBinding](/docs/v2021.08.02/concepts/vault-server-crds/auth-methods/appbinding) that contains connection and authentication information about the Vault server.

```console
$ kubectl get appbinding -n demo
NAME    AGE
vault   50m

$ kubectl get appbinding -n demo vault -o yaml
apiVersion: appcatalog.appscode.com/v1alpha1
kind: AppBinding
metadata:
  name: vault
  namespace: demo
spec:
  clientConfig:
    caBundle: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN1RENDQWFDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFOTVFzd0NRWURWUVFERXdKallUQWUKRncweE9URXhNVEl3T1RFMU5EQmFGdzB5T1RFeE1Ea3dPVEUxTkRCYU1BMHhDekFKQmdOVkJBTVRBbU5oTUlJQgpJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBTUlJQkNnS0NBUUVBdFZFZmtic2c2T085dnM2d1Z6bTlPQ1FYClBtYzBYTjlCWjNMbXZRTG0zdzZGaWF2aUlSS3VDVk1hN1NRSGo2L2YvOHZPeWhqNEpMcHhCM0hCYVFPZ3RrM2QKeEFDbHppU1lEd3dDbGEwSThxdklGVENLWndreXQzdHVQb0xybkppRFdTS2xJait6aFZDTHZ0enB4MDE3SEZadApmZEdhUUtlSXREUVdyNUV1QWlCMjhhSVF4WXREaVN6Y0h3OUdEMnkrblRMUEd4UXlxUlhua0d1UlIvR1B3R3lLClJ5cTQ5NmpFTmFjOE8wVERYRkIydWJQSFNza2xOU1VwSUN3S1IvR3BobnhGak1rWm4yRGJFZW9GWDE5UnhzUmcKSW94TFBhWDkrRVZxZU5jMlczN2MwQlhBSGwyMHVJUWQrVytIWDhnOVBVVXRVZW9uYnlHMDMvampvNERJRHdJRApBUUFCb3lNd0lUQU9CZ05WSFE4QkFmOEVCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBTkJna3Foa2lHCjl3MEJBUXNGQUFPQ0FRRUFabHRFN0M3a3ZCeTNzeldHY0J0SkpBTHZXY3ZFeUdxYUdCYmFUbGlVbWJHTW9QWXoKbnVqMUVrY1I1Qlg2YnkxZk15M0ZtZkJXL2E0NU9HcDU3U0RMWTVuc2w0S1RlUDdGZkFYZFBNZGxrV0lQZGpnNAptOVlyOUxnTThkOGVrWUJmN0paUkNzcEorYkpDU1A2a2p1V3l6MUtlYzBOdCtIU0psaTF3dXIrMWVyMUprRUdWClBQMzFoeTQ2RTJKeFlvbnRQc0d5akxlQ1NhTlk0UWdWK3ZneWJmSlFEMVYxbDZ4UlVlMzk2YkJ3aS94VGkzN0oKNWxTVklmb1kxcUlBaGJPbjBUWHp2YzBRRXBKUExaRDM2VDBZcEtJSVhjZUVGYXNxZzVWb1pINGx1Uk50SStBUAp0blg4S1JZU0xGOWlCNEJXd0N0aGFhZzZFZVFqYWpQNWlxZnZoUT09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    service:
      name: vault
      port: 8200
      scheme: HTTPS
  parameters:
    apiVersion: config.kubevault.com/v1alpha1
    kind: VaultServerConfiguration
    path: kubernetes
    vaultRole: vault-policy-controller
    kubernetes:
      serviceAccountName: vault
      tokenReviewerServiceAccountName: vault-k8s-token-reviewer
      usePodServiceAccountForCSIDriver: true
```

## Enable and Configure MySQL Secret Engine

When a [SecretEngine](/docs/v2021.08.02/concepts/secret-engine-crds/secretengine) crd object is created, the KubeVault operator will enable a secret engine on specified path and configure the secret engine with given configurations.

A sample SecretEngine object for the MySQL  secret engine:

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: SecretEngine
metadata:
  name: mysql-engine
  namespace: demo
spec:
  vaultRef:
    name: vault
  path: mysql-se
  mysql:
    databaseRef:
      name: mysql-app
      namespace: demo
    pluginName: "mysql-rds-database-plugin"
    allowedRoles:
      - "*"
```

To configure the MySQL secret engine, you need to provide the MySQL database connection and authentication information through an [AppBinding](/docs/v2021.08.02/concepts/vault-server-crds/auth-methods/appbinding).

```console
$ kubectl get services -n demo
NAME    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)       AGE
mysql   ClusterIP   10.96.33.240    <none>        3306/TCP      3h41
```

Let's consider `mysql` is the Kubernetes service name that communicate with MySQL servers. The connection `URL` generated using the service will be `mysql.demo.svc:3306`. Visit [AppBinding documentation](/docs/v2021.08.02/concepts/vault-server-crds/auth-methods/appbinding) for more details. A sample AppBinding example with necessary k8s secret is given below:

```yaml
apiVersion: appcatalog.appscode.com/v1alpha1
kind: AppBinding
metadata:
  name: mysql-app
  namespace: demo
spec:
  secret:
    name: mysql-cred # secret name
  clientConfig:
    url: tcp(mysql.demo.svc:3306)/
    insecureSkipTLSVerify: true
---
apiVersion: v1
data:
  username: cm9vdA== # mysql username
  password: cm9vdA== # mysql password
kind: Secret
metadata:
  name: mysql-cred
  namespace: demo
```

Let's deploy SecretEngine:

```console
$ kubectl apply -f docs/examples/guides/secret-engines/mysql/mysql-app.yaml 
appbinding.appcatalog.appscode.com/mysql-app created
secret/mysql-cred created

$ kubectl apply -f docs/examples/guides/secret-engines/mysql/mysqlSecretEngine.yaml
secretengine.engine.kubevault.com/mysql-engine created
```

Wait till the status become `Success`:

```console
$ kubectl get secretengines -n demo
NAME           STATUS
mysql-engine   Success
```

Since the status is `Success`, the MySQL secret engine is enabled and successfully configured. You can use `kubectl describe secretengine -n <namepsace> <name>` to check for error events, if any.

## Create MySQL Role

By using [MySQLRole](/docs/v2021.08.02/concepts/secret-engine-crds/database-secret-engine/mysql), you can create a [role](https://www.vaultproject.io/docs/secrets/databases/mysql-maria#setup) on the Vault server in Kubernetes native way.

A sample MySQLRole object is given below:

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: MySQLRole
metadata:
  name: mysql-role
  namespace: demo
spec:
  vaultRef:
    name: vault
  path: mysql-se
  databaseRef:
    name: mysql-app
    namespace: demo
  creationStatements:
    - "CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}';"
    - "GRANT SELECT ON *.* TO '{{name}}'@'%';"
  defaultTTL: 1h
  maxTTL: 24h
```

Let's deploy MySQLRole:

```console
$ kubectl apply -f docs/examples/guides/secret-engines/mysql/mysqlRole.yaml
mysqlrole.engine.kubevault.com/mysql-role created

$ kubectl get mysqlrole -n demo mysql-role 
NAME         AGE
mysql-role   83m
```

You can also check from Vault that the role is created.
To resolve the naming conflict, name of the role in Vault will follow this format: `k8s.{clusterName}.{metadata.namespace}.{metadata.name}`.

> Don't have Vault CLI? Download and configure it as described [here](/docs/v2021.08.02/guides/vault-server/vault-server#enable-vault-cli)

```console
$ vault list mysql-se/roles
Keys
----
k8s.-.demo.mysql-role

$ vault read mysql-se/roles/k8s.-.demo.mysql-role
Key                      Value
---                      -----
creation_statements      [CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}'; GRANT SELECT ON *.* TO '{{name}}'@'%';]
db_name                  k8s.-.demo.mysql-app
default_ttl              1h
max_ttl                  24h
renew_statements         []
revocation_statements    []
rollback_statements      []
```

If we delete the MySQLRole, then the respective role will be deleted from the Vault.

```console
$ kubectl delete mysqlrole -n demo mysql-role
mysqlrole.engine.kubevault.com "mysql-role" deleted
```

Check from Vault whether the role exists:

```console
$ vault read mysql-se/roles/k8s.-.demo.mysql-role
No value found at mysql-se/roles/k8s.-.demo.mysql-role

$ vault list mysql-se/roles
No value found at mysql-se/roles/
```

## Generate MySQL credentials

By using [DatabaseAccessRequest](/docs/v2021.08.02/concepts/secret-engine-crds/database-secret-engine/databaseaccessrequest), you can generate database access credentials from Vault.

Here, we are going to make a request to Vault for MySQL database credentials by creating `mysql-cred-rqst` DatabaseAccessRequest in `demo` namespace.

```yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: DatabaseAccessRequest
metadata:
  name: mysql-cred-rqst
  namespace: demo
spec:
  roleRef:
    kind: MySQLRole
    name: mysql-role
    namespace: demo
  subjects:
    - kind: ServiceAccount
      name: sa
      namespace: demo
```

Here, `spec.roleRef` is the reference of MySQLRole against which credentials will be issued. `spec.subjects` is the reference to the object or user identities a role binding applies to and it will have read access of the credential secrets.

Now, we are going to create DatabaseAccessRequest.

```console
$ kubectl apply -f docs/examples/guides/secret-engines/mysql/mysqlAccessRequest.yaml
databaseaccessrequest.engine.kubevault.com/mysql-cred-rqst created

$  kubectl get databaseaccessrequest -n demo mysql-cred-rqst 
NAME              AGE
mysql-cred-rqst   48s
```

Database credentials will not be issued until it is approved. The KubeVault operator will watch for the approval in the `status.conditions[].type` field of the request object. You can use [KubeVault CLI](https://github.com/kubevault/cli), a [kubectl plugin](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/), to approve or deny DatabaseAccessRequest.

```console
# using KubeVault CLI as kubectl plugin to approve request
$ kubectl vault approve databaseaccessrequest mysql-cred-rqst -n demo
approved

$ kubectl get databaseaccessrequest -n demo mysql-cred-rqst -o yaml
apiVersion: engine.kubevault.com/v1alpha1
kind: DatabaseAccessRequest
metadata:
  name: mysql-cred-rqst
  namespace: demo
spec:
  roleRef:
    kind: MySQLRole
    name: mysql-role
    namespace: demo
  subjects:
  - kind: ServiceAccount
    name: sa
    namespace: demo
status:
  conditions:
  - lastUpdateTime: "2019-11-19T09:22:21Z"
    message: This was approved by kubectl vault approve databaseaccessrequest
    reason: KubectlApprove
    type: Approved
  lease:
    duration: 1h0m0s
    id: mysql-se/creds/k8s.-.demo.mysql-role/egCyKUcRZH8rVW09kUbtX3Z3
    renewable: true
  secret:
    name: mysql-cred-rqst-nx7gyv
```

Once DatabaseAccessRequest is approved, the KubeVault operator will issue credentials from Vault and create a secret containing the credential. It will also create a role and rolebinding so that `spec.subjects` can access secret. You can view the information in the `status` field.

```console
$ kubectl get databaseaccessrequest mysql-cred-rqst -n demo -o json | jq '.status'
{
  "conditions": [
    {
      "lastUpdateTime": "2019-11-19T09:22:21Z",
      "message": "This was approved by kubectl vault approve databaseaccessrequest",
      "reason": "KubectlApprove",
      "type": "Approved"
    }
  ],
  "lease": {
    "duration": "1h0m0s",
    "id": "mysql-se/creds/k8s.-.demo.mysql-role/egCyKUcRZH8rVW09kUbtX3Z3",
    "renewable": true
  },
  "secret": {
    "name": "mysql-cred-rqst-nx7gyv"
  }
}

$ kubectl get secret -n demo mysql-cred-rqst-nx7gyv -o yaml
apiVersion: v1
data:
  password: QTFhLWYwdk9YeWE=
  username: di1rOHMuLVdjNDJSRGJuNA==
kind: Secret
metadata:
  name: mysql-cred-rqst-nx7gyv
  namespace: demo
  ownerReferences:
  - apiVersion: engine.kubevault.com/v1alpha1
    controller: true
    kind: DatabaseAccessRequest
    name: mysql-cred-rqst
    uid: bc479f76-5a3d-4cbf-9e21-60dc6cb3285b
type: Opaque
```

If DatabaseAccessRequest is deleted, then credential lease (if any) will be revoked.

```console
$ kubectl delete databaseaccessrequest -n demo mysql-cred-rqst 
databaseaccessrequest.engine.kubevault.com "mysql-cred-rqst" deleted
```

If DatabaseAccessRequest is `Denied`, then the KubeVault operator will not issue any credential.

```console
$ kubectl vault deny databaseaccessrequest mysql-cred-rqst -n demo
  Denied
```

> Note: Once DatabaseAccessRequest is `Approved` or `Denied`, you cannot change `spec.roleRef` and `spec.subjects` field.
