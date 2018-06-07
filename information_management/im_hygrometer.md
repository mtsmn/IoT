---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Additional information for Step-by-Step guide 2 - configuring a logical interface for a humidity device

Use the following information to create a scenario in which sensors on two humidity devices publish events to IBM Watson™ IoT Platform.  Readings from the humidity devices are mapped to a single humidity reading. One device is located in meeting room 1, and the other device is located in meeting room 2.


## About this task

Use the same {{site.data.keyword.iot_short_notm}} organization instance and an API key or token for that organization that you used in [Step-by-step guide 1](../GA_information_management/ga_im_index_scenario.html). 

This configuration is a pre-requisite to the tutorial described in the [Step-by-Step guide 2](im_index_scenario_thing.html) documentation.

In this scenario, one device type and two device instances are created.

The device instances are called *humiditySensor1* and *humiditySensor2*. The devices publish humidity events. The humidity event has the following example payload:
```
{
  "hum" : 35
}
```
Humidity events are published on the topic `iot-2/evt/humevt/fmt/json`. 

**Note:** The event identifier is *humevt*. This identifier is required when you add a humidity event of these types to the physical interface and when you define mappings to map a property associated with an inbound event of this type to a property in your logical interface. In this scenario, the property defined in the logical interface is called **humidity**. This logical interface represents the state for devices of this type in the following structure:
```
{
  "humidity" : <current humidity value in Celsius>
}
```

Use the following example scenario to set up your own interfaces environment.

**Important Note:** You must save the IDs that are generated in the curl responses as the IDs are required to complete other tasks.

