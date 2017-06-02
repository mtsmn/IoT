---

copyright:
years: 2016, 2017
lastupdated: "2016-12-07"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Developing application interfaces (Beta)
{: #im_index}

**_Important: You can develop application interfaces by using the Information Management feature. The Information Management feature is currently available as a beta only. Future updates might include changes that are incompatible with the current version of this feature. Register to try it out and [let us know what you think](https://www-304.ibm.com/software/support/trial/cst/forms/nomination.wss?id=7050)._**

Use the Information Management feature of {{site.data.keyword.iot_full}} to help you to organize and integrate data coming in to and going out of {{site.data.keyword.iot_full}}. You might have different types, makes, or models of device that you want to connect to {{site.data.keyword.iot_short_notm}}, and these devices might publish data in differing formats. Use the information management feature to normalize incoming data, and simplify your applications by decoupling them from the complexities of how your specific devices are connected.

For example, you might have a number of environment sensors that are located throughout your facility. These sensors might be different makes or models and pubish events in differing formats. By using the information management feature you can keep your application simple by normalizing the inbound events and mapping them into one outbound event that can be processed your application. The normalization process is achieved by using schemas, and the mapping is achieved by using the device type and associated mapping resource.

The following diagram shows two environment sensors. One sensor records temperature and the other sensor records humidity. The temperature sensor publishes a temperature reading of { t : '34.5' } to {{site.data.keyword.iot_short_notm}}. The humidity sensor publishes a humidity reading of { h : 52% } to {{site.data.keyword.iot_short_notm}}

![Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}.](images/Information Management example th.jpg "Mapping between temperature sensor devices and an application on {{site.data.keyword.iot_short_notm}}")

The temperature and humidity readings are published as separate events to {{site.data.keyword.iot_short_notm}}. By using the Information Management feature, you can normalise these readings into a consistent form and map them to a single outbound event. Your application can process then process the data that is contained within this single outbound event, without the need to understand how the information was published to {{site.data.keyword.iot_short_notm}}.

## Overview
{: #overview}

The following diagram illustrates the logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}:

![The logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}.](images/im_mapping.png "The logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}")


To process inbound events for a specific device and convert them to a device state, you must provide the following information to {{site.data.keyword.iot_short_notm}}:

- The structure of the inbound events. This information is required by the physical interface.
- The structure of the desired device state. This information is required by the application interface.
- Information about how to map the inbound events to the preferred device state. This information is required by the mappings resource.

