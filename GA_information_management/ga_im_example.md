---

copyright:
years: 2016, 2017
lastupdated: "2017-05-15"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Using interfaces to map device data - example
{: #im_example}

The following interface example illustrates a possible solution to normalize readings from two temperature sensors into a consistent form that can be easily consumed by an application. One temperature sensor records temperature in degrees Fahrenheit and the other sensor records temperature in degrees Celsius.

## Example: Mapping heterogeneous temperature sensors to a logical interface
{: #device-type-example}
In this example, we create a logical interface that provides homogeneous temperature state data in one format, no matter what the actual device event message payload format is. TemperatureSensor1 publishes a Celsius temperature reading of `{ "t" : 34.5 }` to {{site.data.keyword.iot_short_notm}}. TemperatureSensor2 publishes a Fahrenheit temperature reading of `{ "temp" : 72.55 }`. The temperature readings are published as separate events.

For a detailed end-to-end scenario that describes this example, see the detailed [Scenario](../ga_im_index_scenario.html).

![Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}.](images/Information Management Device example.svg "Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}")

As part of the logical interface data flow you can perform calculations on incoming data to normalize these readings into a consistent form for processing. This means that you do not need to write your application to understand or convert different temperature scales. The application receives a single, normalized state and uses the **temperature** state property instead of the device specific **t** and **temp** properties.