## If needed, add a Device Type and a Device
{: #step14}

In this scenario, one device type and two device instances are assumed. Device instances *humiditySensor1* and *humiditySensor2* are associated with device type *HumiditySensor*. 

You can create device types and devices by using the [{{site.data.keyword.iot_short_notm}} dashboard ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://internetofthings.ibmcloud.com){: new_window}, or by using REST APIs. For more information about using the {{site.data.keyword.iot_short_notm}} IoT Platform dashboard to add device types and devices, see the [Getting started with data management by using the Web interface](../GA_information_management/im_ui_flow.html) documentation.

The following example shows how to create a device type that is called *HumiditySensor* by using the REST API:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**Important Note:** You can add metadata when you create a device type and device. In this scenario, the following metadata is added to the device type *HumiditySensor*:
```
{
    "maxHumidityThreshold": 70
}
```
This metadata can be used when [creating rules](im_rules.html) that trigger if a humidity event that causes the *humidity* property of the device state to exceed a value of 70% is received by {{site.data.keyword.iot_short_notm}} from *humiditySensor1* or *humiditySensor2*. 

## Register the devices

The following example shows how to register a device instance that is called *humiditySensor1*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
The following example shows how to register a device instance that is called *humiditySensor2*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**Note:** When a device makes an HTTP request through the Watson IoT Platform HTTP REST API, a user name and password is required. The password is the value of the authentication token that is either automatically generated or manually specified when a device is registered. If you are using an MQTT client, you must keep a note of the authentication token of your device as you need the token to retrieve device or Thing state by subscribing to a IoT topic string.

## Step 1: Create an event schema file
{: #step1}

For this scenario, create an event schema file to define the structure of the inbound humidity events.

The following example shows how to create a schema file that is called *humEventSchema.json*. This file defines the structure of an inbound event from a humidity device:

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "defines the structure of a humidity event",
    "properties" : {
        "humidity" : {
            "description" : "Percentage humidity",
            "type" : "number",
            "minimum" : 0,
            "maximum" : 100,
            "default" : 0.0
        }
    },
    "required" : [
      "humidity"
    ]
}
```
**Tip:** Use the **required** parameter to mark one or more properties as required. Required properties must be included in a device message for {{site.data.keyword.iot_short_notm}} to update the device state with the device data. A message that does not include the required property is not processed.   


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

The following example shows how to use cURL to create the event schema resource *humEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**Note:** The example authorization value `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` consists of the following information:
`{API Key}:{authorization token}`,  which is then Base64 encoded.

The following example shows a response to the POST method:

```
{
  "name" : "humEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "humEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e20",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
The schema identifier *5846cd7c6522050001db0e20* that is returned in response to the POST method is required when you add an event schema to your event type.



## Step 3: Create an event type that references the event schema
{: #step3}

Each event type references the relevant event schema that was created in the previous example by using the schema identifier returned in the response to the POST method used to create the event schema resource.

To create an event type, use the following API:

```
POST /draft/event/types
```
The draft event type must reference the schema definition that defines the structure of the inbound MQTT event.


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) documentation.


The following example shows how to use cURL to create an event type for a humidity event:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

The schema identifier *5846cd7c6522050001db0e20* is used to add the event schema to the event type. This identifier was returned in response to the POST method that was used to create the event schema resource *humEventSchema.json*

The following example shows a response to the POST method:

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e20",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20"
  },
  "name" : "humEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e21",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

The event type identifier *5846cd7c6522050001db0e21* that is returned in response to the POST method is used to add an event type to the physical interface.


## Step 4: Create a physical interface
{: #step7}

To create a physical interface, use the following API:

```
POST /draft/physicalinterfaces
```

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) documentation.

In this scenario, we need one physical interface for the event type that we created earlier.

The following example shows how to use cURL to create the physical interface:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

The following example shows a response to the POST method:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

The physical interface identifier *5846cd7c6522050001db0e22* that is returned in the response is used in the URL of the POST method that is called to add a temperature event that is measured in degrees Celsius to the physical interface.


## Step 5: Add the event type to the physical interface
{: #step8}

To add an event type to your physical interface, use the following API:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) documentation.

In this scenario, the following event type is added to the specified phyiscal interfaces:
- the humidity event *humevt* is added to the physical interface with identifier *5846cd7c6522050001db0e22* by using the *eventId* from the topic and the *eventTypeId* from the creation of the event schema resource.

The following example shows how to use cURL to add the humidity event *humevt* to the physical interface with the identifier *5846cd7c6522050001db0e22* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

The following example shows a response to the POST method:

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
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

In this scenario, device type *HumiditySensor* is updated to connect to physical interface *5846cd7c6522050001db0e22*.

The following example shows how to use cURL to update device type *HumiditySensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

The following example shows a response to the POST method:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
The device identifier *HumiditySensor* is required when you add your physical interface and your logical interface.
  


## Step 7: Create a logical interface schema file
{: #step4}

The following example shows how to create a logical interface schema file that is called *hygrometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema that defines the canonical interface for a hygrometer",
    "properties" : {
        "humidity" : {
            "description" : "percentage humidity",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## Step 8: Create a logical interface schema resource
{: #step5}
The following example shows how to use cURL to create a logical interface:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
The following example shows a response to the POST method:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometer",
  "version" : "draft"
}
```

## Step 9: Create a logical interface that references a logical interface schema
{: #step6}

To create a logical interface, use the following API:

```
POST /draft/logicalinterfaces
```

You can optionally specify a meaningful alias name for your logical interface. The alias can be referenced in the API call or topic string subscription that is used to retrieve the state of a device, instead of using the auto-generated logical interface identifier.

**Note:** The alias name must be 1 - 36 characters long and can include alphanumeric, hypen, period, underscore characters. The alias name cannot be a 24 character hex string. 

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) documentation.

In this scenario, use the schema identifier *5846cd7c6522050001db0e23* that was returned in the previous response to add the logical interface schema to the logical interface.

The following example shows how to use cURL to create a logical interface:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

The following example shows a response to the POST method:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometermometer",
  "version" : "draft"
}
```
In this scenario, use the logical interface identifier *5846cd7c6522050001db0e24* that is returned in the response to the POST method to add your logical interface to your device type. You also use this identifier to map an inbound device event to a property that is defined by the logical interface. You can use the logical interface alias *IHygrometer* to retrieve the state of the device, either by using HTTP REST APIs, or by subscribing to a topic string.


## Step 10: Add the logical interface to the device type

The following example shows how to use cURL to add the logical interface 5846cd7c6522050001db0e24 which references logical interface schema identifier 5846cd7c6522050001db0e23 to the device type HumiditySensor:
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
The following example shows a response to the POST method:
```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Hygrometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846cd7c6522050001db0e24",
  "schemaId" : "5846cd7c6522050001db0e23"
}
```

## Step 11: Add the mappings to the device type

    
The following example shows how to use cURL to add a mapping to device type *HumiditySensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

The following example shows a response to the POST method:
```
{
  "logicalInterfaceId": "5846cd7c6522050001db0e24",
  "propertyMappings": {
    "humevt": {
      "humidity": "$event.hum"
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

## Step 12: Activate the configuration for the device type


    
The following example shows how to use cURL to validate and activate your configuration for device type *HumiditySensor*:
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
The following example shows a response to the PATCH method:
```
{
 "message": "CUDRS0520I: State update configuration for device type 'HumiditySensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["HumiditySensor"]
  },
 "failures": []
}
```
