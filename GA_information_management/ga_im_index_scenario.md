---

copyright:
years: 2016, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Step-by-step guide 1: A detailed example about how to work with devices through a common interface
{: #scenario}

Use the following information to create a scenario in which two temperature sensors publish events to {{site.data.keyword.iot_full}}. One sensor measures temperature in degrees Celsius. The other sensor measures temperature in degrees Fahrenheit. These readings are mapped to a single temperature reading that is in degrees Celsius. When a new temperature reading is published by these devices, the value of the property associated with the device state is changed.
{: shortdesc}

## Pre-requisites

You must have a {{site.data.keyword.iot_short_notm}} organization instance and an API key or token for that organization.

## About this task

In this scenario, two devices are configured.

One device is called *TemperatureSensor1*. This device publishes temperature events that are measured in degrees Celsius. The temperature event is published on the topic `iot-2/evt/tevt/fmt/json` and has the following example payload:
```
{
  "t" : 34.5
}
```

**Note:** The event identifier is *tevt*. This identifier is required when you add a temperature event of this type to the physical interface and when you define mappings to map a property associated with an inbound event of this type to a property in your logical interface. In this scenario, the property defined in the logical interface is called **temperature**.

The other device is called *TemperatureSensor2*. This device publishes temperature events that are measured in degrees Fahrenheit. The temperature event is published on the topic `iot-2/evt/tempevt/fmt/json` and has the following example payload:
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

![Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}.](images/Information Management Device example.svg "Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}")  

Use the following example scenario to set up your own interfaces environment.

## If needed, add a Device Type and a Device
{: #step14}

In this scenario, two device types and two device instances are assumed. Device instance *TemperatureSensor1* is associated with device type *EnvSensor1*. Device instance *TemperatureSensor2* is associated with device type *EnvSensor2*.

For information about using REST APIs to add a device type, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html) documentation.

