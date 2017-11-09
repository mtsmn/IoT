---

copyright:

years: 2017
lastupdated: "2017-11-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Quotas
This section provides information about the limits that are set for {{site.data.keyword.iot_full}} services by plan type. The limits are there as part of our fair use policy to help to ensure that performance is not adversely impacted by misuse for all users of the multitenant system. The limits also help to prevent real user workload being inadvertently identified as a Denial of Service attack.

## Introduction
{: #metrics_about}

Quotas specify the upper limits that are set for a resource. When usage exceeds a specified limit, user requests might be delayed or denied. In exceptional circumstances, exceeding limits which affect system stability might result in the {{site.data.keyword.iot_short_notm}} organization being suspended to prevent other users from being affected.

The following tables list the limits for {{site.data.keyword.iot_short_notm}}. Most of the limits that are specified are per {{site.data.keyword.iot_short_notm}} organization, unless explicitly listed as per device. Some of the limits have an enforced maximum, while other limits can be increased upon request. If you need limits that are higher than those specified, please raise a [ticket ![External link icon](../../../icons/launch-glyph.svg)](https://support.ng.bluemix.net/gethelp/){: new_window}.

## Messaging
{: #messaging_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans     | Dedicated plan
------------- | -------------|------------- |
Maximum number of device types |50 |1000 |1000
Maximum number of concurrently connected devices | 500| 500K. If you want to connect more than 50,000 devices, please get in touch to discuss your plans. | 500K
Maximum number of registered devices |500 | 1 million | 100K per increment
Maximum ['A' Applications - shared](../applications/mqtt.html#scalable_apps) |1 | 10 shared subscriptions, each with 50 instances|10 shared subscriptions, each with 50 instances
Maximum ['a' applications - non-shared](../applications/mqtt.html#client_connections) |500 API keys | 2000 API keys | 2000 API keys
Maximum Data per Month | 200 MB | unlimited |unlimited
Device connections|10/sec | 300/sec |300/sec
Device-to-Cloud sends (MQTT) - Device | 5 msg/sec or 20 KB/sec | 10 msg/sec or 40 KB/sec |10 msg/sec or 40 KB/sec
Device-to-Cloud sends (MQTT) - Gateway  | 50 msg/sec or 200 KB/sec | 100 msg/sec or 400 KB/sec| 100 msg/sec or 400 KB/sec
Device-to-Cloud sends (MQTT) - Application | 10 msg/sec or 40 KB/sec | 20 msg/sec or 80 KB/sec| 20 msg/sec or 80 KB/sec
Device-to-Cloud sends (MQTT) - per Organization | 5000 msg/sec | 6000 msg/sec | 6000 msg/sec
Device-to-Cloud sends (HTTP) - Device | 30 msg/min or 2 KB/sec | 1 msg/sec or 4 KB/sec | 1 msg/sec or 4 KB/sec
Device-to-Cloud sends (HTTP) - Gateway | 5 msg/sec or 20 KB/sec | 10 msg/sec or 40 KB/sec | 10 msg/sec or 40 KB/sec
Device-to-Cloud sends (HTTP) - Application | 1 msg/sec or 4 KB/sec| 2 msg/sec or 8 KB/sec | 2 msg/sec or 8 KB/sec
Device-to-Cloud sends (HTTP) - per Organization | 500 msg/sec | 600 msg/sec | 600 msg/sec
Cloud-to-Device sends (MQTT) - Device  |1 msg/sec |2 msg/sec or 8 KB/sec |2 msg/sec or 8 KB/sec
Cloud-to-Device sends (MQTT) - Gateway| 10 msg/sec |20 msg/sec or 80 KB/sec  |20 msg/sec or 80KB/sec  
Cloud-to-Device sends (MQTT) - Application | 10 msg/sec |20 msg/sec or 80 KB/sec |20 msg/sec or 80 KB/sec  
Cloud-to-Device sends (MQTT) - per Organization |1000 msg/sec | 12K msg/sec|12K msg/sec
Cloud-to-Device sends (HTTP) - burst rate per Device | 30 msg/min| 30 msg/min  | 30 msg/min
Cloud-to-Device sends (HTTP) - per Organization |  1000 msg/sec|  1200 msg/sec  |  1200 msg/sec
Maximum number of inbound unacknowledged messages - per Device | 10 |10 |10
Maximum number of inbound unacknowledged messages - per Gateway | 1000 |1000 |1000
Maximum number of inbound unacknowledged messages - per application connection  |2000 | 2000|2000
Maximum number of outbound unacknowledged messages - per Device |10  |10 |10
Device-to-Cloud persistence | 24 hours|24 hours |24 hours
Cloud-to-Device persistence |48 hrs (default). Max 7 days & 50 queue depth| 48 hrs (default). Max 7 days & 50 queue depth  |48 hrs (default). Max 7 days & 50 queue depth
Maximum retry interval for delivering QoS 1 messages | Retry on reconnect |Retry on reconnect |Retry on reconnect
Connection inactivity limit (keep alive interval) | Specified by client on connection |Specified by client on connection  |Specified by client on connection
WebSocket connection duration |Specified by client on connection | Specified by client on connection  |Specified by client on connection
Maximum message size | 128 KB |128 KB |128 KB
Maximum subscriptions per device |50 |50 |50
Maximum subscriptions per application | 500 |500 |500
Queue depth - maximum buffered messages per subscription per device | 50 |50 |50
Maximum buffered messages for 'A' applications |50K durable, 50K non-durable |50K durable, 50K non-durable |50K durable, 50K non-durable


## APIs
{: #api_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
API calls |10/sec |10/sec |10/sec

## Rule Engine
{: #rule_engine_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
Maximum rules per Organization |100|1000|1000
Actions per rule|10|10|10

## Device Management
{: #device_mgmt_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
Maximum size of diagnostic logs per device |500K|500K|500K
Maximum historical versions of diagnostic logs held|2  |2 |2
Maximum time diagnostic logs held|7 days| 7 days|7 days
Maximum number of actions initiated per in-flight message |200 |200 |200
Maximum number of devices per action |5000 |5000 | 5000

## Last event cache
{: #last_event_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
Maximum number of event IDs per device |5|5|5
Maximum number of formats |2|3|3
Expiry from last event cache |12 months |12 months | 12 months

## Extensions
{: #extensions_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
Maximum number of services with which you can bind |12|12|12

## Information management
{: #information_management_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
Maximum nesting depth for JSON payload |5|5|5
Maximum JSON string size |512|512|512

## Security
{: #security_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
Maximum number of custom roles |20|20|20
Maximum number of user members |1000|1000|1000
Maximum number of devices in a resource group |300|300|300
Maximum number of resource groups assigned to a gateway |10|10|10
Maximum number of resource groups to which a device can belong |10|10|10

## User Interface
{: #UI_metrics}

Metric        | Lite plan      | Standard & Advanced Security plans       | Dedicated
------------- | -------------|------------- |
Maximum number of dashboards |50|50|50
Maximum number of cards on board |30|30|30
