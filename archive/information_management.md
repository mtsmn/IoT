---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-27"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Overview
{: #overview}

Use the information management component of the {{site.data.keyword.iot_full}} to help you to organize and integrate data coming in to and going out of the {{site.data.keyword.iot_short_notm}} . The information management component lets you
- Parse and process data that is in different formats.
- Filter out unwanted data.
- Decouple your applications from the complexities of how specific devices are connected.

Information management is currently available as a Beta release.


## Architecture overview
{: #architecture}

<insert diagram here>
maybe explain the key areas?

To process inbound events for a specific device and convert these events to device state, you must provide the following information to the {{site.data.keyword.iot_full}}:
1. The structure of the inbound events. This information is required for the physical interface resource.
2. The structure of the desired device state. This information is required for the application interface resource.
3. Information about how to map the inbound events to the desired device state. This information is required for the mappings resource.


## Key concepts
{: #concepts}
<quick blurb here about resources> I am not clear on the schema and event type and relationships between them and the platform and the other resources listed. also what does reference mean. difference between logical and resource view. think i need to understand the link between the 2 concepts of logical and resource. <try looking at UI to see if this helps and also look at rest api so i can link to that from each resource type.

need a note about how you have a UID returned when you creat a schema by using rest. so you can have schemas with the same name.

You can Create, Rename,  Update, Delete an interfaces / schemas within your organization in the Watson iot plaform. You can can add a Device  Type physical  or application  interfaces / schemas , event type schemas, and command type schemas.
You provision your devices and then define your interfaces.
There are 2 interfaces - physical and application. There are 3 schemas - event type, command type nad property. You add these schemas and interfaces. Interfaces and schemas are used by device types.
Retrieve the property mappings for a specific application interface for the device type.
Retrieve the details of the physical interface with the specified id
Retrieve the details of the application interface with the specified id
Retrieve the details of the event type with the specified id
Retrieve the metadata for a specific Schema



### Schema resources

Use schemas to define the structure of inbound events and the state of the device or thing. Event schemas are used to define the structure of the events that are published to the platform by a device. Device or thing schemas are used to define the
structure of the state that is stored by the platform. For the Beta release, only the JSON-schema type is supported.

### Event type resources

Use event type to .... The events that are published to the platform by a device can be in any format. For example, JSON, CBOR, XML, or Binary. Event types are used to model these events within the platform. Before you start using event types, you must create an event type within your organization so that the platform can process the data that is contained within a specific event.

An Event Type contains all the metadata that is associated with a single event. For Beta, the inbound event must be in JSON format. the event type references a JSON schema and uses that schema to validate the format of the inbound event.

### Physical interface resources

Use the physical interface to model the interface between a physical device and the Watson IoT Platform. Devices use the physical interface to publish events to the platform. Only model the events that you want to process within the plaform.
The physical interface maps an event ID to an event type. The event ID corressponds to the MQTT topic that the event is published to. The following example uses the event ID '1234' - **iot-2/evt/1234/fmt/json**.

### Application interface resources

Use the application interface to model the interface that is exposed by a device or Thing in the Platform. The application interface must reference a JSON schema. The application interface is used to define the structure of the state that
is stored for a device  or thing and to validate the state that is generated when an inbound event is processed.

### Device type resources

Use the device type as a “template” for creating instances of that type of device. Device attributes and metadata get copied to instances of the Device Type on creation. If you change information relating the to device type, changes do not apply to existing instances <reword this to match existing>. In information management, the device type is extended to reference the physical interface for the device and to reference the application interfaces that are used to retrieve the device state. The device type is also used to define how properties from inbound events are mapped to properties on the device state.
A Device Type exposes multiple Application Interfaces. The schemas associated with these application interfaces might define a property with the same name. For example, schema 1 and schema 2 both define the property "temperature". To avoid name collision, the mappings resource defines an alias property to ensure that hat properties from different Application Interfaces are kept separate in the device stat
For more information about device type within the Watson iot platorm, see <link>.


<could add a scenario with a lightbulb here>


##Steps ??
Create Physical (Device to Platform Interface). You define the interfaces for your device type by selecting the interface between your device type and the Watson IoT Platform. This creates a physical interface. The physical interface is the face of your device that you present to the world in terms of allowed users and applications. The physical interface links your device to Watson IOT Platform.
Add an Event Type.  You define the event type and the associated payload properties. These properties are saved to hte event type. You can add one or more properties. For example, temperature and humidity. The event types are added to the physical interface.
If you don't want to add your own event type, you can select a predefined event and payload  from the Watson IOT Platform event type library. You can then edit the defintions if you want to.  Or you can import an event type from an external file.
Then you add a public interface. The public interface links Watson iot platorm to applications and users. You can create a public interface, edit an existing interface, or immport a public interface from a file. Once you have your public interface, you can add properties to that interface. You then map events to the public interface from the private interface (is privtae the same as physical?) by using rest apis. You can also add simple calculations to the mapping, for example converting farenheit to celcius by selecting operators to build the calculation (see rest api xyz). You can add multiple mappings to any public property. In this case both Event1.tempx and Event2.tempx are both mapped to Temperature. <said about doing a scenario so this could be an option for it>. Repeat this process until all properties of the public interface are defined. (scaling?)


##Scenario
This scenario uses 3 simple types of devices and their physical and application interfaces
- Temperature
- Light
- Location

In this scenario, these sample device types are used in Information Management design to
- Define a physical interface that selects the state events and declares the types in the event payload
- Define a (default) application interface that maps to the physical interface
- Define / Import and map an abstract application interface and map to the physical interface

The intefaces and associated properties
- Light interfaces
  - Light – A simple on-off LED light or switch
  - LightLevel – An ambient light level sensor
  - DimmerLight - A LED light dimmer from 0 - 100%
  - HueLight – An LED light with RGB color settings

- Temperature interface – A simple temperature sensor
- Location interface – A GPS location sensor

Note - when you name your device types, use consistent naming structure. For example, <ACME kind><id>

You map your ACME-LED to your ACME-LED physical interface and that maps to multiple application interfaces <insert diagram>. You create you application interface with all the behaviours of the device type and map to a set of abstract interfaces. IE
ACME-LED (application interface)
Light  (application interface)
DimmerLight  (application interface)
HueLight  (application interface)

These light interfaces have the following properties
ACME-LED
• A LED light bulb with on-off switch, intensity, and color settings.
• A LED light bulb also has an ambient light sensor in Lux
• Level – ambient light level in Lux
• State - “on” or “off”
• Intensity – 0 to 100%
• Color – RGB coded string as “rrr.ggg.bbb” each with value 0 to 255
• ACME-LED – Physical interface
• Event Type: Status { “state”, “level”, “intensity”, “color”}
• Event Type: Error{ “code” }
• Command Type: Set {“state”, “level”, “intensity”, “color”) – all parameters optional


<Not sure about the rest of this scenario and if to use??> Think if anything we just stick to light bit and give an eg of someone configuring just that. <could put in code eg escription
: ”Hue Light Bulb"




## Orange
{: #orange}

The Orange extension allows you to view SIM card data from devices which are connected to your {{site.data.keyword.iot_short_notm}} and have an Orange SIM card installed.

https://developer.ibm.com/iotplatform/2016/03/30/watson-iot-platform-integration-with-orange-beta/

### Supported operations for Orange

If you have a device which is connected to your {{site.data.keyword.iot_short_notm}} service and has an Orange SIM card, you can use the Orange extension to view the following SIM card data:

- SIM serial number
- Activation status
- Last status change
- Last status refresh
- Location status

### REST APIs for Orange
To access the REST API for Orange, see the Orange Extension section in the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/ext-orange.html) documentation.

### Configuration for Orange

To enable the Orange extension:

1. From the {{site.data.keyword.iot_short_notm}} dashboard, select **Extensions**.
2. In the **Extensions** page, click **Add Extension**.
3. Click **Add** next to the Orange extension.
4. Enter your Orange user name and password.
6. Click **Done**.

After the Orange extension has been enabled, each device with an Orange SIM card must be configured to display Orange SIM data.

1. In the devices tab of your {{site.data.keyword.iot_short_notm}} dashboard, find the Orange SIM device to be configured.
2. Select the device and scroll down to *Extension Configuration*.
3. Enter the extension configuration by using the following JSON format, and then click **Confirm changes** to save your configuration.

```  
    {
        "orange": {
            "serialnumber": "<serial number of Orange SIM>"
        }
    }

```
When the organization is successfully configured, the *Extensions* section displays under the *Extensions Configuration* section in the *Device Drilldown* view.

## Historical Data Storage
{: #historical_data}

The historical data storage extension lets you locate and configure compatible message storage services such as [{{site.data.keyword.cloudantfull}}](../../cloudant_connector.html) or [{{site.data.keyword.messagehub_full}}](../../message_hub.html) for your IoT data.





copyright:
years: 2016, 2017
lastupdated: "2016-11-10"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Information Management (beta)
{: #im_index}

**Important:** This feature is currently available as part of a limited beta.

<maybe give an example here eg the light bulb and give a link to the scenario section>

Use the information management component of the {{site.data.keyword.iot_full}} to help you to organize and integrate data coming in to and going out of the {{site.data.keyword.iot_short_notm}} . The information management component lets you
- Process data that is in a format other than JSON.
- Filter out unwanted data.
- Decouple your applications from the complexities of how specific devices are connected. <reword>

## Mapping between devices and applications <reword>
{: #mapping}

To process inbound events for a specific device and convert these events to device state, you must provide the following information to the {{site.data.keyword.iot_full}}:
- The structure of the inbound events. This information is required by the physical interface.
- The structure of the desired device state. This information is required by the application interface.
- Information about how to map the inbound events to the desired device state. This information is required by the mappings resource.


The following diagram illustrates the logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}:

![The logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}.](images/im_mapping.png "The logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}")

## APIs
{: #api}

The following diagram illustrates the resources that you can manage by using the Information Management REST APIs:

![The resource mapping between a device and an application on {{site.data.keyword.iot_short_notm}}.](images/im_mapping.png "The logical mapping between a device and an application on {{site.data.keyword.iot_short_notm}}")

For information about the Information Management REST APIs, see the Information Management section in the [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html) documentation.

Use the following information to understand key concepts about the resources that you can manage.

## Schema
{: #schema}
Use schemas to define the structure of inbound events and the state of the device or thing. Event schemas are used by the physical interface. Event schemas define the structure of the events that are published to the {{site.data.keyword.iot_short_notm}} by a device. Application interface schemas are used to define the structure of the state that is stored by the {{site.data.keyword.iot_short_notm}}.

## Event Type
{: #event_type}
Use event type to model events within the {{site.data.keyword.iot_short_notm}}. An Event Type contains all the metadata that is associated with a single event. The events that are published to the {{site.data.keyword.iot_short_notm}} by a device can be in any format. For example, JSON, CBOR, XML, or Binary.  Before you start using event types, you must create an event type within your organization so that the {{site.data.keyword.iot_short_notm}} can process the data that is contained within a specific event. All event types must reference a JSON schema. The JSON schema validates the inbound event if that event is in JSON format. If the inbound event is not in JSON format, then the data must first be translated into JSON before validation occurs. For the Beta, inbound events  in JSON format only are supported.

## Physical interface
{: #physical_interface}
Use the physical interface to model the interface between a physical device and the {{site.data.keyword.iot_short_notm}}. Devices use the physical interface to publish events to the {{site.data.keyword.iot_short_notm}}. Only model the events that you want to process within the plaform. The physical interface maps an event ID to an event type. The event ID corressponds to the MQTT topic that the event is published to. The following example uses the event ID '1234' - **iot-2/evt/1234/fmt/json**. You can use the same event type for different devices.

## Application Interface
{: #application_interface}
Use the application interface to model the properties that are exposed by a device in the {{site.data.keyword.iot_short_notm}}. The application interface must reference a JSON schema. This schema defines the properties of the application interface. The application interface defines the structure of the state that is stored for a device and validates the state that is generated when an inbound event is processed. A device state is a set of properties that are described by the application interface or interfaces.


## Device Type
{: #device_type}
Use the device type as a “template” for creating instances of that type of device. Device attributes and metadata get copied to instances of the Device Type on creation. If you change information relating the to device type, changes do not apply to existing instances <reword this to match existing>. In information management, the device type is extended to reference the physical interface for the device and to reference the application interfaces that are used to retrieve the device state. The device type is also used to define how properties from inbound events are mapped to properties on the device state.
A Device Type exposes multiple Application Interfaces. The schemas associated with these application interfaces might define a property with the same name. For example, schema 1 and schema 2 both define the property "temperature". To avoid name collision, the mappings resource defines an alias property to ensure that hat properties from different Application Interfaces are kept separate in the device stat
For more information about device type within the Watson iot platorm, see <link>.


## Mappings
{: #mappings}
<add info about mappings>

## High-level workflow
{: #workflow}

Use the following high-level steps that you need to complete in order to create mappings between your own application and device so that you can generate a change of state in response to an inbound event:

1. Create a schema for your event type and then link your event to this event type. You can then link your event type to your physical interface. You can have multiple event types in a physical interface schema.  
2. Create a schema for your application interface and then link your application interface to this schema.
3. Create a device type to connect your application interface and your physical interface.
4. Define mappings to link an inbound event to a property or properties in  your application interface.


## End to end scenario
{: #scenario}

Use the following scenario to help you to create an inbound event in JSON format of a switch being turned on or off. The inbound event triggers a change of state to a property on the application interface. The change of state causes a lightbulb to be turned on or off.

Add an event schema:

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

Add an event type:

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

Add your physical interface:


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

Link your event type to your physical interface:


    ```
    {
      logger.info("# ---- add the event type to the physical interface -------")
    url = "/api/v0002/physicalinterfaces/%s/events" % (ids["physicalinterface"], )
    body = {"eventId" : "onoroff", "eventTypeId" : ids["onoroffeventtype"]}
    result, body = http.post(url, body)
      }
      ```


Add your application interface schema:

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

Add your application interface:

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

Add a device type with a physical interface:

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

Associate the application interface to the device type:


```
{
  logger.info("# ---- associate application interface to the device type -------")
    url = "/api/v0002/device/types/%s/applicationinterfaces" % (devicetypename, )
    body = {"applicationInterfaceId" : ids["basiclight app interface"]}
    result, body = http.post(url, body)
    self.assertEqual(result, 200)
  }
```


Add mappings to the device type:


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

Add a device:

```
{
  logger.info("# ---- add a device -------")
    url = "/api/v0002/device/types/Siemens light/devices"
    body = {"deviceId" : "light1", "deviceInfo":{}, "location":{}, "metadata":{}}
    result = http.post(url, body)
  }
```

Publish a device event:

```
{
  logger.info("# ---- publish a device event ----")
    topic = "iot-2/type/%s/id/%s/evt/%s/fmt/%s" % ("Siemens light", "light1", "onoroff", "json")
    event = '{"on": false}'
    mqtt.app_publish(topic, event)
  }
```

Get the state of the device:


```
logger.info("# get the state of the device")
    url = "/api/v0002/device/types/%s/devices/%s/state" % ("Siemens light", "light1")
    result = http.get(url)
    self.assertEqual(result[0], 200)
    self.assertEqual(json.loads(result[1]), {"basic_light": {"state": False}})
```


## Next Steps

Try out the recipes <here> to help you get going with developing your own .... or add your own recipe to the .....
