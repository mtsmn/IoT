---
copyright:
  years: 2016, 2017
lastupdated: "2017-10-18"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Billing

Paid {{site.data.keyword.iot_full}} service plans (plans other than 'Lite'), are based on the concept of megabytes exchanged and analyzed over the period of a month.  This document details how {{site.data.keyword.iot_short_notm}} meters the data to create usage information that determines the cost of using the service.  Usage information can be used to approximate the cost of using {{site.data.keyword.iot_short_notm}} based on the design and number of devices, applications and gateways.

For information about the cost of each megabyte of data exchanged or analyzed, see the {{site.data.keyword.iot_short_notm}} service in the {{site.data.keyword.Bluemix_notm}} catalog for the region required.

You can use the [Pricing Calculator ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://iot-cost-calculator.ng.bluemix.net/) to help you to calculate the cost of an {{site.data.keyword.iot_short_notm}} service.

The following three items are each metered separately and billed according to usage: 
- data exchanged
- data analyzed
- edge data analyzed

## Data Exchanged
The *data exchanged* calculation includes account data that is exchanged by applications, devices and gateways by using MQTT or HTTP messaging, as well as data that is exchanged by applications by using the HTTP API.

### MQTT Data Exchanged
When using MQTT, the TCP stream content counts towards *data exchanged*.  The payload of any MQTT message is included in the cumulation of the data that is exchanged by using the MQTT protocol.  Cumulation of the payload data size occurs when the message is published, as well as when the message is delivered to a subscriber.

The MQTT protocol is designed to be as light-weight as possible.  Although {{site.data.keyword.iot_short_notm}} includes the sizes of MQTT packets when cumulating *data exchanged*, because it is a light-weight protocol, this overhead is low.  The following table gives an indication of the minimum size of each MQTT packet:

|MQTT PACKET                    |Minimum size bytes  |Variables and associated minimum size in bytes|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |Client ID (12 if d:xxxxxx:t:i), will topic (0) and will message (0) if set, username (14 for devices - 'use-token-auth') and password (18 if auto-generated)|
|CONNACK                        |4                   |Nothing|
|PUBLISH                        |21                  |Topic name (17 if iot-2/evt/i/fmt/f) and payload (0 is the minimum).  Applies to both inbound and outbound PUBLISH packets.|
|PUBACK                         |4                   |Nothing|
|PUBREC                         |4                   |Nothing|
|PUBREL                         |4                   |Nothing|
|PUBCOMP                        |4                   |Nothing|
|SUBSCRIBE                      |26                  |Can include multiple of both topic filter (19 if iot-2/evt/i/fmt/f) and QoS (1)|
|SUBACK                         |5                   |(1) for every topic filter in SUBSCRIBE|
|UNSUBSCRIBE                    |23                  |Can include multiple topic filters (19 if iot-2/evt/i/fmt/f)|
|UNSUBACK                       |4                   |Nothing|
|PINGREQ                        |2                   |Nothing|
|PINGRESP                       |2                   |Nothing|
|DISCONNECT                     |2                   |Nothing|

When using TLS, the secure handshake is also considered. The handshake is approximately 8 kilobytes. It is therefore more cost effective to keep MQTT connections open as long as possible. Open connections incur only the PINGREQ and PINGRESP overhead (4 bytes) per keep alive interval which is client specific and depends on the keep alive period that is specified.  Reopening a TLS connection by using an existing TLS session decreases the number of bytes that are exchanged because the certificates are not sent in either direction.  Many TLS clients support this by default, but the session has a limited lifetime.  Using client certificates increases the size of the TLS handshake, depending on the size of the client certificate. 

TLS records, and TCP and IP overheads are not considered.

**Note** - When using the {{site.data.keyword.iot_short_notm}} dashboard to view a device, an MQTT subscription is made so that the dashboard can display live events.  This MQTT subscription also counts towards the total amount of data exchanged.

### HTTP Messaging Data Exchanged
When using HTTP messaging, the message payload is included in the cumulation of *data exchanged*, both when publishing events or receiving commands.

As with MQTT, the HTTP overhead counts.  The HTTP overhead is significantly greater than MQTT overhead. The HTTP overhead per request is approximately 300 bytes. The message payload size also needs to be added.

When using TLS, the secure handshake is considered.  The handshake is approximately 8 kilobytes.  It is therefore more cost effective to keep HTTPS connections open as long as possible.  Open connections incur no additional overhead in terms of *data exchanged*.  Reopening a TLS connection by using an existing TLS session decreases the number of bytes that are exchanged because the certificates are not sent in either direction.  Many TLS clients support this by default, but the session has a limited lifetime.  Using client certificates increases the size of the TLS handshake, depending on the size of the client certificate.

TLS records, and TCP and IP overheads are not considered.

### HTTP API Data Exchanged
When any API is invoked by an application, both the request and the response bodies cumulate towards *data exchanged*.  The sizes of each can vary significantly depending on the API.

Unlike MQTT and HTTP messaging, neither the HTTP overhead nor the TLS overhead are considered for HTTP API data exchanged.

**Note** - When using the {{site.data.keyword.iot_short_notm}} dashboard, the HTTP APIs are used by the dashboard to list information inlcuding devices, device types, and device connection logs.  These HTTP API calls count towards *data exchanged*.

## Data Analyzed
The *data analyzed* calculation measures event data that is processed by the rules engine within the platform.  Data is considered processed by the rules engine when device events are evaluated by one or more rules, based on a specific device and event type. 

## Edge Data Analyzed
The *edge data analyzed* calculation measures event data that is processed on a gateway device by the {{site.data.keyword.iot_short_notm}} Edge Analytics Agent.  Data is considered processed by the edge agent when device events are evaluated by one or more edge rules, based on a specific device and event type. 
