---
title: Dynamodb | Vault Server Storage
menu:
  docs_v2025.5.26:
    identifier: dynamodb-storage
    name: Dynamodb
    parent: storage-vault-server-crds
    weight: 15
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

# DynamoDB

In DynamoDB storage backend, Vault data will be stored in [DynamoDB](https://aws.amazon.com/dynamodb/). Vault documentation for DynamoDB storage can be found in [here](https://www.vaultproject.io/docs/configuration/storage/dynamodb.html).

```yaml
apiVersion: kubevault.com/v1alpha1
kind: VaultServer
metadata:
  name: vault-with-dynamodb
  namespace: demo
spec:
  replicas: 1
  version: "1.2.0"
  backend:
    dynamodb:
      table: "my-vault-table"
      region: "us-west-1"
      readCapacity: 5
      writeCapacity: 5
```

## spec.backend.dynamodb

To use dynamoDB as backend storage in Vault specify `spec.backend.dynamodb` in [VaultServer](/docs/v2025.5.26/concepts/vault-server-crds/vaultserver) CRD.

```yaml
spec:
  backend:
    dynamodb:
      table: <table_name>
      region: <region>
      endpoint: <endpoint>
      haEnabled: <true/false>
      readCapacity: <read_capacity>
      writeCapacity: <write_capacity>
      credentialSecret: <secret_name>
      sessionTokenSecret: <secret_name>
      maxParallel: <max_parallel>
```

Here, we are going to describe the various attributes of the `spec.backend.dynamodb` field.

### dynamodb.table

`dynamodb.table` is a required field that specifies the name of the DynamoDB table. If the specified table does not exist, then Vault will create it during initialization. If it is not initialized, then Vault will set value to `vault-dynamodb-backend`.

```yaml
spec:
  backend:
    dynamodb:
      table: "my-vault-table"
```

### dynamodb.endpoint

`dynamodb.endpoint` is an optional field that specifies an alternative, AWS compatible, DynamoDB endpoint.

```yaml
spec:
  backend:
    dynamodb:
      endpoint: "endpoint.com"
```

### dynamodb.region

`dynamodb.region` is an optional field that specifies the AWS region. If this field is not specified, then Vault will set value to `us-east-1`.

```yaml
spec:
  backend:
    dynamodb:
      region: "us-east-1"
```

### dynamodb.credentialSecret

`dynamodb.credentialSecret` is an optional field that specifies the secret name containing AWS access key and AWS secret key. The secret contains the following keys:
  
- `access_key`
- `secret_key`

Leaving the `access_key` and `secret_key` fields empty will cause Vault to attempt to retrieve credentials from the AWS metadata service.

```yaml
spec:
  backend:
    dynamodb:
      credentialSecret: "aws-credential"
```

### dynamodb.sessionTokenSecret

`dynamodb.sessionTokenSecret` is an optional field that specifies the secret name containing the AWS session token. The secret contains the following key:
  
- `session_token`

```yaml
spec:
  backend:
    dynamodb:
      sessionTokenSecret: "aws-session-token"
```

### dynamodb.maxParallel

`maxParallel` is an optional field that specifies the maximum number of parallel operations to take place. This field accepts integer value. If this field is not specified, then Vault will set value to `128`.

```yaml
spec:
  backend:
    dynamodb:
      maxParallel: 124
```

### dynamodb.readCapacity

`dynamodb.readCapacity` is an optional field that specifies the maximum number of reads consumed per second on the table. If it is not specified, then Vault will set value to `5`.

```yaml
spec:
  backend:
    dynamodb:
      readCapacity: 10
```

### dynamodb.writeCapacity

`dynamodb.writeCapacity` is an optional field that specifies the maximum number of writes performed per second on the table. If it is not specified, then Vault will set value to `5`.

```yaml
spec:
  backend:
    dynamodb:
      writeCapacity: 10
```

### dynamodb.haEnabled

`dynamodb.haEnabled` is an optional field that specifies whether this backend should be used to run Vault in high availability mode. This field accepts boolean value. The default value is `false`.

```yaml
spec:
  backend:
    dynamodb:
      haEnabled: true
```
