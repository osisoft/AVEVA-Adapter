---
uid: ReferenceSecrets
---

# Reference Secrets

Secrets may be referenced by their Id in any configuration that has a protected property, such as client secret or password.

## Reference Secrets by Id

To use a secret in another configuration the value of the protected property may be replaced with "{{secret-Id}}", where secret-id 
is an existing id in the secret management configuration.

### Example

If a secret Id and value already exist in the secret management configuration, for example:

```code
{
    ""Id": "my-secret-id",
    "Value": "<secretValue>""
}
```
The secret Id can be referenced in any configuration with a protected property, such as client secret or password.

For example, in a health configuration with a PWA and OCS endpoint:

```code
[
    {
        "Id": "OCS",
        "Endpoint": "https://<OCS OMF endpoint>",
        "ClientId": "<clientid>",
        "ClientSecret": "{{my-secret-id}}"
    },
    {
        "Id": "PI Web API",
        "Endpoint": "https://<pi web api server>:<port>/piwebapi/omf/",
        "UserName": "<username>",
        "Password": "{{my-secret-id}}"
    }
]
```

## Adding Secrets by Value

The default behavior of any configuration with a protected property is to add the value into the secrets management configuration, automatically generate a secret id, 
and replace the value of the protected property on the configuration to be the generated secret id.

### Example

Adding in a health configuration as follows, with a plain client secret

```code
[
    {
        "Id": "OCS",
        "Endpoint": "https://<OCS OMF endpoint>",
        "ClientId": "<clientid>",
        "ClientSecret": "<clientsecret>"
    }
]
```

updates the secrets management configuration to add the following id and value:

```code
{
    "Id": "System.HealthEndpoints.OCS.ClientSecret",
    "Value": "<clientsecret>"
}
```
and replaces the plain text password in the health configuration to now reference the client secret value by secret Id:

```code
[
    {
        "Id": "OCS",
        "Endpoint": "https://<OCS OMF endpoint>",
        "ClientId": "<clientid>",
        "ClientSecret": "{{System.HealthEndpoints.OCS.ClientSecret}}"
    }
]
```
