---
uid: FailoverHealth
---

# Failover Health

PI Adapters produce various kinds of health data that can be egressed to different health endpoints. If an adapter has failover enabled, additional health information pertaining to failover is available.

To egress health-related data, you have to configure an adapter health endpoint first. For more information, see [Health endpoint configuration](xref:HealthEndpointConfiguration).

## Available health data

### Static health data

Static health data is only updated when the client failover configuration is updated. For more information, see [Client Failover Configuration](xref:ClientFailoverConfiguration).

The following static health data is available when an adapter has failover enabled:

| Property | Description |
---------|---------
| **Name** | The name of the node. For more information see [AF structure](#af-structure). |
| **Description** | Description of the health asset. |
| **Host** | The hostname of the machine. | 
| **Version** | The current version of the failover component. |
| **Failover Group Id** | The `FailoverGroupId` of the client failover configuration. See  [Client Failover Configuration](xref;ClientFailoverConfiguration) for more.|
| **Failover Mode** | The `Mode` of the client failover configuration. See [Client Failover Configuration](xref;ClientFailoverConfiguration) for more. |
| **Failover Endpoint** | The `Endpoint` of the client failover configuration. See [Client Failover Configuration](xref;ClientFailoverConfiguration) for more. |

### Dynamic health data

Dynamic data is sent every minute to configured health endpoints.

The following dynamic health data is available when an adapter has failover enabled:

- [Device status](xref:DeviceStatus)
- [Next Health Message Expected](xref:NextHealthMessageExpected)
