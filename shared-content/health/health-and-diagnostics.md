---
uid: HealthAndDiagnostics
---

# Health and Diagnostics

AVEVA Adapters produce various types of health data. You can use health data to ensure that your adapters are running properly and that data flows to the configured OMF endpoints. For more information on available adapter health data, see [health](xref:AdapterHealth).

AVEVA Adapters also produce diagnostic data. You can use diagnostic data to find more information about a particular adapter instance. Diagnostic data is egressed and stored alongside of the health data. It can be optionally disabled by setting `EnableDiagnostics` to`false`. You can configure it in the system's [General configuration](xref:GeneralConfiguration). For more information on available adapter diagnostics data, see [diagnostics](xref:AdapterDiagnostics).

In AVEVA Data Hub (ADH), both health and diagnostics data are created as assets. The data are available in the Asset Explorer and you can use them in the ADH Trend feature. For more information, see the ADH documentation [Assets](https://docs.aveva.com/bundle/data-hub/page/add-organize-data/organize-data/assets/asset-concept.html).

## AF structure

With a health endpoint configured to a PI server, you can use PI System Explorer to view the health and diagnostics of an adapter. The element hierarchy is shown in the following image.

  ![AdapterHealthAFHierarchy](../images/adapter-health-af-hierarchy.png)

- The **Elements** root contains a link to an **Adapters** node. This is the root node for all adapter instances.
  
- Below **Adapters**, you will find one or more adapter nodes. Each node's title is defined by the node's corresponding computer name and service name in this format: `{ComputerName}.{ServiceName}`. For example, in the following image, **MachineName** is the computer name and **OpcUa** is the service name.
  
- To see the health and diagnostics values, click on an adapter node and select **Attributes**.

