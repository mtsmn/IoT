---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Definitions and resources
{: #definitions_resources}

The following diagram illustrates the logical mapping between devices and applications on {{site.data.keyword.iot_short_notm}} when using logical interfaces.

![The logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}.](images/ga_im_resources.svg "The mapping between devices, things, and applications on {{site.data.keyword.iot_short_notm}}")

## Concepts

Concepts                        | Description       
------------- | ------------- | -------------  
Event | Events are the mechanism by which devices publish data to {{site.data.keyword.iot_short_notm}}. The device controls the content of the event and assigns a name for each event that it sends.
Property | Data carrying part of a device event payload.
State | The latest value of a mapped state property.


## Information management resources
{: #resources}

You can manage the resources by using REST APIs. For information about the REST APIs, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/info-mgmt-beta.html) documentation.

Type Resources                        | Description       
------------- | ------------- | -------------  
Event type                         | A programmatic construct that connects a physical interface to an event schema. </br>**Important:** For the beta, all inbound events to be used in a logical interface must be in JSON format.   
Device type                         |  A programmatic construct that lets you group devices that share characteristics or behaviors. In interface mapping the device type is extended to include one physical interface for a device and one or more logical interfaces that are used to retrieve the device state. </br>For more information, see the "Identifiers and device types" section in the [Device Model](../reference/device_model.html#id_and_device_types) topic.
Schema resources                         |  Programmatic constructs that define the data structure of the device type physical interfaces and the outgoing logical interfaces. The following [JSON Schemas ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://json-schema.org/){:new_window} are used: <ul><li>*Event schemas* define the structure of the events that are published to {{site.data.keyword.iot_short_notm}} by a device. Each event schema defines the structure of one inbound event and is associated with one event type. <li>*Logical interface schemas* define the structure of the device state that is stored on {{site.data.keyword.iot_short_notm}}</ul>.

Interface resources                        | Description       
------------- | ------------- | -------------  
Logical interface | A programmatic construct that your applications can connect to or subscribe to to see the state of a device. The logical interface is defined by a logical interface schema that shapes the structure of the state data that is stored as the device state. The state is updated in response to inbound state events. A logical interfaces associated with a device type can have one physical interface as input.

Instance resources                        | Description       
------------- | ------------- | -------------  
Device                         | A programmatic construct that represents an asset, system, or component that is registered with {{site.data.keyword.iot_short_notm}} and sends IoT data in the form of events.  

Supporting resources                        | Description       
------------- | ------------- | -------------  
Physical interface                         | A programmatic construct that defines the event types and associated device properties that are associated with a single device type. The physical interface is defined by event schemas.   
Mappings                         | A programmatic construct that defines how properties that are associated with inbound events are mapped to properties that are defined on a logical interface. </br>**Important:** At least one logical interface must be associated with a device type before any mappings can be defined.
