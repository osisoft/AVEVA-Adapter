---
uid: system-components
---

# System components

Each PI Adapter is composed of different components to facilitate data collection, data conversion to the OMF message format, and egress to configured egress endpoints.

## Adapter components

_Adapter components_ collect data from a single data source on the host. Adapter components are configured in a modular fashion. After installing the adapter on a host, you may install adapter components in any of the following combinations:

* **a single adapter component**, to collect data from a single data source over a given communication protocol.

* **multiple instances of the same adapter component**, to collect data from multiple data sources over a single communication protocol. For example, if the host contains two data sources for the [!include[product-name](../_includes/inline/product-name.md)] protocol, configure two instances of the adapter component for [!include[product-name](../_includes/inline/product-name.md)] within the system configuration.

## OMF egress components

OMF Egress components take data collected by adapter components, translate it into the OMF format, and then route that data to configured endpoints. To support egress of data to multiple endpoints, create multiple instances of the OMF Egress component during configuration of system components.
