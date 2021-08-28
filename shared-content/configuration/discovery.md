---
uid: DiscoveryConfiguration
---

# Discovery

You can perform a data discovery for existing data pieces on demand. Data discovery is initiated through REST calls and it is tied to a specific discovery ID, which you can either specify or let the adapter generate.

Data discovery includes different routes. For example, you can choose to do the following:

- Retrieve the discovery results
- Query the discovery status
- Cancel or delete discoveries
- Merge discovery results with the data selection item
- Retrieve results from a current discovery and compare it with results from a previous discovery
- Retrieve results from a current discovery and compare it with results from a current data selection configuration

## Configure discovery

1. Start any of the [Configuration tools](xref:ConfigurationTools) capable of making HTTP requests.
2. Run a `POST` command with the `Id` of the discovery and `autoSelect` set to either `true` or `false` to the following endpoint: `http://localhost:5590/api/v1/configuration/<ComponentId>/Discoveries`.

   **Notes:**

    * Including an `Id` is optional. If you do not include one, the adapter automatically generates one.
    * `5590` is the default port number. If you selected a different port number, replace it with that value.

    Example using `curl`:

    ```bash
    curl -d "{ \"Id\":\"TestDiscovery\", \"autoSelect\":true }" -H "Content-Type:application/json" -X POST "http://localhost:5590/api/v1/configuration/<ComponentId>/Discoveries"
    ```

## Discovery parameters

Parameter | Type| Description
---------|----------|---------
**id** | `string` | The Id of the discovery <br><br> **Notes:**<br>&bull; You cannot run multiple discoveries with the same Id.<br>&bull; Including an `id` is optional. If you do not include one, the adapter automatically generates one.
**query** | `string` | A filter that is specific to the data source. The query filter can limit the scope of the discovery.<br><br>For more information, see the Data source configuration topic of your specific adapter.
**startTime** | `datetime` | Time when the discovery started.
**endTime** | `datetime`| Time when the discovery ended.
**progress** | `double` | Progress of the discovery (number of topics found through the discovery) 
**itemsFound** | `integer` | Number of data pieces that the discovery found on the data source.
**newItems** | `integer` | Number of new data pieces that the discovery found in comparison to the previous discovery.
**newAssets** | `integer` | Number of new assets that the discovery found in comparison to the previous discovery.
**resultUri** | `integer` | URL at which you can access the results of the discovery.
**autoSelect** | `boolean` | When set to `true`, the result of the discovery gets pushed to the data selection.
**status** | `reference` | Status of the discovery, for example `Active` or `Complete`.
**errors** | `string`| Errors encountered during the discovery.

**Note:** If, during data source discovery, the PI adapter encounters characters that are used in the stream Id but that are not allowed in PI tag names, the adapter replaces the characters with a unicode escape sequence in the auto-generated `<componentId>_DataSelection.json` file.

## Discoveries status example

The following example shows the status of all discoveries. The discovery id in this example was auto-generated.<br>

```json
[
    {
        "id": "8ff855f1-a636-490a-bb31-207410a6e607",
        "query": null,
        "startTime": "2020-09-30T19:34:01.8180401+02:00",
        "endTime": "2020-09-30T19:34:01.8368776+02:00",
        "progress": 30,
        "itemsFound": 4,
        "newItems": 0,
        "newAssets": 0,
        "resultUri": "http://127.0.0.1:5590/api/v1/Configuration/<ComponentId>/Discoveries/8ff855f1-a636-490a-bb31-207410a6e607/result",
        "autoSelect": false,
        "status": "Complete",
        "errors": null
    }
]
```

## REST URLs

| Relative URL                                                                          | HTTP verb | Action                                                                                                                                  |
|---------------------------------------------------------------------------------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------|
| api/v1/configuration/_componentId_/discoveries                                        | GET       | Returns status of all discoveries                                                                                                       |
| api/v1/configuration/_componentId_/discoveries                                        | POST      | Initiates a new discovery and returns its Id                                                                                            |
| api/v1/configuration/_componentId_/discoveries                                        | DELETE    | Cancels and deletes all saved discoveries                                                                                               |
| api/v1/configuration/_componentId_/discoveries/_discoveryId_                          | GET       | Gets the status of an individual discovery<br><br>**Note:** If a discovery with the specified ID does not exist, you will get an error message                                                                                           |
| api/v1/configuration/_componentId_/discoveries/_discoveryId_                          | DELETE    | Cancels and deletes discovery and result                                                                                                |
| api/v1/configuration/_componentId_/discoveries/_discoveryId_/result                   | GET       | Returns the result of a discovery                                                                                                       |
| api/v1/configuration/_componentId_/discoveries/_discoveryId_/result?diff=_previousId_ | GET       | Returns the difference between the result and the previous result                                                                       |
| api/v1/configuration/_componentId_/dataselection?diff=_discoveryId_                   | GET       | Returns the difference between the data selection configuration and the discovery results
| api/v1/configuration/_componentId_/discoveries/_discoveryId_/result                   | DELETE    | Cancels and deletes discovery result.<br><br>**Note:** The discovery ID is still valid, but the **Status** property of a discovery query will contain a status of `canceled` |
| api/v1/configuration/_componentId_/dataselection/select?discoveryid=_discoveryId_     | POST      | Adds the discovered items to data selection with selected set to `true`                                                                   |
| api/v1/configuration/_componentId_/dataselection/unselect?discoveryid=_discoveryId_   | POST      | Adds the discovered items to data selection with selected set to `false`

**Note:** Replace _ComponentId_ with the Id of your adapter component.<br>Replace _DiscoveryId_ with the Id of the discovery for which you want to perform the action.
