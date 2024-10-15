---
title: "Retrieve Secrets from Secrets Manager"
linkTitle: "Retrieve Secrets from Secrets Manager"
weight: "12"
aliases:
  - /en/docs/installation/serverless-agents/aws-fargate-serverless-agents/fetching-secrets-from-secretsmanager/
  - /en/ecs-fetching-secrets
Description: "You can retrieve the Sysdig Access Key and the HTTP Proxy password from the AWS Secrets Manager when deploying the Orchestrator Agent."
---

It is supported as of [serverless agent version 4.0.0](/en/docs/release-notes/serverless-agent-release-notes/#400-february-10-2023).

## Reference AWS Secrets
A secret reference has the following pattern, as described in the [AWS documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/secrets-envvar-secrets-manager.html#secrets-envvar-secrets-manager-create-secret):

```
arn:aws:secretsmanager:region:aws_account_id:secret:secret-name:json-key:version-stage:version-id
```

A valid secret reference has two parts:
 - `arn:aws:secretsmanager:region:aws_account_id:secret:secret-name` which is the mandatory Secret ARN that identifies the secret.
 - `:json-key:version-stage:version-id` which are the optional parameters specifying the JSON key, the version stage, and the version ID of the secret. 
   Note that you must include all the colons `:` when using one of these parameters. If neither the stage version nor the version ID are specified, the default behavior is to retrieve the secret with the `AWSCURRENT` staging label.

### Reference a Plaintext Secret
For example, the following string references the plaintext secret `plaintext-secret` depicted in the image below.

```
arn:aws:secretsmanager:us-east-1:############:secret:plaintext-secret-1LuuAy
```

Note that plaintext secrets can be referenced by using only the Secret ARN.

![](/image/serverless-secrets-manager-plaintext-secret.png)

### Reference a Key inside a JSON Secret
As another example, the following string references the JSON key `SysdigAccessKey` in the JSON secret `json-secret` depicted in the image below.

```
arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::
```

In this case, the secret reference contains:
 - `arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB` which is the Secret ARN;
 - `:SysdigAccessKey::` which identifies the field `SysdigAccessKey` in the JSON secret.

![](/image/serverless-secrets-manager-json-secret.png)

### Referencing a version-stage of a secret
Moreover, you can refer a certain version-stage of a secret. Note that if a version-stage is specified, you cannot specify a version-id.

For example, the following string references the version stage `AWSPREVIOUS` of the secret `json-secret` depicted in the image below.:

```
arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey:AWSPREVIOUS:
```

In this case, the secret reference contains:
 - `arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB` which is the Secret ARN;
 - `:SysdigAccessKey:AWSPREVIOUS:` which identifies the field `SysdigAccessKey` in the JSON secret and the version stage `AWSPREVIOUS`.

![](/image/serverless-secrets-versions.png)

### Reference a Version ID of a Secret
You can also refer either a certain version-id of a secret. Note that if a version-id is specified, you cannot specify a version-stage label.

For example, the following string references the version id `65aaeb##-####-####-####-############` of the secret `json-secret` depicted in the image below.

```
arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::65aaeb##-####-####-####-############
```

In this case, the secret reference contains:
 - `arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB` which is the Secret ARN;
 - `:SysdigAccessKey::65aaeb##-####-####-####-############` which identifies the field `SysdigAccessKey` in the JSON secret and the version id `65aaeb##-####-####-####-############`.

![](/image/serverless-secrets-versions.png)


## CloudFormation

When using the CloudFormation template [orchestrator-agent.yaml >= 4.0.0](https://download.sysdig.com/dependencies/serverless/fargate/orchestrator-agent.yaml) to deploy the orchestrator, you can fetch the Sysdig Access Key from AWS SecretsManager.
To do so, you need to provide the template parameter `Sysdig Access Key` with the reference to the secret containing the access key.

The same applies to the HTTP Proxy password which can be configured through the field `Configuration.HttpProxy.ProxyPassword` under `Mappings`.

```
Mappings:
  ...
  Configuration:
    HttpProxy:
      ...
      ProxyPassword: ""
```

### Examples

##### Fetching the Sysdig Access Key from a plaintext secret
The example below shows how to fetch the Sysdig Access Key stored in the plaintext secret `plaintext-secret`.

![](/image/serverless-cloudformation-access-key-secret-plaintext.png)

##### Fetching the Sysdig Access Key from a JSON secret
Instead, the example below shows how to fetch the Sysdig Access Key stored in the JSON secret `json-secret`.
Since the JSON key `SysdigAccessKey` has been specified, the trailing colons `:` are required.

![](/image/serverless-cloudformation-access-key-secret-json.png)


## Terraform

When using the Terraform module [fargate-orchestrator-agent >= 0.3.0](https://registry.terraform.io/modules/sysdiglabs/fargate-orchestrator-agent/aws/latest) to deploy the orchestrator, you can fetch the Sysdig Access Key from the AWS SecretsManager.
To do so, you need to provide the template parameter `access_key` with the reference to the secret containing the access key.

The same applies to HTTP Proxy passwords, see `http_proxy_configuration.proxy_password` in the Terraform [fargate-orchestrator-agent](https://registry.terraform.io/modules/sysdiglabs/fargate-orchestrator-agent/aws/latest) module. 

```
module "fargate-orchestrator-agent" {
  source  = "sysdiglabs/fargate-orchestrator-agent/aws"
  version = "0.3.1"

  ...

  http_proxy_configuration = {
    ...
    proxy_password = ""
  }
}
```

### Examples

##### Fetching the Sysdig Access Key from a plaintext secret
The example below shows how to fetch the Sysdig Access Key stored in the plaintext secret `plaintext-secret`.

```
module "fargate-orchestrator-agent" {
  source         = "sysdiglabs/fargate-orchestrator-agent/aws"
  version        = "0.3.1"

  vpc_id         = "my_vpc_id"
  subnets        = ["my_subnet_a", "my_subnet_b"]

  ...

  access_key     = "arn:aws:secretsmanager:us-east-1:############:secret:plaintext-secret-1LuuAy"
}
```

##### Fetching the Sysdig Access Key from a JSON secret
Instead, the example below shows how to fetch the Sysdig Access Key stored in the JSON secret `json-secret`.
Since the JSON key `SysdigAccessKey` has been specified, the trailing colons `:` are required.

```
module "fargate-orchestrator-agent" {
  source         = "sysdiglabs/fargate-orchestrator-agent/aws"
  version        = "0.3.1"

  vpc_id         = "my_vpc_id"
  subnets        = ["my_subnet_a", "my_subnet_b"]

  ...

  access_key     = "arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::"
}
```


## Known Limitations

### Regional service
The secret must be in the same region in which the stack is deployed.

### Secret update/rotation
Automatic secret rotation/update is not supported because sensitive data are injected into the Orchestrator Agent when the container starts. 
If the secret is subsequently updated (or rotated) after deploying the Orchestrator Agent, the container does not receive the updated value automatically.

### Custom KMS encryption keys
Currently, both the CloudFormation and Terraform installers support only the AWS-managed KMS encryption keys.
Custom KMS encryption keys are not supported.