## Step 1: Create a draft event schema file
{: #step1}

For this scenario, create two draft event schema files to define the structure of each of the inbound temperature events.

The following example shows how to create a draft schema file that is called *tEventSchema.json*. This file defines the structure of an inbound event from a temperature sensor that measures temperature in degrees Celsius:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor1 tEvent Schema",
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
**Tip:** Use the “required” parameter to mark one or more properties as required. Required properties must be included in a device message for {{site.data.keyword.iot_short_notm}} to update the device state with the device data. A message that does not include the required property is not processed.   

The schema file name *tEventSchema* is used when you create an event schema resource for your event type.

The following example shows how to create a draft schema file that is called *tempEventSchema.json*. This file defines the structure of an inbound event from a temperature sensor that measures temperature in degrees Fahrenheit:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor2 tempEvent Schema",
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

## Step 2: Create a draft event schema resource for your event type
{: #step2}

To create an event schema resource, use the following API:

```
POST /draft/schemas
```

The following parameters are required:  

Parameter	|	Description  
------	|	-----  
name	|	Provide a name for the event schema that you are creating.  
schemaFile	|	Path to the local event schema JSON file.  



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

## Step 3: Create a draft event type that references the event schema
{: #step3}

Each event type references the relevant event schema that was created in the previous example by using the schema identifier returned in the response to the POST method used to create the event schema resource.

To create a draft event type, use the following API:

```
POST /draft/event/types
```

The following parameters are required:  

Parameter	|	Description
------	|	-----
name	|	Provide a name for the event type that you are creating.
schemaId	|	The identifier created for the event schema resource.


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

## Step 4: Create a draft physical interface
{: #step7}

To create a draft physical interface, use the following API:

```
POST /draft/physicalinterfaces
```

The following parameters are required:  

Parameter	|	Description
------	|	-----
name	|	Provide a name for the physical interface that you are creating.


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) documentation.

In this scenario, we need two physical interfaces - one for each event type.

The following example shows how to use cURL to create the first physical interface:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Env sensor physical interface 1"}'
```

The following example shows a response to the POST method:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "Env sensor physical interface 1",
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
  --data '{"name" : "Env sensor physical interface 2"}'
```

The following example shows a response to the POST method:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

The physical interface identifier *5847d1df6522050001db0e1b* that is returned in the response is used in the URL of the POST method that is called to add a temperature event that is measured in degrees Fahrenheit to the physical interface.   

## Step 5: Add the event type to the draft physical interface
{: #step8}

To add an event type to your physical interface, use the following API:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

The following parameters are required:  

Parameter	|	Description
------	|	-----
eventId	|	Enter the event name from your device event payload.
eventTypeId	|	The identifier created for the event type.


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

## Step 6: Update the draft device type to connect the draft physical interface
{: #step9}

To update a draft device type, use the following API:

```
POST /draft/device/types/{typeId}/physicalinterface
```

where *typeId* is the device type identifier.


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.

In this scenario, device type *EnvSensor1* is updated to connect to physical interface *5847d1df6522050001db0e1a* and device type *EnvSensor2* is updated to connect to physical interface *5847d1df6522050001db0e1b*.

The following example shows how to use cURL to update device type *EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/physicalinterface \
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
  "name" : "Env sensor physical interface 1",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
The device identifier *EnvSensor1* is required when you add your physical interface and your logical interface.

The following example shows how to use cURL to update device type *EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/physicalinterface \
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
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
The device identifier *EnvSensor2* is required when you add your physical interface and your logical interface.



## Step 7: Create a draft logical interface schema file
{: #step4}

The following example shows how to create a draft logical interface schema file that is called *envSensor.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Environment Sensor Schema",
    "description" : "Schema to represent a canonical environment sensor device",
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

## Step 8: Create a draft logical interface schema resource
{: #step5}

To create a draft logical interface schema resource, use the following API:

```
POST /draft/schemas
```

The following parameters are required:  

Parameter	|	Description
------	|	-----
name	|	Provide a name for the logical interface schema that you are creating.
schemaFile	|	Path to the local logical interface schema JSON file.


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) documentation.

The following example shows how to use cURL to create the logical interface schema:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=temperatureEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/envSensor.json"'
```

The following example shows a response to the POST method:

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "temperatureEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "envSensor.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
Use the schema identifier *5846ec826522050001db0e11* that is returned in the response to the POST method to add the logical interface schema to the logical interface.

## Step 9: Create a draft logical interface that references a draft logical interface schema
{: #step6}

To create a draft logical interface, use the following API:

```
POST /draft/logicalinterfaces
```

The following parameters are required:  

Parameter	|	Description
------	|	-----
name	|	Provide a name for the logical interface that you are creating.
schemaId	|	The identifier created for the logical interface schema resource.

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
  --data '{"name" : "environment sensor interface", "schemaId" : "5846ec826522050001db0e11"}'
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
  "name" : "environment sensor interface",
  "version" : "draft"
}
```
In this scenario, use the logical interface identifier *5846ed076522050001db0e12* that is returned in the response to the POST method to add your logical interface to your device type. You also use this identifier to map an inbound device event to a property that is defined by the logical interface.

## Step 10: Add the draft logical interface to a device type
{: #step10}

To add a draft logical interface to a device type, use the following API:

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

where *typeId* is the name of the device type. 

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.  
**Note:** In this scenario, the same logical interface *5846ed076522050001db0e12* is associated with *EnvSensor1* and *EnvSensor2*.

The following example shows how to use cURL to add the logical interface *5846ed076522050001db0e12* which references logical interface schema identifier *5846ec826522050001db0e11* to the device type *EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/logicalinterfaces \
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
  "name" : "environment sensor interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

The following example shows how to use cURL to add the logical interface *5846ed076522050001db0e12* associated with logical schema identifier *5846ec826522050001db0e11* to the device type *EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/logicalinterfaces \
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
  "name" : "environment sensor interface",
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

The following parameters are required:  

Parameter	|	Description
------	|	-----
logicalInterfaceId	|	The identifier that is created for the logical interface.
propertyMappings	|	A valid JSON structure that maps properties that are defined for the logical interface with the properties of the device event payload.  
You can optionally set the *notificationStrategy* parameter.  

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.

In this scenario, we define mappings for device type *EnvSensor1* to map the **t** property in the inbound event *tevt* to the **temperature** property on the logical interface. We also define mappings for device type *EnvSensor2* to map the **temp** property in the inbound event *tempevt* to the **temperature** property on the logical interface.  The notification settings are set to "on-state-change". 

The following example shows how to use cURL to add a mapping to device type *EnvSensor1*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Important:** For structured [JSON ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://json-schema.org/){:new_window} event payloads make sure that the $event.*property* entry correctly matches the JSON structure of your properties. For example, if the payload is `{"d":{"t":<temp>}}` use `$event.d.t`

Specify the logical interface identifier *5846ed076522050001db0e12* that is returned in the response to the POST method used to create the logical interface and the device type *EnvSensor1*.

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
The following example shows how to use cURL to add a mapping to device type *EnvSensor2*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Specify the logical interface identifier *5846ed076522050001db0e12* that is returned in the response to the POST method used to create the logical interface and the device type *EnvSensor2*.
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

Validate and activate the configuration that is related to device state update for each draft device type. This configuration includes your draft schemas, event types, physical interfaces, logical interfaces, and mappings.

To validate and activate your draft device type configuration, use the following API:

```
PATCH /draft/device/types/{typeId}
```

where *typeId* is the device type identifier.

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) documentation.

In this scenario, we need to validate and activate configuration for two draft device types.

The following example shows how to use cURL to validate and activate your configuration for draft device type *EnvSensor1*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

The following example shows a response to the PATCH method:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor1' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor1"]
  },
 "failures": []
}
```
The following example shows how to use cURL to validate and activate your configuration for draft device type *EnvSensor2*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
The following example shows a response to the PATCH method:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor2' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor2"]
  },
 "failures": []
}
```

## Step 13: Publish an inbound device event
{: #step12}

Publish a temperature event from *TemperatureSensor1* on topic `iot-2/evt/tevt/fmt/json` with the following example payload:
```
{
  "t" : 34.5
}
```
Publish a temperature event from *TemperatureSensor2* on topic `iot-2/evt/tempevt/fmt/json`with the following example payload:
```
{
  "temp" : 72.55
}
```

For information about publishing an inbound event from a device, see [MQTT connectivity for applications](../applications/mqtt.html#publishing_device_events).


## Step 14: Retrieve the state of the device
{: #step13}

You can retrieve the state of the device either by using HTTP REST APIs, or by subscribing to a topic.

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

The following example shows how to use cURL to retrieve the current state of *TemperatureSensor1* by referencing the identifier of the logical interface that was created:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor1/devices/TemperatureSensor1/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

The logical interface identifier *5846ed076522050001db0e12* is used in the GET method. This identifier is returned in the response to the POST method that was used to create the logical interface.
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
The following example shows how to use cURL to retrieve the current state of *TemperatureSensor2* by referencing the identifier of the logical interface that was created:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor2/devices/TemperatureSensor2/state/5846ed076522050001db0e12 \
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
Note that the temperature reading that is returned is in degrees Celsius and not in degrees Fahrenheit.

Your application can process this normalized data without requiring configuration to understand or convert the different temperature scales.


