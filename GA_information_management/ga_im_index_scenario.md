---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Step-by-step guide 1: A detailed example about how to work with devices through a common interface
{: #scenario}

Use the following information to create a scenario in which two temperature devices publish events to {{site.data.keyword.iot_full}}. One device measures temperature in degrees Celsius. The other device measures temperature in degrees Fahrenheit. These readings are mapped to a single temperature reading that is in degrees Celsius. When a new temperature reading is published by these devices, the value of the property associated with the device state is changed.
{: shortdesc}

## Before you begin

When you create a [resource](ga_im_definitions.html#definitions_resources), that resource is created as a draft version. The draft version is a working copy of your resource that you can query, update and delete directly by using APIs. When using REST APIs, the prefix **draft/** is used to identify resources that are in a draft state. 

For more information about draft and active versions of resources, see [Understanding data management](ga_im_definitions.html).

## Pre-requisites

You must have a {{site.data.keyword.iot_short_notm}} [organization instance](../iotplatform_overview.html#organizations) and an API key and authentication token for that organization to authenticate requests. 

For information about generating an API key, see the [API](../reference/api.html) documentation. For more information about generating an API token, see the [Getting started tutorial](../getting-started.html) documentation.


## About this task

In this scenario, two devices are created.

One device is called *tSensor*. This device publishes temperature events that are measured in degrees Celsius. The temperature event is published on the topic `iot-2/evt/tevt/fmt/json` and has the following example payload:
```
{
  "t" : 34.5
}
```

**Note:** The event identifier is *tevt*. This identifier is required when you add a temperature event of this type to the physical interface and when you define mappings to map a property associated with an inbound event of this type to a property in your logical interface. In this scenario, the property defined in the logical interface is called **temperature**.

The other device is called *tempSensor*. This device publishes temperature events that are measured in degrees Fahrenheit. The temperature event is published on the topic `iot-2/evt/tempevt/fmt/json` and has the following example payload:
```
{
  "temp" : 72.55
}
```

**Note:** The event identifier is *tempevt*. This identifier is required when you add a temperature event of this type to the physical interface and when you define mappings to map a property associated with an inbound event of this type to a property in your logical interface. In this scenario, the property defined in the logical interface is called **temperature**.

A logical interface is also configured. This logical interface represents the state for devices of this type in the following structure:
```
{
  "temperature" : <current temperature value in Celsius>
}
```
This configuration means that you can configure your application to process the value that is associated with **temperature**, rather than configuring your application to process the value that is associated with **t** and to process the value that is associated with **temp** after converting that value to degrees Celsius.

![Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}.](../information_management/images/Information Management Device example.svg "Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}")  

Use the following example scenario to set up your own interfaces environment.

**Important Note:** You must save the IDs that are generated in the curl responses as the IDs are required to complete other tasks.
A table listing the resource property names, values, and identifiers that are used in this guide is documented in [Additional information for Step-by-Step guides 1 and 2 - resource names and identifiers](../information_management/im_id_reference.html).

## If needed, add a Device Type and a Device
{: #step14}

In this scenario, two device types and two device instances are assumed. Device instance *tSensor* is associated with device type *TSensor*. Device instance *tempSensor* is associated with device type *TempSensor*. 

You can create device types and devices by using the [{{site.data.keyword.iot_short_notm}} dashboard ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://internetofthings.ibmcloud.com){: new_window}, or by using REST APIs. For more information about using the {{site.data.keyword.iot_short_notm}} dashboard to add device types and devices, see the [Getting started with data management by using the Web interface](im_ui_flow.html) documentation.

The following example shows how to create a device type that is called *TSensor* by using the REST API:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TSensor", "description" : "The Celsius sensor device type", "metadata": {"tempThresholdMax": 44,
    "tempThresholdMin": 10}}' \
 ```
 
 The following example shows how to create a device type that is called *TempSensor* by using the REST API:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TempSensor", "description" : "The Fahrenheit sensor device type"}' \
 ```

**Note:** You can add metadata when you create a device type and device. In this scenario, the following metadata is added to the device type *TSensor*:
```
{
    "tempThresholdMax": 44,
    "tempThresholdMin": 10 
}
```
This metadata is used when creating [rules](../information_management/im_rules.html) that trigger when a temperature event that causes the *temperature* property of the device state to exceed 44 degrees Celsius is received by {{site.data.keyword.iot_short_notm}} from the *tSensor* device. 


You then need to register a device instance that is associated with a device type. The following example shows how to register a device instance that is called *tSensor* that is associated with device type *TSensor* by using the REST API:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tSensor", "authToken": "password"}' \
```

The following example shows how to register a device instance that is called *tempSensor* that is associated with device type *TempSensor* by using the REST API:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tempSensor", "authToken": "password"}' \
```

For information about using REST APIs to add device types and devices, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window} documentation.

**Note:** When a device makes an HTTP request through the Watson IoT Platform HTTP REST API, a user name and password is required. The password is the value of the authentication token that is either automatically generated or manually specified when a device is registered. If you are using an MQTT client, you must keep a note of the authentication token of your device as you need the token to retrieve device or thing state by subscribing to a topic string.

## Step 1: Create an event schema file
{: #step1}

For this scenario, create two event schema files to define the structure of each of the inbound temperature events.

The following example shows how to create a schema file that is called *tEventSchema.json*. This file defines the structure of an inbound event from a temperature device that measures temperature in degrees Celsius:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tEventSchema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
      "description" : "temperature in degrees Celsius",
      "type" : "number",
      "minimum" : -273.15,
      "default" : 0.0
    }
  },
  "required" : ["t"]
}
  ```
**Tip:** Use the **required** parameter to mark one or more properties as required. Required properties must be included in a device message for {{site.data.keyword.iot_short_notm}} to update the device state with the device data. A message that does not include the required property is not processed.   

The schema file name *tEventSchema* is used when you create an event schema resource for your event type.

The following example shows how to create a schema file that is called *tempEventSchema.json*. This file defines the structure of an inbound event from a temperature device that measures temperature in degrees Fahrenheit:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tempEventSchema",
  "description" : "defines the structure of a temperature event in degrees Fahrenheit",
  "properties" : {
    "temp" : {
      "description" : "temperature in degrees Fahrenheit",
      "type" : "number",
      "minimum" : -459.67,
      "default" : 0.0
    }
  },
  "required" : ["temp"]
}
  ```
