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

# Device twins
{: #device_twins}

A device twin is a digital representation in the cloud of a physical device or sensor that is connected to {{site.data.keyword.iot_full}}, regardless of whether the actual device is online or offline. A device twin is a logical model of the properties and events coming from a particular sensor/device. Once defined and instantiated, it provides a consistent means of interacting with a device in a REST-like manner. Because the models can be shared by multiple devices of different makes and models, the IoT application is now insulated from variability and change within the device ecosystem.
{: shortdesc}

Device twins provide more efficient ways to interact with large and diverse ecosystems of devices. {{site.data.keyword.iot_short_notm}} maintains a device twin for each connected physical device. Each device twin is uniquely identified by its name and contains a set of properties, which are stored in a JSON document. The properties of a device, including information about the current state of the device, can be retrieved by using a HTTP request to query the JSON document that represents the device twin, by using a user interface, or by subscribing to a topic.

Device Twins can help you:
- Provide your application developers with consistent interfaces to access event-driven device data in a REST-like manner.
- Normalize data from devices of different makes or models that publish data in different formats.

With {{site.data.keyword.iot_short_notm}} you can realize the device twin concept using:
- Logical Interface metadata
- Device type metadata
- Device instance metadata

# Next steps
{: #device_twins_next_steps}

- Create your own device twin in {{site.data.keyword.iot_short_notm}} by completing the steps outlined in the [High-level worklow](ga_im_workflow.html).

For more detailed information about each of the steps outlined in the High-level worklow, see the example scenario that is documented in [Scenario](ga_im_index_scenario.html#scenario). For more information about the concepts and resources that can help you to create your device twin, see [Logical interfaces](ga_im_overview.html) and [Definitions and resources](ga_im_definitions).
