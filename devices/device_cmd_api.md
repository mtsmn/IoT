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

# HTTP messaging APIs for devices
{: #api}

You can use HTTP messsaging APIs to pubish events from devices to the cloud, and to receive commands from applications in the cloud.

To access the {{site.data.keyword.iot_short_notm}} HTTP messaging APIs, see [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![External link icon](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

## Publishing events
{: #event_publication}

In addition to using the MQTT messaging protocol, you can also configure your devices to publish events to the {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP Messaging API commands.

Use one of the following URLs to submit a ``POST`` request from a device that is connected to {{site.data.keyword.iot_short_notm}}:

### Non-secure POST request or publishing events

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Secure POST request for publishing events

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Note:** Port 443, the default SSL port, can also be specified for secure HTTP API calls.

## Receiving commands
{: #receive_commands}

In addition to using the MQTT messaging protocol, you can also configure your devices to receive commands from {{site.data.keyword.iot_short_notm}} over HTTP by using HTTP messaging API commands. A device can receive commands that are directed at itself.

Use one of the following URLs to submit a ``POST`` request from a device that is connected to {{site.data.keyword.iot_short_notm}}:

### Non-secure POST request for receiving commands

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Secure POST request for receiving commands

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Note:** Port 443, the default SSL port, can also be specified for secure HTTP API calls.

You can optionally include the parameter *waitTimeSecs* in the body of the HTTP request to specify an integer that represents the maximum number of seconds to wait for a command:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Important notes:**
- The value of *waitTimeSecs* must be an integer in the range 0 - 3600 seconds. The default value is 0.
- To receive any command, use the "any" wildcard character (+) for the {command} component. If the wildcard character is used, the command identifier is contained in the response header field *X-commandId*.
- If the HTTP response status code is 200, the command data is contained in the body of the response. Review the response header field *Content-Type* to find the content type.
- If the HTTP response status code is 204, no command data is available.


For information about developing devices, see [Developing devices on {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).