## Data flow between devices and applications
{: #mapping}

The following flow diagram shows how the different resources within the Information Management feature are used:

![The flow between a device and an application on {{site.data.keyword.iot_short_notm}}.](images/Information Management flow.jpg "The flow between a device and an application on {{site.data.keyword.iot_short_notm}}")

The event data passes through a number of different resouces that can be managed by using REST APIs. These resources are defined in [Key concepts](https://console.ng.bluemix.net/docs/services/IoT/information_management/im_index.html#key_concepts).

The following diagram illustrates how schemas are used within this flow:

![The flow between a device and an application on {{site.data.keyword.iot_short_notm}}.](images/Information Management schema.jpg "The flow between a device and an application on {{site.data.keyword.iot_short_notm}}")

These JSON schemas are used to define and validate incoming and outgoing data. For more information about the schemas that are used in Information Management, see [Schemas](im_index.html#scenario).


## Key concepts
{: #key_concepts}

The following resources are used within the Information Management feature. You can manage the resources that are illustrated in the previous diagrams by using REST APIs. For information about the Information Management REST APIs, see the Information Management section in the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

For examples in cURL that use these REST APIs to configure and manage resources, see [End-to-end scenario](im_index.html#scenario).


## Schemas
{: #schema}
JSON schemas are used to define the structure of the payload of inbound events that are published to the {{site.data.keyword.iot_short_notm}} from devices, and the state of the device. For more information about JSON Schema, see [JSON Schema](http://json-schema.org/).

In Information Management, two JSON schemas are referenced - event schemas and application interface schemas.

Event schemas are used by the physical interface and define the structure of the events that are published to {{site.data.keyword.iot_short_notm}} by a device. Incoming events are validated against the event schema. For example, you might want to create an event schema that validates that the inbound event is in JSON format, contains two fields, and that the values associated with these fields are numeric.

Application interface schemas are used to define the structure of the device state that is stored by {{site.data.keyword.iot_short_notm}}. The device state is validated against the application interface schema.

## Event type
{: #event_type}

Before you start using event types, you must create an event type within your organization so that {{site.data.keyword.iot_short_notm}} can process the data that is contained within a specific event. All event types must reference an event schema. For the beta, all inbound events must be in JSON format if you want to use the Information Management feature.

## Physical interface
{: #physical_interface}

Devices use the physical interface to publish events to {{site.data.keyword.iot_short_notm}}. The physical interface is associated with one or more event types. For example, you might have a two devices publishing events to {{site.data.keyword.iot_short_notm}}. One device is a temperature sensor and the other devices monitors humidity. These devices publish temperature and humidity readings to {{site.data.keyword.iot_short_notm}} by using a physical interface. In this scenario, you might want to associate your physical interface with two event types - one event type that represents temperature, and another event type that represents humidity.

The physical interface maps these inbound events to different event types by using an event identifier. The event identifier corresponds to the MQTT topic that the event is published to.

The following topic provides an example of an event that has an event ID of '1234':
```
iot-2/evt/1234/fmt/json.
```

### Application interface
{: #application_interface}

The application interface defines the structure of the state that is stored for a device and validates the state that is generated when an inbound event is processed. The device state is a set of properties that are described by the application interface or interfaces.
The application interface must associated with a device type and must reference an application interface schema which defines the properties of the application interface.

### Device type and mapping
{: #device_type}
Use the device type to define how properties from inbound events are mapped to properties on the device state.

Every device that is connected to the Watson IoT Platform is associated with a device type. Device types are groups of devices that share characteristics or behaviors. In information management, the device type is extended to reference the physical interface for the device and to reference the application interface that is used to retrieve the device state. For example, you might have a device publishing temperature readings to {{site.data.keyword.iot_short_notm}} and another device publishing humidity readings to {{site.data.keyword.iot_short_notm}}. These event types can be mapped to a schema that is associated with an application interface to create a single outbound event that contains information about temperature and humidity. This single outbound event is validated by using the application interface schema, and can then be processed by your application.

A device type can expose multiple application interfaces. For more information about device types, see the "Identifiers and device types" section in [Device Model](https://console.ng.bluemix.net/docs/services/IoT/reference/device_model.html#id_and_device_types).


## Getting started with developing application interfaces
{: #workflow}

After you have set up an instance of Watson IoT Platform in your Bluemix organization and registered and connected some devices, you can start using the Information Management feature. For more information about getting started with {{site.data.keyword.iot_short_notm}}, see [Getting started with Watson IoT Platform](https://console.stage1.ng.bluemix.net/docs/services/IoT/index.html).

Ensure that you have registered for a {{site.data.keyword.Bluemix_notm}} account and created an instance of the {{site.data.keyword.iot_short_notm}} service in your {{site.data.keyword.Bluemix_notm}} organization. You also need to register your device or devices. After registering, you will receive a 6 character organization ID.  This ID is required in the hostname for any API calls. The base URL for your organization can be accessed at the following address: [https://**orgId**.internetofthings.ibmcloud.com/api/v0002/](https://**orgId**.internetofthings.ibmcloud.com/api/v0002/).

Use the following high-level steps to help you to start using the Information Management feature. For more detailed information about each of the following steps, see [End-to-end scenario](im_index.html#scenario).

1. Create an event schema for your event type.
2. Create an event type and link the event type to your event schema.
2. Create a physical interface and link your event type to your physical interface.
3. Create an application interface schema.  
4. Create an application interface and link the application interface to your application interface schema.
5. Create a device type to connect your application interface and your physical interface.
6. Define mappings to link your inbound event to a property or properties that are in your application interface.
7. Publish a device event and check that the state of the device is changed.


## End-to-end scenario
{: #scenario}

Use the following scenario which is written in cURL to help you to create an inbound event in JSON format to turn a switch on or off. The inbound event triggers a change of state in a property that is defined on the application interface. The change of state causes a lightbulb to be turned on or off.

### Create an event schema
Create an event schema to define the structure of the inbound event. In this scenario, the inbound event is a boolean value that can be set to either "on" or "off".

To create an event schema, use the following API:

```
{
POST /schemas/{schemaId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to create an event schema:

```
{
logger.info("# ---- add an event schema -------")
url = "/api/v0002/schemas"
schemaFile = json.dumps({ "type" : "object", "properties" : { "on" : { "type" : "boolean" } } })
body = {"name" : "onoroff", "description" : "on or off event", "schemaType": "json-schema", \
             "schemaFile" : schemaFile, "schemaFileName" : "onoroff.json", "Content-Type":"application/json"}
    result, body = http.post(url, body)
    self.assertEqual(result, 201)
    loaded = json.loads(body)
    self.assertEqual(loaded["contentType"], "application/json")
    ids["onoroffevent"] = loaded["id"]
  }
  ```

### Create an event type
Create an event type. Use the event schema identifier that is returned in the response when the event schema was created.

To create an event type, use the following API:

```
{
POST /event/types/{eventTypeId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to create an event type:

```
{
  logger.info("# ---- add an event type -------")
      url = "/api/v0002/event/types"
      body = {"name" : "onoroff", "description" : "light bulb on or off", "schemaId" : ids["onoroffevent"]}
      result, body = http.post(url, body)
      self.assertEqual(result, 201)
      loaded = json.loads(body)
      ids["onoroffeventtype"] = loaded["id"]
  }
  ```

### Create a physical interface
Add your physical interface. Use the identifier that you receive in the response to link your physical interface to your event type.

To create a physical interface, use the following API:

```
{
POST /physicalinterfaces/{physicalInterfaceId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to create a physical interface:

  ```
  {
    logger.info("# ---- add a physical interface -------")
    url = "/api/v0002/physicalinterfaces"
    body = {"name" : "Siemens light 1", "description" : "The physical interface for a Siemens light bulb"}
    result, body = http.post(url, body)
    self.assertEqual(result, 201)
    loaded = json.loads(body)
    ids["physicalinterface"] = loaded["id"]
    }
    ```

### Link your event type
- Link your event type to your physical interface by using the physical interface identifier.

To link your event type to your physical interface, use the following API:

```
{
POST /physicalinterfaces/{physicalInterfaceId}/events/{eventId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to link an event type to a physical interface:

    ```
    {
      logger.info("# ---- add the event type to the physical interface -------")
    url = "/api/v0002/physicalinterfaces/%s/events" % (ids["physicalinterface"], )
    body = {"eventId" : "onoroff", "eventTypeId" : ids["onoroffeventtype"]}
    result, body = http.post(url, body)
      }
      ```

### Create an application interface schema
Create your application interface schema. Use the application interface schema identifier that is returned in the response to link your application interface to the application interface schema.  

To create your application interface schema, use the following API:

```
{
POST /applicationinterfaces/{applicationInterfaceId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to create an application interface schema:

```
{
 logger.info("# ---- add an application interface schema -------")
 url = "/api/v0002/schemas"
 schemaFile = json.dumps({ "type" : "object", "properties" : { "state" : { "type" : "string", "enum" : ["on", "off"] } } })
 body = {"name" : "basic light", "description" : "basic light", "schemaType": "json-schema", \
       "schemaFile" : schemaFile, "schemaFileName" : "basiclight.json", "Content-Type":"application/json"}
 result, body = http.post(url, body)
 self.assertEqual(result, 201)
 loaded = json.loads(body)
 self.assertEqual(loaded["contentType"], "application/json")
 ids["basiclight schema"] = loaded["id"]
    }
      ```

### Create an application interface
Add your application interface. Link your application interface to your application interface schema by using the application interface schema identifier.

To add an application interface and link it to your application interface schema, use the following API:

```
{
POST /applicationinterfaces/{applicationInterfaceId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to add an application interface and link it to an application interface schema:

```
{
  logger.info("# ---- add an application interface -------")
    url = "/api/v0002/applicationinterfaces"
    body = {"name" : "basic light application interface", "description" : "basic light application interface", "schemaId" : ids["basiclight schema"]}
    result, body = http.post(url, body)
    self.assertEqual(result, 201)
    loaded = json.loads(body)
    ids["basiclight app interface"] = loaded["id"]
  }
  ```

### Create a device type
Add a device type. Link the device type to a physical interface by using the identifier that was returned in the response received when you created the physical interface.

To add a device type and link it to your physical interface, use the following API:

```
{
POST /device/types/{typeId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to add a device type and link it to a physical interface:

```
{
  logger.info("# ---- add a device type with a physical interface -------")
    url = "/api/v0002/device/types"
    devicetypename = "Siemens light"
    body = {"id" : devicetypename, "description" : "Siemens light", "deviceInfo":{ "serialNumber" : "1234"}, "physicalInterfaceId" : ids["physicalinterface"]}
    result, body = http.post(url, body)
    self.assertEqual(result, 201)
  }
```

### Link your application interface
Link the application interface to the device type by using the identifier that was returned in the response that is received when the application interface is created.

To link an application interface to a device type, use the following API:

```
{
POST /device/types/{typeId}/applicationinterfaces/{applicationInterfaceId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to link an application interface to a device type:


```
{
  logger.info("# ---- associate application interface to the device type -------")
    url = "/api/v0002/device/types/%s/applicationinterfaces" % (devicetypename, )
    body = {"applicationInterfaceId" : ids["basiclight app interface"]}
    result, body = http.post(url, body)
    self.assertEqual(result, 200)
  }
```

### Define mappings
- Create the property mappings for an application interface for a device type.

To create property mappings for an application interface for a device type, use the following API:

```
{
POST /device/types/{typeId}/mappings/{applicationInterfaceId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to create property mappings for an application interface for a device type:

```
{
  logger.info("# ---- add mappings to the device type -------")
    url = "/api/v0002/device/types/%s/mappings" % (devicetypename, )
    body = {"applicationInterfaceId" : ids["basiclight app interface"], "alias" : "basic_light",
            "propertyMappings" : {
              # eventid -> { property -> eventid property expression }
              "onoroff" :  { "state" : "onoroff[\'on\']" },
            }
        }
    result, body = http.post(url, body)
    self.assertEqual(result, 201)
  }
```
- Map an event id to a specific event type for the specified physical interface.

To map an event to your physical interface, use the following API:

```
{
POST /physicalinterfaces/{physicalInterfaceId}/events/{eventId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to map an event to a physical interface:

```
{
  logger.info("# ---- add mappings to the device type -------")
    url = "/api/v0002/device/types/%s/mappings" % (devicetypename, )
    body = {"applicationInterfaceId" : ids["basiclight app interface"], "alias" : "basic_light",
            "propertyMappings" : {
              # eventid -> { property -> eventid property expression }
              "onoroff" :  { "state" : "onoroff[\'on\']" },
            }
        }
    result, body = http.post(url, body)
    self.assertEqual(result, 201)
  }
```

## Add a device

To add a device, use the following API:

```
{
POST /device/types/{typeId}/devices
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Devices/post_device_types_typeId_devices) documentation.

The following example uses cURL to add a device:

```
{
  logger.info("# ---- add a device -------")
    url = "/api/v0002/device/types/Siemens light/devices"
    body = {"deviceId" : "light1", "deviceInfo":{}, "location":{}, "metadata":{}}
    result = http.post(url, body)
  }
```

## Publish an inbound event from a device

Publish a device event to {{site.data.keyword.iot_short_notm}}.

The following example uses cURL to publish an inbound event that turns a lightbulb off:


```
{
  logger.info("# ---- publish a device event ----")
    topic = "iot-2/type/%s/id/%s/evt/%s/fmt/%s" % ("Siemens light", "light1", "onoroff", "json")
    event = '{"on": false}'
    mqtt.app_publish(topic, event)
  }
```

## Query the device state

Get the state of the device.

To query the current state of a device, use the following API:

```
{
GET /device/types/{typeId}/devices/{deviceId}/state/{applicationInterfaceId}
}
```
For details of the request schema, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Information_Management) documentation.

The following example uses cURL to query the state of the device:

```
{
logger.info("# get the state of the device")
    url = "/api/v0002/device/types/%s/devices/%s/state" % ("Siemens light", "light1")
    result = http.get(url)
    self.assertEqual(result[0], 200)
    self.assertEqual(json.loads(result[1]), {"basic_light": {"state": False}})
  }
```
