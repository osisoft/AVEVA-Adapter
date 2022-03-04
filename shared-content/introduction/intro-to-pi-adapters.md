---
uid: IntroToPiAdapters
---

# About [!include[product-name](../_includes/inline/product-name.md)]

[!include[product-name](../_includes/inline/product-name.md)] is an OSIsoft data collection technology that collect operations data from a data source and then sends it to a supported storage location over the [!include[product-protocol](../_includes/inline/product-protocol.md)] protocol.
   
[!include[product-name](../_includes/inline/product-name.md)] performs the following actions:

* Collect data from assets in remote locations over the [!include[product-protocol](../_includes/inline/product-protocol.md)] protocol, a process known as data ingress.

* Convert ingress data to the OSIsoft Message Format (OMF).

* Send converted data to a supported egress endpoint, a process known as data egress. Supported egress endpoints include:

  * PI Server
  * OSIsoft Cloud Services
  
## [!include[product-name](../_includes/inline/product-name.md)] data flow

The following diagram depicts the process of the [!include[product-name](../_includes/inline/product-name.md)] collecting and processing data. It collects data from a [!include[product-protocol](../_includes/inline/product-protocol.md)] data source, such as edge devices or other industrial data sources. The adapter then converts that data to the OMF format, a format readable by all supported egress endpoints. Finally, the adapter then sends the data to a supported egress endpoint. 

<!-- Mark Bishop 6/22/21: The SVG file referenced below can be opened and edited using https://app.diagrams.net/ -->

![Adapter Data Flow](../images/adapter-data-flow.svg)

The [!include[product-name](../_includes/inline/product-name.md)] is installed on a host system. You can configure the PI Adapter and other system components using either a REST interface or EdgeCmd, a command line utility specifically designed for interfacing with edge systems.
