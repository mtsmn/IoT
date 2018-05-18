---

copyright:
 years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# HTTP messaging APIs for gateway devices
{: #api}

## Accessing the HTTP messaging API documentation for gateway devices
{: #rest_messaging_api}

To access the {{site.data.keyword.iot_full}} HTTP messaging APIs, see [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Publishing events
{: #event_publication}

In addition to using the MQTT messaging protocol, you can also configure your gateway devices to publish events to {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP messaging API commands.

To submit a ``POST`` request from a device that is connected to {{site.data.keyword.iot_short_notm}}, use one of the following URLs:

### Non-secure POST request for publishing events

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Secure POST request for publishing events

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Important notes:**
- Port 443, the default SSL port, can also be specified for secure HTTP API calls.
- If a gateway is not assigned the *Standard Gateway* role, it can publish events on behalf of any devices in the organization. If the device that is connected to the gateway is unregistered, the gateway automatically registers that device.
- Assign the *Standard Gateway* role if you want to check device authorization levels.

For more information about the role of gateways and resource groups, see [Gateway Access Control](../gateways/gateway-access-control.html).


### Quality of service

Similar to the MQTT quality of service "at most once" delivery service level 0, HTTP messaging APIs provide non-persistent message delivery, but it validates that the request is correct and that it can be delivered to the server before it sends the HTTP response. A reply that contains an HTTP status code of 200 confirms that the message was delivered to the server. When you use either the "at most once" MQTT quality of service level or the HTTP equivalent to deliver event messages, the device or application must implement retry logic to guarantee delivery.

For more information about the MQTT protocol and the quality of service levels for {{site.data.keyword.iot_short_notm}}, see [MQTT messaging](../reference/mqtt/index.html).

For more information about managing gateway devices by using APIs, see [HTTP REST APIs for gateway devices](../gateways/gw_api.html).

## Receiving commands
{: #receive_commands}

In addition to using the MQTT messaging protocol, you can also configure your gateway devices to receive commands from {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP messaging API commands. A gateway device can receive commands that are directed to devices within its associated resource group. For more information about gateway resource groups, see [Gateway Access Control](../gateways/gateway-access-control.html).

Use one of the following URLs to submit a ``POST`` request from a gateway that is connected to {{site.data.keyword.iot_short_notm}}:

### Non-secure POST request for receiving commands

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Secure POST request for receiving commands

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
