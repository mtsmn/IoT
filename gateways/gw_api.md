---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# HTTP APIs for gateway devices
{: #api_link}

There are two types of HTTP APIs that can be used with {{site.data.keyword.iot_full}}. You can use HTTP REST APIs to
create, update, list, and delete gateway devices. Use HTTP Messaging APIs to send event information from gateway devices to the cloud, and to accept commands from applications in the cloud. 



## Client connections
{: #client_connections}

For information about client security and how to connect clients to gateway devices in {{site.data.keyword.iot_short_notm}}, see [Connecting applications, devices, and gateways to {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

**Note:** HTTP port 1883 is disabled by default. For information about changing the default setting, see [Configuring security policies](../reference/security/set_up_policies.html#set_up_policies.md).


## Authentication

All requests must include an authorization header. Basic authentication is the only method that is supported. When a device makes an HTTP request by using the {{site.data.keyword.iot_short_notm}} HTTP REST API, the following credentials are required:

|Credential|Required input|
|:---|:---|
|User name| `g/{orgId}/{gwType}/{gwDevId}` or `g-{orgId}-{gwType}-{gwDevId}`
|Password| The authentication token that was either automatically generated or manually specified when you registered the gateway device.

where:

<dl>
<dt>orgId</dt>  
<dd>The organization name, which must match the name that is specified in the host header.</dd>

<p></p>
<dt>gwType</dt>  
<dd>The type of gateway.</dd> 
<dd>If you use the hyphen "-" character as a delimiter in the user name, this value must not include a hyphen character. </dd>
<p></p>
<dt>gwDevId</dt>  
<dd>The device identifier of gateway. </dd>
</dl>


## Content-Type request headers

A `Content-Type` request header must be provided with the request. The following table shows how the supported types are mapped to the {{site.data.keyword.iot_short_notm}} internal formats:

|Content-Type header|Format in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

## Quality of service

Similar to the MQTT quality of service "at most once" delivery service level 0, HTTP REST messaging provides non-persistent message delivery but it validates that the request is correct and that it can be delivered to the server before it sends the HTTP response. A reply that contains a HTTP status code of 200 confirms that the message was delivered to the server. When you use either the "at most once" MQTT quality of service level or the HTTP equivalent to deliver event messages, the device or application must implement retry logic to guarantee delivery.

For more information about the MQTT protocol and the quality of service levels for {{site.data.keyword.iot_short_notm}}, see [MQTT messaging](../reference/mqtt/index.html).

You can use HTTP messsaging APIs to pubish events from gateway devices to the cloud, and to receive commands from applications in the cloud.

## Publishing events
{: #event_publication}

In addition to using the MQTT messaging protocol, you can also configure your gateway devices to publish events to {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP messaging API commands.

To submit a ``POST`` request from a device that is connected to {{site.data.keyword.iot_short_notm}}, use one of the following URLs:

### Non-secure POST request
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Secure POST request
<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Important notes:**
- Port 443, the default SSL port, can also be specified for secure HTTP API calls.
- If a gateway is not assigned the *Standard Gateway* role, it can publish events on behalf of any devices in the organization. If the device that is connected to the gateway is unregistered, the gateway automatically registers that device.
- Assign the *Standard Gateway* role if you want to check device authorization levels.

For more information about the role of gateways and resource groups, see [Gateway Access Control (Beta)](../gateways/gateway-access-control.html).

## Receiving commands
{: #receive_commands}

In addition to using the MQTT messaging protocol, you can also configure your gateway devices to receive commands from {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP messaging API commands. A gateway device can receive commands that are directed to devices within its associated resource group. For more information about gateway resource groups, see [Gateway Access Control (Beta)](../gateways/gateway-access-control.html).

Use one of the following URLs to submit a ``POST`` request from a gateway that is connected to {{site.data.keyword.iot_short_notm}}:

### Non-secure POST request
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Secure POST request

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Note:** Port 443, the default SSL port, can also be specified for secure HTTP API calls.

You can optionally include the parameter *waitTimeSecs* in the body of the HTTP request to specify an integer that represents the maximum number of seconds to wait for a command:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Important notes:**
- The value of *waitTimeSecs* must be an integer in the range 0 - 3600 seconds. The default value is 0.
- To receive commands for any device type, use the "any" wildcard character (+) for the `typeId` component. If the wildcard character is used, the device type is contained in the response header field *X-deviceType*.
- To receive commands for any device, use the "any" wildcard character (+) for the `deviceId` component. If the wildcard character is used, the device identifier is contained in the response header field *X-deviceId*.
- To receive any command, use the "any" wildcard character (+) for the `command` component. If the wildcard character is used, the command identifier is contained in the response header field *X-commandId*.
- If the HTTP response status code is 200, the command data is contained in the body of the response. Review the response header field *Content-Type* to find the content type.
- If the HTTP response status code is 204, no command data is available.

To access the {{site.data.keyword.iot_short_notm}} HTTP REST APIs, see [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

To access the {{site.data.keyword.iot_short_notm}} HTTP Messaging APIs, see [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

## Last event cache
{: #last-event-cache}

By using the {{site.data.keyword.iot_short_notm}} Last Event Cache API, you can retrieve the last event that was sent by a device. This API works whether the device is online or offline, which allows you to retrieve device status regardless of the device's physical location or use status. You can retrieve the last recorded value of an event ID for a specific device, or you can retrieve the last recorded value for each event ID that was reported by a specific device. Last event data of a device can be retrieved for any specific event that occurred up to 365 days ago.

To request the most recent value for a specific event ID, use the following API request, which returns the last recorded value for the “power” event ID:

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

The response includes all event IDs that were sent by the device. In the following example, values are returned for the “power” and “temperature” events.

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