The schema file name *tempEventSchema* is used when you create an event schema resource for your event type.   

## Step 2: Create an event schema resource for your event type
{: #step2}

To create an event schema resource, use the following API:

```
POST /draft/schemas
```

The schema definition file is passed to the Watson IoT Platform within a multipart POST (multipart/form-data). The body of the POST must contain at least two parts:

- One with a name of **schemaFile** that contains the actual content of the file as the body of the part.
- One with a name of **name** that contains a string that defines the name of the schema resource in the body of the part.

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) documentation.

The following example shows how to use cURL to create the event schema resource *tEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

**Tip:** The example authorization value `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` consists of the following information:
```{API Key}:{authorization token}```,  which is then Base64 encoded.

The following example shows a response to the POST method:

```
{
  "name" : "tEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "tEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e0d",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
The schema identifier *5846cd7c6522050001db0e0d* that is returned in response to the POST method is required when you add an event schema to your event type.

The following example shows how to use cURL to create the event schema resource *tempEventSchema.json*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

The following example shows a response to the POST method:

```
{
  "schemaType" : "json-schema",
  "schemaFileName" : "tempEventSchema.json",
  "updated" : "2016-12-06T14:44:51Z",
  "name" : "tempEventSchema",
  "version" : "draft",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "created" : "2016-12-06T14:44:51Z",
  "id" : "5846cee36522050001db0e0e",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e/content"
  },
  "contentType" : "application/octet-stream",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
The schema identifier *5846cee36522050001db0e0e* that is returned in response to the POST method is required when you add an event schema to your event type.

## Step 3: Create an event type that references the event schema
{: #step3}

Each event type references the relevant event schema that was created in the previous example by using the schema identifier returned in the response to the POST method used to create the event schema resource.

To create an event type, use the following API:

```
POST /draft/event/types
```
The draft event type must reference the schema definition that defines the structure of the inbound MQTT event.


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) documentation.


