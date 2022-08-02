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
| **Mode** | Required | `string` | The failover mode of the registered adapter. <br><br>Allowed value: `Hot`, `Warm`, `Cold`, `None` <br>Default value: `None` <br>For more information see [Failover Modes](#failover-modes). |
| **Endpoint** | Required | `string` | Destination that support client failover registration. Supported destinations include OCS and PI Server.<br><br>Allowed value: well-formed http or https endpoint string<br>Default: `null` |
| **UserName** | Required for PI server endpoint | `string` | Basic authentication to the PI Web API OMF endpoint <br><br>Allowed value: any string<br>Default: `null`<br><br>**Note:** If your username contains a backslash, you must add an escape character, for example, type `OilCompany\TestUser` as `OilCompany\\TestUser`. |
| **Password** | Required for PI server endpoint | `string` | Basic authentication to the PI Web API OMF endpoint <br><br>Allowed value: any string<br>Default: `null` |
| **ClientId** | Required for OCS endpoint | `string` | Authentication with the OCS OMF endpoint <br><br>Allowed value: any string, can be null if the endpoint URL schema is `HTTP`<br>Default: `null` |
| **ClientSecret** | Required for OCS endpoint | `string`| Authentication with the OCS OMF endpoint <br><br>Allowed value: any string, can be null if the endpoint URL schema is `HTTP`<br>Default: `null` |
| **TokenEndpoint** | Optional for OCS endpoint | `string`| Retrieves an OCS token from an alternative endpoint <br><br>Allowed value: well-formed http or https endpoint string <br>Default value: `null` |
| **ValidateEndpointCertificate** | Optional | `bool`| Disables verification of destination certificate.<br><br>**Note:** Only use for testing with self-signed certificates. <br><br>Allowed value: `true` or `false`<br>Default value: `true` |

### Failover Modes
| Mode | Description
---------|---------
| **Hot** | When the adapter is in `Hot` failover mode, any configured components are started and are collecting data from the data source. Collected data is buffered into the Failover specific buffer folder. Data is not egressed to the data endpoint(s). |
| **Warm** | When the adapter is in `Warm` failover mode, any configured components are started and are connected to the data source but are NOT collecting data from the data source. Since data is not being collected, data is not buffered. Data is not egressed to the data endpoint(s). |
| **Cold** | When the adapter is in `Cold` failover mode, any configured components are NOT started. The adapter does not connect to the data source and is not collecting data. Data is not egressed to the data endpoint(s).|
| **None** | When the adapter is in `None` failover mode, any configured components are started and are collecting data from the data source. Data is egressed to the data endpoint(s). <br><br>**Note:** An adapter in the `Secondary` role with mode of `None` has the same behavior as an adapter in the `Primary` role. |

**Note:** The adapter behaves according to failover mode when the failover role is `Secondary`. For example, if the current failover role is `Primary` and the client failover configuration has mode set to `Cold`, the adapter start, connect to the data source, and egress data to the data endpoint(s). Available failover modes may vary based on adapter.

## Example client failover configuration

The following is an example of a complete client failover configuration.

```json
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

### Failover Role 

The current failover `Role` is determined by the client failover endpoint. The current failover role is visible by querying the failover state, or by looking at the failover health asset if a health endpoint is configured.

| Role | Description
---------|---------
| **Primary** | When the adapter is in the `Primary` role, any configured components are started, connected to the data source, and collecting data. Data is egressed to the data endpoint(s). <br><br>**Note:** When the adapter is in the `Primary` role, any mode change does not affect adapter behavior and data will continue to be egressed regardless of mode. |
| **Secondary** | When the adapter in in the `Secondary` role, adapter behavior varies based on the failover mode. For more information see [Failover Modes](#failover-modes). |

When an adapter client failover configuration registers with an endpoint, if the adapter is the only adapter registered in that group it becomes the `Primary` adapter instance. If the adapter is not the only adapter registed in the client failover group, the `Primary` adapter instance is that with the highest `FailoverStatus` value. For more information on `FailoverStatus`. see [Failover Status](xref:FailoverStatus).

## Health

If the adapter has health endpoints configured, the client failover configuration values `Mode` and `FailoverGroupId` will be included in the static failover health data. For more information see [Failover Health](xref:FailoverHealth).

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/System/ClientFailover      | GET       | Gets the client failover configurations |
| api/v1/configuration/System/ClientFailover      | DELETE    | Deletes the configured client failover
| api/v1/configuration/System/ClientFailover      | POST      | Adds a client failover configuration. Fails if the client failover configuration already exists |
| api/v1/configuration/System/ClientFailover      | PUT       | Replaces the existing client failover configuration |
| api/v1/diagnostics/FailoverState                | GET     | Get the current failover state |
