---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-14"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
# MQTT standards 
{: #standards_and_requirements}

{{site.data.keyword.iot_full}} supports any content that is permitted by the MQ Telemetry Transport (MQTT) standard.
{:shortdesc}



## MQTT
{: #mqtt}

MQTT is a light weight event and message oriented protocol allowing devices to asynchronously communicate efficiently across constrained 
networks to remote systems. MQTT supports many different data formats, so it's possible to send images, texts that are in any encoding, encrypted data, and virtually every type of data in binary format. 

For more information about the MQTT standard, see [MQTT.org ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://mqtt.org/){: new_window}.

For information about message payload size limits and format restrictions for specific use cases for {{site.data.keyword.iot_short_notm}}, see [MQTT messaging](mqtt/index.html).


{{site.data.keyword.iot_short_notm}} supports the following versions of the MQTT messaging protocol:

MQTT version | Notes
--- | --- | ---
[3.1.1 ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://www.oasis-open.org/standards#mqttv3.1.1){: new_window}  | <ul><li>OASIS Standard.<li>ISO standard (ISO/IEC PRF 20922) <li>Improved interoperability between various clients and servers because of a more precise definition of the protocol compared to V3.1.   <li>The maximum length of the MQTT client identifier (ClientId) is increased to 256 from the 23 character limit that is imposed by V3.1. </br>The {{site.data.keyword.iot_short_notm}} service often requires longer client IDs. </br>Long client IDs are supported regardless of the MQTT protocol version. However, some V3.1 client libraries check the length of the ClientId value and enforce the 23 character limit.</ul>
3.1 | MQTT V3.1 version of the protocol.
[5.0 ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/eclipse/paho.mqtt.c/releases/tag/v1.3.0){: new_window} </br>(Beta now available in Frankfurt)| <ul><li>Enhancements for scalability   <li>Improved error reporting   <li>Formulation of common patterns including capability discovery and request response   <li>Extensibility mechanisms including user properties   <li>Performance improvements and support for small clients</ul>


## MQTT version 5 
{: #mqttv5_highlights}

**Important:** MQTT versions 3.1 and 3.1.1 are already fully supported. MQTT version 5 is now available to try with {{site.data.keyword.iot_short_notm}} as part of a limited beta program in the Frankfurt deployment. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Start using the new functionality by downloading an eclipse client that has been updated to support MQTT version 5 from the [Eclipse Paho ![External link icon](../../../icons/launch-glyph.svg)](https://www.eclipse.org/paho/){: new_window} library. {{site.data.keyword.iot_short_notm}} retains full backward compatibility for MQTT version 3 and MQTT version 3.1.1 clients.  

The following functionality is available as part of MQTT v5:
<dl>
<dt>Error reporting
<dd>Enhancements to the information that is provided when an error occurs makes it easier to determine the cause of the failure. 
</dd></dt>

<dt>New session start and expiry support
<dd>Clients can control the duration of a session by indicating whether to start a new session on connect, and how 
long after a disconnect to keep a session.</dd></dt>

<dt>Message expiry
<dd>Messages can be set to expire without being delivered by including an expiration time in the message. This feature is particularly useful if the message that is being sent to a device is only important if that device is online. </dd></dt>

<dt>Message properties including user properties
<dd>Message properties can now be included in the message header; for example, whether the content is text or binary, the content type, and application defined user properties. The metadata can be changed without modifying the message content, enabling rapid routing in the back-end application by removing the need to parse the message payload. If the payload is encrypted, the additional information that is contained in the header outside of the encrypted envelope can be used to maximise processing efficiency.</dd></dt>

<dt>Subscription identifiers
<dd>Subscriptions can be tagged with an identifier which is returned along with messages that are received by that subscription. This feature enables the routing of messages in the client application.</dd></dt>

<dt>Subscription options
<dd>The addition of a *NoLocal* option provides the ability to not send messages that originate on a specified client
and also includes options for handling retained messages on subscribing to a topic.</dd></dt>

<dt>Topic alias
<dd>Topic alias is used to decrease the number of bytes that are sent across the network by encoding long topic names into a short integer encoding. </dd></dt>

<dt>Capability discovery
<dd>Clients can discover the capabilities of the MQTT server; for example, the maximum packet size that is accepted. 
Many capabilities are handled by the client library but are now made available to the client application.</dd></dt></dl>