The following example shows how to use cURL to create an event type for a temperature event that is measured in degrees Celsius:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

The schema identifier *5846cd7c6522050001db0e0d* is used to add the event schema to the event type. This identifier was returned in response to the POST method that was used to create the event schema resource *tEventSchema.json*

The following example shows a response to the POST method:

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e0d",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d"
  },
  "name" : "tEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846d0fd6522050001db0e0f",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

The event type identifier *5846d0fd6522050001db0e0f* that is returned in response to the POST method is used to add an event type to the physical interface.

The following example shows how to use cURL to create an event type for a temperature event that is measured in degrees Fahrenheit:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
The schema identifier *5846cee36522050001db0e0e* is used to add the event schema to the event type. This identifier was returned in response to the POST method that was used to create the event schema resource *tempEventSchema.json*

The following example shows a response to the POST method:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "schemaId" : "5846cee36522050001db0e0e",
  "created" : "2016-12-06T15:00:20Z",
  "id" : "5846d2846522050001db0e10",
  "updated" : "2016-12-06T15:00:20Z",
  "name" : "tempEvent",
  "version" : "draft",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e"
  },
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
The event type identifier *5846d2846522050001db0e10* that is returned in response to the POST method is used to add an event type to the physical interface.

## Step 4: Create a physical interface
{: #step7}

To create a physical interface, use the following API:

```
POST /draft/physicalinterfaces
```

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) documentation.

In this scenario, we need two physical interfaces - one for each event type.

The following example shows how to use cURL to create the first physical interface:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TSensor Physical Interface"}'
```

The following example shows a response to the POST method:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

The physical interface identifier *5847d1df6522050001db0e1a* that is returned in the response is used in the URL of the POST method that is called to add a temperature event that is measured in degrees Celsius to the physical interface.

The following example shows how to use cURL to create the second physical interface:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TempSensor Physical Interface"}'
```

The following example shows a response to the POST method:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

The physical interface identifier *5847d1df6522050001db0e1b* that is returned in the response is used in the URL of the POST method that is called to add a temperature event that is measured in degrees Fahrenheit to the physical interface.   

## Step 5: Add the event type to the physical interface
{: #step8}

To add an event type to your physical interface, use the following API:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) documentation.

In this scenario, the following event types are added to the specified phyiscal interfaces:
- the Celsius temperature event *tevt* is added to the physical interface with identifier *5847d1df6522050001db0e1a* by using the *eventId* from the topic and the *eventTypeId* from the creation of the event schema resource.
- the Fahrenheit temperature event *tempevt* is added to the physical interface with identifier *5847d1df6522050001db0e1b* by using the *eventId* from the topic and the *eventTypeId* from the creation of the event schema resource.


The following example shows how to use cURL to add the temperature event *tevt* to the physical interface with the identifier *5847d1df6522050001db0e1a* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

The following example shows a response to the POST method:

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

The following example shows how to use cURL to add the temperature event *tempevt* to the physical interface with the identifier *5847d1df6522050001db0e1b*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

The following example shows a response to the POST method:

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
}
```

## Step 6: Update the device type to connect the physical interface
{: #step9}

To update a device type, use the following API:

```
POST /draft/device/types/{typeId}/physicalinterface
```

where *typeId* is the device type identifier.


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.

In this scenario, device type *TSensor* is updated to connect to physical interface *5847d1df6522050001db0e1b* and device type *TempSensor* is updated to connect to physical interface *5847d1df6522050001db0e1a*.


The following example shows how to use cURL to update device type *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

The following example shows a response to the POST method:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
The device identifier *TSensor* is required when you add your physical interface and your logical interface.

The following example shows how to use cURL to update device type *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

The following example shows a response to the POST method:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
The device identifier *TempSensor* is required when you add your physical interface and your logical interface.


## Step 7: Create a logical interface schema file
{: #step4}

The following example shows how to create a logical interface schema file that is called *thermometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "thermometerSchema",
    "description" : "Schema that defines the canonical interface for a thermometer",
    "properties" : {
        "temperature" : {
            "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## Step 8: Create a logical interface schema resource
{: #step5}

To create a logical interface schema resource, use the following API:

```
POST /draft/schemas
```

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) documentation.

The following example shows how to use cURL to create the logical interface schema:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=thermometerSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/thermometer.json"'
```

