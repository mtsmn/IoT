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

# Logical interfaces
{: #li_overview}

Logical interfaces extend the [device type](ga_im_definitions.html#resources) concept to better control the data that flows through {{site.data.keyword.iot_short_notm}} and to provide a device-agnostic view of IoT data.
{: shortdesc}

Use logical interfaces to create shared abstractions of devices. These abstractions can be used to store and query the state of the device, improve reuse and maintenance of that data, and to manage the complexities of an IoT ecosystem while keeping the application insulated from data change. Logical interfaces are decoupled from the variability in the message data that devices communicate to {{site.data.keyword.iot_short_notm}}.

Applications can access the current state of a device through logical interfaces. Device state information includes information about the metadata, configuration, and the condition of a device. The device state information is stored as a set of state properties that are defined by the logical interface. As devices send events about state changes, the most recent values of these properties are stored in {{site.data.keyword.iot_short_notm}} and are made available to the application on request by using an HTTP request command, or by subscribing to the following topic:
```
*iot-2/%s/type/%s/id/%s/intf/%s/evt/state*
```

If you subscribe to topic *iot-2/%s/type/%s/id/%s/intf/%s/evt/state*, you can configure one of the following notification settings:

Notification setting                        | Description       
------------- | ------------- | -------------  
never                         | No notifications are sent
on-every-event                         | Notifications are sent on each event, regardless of whether the device state is changed
on-state-change                         | Notifications are sent only when an event results in a change of device state

If an error occurs during processing, a message is sent to topic *iot-2/%s/type/%s/id/%s/err/data*.

## Benefits of logical interfaces
{: #li_benefits}

By using logical interfaces, you can:
- Map state properties to event message data
- Define the data structure that you prefer
- Define more than one representation or view of the device state
- Subscribe to device states or query them at any time through an HTTP API

Some common use cases for logical interfaces include:
- Providing your application developers consistent interfaces to access event-driven device data in a REST-like manner.
- Normalizing data from devices of different makes or models that publish data in different formats.
- Modifying and converting data formats to fit your application model.  

For logical interfaces API documentation, see [{{site.data.keyword.iot_short_notm}} HTTP REST API  ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/info-mgmt-beta.html){: new_window}.   
