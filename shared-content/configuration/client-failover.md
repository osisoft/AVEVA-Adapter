---
uid: ClientFailoverConfiguration
---

# Client failover configuration

You can configure the adapter for failover by using client failover to register the adapter to a failover group managed by a failover endpoint. When you register the adapter to a failover group it allows the adapter to work with other adapter instances in the group as mutual backups. This minimizes the likelihood of data loss if an adapter in the group goes offline due to incidences like scheduled maintenance or an unexpected power outage.

Using client failover, you can do the following:

- Register the adapter instance to a failover group managed by a failover endpoint.
- Unregister the adapter instance from a failover group managed by a failover endpoint.
- Perform runtime failover parameter changes such as the failover mode and failover timeout.
- Query the current failover state including the failover role, last data process time, failover status and adapter state.

## Configure client failover

Complete the following steps to configure client failover.

1. Using a text editor, create a file that contains the client failover configuration in the JSON format.
    - For sample JSON, see the [example client failover configuration](#example-client-failover-configuration).
    - For all available parameters, see the [client failover parameters](#client-failover-parameters).

2. Save the file. For example, `ConfigureClientFailover.json`.

3. Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to run a PUT command with the contents of the file to the following endpoint: `http://localhost:5590/api/v1/configuration/System/ClientFailover`.

    **Note:** `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    **Note:** Run this command from the same directory where the file is located.

    ```bash
    curl -d "@ConfigureClientFailover.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/System/ClientFailover"
    ```

On successful execution, the client failover change takes effect immediately during runtime.

## Client failover parameters

| Parameter | Required | Type | Description
---------|---------|----------|---------
| **Id** | Optional | `string` | The Id of the client failover configuration <br><br> **Notes:**<br>&bull; You cannot configure multiple client failover configurations. In order to create a new failover configuration, you need to delete the existing one.<br>&bull; Including an `id` is optional. If you do not include one, the adapter automatically generates one. <br><br>Allowed value: any string identifier<br>Default value: new GUID |
| **FailoverGroupId** | Required | `string` | The ID of the failover group to register the adapter instance <br><br>Allowed value: any string identifier<br>Default value: `null` |
| **Name** | Optional | `string` | The friendly name of the client failover configuration <br><br>Allowed value: any string value<br>Default value: `null` |
| **Description** | Optional | `string`| The description of the client failover configuration <br><br>Allowed value: any string value<br>Default value: `null` |
| **FailoverTimeout** | Optional | `datetime` | The max time for the adapter instance to wait for responses from the failover endpoint before timing out. <br><br>Allowed value: a string representation of date time using `hh:mm:ss` <br>Default value: `00:01:00` |
| **Mode** | Required | `string` | The failover mode of the registered adapter. <br><br>Allowed value: `Hot`, `Warn`, `Cold`, `None` <br>Default value: `None` |
| **Endpoint** | Required | `string` | Destination that support client failover registration. Supported destinations include OCS and PI Server.<br><br>Allowed value: well-formed http or https endpoint string<br>Default: `null` |
| **UserName** | Required for PI server endpoint | `string` | Basic authentication to the PI Web API OMF endpoint <br><br>Allowed value: any string<br>Default: `null`<br><br>**Note:** If your username contains a backslash, you must add an escape character, for example, type `OilCompany\TestUser` as `OilCompany\\TestUser`. |
| **Password** | Required for PI server endpoint | `string` | Basic authentication to the PI Web API OMF endpoint <br><br>Allowed value: any string<br>Default: `null` |
| **ClientId** | Required for OCS endpoint | `string` | Authentication with the OCS OMF endpoint <br><br>Allowed value: any string, can be null if the endpoint URL schema is `HTTP`<br>Default: `null` |
| **ClientSecret** | Required for OCS endpoint | `string`| Authentication with the OCS OMF endpoint <br><br>Allowed value: any string, can be null if the endpoint URL schema is `HTTP`<br>Default: `null` |
| **TokenEndpoint** | Optional for OCS endpoint | `string`| Retrieves an OCS token from an alternative endpoint <br><br>Allowed value: well-formed http or https endpoint string <br>Default value: `null` |
| **ValidateEndpointCertificate** | Optional | `bool`| Disables verification of destination certificate.<br><br>**Note:** Only use for testing with self-signed certificates. <br><br>Allowed value: `true` or `false`<br>Default value: `true` |

## Example client failover configuration

The following is an example of a complete client failover configuration.

```json
[
  {
    "Id": "FailoverConfiguration1",
    "FailoverGroupId": "FailoverGroup1",
    "Name": "NameExample",
    "Description": "DescriptionExample",
    "FailoverTimeout": "00:01:00",
    "Mode": "hot",
    "Endpoint": "http://test-endpoint.com",
    "UserName": "UserName1",
    "Password": "Password1",
    "TokenEndpoint": "http://token.com",
    "ValidateEndpointCertificate": false
  }
]
```

**Note:** You can only register the adapter to a single failover endpoint by providing one client failover configuration. Additional configuration entries will be rejected.

## Query current failover state

You can query the current failover state with the adapter's diagnostics. Follow the instruction below to query the current failover state:
 
Use any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests to run a GET command to the following endpoint: `http://localhost:5590/api/v1/diagnostics/FailoverState`.

The following is an example of failover state returned from the adapter:

```json
{
    "Role": "Primary",
    "LastDataProcessedTime": "2021-01-01T00:00:00",
    "FailoverStatus": "95",
    "AdapterState": "Running"
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/System/ClientFailover      | GET       | Gets all client failover configurations <br><br> **Note:**<br>&bull; There is always one entry in the list of returned failover configurations. |
| api/v1/configuration/System/ClientFailover      | DELETE    | Deletes the configured client failover
| api/v1/configuration/System/ClientFailover      | POST      | Adds a single client failover configuration. Fails if any client failover configuration already exists |
| api/v1/configuration/System/ClientFailover      | PUT       | Replaces the existing client failover configuration |
| api/v1/configuration/System/ClientFailover/*id* | GET       | Gets configured client failover by *id* |
| api/v1/configuration/System/ClientFailover/*id* | DELETE    | Deletes configured client failover by *id* |
| api/v1/configuration/System/ClientFailover/*id* | PUT       | Replaces client failover by *id*. Fails if the failover configuration does not exist |
| api/v1/configuration/System/ClientFailover/*id* | PATCH     | Allows partial updating of configured client failover by *id* |
| api/v1/diagnostics/FailoverState | GET     | Get the current failover state |