The following example shows a response to the POST method:

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "thermometerSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "thermometer.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
Use the schema identifier *5846ec826522050001db0e11* that is returned in the response to the POST method to add the logical interface schema to the logical interface.

## Step 9: Create a logical interface that references a logical interface schema
{: #step6}

To create a logical interface, use the following API:

```
POST /draft/logicalinterfaces
```

You can optionally specify a meaningful alias name for your logical interface. The alias can be referenced in the API call or topic string subscription that is used to retrieve the state of a device, instead of using the auto-generated logical interface identifier.

**Note:** The alias name must be 1 - 36 characters long and can include alphanumeric, hypen, period, underscore characters. The alias name cannot be a 24 character hex string. 

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) documentation.

In this scenario, use the schema identifier *5846ec826522050001db0e11* that was returned in the previous response to add the logical interface schema to the logical interface.

The following example shows how to use cURL to create a logical interface:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Thermometer Interface", "alias" : "IThermometer", "schemaId" : "5846ec826522050001db0e11"}'
```

The following example shows a response to the POST method:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "schemaId" : "5846ec826522050001db0e11",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846ed076522050001db0e12",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Thermometer Interface",
  "alias" : "IThermometer",
  "version" : "draft"
}
```
In this scenario, use the logical interface identifier *5846ed076522050001db0e12* that is returned in the response to the POST method to add your logical interface to your device type. You also use this identifier to map an inbound device event to a property that is defined by the logical interface. You can use the logical interface alias *IThermometer* to [retrieve the state of the device](##step13), either by using HTTP REST APIs, or by subscribing to a topic string.

## Step 10: Add the logical interface to a device type
{: #step10}

To add a logical interface to a device type, use the following API:

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

where *typeId* is the name of the device type. 

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.  
**Note:** In this scenario, the same logical interface *5846ed076522050001db0e12* is associated with *TSensor* and *TempSensor*.

The following example shows how to use cURL to add the logical interface *5846ed076522050001db0e12* associated with logical schema identifier *5846ec826522050001db0e11* to the device type *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

The following example shows a response to the POST method:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

The following example shows how to use cURL to add the logical interface *5846ed076522050001db0e12* which references logical interface schema identifier *5846ec826522050001db0e11* to the device type *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

The following example shows a response to the POST method:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

## Step 11: Define mappings to map properties in the inbound event to properties in the logical interface
{: #step11}  
**Important:** If you are using an MQTT client application and want to subscribe to receive notifications about device state updates, you need to configure notifications settings when you define mappings. By configuring your notification strategy in this way, you can reuse a logical interface across multiple device types, with each device type configured to receive state change notifications in a way of your choosing. The notification strategy can be defined regardless of whether an MQTT client subscribes to the specified topic string.  
  
The following notification settings can be configured:  

Parameter	|	Description
------	|	-----
never	|	No notifications are sent - this setting is the default
on-every-event	|	Notifications are sent on each event, regardless of whether the device state is changed
on-state-change	|	Notifications are sent only when an event results in a change of device state  

**Note:** If you select a notification setting of *never*, then your application never receives a notification about a change in device state. Therefore, you might want to choose a notification strategy of *on-every-event* or *on-state-change*.  

To map events, use the following API:

```
POST /draft/device/types/{typeId}/mappings
```

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.

In this scenario, we define mappings for device type *TSensor* to map the **t** property in the inbound event *tevt* to the **temperature** property on the logical interface. We also define mappings for device type *TempSensor* to map the **temp** property in the inbound event *tempevt* to the **temperature** property on the logical interface.  The notification settings are set to "on-state-change". 

The following example shows how to use cURL to add a mapping to device type *TSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Important:** For structured [JSON ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://json-schema.org/){:new_window} event payloads make sure that the $event.*property* entry correctly matches the JSON structure of your properties. For example, if the payload is `{"d":{"t":<temp>}}` use `$event.d.t`

Specify the logical interface identifier *5846ed076522050001db0e12* that is returned in the response to the POST method used to create the logical interface and the device type *TSensor*.

The following example shows a response to the POST method:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt": {
      "temperature": "$event.t"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```
The following example shows how to use cURL to add a mapping to device type *TempSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Specify the logical interface identifier *5846ed076522050001db0e12* that is returned in the response to the POST method used to create the logical interface and the device type *TempSensor*.
A conversion is applied to change the value from a measurement in degrees Fahrenheit to a measurement in degrees Celsius.


The following example shows a response to the POST method:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
     "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## Step 12: Validate and activate the configuration
{: #step15}

Validate and activate the configuration that is related to device state update for each device type. This configuration includes your schemas, event types, physical interfaces, logical interfaces, and mappings.

To validate and activate your device type configuration, use the following API:

```
PATCH /draft/device/types/{typeId}
```

where *typeId* is the device type identifier.

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.

In this scenario, we need to validate and activate configuration for two device types.

The following example shows how to use cURL to validate and activate your configuration for device type *TSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
The following example shows a response to the PATCH method:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TSensor"]
  },
 "failures": []
}
```

The following example shows how to use cURL to validate and activate your configuration for device type *TempSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

The following example shows a response to the PATCH method:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TempSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TempSensor"]
  },
 "failures": []
}
```

## Step 13: Publish an inbound device event
{: #step12}

Publish a temperature event from *tSensor* on topic `iot-2/evt/tevt/fmt/json` with the following example payload:
```
{
  "t" : 34.5
}
```
Publish a temperature event from *tempSensor* on topic `iot-2/evt/tempevt/fmt/json`with the following example payload:
```
{
  "temp" : 72.55
}
```

For information about publishing an inbound event from a device, see [MQTT connectivity for applications](../applications/mqtt.html#publishing_device_events).


## Step 14: Retrieve the state of the device
{: #step13}

You can retrieve the state of the device either by using HTTP REST APIs, or by subscribing to a topic string. If you specified an alias name when you created your logical interface, you can use the alias name instead of the *logicalInterfaceId* parameter to retrieve the state of the device. 

- If you have an MQTT client application, you can subscribe to the following topic string:  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- Alternatively, you can retrieve the latest device state by using the following HTTP REST API:  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

The following parameters are required:  

Parameter	|	Description
------	|	-----
typeId	|	The device type identifier.
deviceId	|	The device identifier.
logicalInterfaceId or alias|	The identifier created for the logical interface or the user-specified alias name. 


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.

The following example shows how to use cURL to retrieve the current state of *tSensor* by referencing the identifier of the logical interface that was created:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/tSensor/devices/tSensor/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

The logical interface identifier *5846ed076522050001db0e12* is used in the GET method. This identifier is returned in the response to the POST method that was used to create the logical interface.
The following example shows a response to the GET method:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
  }
}
```

The following example shows how to use cURL to retrieve the current state of *tempSensor* by referencing the alias of the logical interface that was created:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices/tempSensor/state/IThermometer \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

The logical interface alias *IThermometer* is used in the GET method. This identifier is returned in the response to the POST method that was used to create the logical interface.
The following example shows a response to the GET method:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
  }
}
```

Note that the temperature reading that is returned is in degrees Celsius and not in degrees Fahrenheit.

Your application can process this normalized data without requiring configuration to understand or convert the different temperature scales.

## Next Steps

You might want to build on this scenario to utilize the asset twin feature of data management by configuring a Thing type and Thing instance. For information about configuring Things, see [Step-by-step guide 2: A detailed example about how to work with Things through a common interface (Beta)](../information_management/im_index_scenario_thing.html).

You can also create rules that you can use to trigger an actions when an event that is received by {{site.data.keyword.iot_short_notm}}  causes a change in device or Thing state. For information about creating rules, see [Creating embedded rules (Beta)](../information_management/im_rules.html).

