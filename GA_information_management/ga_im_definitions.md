---

copyright:
years: 2016, 2018
lastupdated: "2018-11-19"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:important: .important}

# Understanding data management
{: #definitions_resources}
You might have a number of different devices or Things that you want to connect to {{site.data.keyword.iot_full}}, and these devices or Things might publish data in different formats. By using the device twin and asset twin features of the data management component, you can normalize and transform the data output from your devices and Things into a single logical view that can be easily consumed by your applications. By using a single logical view, you remove the need to configure your applications to understand the different data formats that are output by each device or Thing. You can then aggregate multiple devices or Things together to define a new Thing in {{site.data.keyword.iot_short_notm}}. Use the Thing to help you to organize and analyze data that is coming into {{site.data.keyword.iot_short_notm}} from a range of inputs.

{: shortdesc}

## Overview
{: #overview}

Use the device twin feature to create a logical model of a device, and then use the asset twin feature to aggregate these logical models to define new Things. These logical models help you to improve reuse and maintenance of code and to manage the complexities of an IoT ecosystem by keeping your applications insulated from data change.

Applications can access the current state of a device or Thing on request by using an HTTP API, or by subscribing to an IoT topic string. The state consists of a set of state properties that are defined by a logical interface. If the state of a device or Thing changes as a result of an event being published to {{site.data.keyword.iot_short_notm}}, the values of these properties are updated and stored in {{site.data.keyword.iot_short_notm}}.

By using the device and asset twin features, you can achieve the following benefits:
- Map state properties to event message data.
- Aggregate multiple devices or Things together to define new Things.
- Define the data structure that you prefer.
- Define more than one representation or view of the device or Thing state.
- Subscribe to device or Thing states, or query them at any time through an HTTP API.

Some common use cases for implementing the device and asset twin features include:
- Providing your application developers with consistent interfaces to access event-driven device data in a REST-like manner.
- Normalizing data from devices of different makes or models that publish data in different formats.
- Modifying and converting data formats to fit your application model.
- Formatting big data from a range of devices or Things so that the data can be analyzed and presented in the most effective way in order to help to predict failures, schedule maintenance, track assets and improve operational efficiency.

## Examples
{: #examples}
The following examples illustrate two possible solutions. Example 1 illustrates how you can use the device twin feature and Example 2 illustrates how you can use the asset twin feature.

### Example 1: Mapping heterogeneous temperature sensors to a logical interface
{: #device-type-example}
In this example, we create a logical interface that provides homogeneous temperature state data in one format, no matter what the actual device event message payload format is. The *tSensor* device publishes a Celsius temperature reading of `{ "t" : 34.5 }` to {{site.data.keyword.iot_short_notm}}. The *tempSensor*  device publishes a Fahrenheit temperature reading of `{ "temp" : 72.55 }`. The temperature readings are published as separate events.

For a detailed end-to-end scenario that describes this example, see [Step-by-step guide 1](ga_im_index_scenario.html).

![Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}](../information_management/images/Information Management Device example.svg "Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}")

As part of the logical interface data flow, you can perform calculations on incoming data to normalize these readings into a consistent form for processing. This means that you do not need to write your application to understand or convert different temperature scales. The application receives a single, normalized state and uses the **temperature** state property instead of the device specific **t** and **temp** properties.

### Example 2: Mapping multiple climate devices to one Thing type logical interface
{: #thing-type-example}  
In this example, we expand on the device type example by adding a set of humidity sensors in the form of separate hygrometer devices. By using a Thing type logical interface, we can seamlessly merge data from separate device types into one logical interface that represents all devices and sensors in a room. An application can now get the collected climate data for a room by connecting to the logical interface associated with the "RoomType" Thing type. The following diagram shows the configuration for Meeting Room 1.

For a detailed end-to-end scenario that describes this example, see [Step-by-step guide 2](../information_management/im_index_scenario_thing.html).

![Mapping between temperature and humidity thing and an application on {{site.data.keyword.iot_short_notm}}](../information_management/images/Information Management Thing example scenario.svg "Mapping between multiple environmental sensors in one room and an application on {{site.data.keyword.iot_short_notm}}")

A temperature device that is called *tSensor* and a humidity device that is called *humiditySensor1* publish environmental data that is collected in room *Meeting Room 1*. The temperature and humidity sensor data is separately mapped to two device type logical interfaces; one for the thermometer device type and one for the hygrometer device type. We now create a Thing type called *RoomType* and instantiate a room Thing instance that is called *Meeting Room 1*.

In a second meeting room, a temperature device that is called *tempSensor* and a humidity device that is called *humiditySensor2* publish environmental data that is collected in room *Meeting Room 2*. Another room Thing instance called *Meeting Room 2* is created, based on the *RoomType* Thing type.

We can now set up a composition that includes the thermometer and hygrometer logical interfaces and then map the correct environmental sensors to each of the room instances, for example, *tSensor* and *humiditySensor1* mapped to *Meeting Room 1* and *tempSensor* and *humiditySensor2* mapped to *Meeting Room 2*.

The end-user application can now request the state of a specific room Thing ID and get the room temperature and humidity states without having to know about the underying device infrastructure.

## Definitions and resources
{: #resources}

The following diagrams illustrate the logical mapping between devices and applications on {{site.data.keyword.iot_short_notm}} when using logical interfaces.

![The logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}.](images/im_resources.svg "The mapping between devices, Things, and applications on {{site.data.keyword.iot_short_notm}}")

### Concepts

Concepts                        | Description       
------------- | ------------- | -------------  
Event | Events are the mechanism by which devices publish data to {{site.data.keyword.iot_short_notm}}. The device controls the content of the event and assigns a name for each event that it sends.
Property | Data carrying part of a device event payload.
State | The latest representation of the state of the physical device, which can include all properties that have been mapped across from multiple inbound events.
Composition                         | A logical construct that defines the logical interfaces that are associated with a Thing type. The composition is specified by a Thing type schema.   

### Data management resources
You can manage the resources by using REST APIs. For information about the REST APIs, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html) documentation.

Type Resources                        | Description       
------------- | ------------- | -------------  
Event type                         | Use the event type resource to model an event that is published by a device. An event type must reference an event schema resource. The schema resource defines the structure of the event that is published. </br>**Important:** Inbound events that are used in a logical interface must be in JSON format.    
Device type                         |  Use the device type resource to group devices that share characteristics or behaviors. In data management, the device type is extended to include one physical interface for a device and one or more logical interfaces that are used to retrieve the device state. </br>For more information, see the "Identifiers and device types" section in the [Device Model](../reference/device_model.html#id_and_device_types) topic.
Thing type                         | A programmatic construct that represents a collection of one or more separate device types, Thing types, or both. </br>**Important:** The Beta supports ten levels of nesting for a Thing type logical interface.
Schema resources                         |  Use schema resources to define the structure of either an event, device or Thing state. The following [JSON Schemas ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://json-schema.org/){:new_window} are used: <ul><li>A schema that is associated with an event type. This schema is used to define the structure of the event that is published to {{site.data.keyword.iot_short_notm}} by a device. These schemas are referred to as event schemas. <li>A schema that is associated with a logical interface. This schema is used to define the structure of the device or Thing state that is stored on {{site.data.keyword.iot_short_notm}}. These schemas are referred to as logical interface schemas</ul>.</ul>

Interface resources                        | Description       
------------- | ------------- | -------------  
Logical interface | A programmatic construct that your applications can connect to or subscribe to to see the state of a device. A logical interface is used to define the normalized view onto the device state in {{site.data.keyword.iot_short_notm}}. A logical interface must be associated with a logical interface schema. The state is updated in response to inbound device events. **Note:** You can optionally specify a meaningful alias name for your logical interface. The alias can be referenced in the API call or topic string subscription that is used to retrieve the state of a device, instead of using the auto-generated logical interface identifier.  
Physical interface                         | A physical interface is used to model the interface between a physical device and {{site.data.keyword.iot_short_notm}}. Event types can be associated with physical interface.  

Instance resources                        | Description       
------------- | ------------- | -------------  
Device                         | A device represents an asset, system, or component that is registered with {{site.data.keyword.iot_short_notm}} and sends IoT data in the form of events.  
Thing                         | A programmatic construct that logically represents a unique instance of a Thing type. A Thing instance serves the same purpose as a registered device of a device type.


Supporting resources                        | Description       
------------- | -------------   
Mappings       |Use mappings to define how properties that are associated with inbound events are mapped to properties that are defined on a logical interface. </br>**Important:** At least one logical interface must be associated with a device or Thing type before any mappings can be defined.

## Naming restrictions for resources
{: #naming_restrictions}
Schemas, event types, logical and physical interfaces have the following naming restrictions:
- The name must be between 1 - 128 characters
- The name must consist of unicode characters
- Valid special characters are space, hyphen ( - ), underscore ( _ ), period ( . )
- The name cannot consist only of spaces

## Creating, updating, activating, and deactivating your resources
{: #draft_active_resources}

There can be two versions of a resource; a draft version and an active version. When you create a resource, that resource is created as a draft version.
{: shortdesc}

The draft version is a working copy of your resource that you can query, update and delete directly by using APIs. Create an active version of a draft resource by activating either a draft device type, draft Thing type or draft logical interface. To activate other resources, for example schemas, you must activate a draft device type, draft Thing type or draft logical interface that references the resource that you want to activate.

To differentiate between draft and active resources when using REST APIs, the prefix *draft/* is used to identify resources that are in a draft state.

The following example retrieves metadata for a draft schema definition by using a specified id:

```
GET /api/v0002/draft/schemas/{schemaId}
```
The following example retrieves metadata for an active schema definition by using a specified id:
```
GET /api/v0002/schemas/{schemaId}
```
*Note:* The identifier is the same for the draft and active version of a given resource.


- Activating a resource
{: #activate_resources}  

Use the **activate-configuration** operation to validate and activate the configuration that is associated with a device or Thing type. This configuration includes your draft schemas, event types, physical interfaces, logical interfaces, and mappings. The **activate-configuration** operation must be performed on the draft version of a logical interface, device type or Thing type.

The following example shows a PATCH request where an **activate-configuration** operation is performed on a draft version of a device type:
```
PATCH /api/v0002/draft/device/types/TSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "activate-configuration"
  }   
```
To activate a draft version of a Thing type, use the following PATCH method:
```
PATCH /api/v0002/draft/thing/types/RoomType
```

- List differences
{: #list_differences}  

Use the **list-differences** operation to return a list of any differences between the active and draft configuration for a logical interface, device type or Thing type resource. The **list-differences** operation must be performed on the draft version of a logical interface, device or Thing type. The following example shows a PATCH request where a **list-differences** operation is performed on a draft version of a device type:
```
PATCH /api/v0002/draft/device/types/TSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "list-differences"
  }
```
To return a list of any differences between the active and draft configuration for a Thing type resource, use the following PATCH method:
```
PATCH /api/v0002/draft/thing/types/meetingroom1
```

- Deactivating a resource  
{: #deactivate_resources}  

Use the **deactivate-configuration** operation to remove the active configuration that is associated with a resource. The deactivate-configuration operation can be performed only on the active version of a logical interface, device type or Thing type. The following example shows a PATCH request where a **deactivate-configuration** operation is performed on an active version of a device type:
```
PATCH /api/v0002/device/types/TSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "deactivate-configuration"
  }
```
To deactivate a Thing type, use the following PATCH method:
```
PATCH /api/v0002/thing/types/RoomType
```

*Notes:*
- An active resource is read only. You can filter and sort draft and active resources by using query parameters.
- You cannot delete a device type if there are device instances associated with that device type. The state of the device is cleared when the device instance is deleted.
- You cannot delete a Thing type if there are device or Thing instances associated with that Thing type. The state of the Thing is cleared when the device or thing instances are deleted.
- You can activate only logical interfaces, device types and Thing types directly by using APIs. Other resources, for example schemas, physical interfaces, Thing type interfaces and event types are activated if they are referenced by a logical interface, device type or Thing type that is made active.  
- The **activate-configuration** operation must be performed on a draft version of a logical interface that is associated with a device or Thing type, or on the device or Thing type itself. The **activate-configuration** operation checks that the resource configuration is valid before activating the resource. Once activation is successfully completed, state is generated for each device or Thing instance of the device or Thing type.

## Troubleshooting your configuration
{: #troubleshooting}
If your activation fails, check that all the required configuration for a given device or Thing type is provided.

The following configuration must be provided and associated with a device type:
  - A physical interface that is associated with at least one event
  - At least one logical interface
  - Mappings for at least one of the associated logical interfaces

The following configuration must be provided and associated with a Thing type:
  - A Thing interface that is associated with at least one device or Thing type
  - At least one logical interface
  - Mappings for at least one of the associated logical interfaces  

You can also perform a **validate-configuration** operation on a draft version of the device type, Thing type or logical interface resource to ensure that the associated metadata is valid. If the metadata is invalid, a list of issues is returned in the body of the response.  

The following example shows a PATCH request where a **validate-configuration** operation is performed on a draft version of a device type that is called "TSensor":  
```
PATCH /api/v0002/draft/device/types/TSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "validate-configuration"
  }
```  
The following example shows an unsuccessful response to the PATCH request:  
```
{
"message": "CUDIM0303I: State update configuration for Device Type 'TSensor' is not valid.",
"details": {
  "id": "CUDIM0303I",
  "properties": [
    "Device Type",
    "Sensor"
  ]
},
"failures": [
  {
    "message": "CUDVS0301E: The device type 'TSensor' does not have any mappings defined for it",
    "details": {
      "id": "CUDVS0301E",
      "properties": [
        "TSensor"
      ]
    }
  }
]
}
```  
The following example shows a successful response to the PATCH request:  
```  
{
"message": "CUDIM0303I: State update configuration for Device Type 'TSensor' is valid.",
"details": {
  "id": "CUDIM0303I",
  "properties": [
    "Device Type",
    "TSensor"
  ]
},
"failures": []
}
```  
If all the required resources are associated with the device or Thing type, check that the property mappings are valid. The following examples show possible errors that might occur:

  - An expression references a property on an event that is not defined by the event schema
  - An expression references a property on the state that is not defined by the logical interface schema
  - A mapping is defined for a property that is not defined by the logical interface schema


You can refer to the following error log to help you to diagnose run time errors for device types:
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
You can refer to the following error log to help you to diagnose run time errors for Thing types:
```
iot-2/type/${typeId}/id/${thingId}/err/data
```

### Resource limits

<p>The {{site.data.keyword.iot_short_notm}} pricing plans were updated on November 27, 2018.   
For more information, including upgrade information, see [{{site.data.keyword.iot_short_notm}} service plans](plans_overview.html). The contents of this [IBM Cloud documentation collection](https://console.bluemix.net/docs/services/IoT/) pertain to the {{site.data.keyword.iot_short_notm}} Lite plan, and to the previous Standard and Advanced Security plans. For documentation about the {{site.data.keyword.iot_short_notm}} Connection and Analytics Service plans, with their extended feature set, see the [{{site.data.keyword.iot_short_notm}} knowledge center ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/overview/overview.html).
</p>
{: important}

The following table shows the maximum number of resources that can be configured based on plan type.

Resource                   |Standard plan                  | Lite plan
------------- | ------------- | -------------
Logical Interfaces | 1000 | 10
Physical Interfaces           | 1000 | 5
Event Types | 1000 | 10
Schemas |2000 | 20
Logical Interface references (Number of logical interfaces that a device type can map to)  |20 | 5
Event Type references (Number of event ID to event type associations that a physical interface can have)| 40 | 10
