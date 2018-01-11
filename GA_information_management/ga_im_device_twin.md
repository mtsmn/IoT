---

copyright:
years: 2016, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduction to data management
{: #device_twins}

An unprecedented number of devices and sensors exist in the modern world. Many of these devices provide similar functionality, but variations in make, model and version mean that the data is output in different formats. For example, a temperature sensor might record temperature in degrees Fahrenheit or in degrees Celsius. It is not efficient to code applications to be able to consume data in all these formats; instead the data needs to be normalized to create a single logical view that can be used by applications. 
{: shortdesc}

Use the data management feature in {{site.data.keyword.iot_full}}, to configure a device twin to expose a normalized view of the data to your applications.

A device twin is a digital representation in the cloud of a physical device or sensor that is connected to {{site.data.keyword.iot_short_notm}}. A device twin creates a logical model of the properties and events coming from a particular sensor or device. Once defined and instantiated, the device twin provides a consistent means of interacting with a device in a REST-like manner, regardless of whether the device is online or offline. Because a logical model can be shared by multiple devices of different makes and models, the IoT application is now insulated from variability and change within the device ecosystem. The properties of a device, including information about the current state of the device (device state), can be retrieved by using a HTTP request, or by subscribing to a topic.

Device twins can help you:
- Provide your application developers with consistent interfaces to access event-driven device data in a REST-like manner.
- Normalize data from devices of different makes or models that publish data in different formats.

To use the data management feature to configure a device twin, you need to define the following information by configuring resources in {{site.data.keyword.iot_short_notm}}:
- The structure of the events that are sent by your device. The structure of an inbound event is defined in the physical interface, event type, and event schema resources. 
- The properties that you want to record. These properties define the logical structure of the device state that can be consumed by  your applications. The properties are defined in the logical interface and logical schema resources.
- How the physical interface events map onto the logical interface properties. Use the mappings resource to map events to properties.

The following diagram shows device data in differing formats coming in to {{site.data.keyword.iot_short_notm}}, and being transformed and normalized into a single, logical view that can be easily consumed by back-end applications.  

![Overview of managing data in {{site.data.keyword.iot_short_notm}}.](images/ga_im_resources_overview.svg "Overview of managing data in {{site.data.keyword.iot_short_notm}}")

For more information about defining and configuring key information and resources, see [Understanding data management](ga_im_definitions.html). You can create your own device twin in {{site.data.keyword.iot_short_notm}} by completing the steps outlined in the [Getting started with data management](ga_im_example.html). For more detailed information about each of the steps outlined in the guide, see the example scenario that is documented in [Step-by-step guide: A detailed example about how to work with devices through a common interface](ga_im_index_scenario.html#scenario). 
