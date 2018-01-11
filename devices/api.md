---

copyright:
years: 2016, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# HTTP APIs for devices
{: #api}

There are two types of HTTP APIs that can be used with {{site.data.keyword.iot_full}}. You can use HTTP REST APIs to
create, update, list, and delete devices. Use HTTP Messaging APIs to send event information from devices to the cloud, and to accept 
commands from applications in the cloud. 

## Client connections
{: #client_connections}

For information about client security and how to connect clients to devices in {{site.data.keyword.iot_short_notm}}, see [Connecting applications, devices, and gateways to {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

**Note:** HTTP port 1883 is disabled by default. For information about changing the default setting, see [Configuring security policies](../reference/security/set_up_policies.html#set_up_policies.md).

## Authentication

All authentication requests must include an authorization header. Basic authentication is the only method that is supported. When a device makes an HTTP request through the {{site.data.keyword.iot_short_notm}} HTTP REST API, the following credentials are required:

|Credential|Required input|
|:---|:---|
|User name|`use-token-auth`
|Password| The authentication token that was either automatically generated or manually specified when you registered the device.


## Content-Type request headers

A `Content-Type` request header must be provided with the request. The following table shows how the supported types are mapped to the {{site.data.keyword.iot_short_notm}} internal formats.

|Content-Type header|Format in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

## Quality of service

Similar to the MQTT quality of service "at most once" delivery service level 0, HTTP REST messaging provides non-persistent message delivery but it validates that the request is correct and that it can be delivered to the server before it sends the HTTP response. A reply that contains a HTTP status code of 200 confirms that the message was delivered to the server. When you use either the "at most once" MQTT quality of service level or the HTTP equivalent to deliver event messages, the device or application must implement retry logic to guarantee delivery.

For more information about the MQTT protocol and the quality of service levels for {{site.data.keyword.iot_short_notm}}, see [MQTT messaging](../reference/mqtt/index.html).

You can use HTTP messsaging APIs to pubish events from devices to the cloud, and to receive commands from applications in the cloud.

## Publishing events
{: #event_publication}

In addition to using the MQTT messaging protocol, you can also configure your devices to publish events to the {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP Messaging API commands.

Use one of the following URLs to submit a ``POST`` request from a device that is connected to {{site.data.keyword.iot_short_notm}}:

### Non-secure POST request
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Secure POST request

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Note:** Port 443, the default SSL port, can also be specified for secure HTTP API calls.

## Receiving commands
{: #receive_commands}

In addition to using the MQTT messaging protocol, you can also configure your devices to receive commands from {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP messaging API commands. A device can receive commands that are directed at itself.

Use one of the following URLs to submit a ``POST`` request from a device that is connected to {{site.data.keyword.iot_short_notm}}:

### Non-secure POST request
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Secure POST request

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Note:** Port 443, the default SSL port, can also be specified for secure HTTP API calls.

You can optionally include the parameter *waitTimeSecs* in the body of the HTTP request to specify an integer that represents the maximum number of seconds to wait for a command:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Important notes:**
- The value of *waitTimeSecs* must be an integer in the range 0 - 3600 seconds. The default value is 0.
- To receive any command, use the "any" wildcard character (+) for the {command} component. If the wildcard character is used, the command identifier is contained in the response header field *X-commandId*.
- If the HTTP response status code is 200, the command data is contained in the body of the response. Review the response header field *Content-Type* to find the content type.
- If the HTTP response status code is 204, no command data is available.

To access the {{site.data.keyword.iot_short_notm}} HTTP REST APIs, see [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

To access the {{site.data.keyword.iot_short_notm}} HTTP Messaging APIs, see [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

For information about developing devices, see [Developing devices on {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).

## Example of a secure POST request

The following example sends a message from a device to {{site.data.keyword.iot_short_notm}} by using the HTTP protocol. An application receives the message by subscribing to the topic on which the message is published. The following table provides information about the device. 

|Parameter|Value|
|:---|:---|
|Organization ID |myOrgID
|Device Type| TestDevices
|Device ID | TestPublishEvent
|Authentication Method|use-token-auth
|Authentication Token|passw0rd


The following example uses curl to sends an event that is called *TestMessage* from the device *TestPublishEvent* to  {{site.data.keyword.iot_short_notm}} over the HTTP protocol: 

```
curl -v -X POST -H "Content-Type: application/json" -u "use-token-auth:passw0rd" -d @message.txt  https://myOrgID.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/TestDevices/devices/TestPublishEvent/events/TestMessage
```

The message is contained in a text file that is called *message.txt*. The content of the *message.txt* file is shown in the following example: 
```
{ "d": { "myName": "Publish Test","cputemp": 46,"sine": -10,"cpuload": 1.45 }  } 
```
The message is published on topic *iot-2/evt/TestMessage/fmt/json*. An application that is called *testSubscriber* is created that subscribes to a topic filter *iot-2/type/+/id/+/evt/+/fmt/+*. The topic filter matches the topic on which the message is published.

You can use the python sample *simpleApp.py* that is provided in the [python library ![External link icon](../../../icons/launch-glyph.svg)](https://github.com/ibm-watson-iot/iot-python/tree/master/samples/simpleApp) to help you configure your application to subscribe to the topic filter *iot-2/type/+/id/+/evt/+/fmt/+*. 

The following python script subscribes to a topic filter that matches the topic string on which the message is published:

```
python simpleApp.py -o "myOrgID" -i "testSubscriber" -k "a-myOrgID-jg95dkpscn" -t "L(r4gfPa3bLP2kz4Nb"
```
where:

- **-o** is the organization name.
- **-i** is the name of the application.
- **-k** is API key that is generated when the application is created.  
- **-t** is the authentication token that is generated when the application is created.   


The following example shows a successful response to the receipt of the published message from *TestPublishEvent* device. 
```
=============================================================================
Timestamp                        Device                        Event
2017-12-06 15:09:40,438   ibmiotf.application.Client  INFO    Connected successfully: a:myOrgID:testSubscriber
=============================================================================
2017-12-06T15:09:59.691290+00:00 TestDevices:TestPublishEvent  TestMessage: {"d": {"myName": "Publish Test", "cputemp": 46, "sine": -10, "cpuload": 1.45}}
2017-12-06T15:10:01.056000+00:00 TestDevices:TestPublishEvent  Connect 199.999.99.99
2017-12-06T15:10:01.446000+00:00 TestDevices:TestPublishEvent  Disconnect 199.999.99.99 (None)

```

## Last event cache
{: #last-event-cache}

By using the {{site.data.keyword.iot_short_notm}} Last Event Cache API, you can retrieve the last event that was sent by a device. This works whether the device is online or offline, which allows you to retrieve device status regardless of the device's physical location or use status. You can retrieve the last recorded value of an event ID for a specific device, or the last recorded value for each event ID that was reported by a specific device. Last event data of a device can be retrieved for any specific event that occurred up to 365 days ago.

To request the most recent value for a specific event ID, use the following API request, which returns the last recorded value for the “power” event ID.

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

The response is returned in the following JSON format:

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**Note:** While the API response is in JSON format, event payloads can be written in any format. Payloads returned by Last Event Cache API are encoded in base64.

To request the most recent value for each event ID that was reported by a device, use the following API request:

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

The response will include all event IDs that were sent by the device. In the following example, values are returned for the “power” and “temperature” events.

```
[
    {
        "deviceId": "<device-id>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

To access the {{site.data.keyword.iot_short_notm}} Last event cache APIs, see [{{site.data.keyword.iot_short_notm}} HTTP Information Management API ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}.
