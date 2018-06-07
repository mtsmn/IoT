---

copyright:
years: 2016, 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Step-by-step guide 2: A detailed example about how to work with Things through a common interface (Beta)
{: #scenario}

This scenario builds on the previous [Step-by-step guide 1: A detailed example about how to work with devices through a common interface](../GA_information_management/ga_im_index_scenario.html).

**Important:** The {{site.data.keyword.iot_full}} Things feature for data management is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

In this scenario, temperature and humidity devices publish environmental data that is collected in two meeting rooms - Meeting Room 1 and Meeting Room 2. For each meeting room, the temperature and humidity device data is separately mapped to two device type logical interfaces. 

For Meeting Room 1, the temperature device data that is associated with the *TSensor* device type is mapped to the logical interface *Thermometer Interface* and the humidity device data that is associated with the device type *HumiditySensor* is mapped to the logical interface *Hygrometer Interface*. 

For Meeting Room 2, the temperature device data that is associated with the TempSensor device type is mapped to the *Thermometer Interface*  logical interface, and the humidity device data that is associated with the device type *HumiditySensor* is mapped to the *Hygrometer Interface* logical interface. 

A Thing type called *RoomType* is then created, along with two room Thing instances: *meetingroom1* and *meetingroom2*.

This scenario sets up a composition that includes the thermometer and hygrometer logical interfaces and then maps the correct environmental device to each of the room instances, for example, *tSensor* and *humiditySensor1* are mapped to *meetingroom1*.

## Pre-requisites
{: #pre_req}

Before you continue, make sure that you:
- Use the same {{site.data.keyword.iot_full}} organization instance and an API key or token for that organization that you used in Step-by-step guide 1. For more information about API keys and tokens, see the Authentication section of the [HTTP REST API for applications](../applications/api.html#authentication) documentation.
- Have two logical interfaces configured, one for a temperature device and one for a humidity device. For information about configuring a logical interface for a temperature device, see the [Step-by-step guide 1: A detailed example about how to work with devices through a common interface](../GA_information_management/ga_im_index_scenario.html#step4) documentation. For information about configuring a logical interface for a humidity device, see [Additional information for Step-by-Step guide 2 - configuring a logical interface for a humidity device](im_hygrometer.html).

## About this task
{: #about}

In {{site.data.keyword.iot_short_notm}}, a Thing can consist of a number of devices and Things. A Thing type defines how instances of a Thing are composed. 

A logical interface is associated with a Thing type. This association defines the structure of the state that is generated for a Thing type instance. Mappings are used to define how properties from the aggregated devices and Things are mapped to properties on a Thing state.

The logical interface is used to remove the requirement for the application to understand how a device or Thing is configured. For example, you might measure the temperature of a room by using a single device, or you might calculate the room temperature by taking the average reading of a number of devices. The application requires information on the state of a room or rooms, one component of which is a temperature property. It does not matter how the temperature value that is received by the application is calculated.

In this scenario, two temperature devices and two humidity devices publish events to {{site.data.keyword.iot_short_notm}}. One temperature device and one humidity device are in Meeting Room 1 of an office block. The other temperature and humidity devices are in Meeting Room 2. The following diagram illustrates the configuration for Meeting Room 1:

![Mapping between temperature and humidity Thing and an application on {{site.data.keyword.iot_short_notm}}.](images/Information Management Thing example scenario.svg "Mapping between multiple environmental devices in one room and an application on {{site.data.keyword.iot_short_notm}}")

A Thing type called *RoomType* is used to define how instances of rooms are composed. A logical interface is associated with the *RoomType* and defines that inbound events are mapped to a single reading that shows both temperature and humidity. This single reading is the Thing state. Mappings are used to define how properties from the temperature and humidity devices are mapped to this Thing state. When a new reading is published by these devices, the value of the property that is associated with the Thing state is changed.

The following table shows the four devices that are used in our example, the topic that each device publishes on, and an example payload for each device.

Device/Type | Event | Event Payload/Property
------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**Note:** The event identifiers *tevt*, *tempevt*, *humevt* are required when you define mappings to make a property that is associated with an inbound event of that type to a property in the logical interface. In this scenario, two properties are defined in the logical interface - *temperature* and *humidity*.

A logical interface is also configured. The logical interface represents the Thing state in the following structure:
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

Use the following example scenario to set up your own interfaces environment. 

**Note:** A table listing the resource property names, values, and identifiers that are used in this guide is documented in [Resource properties and identifiers that are referenced in the Step-by-Step guides 1 and 2 documentation](im_id_reference.html).

## If needed, add a device type and a device  
{: #add_device}  
In this scenario, three device types and four device instances are used. Device instance *tSensor* is associated with device type *TSensor*. Device instance *tempSensor* is associated with device type *TempSensor*. Device instances *humiditySensor1* and *humiditySensor2* are associated with device type *HumiditySensor*.

You can create device types and devices by using the [{{site.data.keyword.iot_short_notm}} dashboard ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://internetofthings.ibmcloud.com){: new_window}, or by using REST APIs. 

For more information about using the {{site.data.keyword.iot_short_notm}} IoT Platform dashboard to add device types and devices, see the [Getting started with data management by using the Web interface](../GA_information_management/im_ui_flow.html) documentation.

For information about using REST APIs to add device types and devices, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window} documentation.



## Step 1: Create a Thing type schema file.  
{: #crt_composition_file}  
Create a Thing type schema file that references the device logical interface identifiers for the temperature and the humidity device types.  

The following example shows how to create a Thing type schema file that is called *roomTypeSchema*.   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON Schema that defines the structure of the Room Thing Type.",
    "properties" : {
        "thermometer": {
            "type": "object",
            "description": "The thermometer device",
            "$logicalInterfaceRef": "IThermometer"
        },
        "hygrometer": {
            "type": "object",
            "description": "The hygrometer device",
            "$logicalInterfaceRef": "IHygrometer"
        }
    },
    "required" : [
        "temperature",
        "humidity"
    ]
}
```
**Tip:** Use the **required** parameter to mark one or more properties as required. Required properties must be included in a device message for Watson IoT Platform to update the device state with the device data. A message that does not include the required property is not processed.   
**Note:** The schema identifier that is generated when you create a Thing type schema file must be specified when you create your Thing type.  

## Step 2: Create a Thing schema resource.  
{: #crt_composition_resource}  

Create the schema resouce by uploading the Thing type schema file that was generated in the previous step.  
Upload the Thing type schema file by using the following API:  
```
POST /draft/schemas
```  
The schema definition file is passed to the Watson IoT Platform within a multipart POST (multipart/form-data). The body of the POST must contain at least two parts:

  - One with a name of **schemaFile** that contains the actual content of the file as the body of the part.
  - One with a name of **name** that contains a string that defines the name of the schema resource in the body of the part.


For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) documentation.  

The following example shows how to use cURL to create the Thing type schema resource:  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
The following example shows a response to the POST method:
```
{
  "name" : "roomTypeSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "roomType.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5a72ea48d60180002c4f5e58",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
The schema identifier *5a72ea48d60180002c4f5e58* that is returned in response to the POST method is required when you create a Thing type.


## Step 3: Create a Thing type  
{: #crt_thing_type}  

Thing types are used to model Thing instances. A Thing type must be created in an organization before a Thing instance can be created. For this scenario, create one Thing type.  
The schema that is associated with a Thing type defines the type of objects that are aggregated together to make an instance of a Thing. The Thing type must reference the Thing type schema resource that you created in the previous step.  
Create a Thing type by using the following API:
```
POST /draft/thing/types
```
The following parameters are required in the body of the POST method:  
<table>
<tr><th>Parameter</th><th>Description</th></tr>
<tr><td>id</td><td>Provide an identifier for the Thing type that you are creating.</td></tr>
<tr><td>name</td><td>Provide a name for the Thing type that you are creating.</td></tr>
<tr><td>schemaId</td><td>The identifier created for the composition schema resource.</td></tr>
</table>

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) documentation.  

The following example shows how to use cURL to create a Thing type that is called *RoomType*.
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

Response: 
```
{
 "name": "RoomType",
 "description": "Room Thing Type",
 "id": "RoomType",
 "schemaId": "5a72ea48d60180002c4f5e58",
 "metadata": {},
  "refs": {
    "schema": "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58",
    "mappings": "/api/v0002/draft/thing/types/RoomType/mappings",
    "logicalInterfaces": "/api/v0002/draft/thing/types/RoomType/logicalinterfaces"
   },
 "version": "draft",
 "created": "2018-02-01T10:22:43Z",
 "createdBy": "ANOther",
 "updated": "2018-02-01T10:22:43Z",
 "updatedBy": "ANOther"
}
```

## Step 4: Create a logical interface schema file  
{: #crt_ai_schema_file}
In your logical interface, you can define the structure of the data that is stored as the Thing state. For this scenario, create a logical interface that defines temperature and humidity properties. Associate the logical interface with the Thing type *RoomType* by referencing the logical interface identifier that is generated when you create the logical interface resource.  
The following example shows how to create a logical interface schema file that is called *roomSchema*.

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON Schema that defines the canonical room state structure",
    "properties" : {
        "temperature" : {
            "description" : "Temperature in degrees celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        },
        "humidity" : {
            "description" : "Percentage humidity",
            "type" : "number",
            "minimum" : 0,
            "maximum" : 100,
            "default" : 0.0
        }
    },
    "required" : [
        "temperature",
        "humidity"
    ]
}
```  
## Step 5: Create a logical interface schema resource.  
{: #crt_ai_schema_resource}  
Upload the logical interface schema file that you created in the previous step to create a logical interface schema resource for your Thing type by using the following API:  
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
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
The following example shows a response to the POST method:
```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "roomSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "room.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5a4b9847d60180002efce645/content"
  },
  "id" : "5a4b9847d60180002efce645"
}
```
Use the schema identifier *5a4b9847d60180002efce645* that is returned in the response to the POST method to add the logical interface schema resource to the logical interface for your Thing type.  


## Step 6: Create a logical interface for the Thing type.  
{: #crt_thing_ai}  
The logical interface must reference the logical interface schema resource that you created in the previous step.  
To create a logical interface, use the following API:  
```
POST draft/logicalinterfaces
```  
You can optionally specify a meaningful alias name for your logical interface. The alias can be referenced in the API call or topic string subscription that is used to retrieve the state of a Thing, instead of using the auto-generated logical interface identifier.

**Note:** The alias name must be 1 - 36 characters long and can include alphanumeric, hypen, period, underscore characters. The alias name cannot be a 24 character hex string.

In this scenario, use the schema identifier *5a4b9847d60180002efce645* that was returned in the previous response to add the logical interface schema to the logical interface.

The following example shows how to use cURL to create a logical interface with the alias *IRoom*:
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
The following example shows a response to the POST method:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58"
  },
  "schemaId" : "5a72ea48d60180002c4f5e58",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5a4b9847d60180002efce645",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "IRoom",
  "alias" : "IRoom",
  "version" : "draft"
}
```
In this scenario, use the logical interface identifier *5a4b9847d60180002efce645* that is returned in the response to the POST method to add your logical interface to your device type. You also use this identifier to map an inbound device event to a property that is defined by the logical interface.

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) documentation.  


## Step 7: Add the logical interface to the Thing type.  
{: #add_thing_ai}  
To add a logical interface to a Thing type, use the following API:  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) documentation.  
In this scenario, the logical interface is associated with Thing type *RoomType*.

The following example shows how to use cURL to add the Thing logical interface *IRoom* to the Thing type *RoomType*:  
```
{   
  "id": "5a4b9847d60180002efce645"
}
```
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id": "5a4b9847d60180002efce645"}'
```

The following example shows a response to the POST method:
```
{
 "name": "Room Logical Interface",
 "description": "This is a Room logical interface",
 "id": "5a4b9847d60180002efce645",
 "schemaId": "5a4b9817d60180002efce644",
 "refs": {
   "schema": "/api/v0002/draft/schemas/5a4b9817d60180002efce644"
 },
 "version": "draft",
 "created": "2018-01-02T14:33:43Z",
 "createdBy": "ANOther",
 "updated": "2018-01-02T14:33:43Z",
 "updatedBy": "ANOther"
}
```

## Step 8: Define mappings
{: #define_Thing_type_mappings}

Define the mappings for the Thing type that describe how to map properties from the state of the aggregated devices or Things to the properties that are defined on the Thing type logical interface.

To map events, use the following API:  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

where *thingtypeId* is the identifier that is returned in the response to the POST request when the Thing type is created. 

The following parameters are required in the body of the POST request:  
<table>
<tr>
<th>	Parameter	</th><th>	Description	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	A valid JSON structure that maps properties defined for the logical interface with the properties of the device event payload.	</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	The logical interface identifer is required in the body of the payload.	</td>
</tr>
</table>  

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) documentation.

The following example shows how to use cURL to add a mapping to Thing type *RoomType*:

```
{
  "logicalInterfaceId": "5a4b9847d60180002efce645",
  "notificationStrategy": "on-state-change",
  "propertyMappings": {
       "thermometer": {
         "temperature": "$event.temperature"
       },
       "hygrometer": {
         "humidity": "$event.humidity"
       }
   },
}
```  
The *thermometer* device is defined in [roomTypeSchema](#crt_composition_file). The *$event.temperature* property is defined in the logical interface schema with the identifier *5846ed076522050001db0e12* and alias *IThermometer*.  
The *hygrometer* device is defined in the [roomTypeSchema](#crt_composition_file). The *$event.humidity* property is defined in the logical interface schema with the identifier *5846cd7c6522050001db0e24* and alias *IHygrometer*.


## Step 9: Validate and activate the configuration
{: #activate}

Validate and activate the configuration that is related to Thing state update for each Thing type. This configuration includes your schemas, logical interfaces and mappings. 

To validate and activate your Thing type configuration, use the following API:
```
PATCH /draft/thing/types/{thingTypeId}
```
where *thingTypeId* is the Thing type identifier. 

The following example shows how to use cURL to validate and activate the configuration that is associated with the Thing type *RoomType*:
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

The following example shows a response to the PATCH method:
```
{
 "message": "CUDIM0300I: State update configuration for Thing Type 'Room Thing Type' has been successfully submitted for  activation.",
 "details": {
   "id": "CUDIM0300I",
   "properties": [
     "Thing Type",
     "Room Thing Type"
   ]
 },
 "failures": []
}
```

## Step 10: Create an instance of a Thing type
{: #create_Thing_instances}

A Thing is an instance of a Thing type. A Thing enables you to aggregate one or more instances of a device or Thing together into a single object.

To create a Thing, use the following API:

```
POST /thing/types/{thingTypeId}/things
```
where *thingtypeId* is the identifier that is returned in the response to the POST request when the Thing type is created. 

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things) documentation.

In this scenario, we need to create two Thing instances that are of Thing type *RoomType*. One Thing instance is called *meetingroom1* and the other Thing instance is called *meetingroom2*.

The following example shows how to use cURL to create a Thing instance that is called *meetingroom1*. The *meetingroom1* Thing instance is associated with *tSensor* and *humiditySensor1* device instances.

```
thingId = "meetingroom1"
 meetingroom1AggregatedObjects = {
   "thermometer": {
     "type": "device",
     "typeId": "TSensor",
     "id": "tSensor"
   },
   "hygrometer": {
     "type": "device",
     "typeId": "HumiditySensor",
     "id": "humiditySensor1"
   }
 }
``` 

The Thing identifier that is created is used in the URL of the POST method that is called to add a temperature event to the Thing logical interface.

The following example shows how to use cURL to create a Thing instance that is called *meetingroom2*. *meetingroom2* is associated with the *tempSensor* and *humiditySensor2* device instances.

```
thingId = "meetingroom2"
   meetingroom2AggregatedObjects = {
    "thermometer": {
      "type": "device",
      "typeId": "TempSensor",
      "id": "tempSensor"
    },
    "hygrometer": {
      "type": "device",
      "typeId": "HumiditySensor",
      "id": "humiditySensor2"
    }
  }

``` 

## Step 11: Verify that mapped device events are published to the logical interface  
{: #publish_event}  
Publish the following events for devices that are aggregated into the Thing *meetingroom1*:  
 - a temperature event from *tSensor* on topic `iot-2/evt/tevt/fmt/json`  
 - a humidity event from *humiditySensor1* on topic `iot-2/evt/humevt/fmt/json`  
 
Publish the following events for devices that are aggregated into the Thing *meetingroom2*:  
 - a temperature event from *tempSensor* on topic `iot-2/evt/tempevt/fmt/json`  
 - a humidity event from *humiditySensor2* on topic `iot-2/evt/humevt/fmt/json`  
 
For information about publishing an inbound event from a device, see [MQTT connectivity for applications](../applications/mqtt.html#publishing_device_events).  

## Step 12: Check that the state of the Thing is changed.  
{: #verify_Thing_state}  

You can retrieve the state of the Thing either by using HTTP REST APIs, or by subscribing to a topic.

If you have an MQTT client application, you can subscribe to the following topic string:
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

Alternatively, you can retrieve the latest Thing state by using the following HTTP REST API:

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

The following parameters are required:  
<table>
<tr>
<th>Parameter	</th><th>	Description</th>
</tr>
<tr>
<td>thingTypeId	</td><td>The Thing type identifier.</td>
</tr>
<tr>
<td>thingId	</td><td>	The Thing identifier.</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>The identifier created for the logical interface.</td>
</tr>
</table>  

For more details, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) documentation.  


## Next Steps

Create rules that you can use to initiate an action when an event that is received by {{site.data.keyword.iot_short_notm}}  causes a change in device or Thing state. For information about creating rules, see [Creating embedded rules (Beta)](im_rules.html).
