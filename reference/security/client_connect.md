---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Monitoring client connectivity (Beta)
{: #connect_status}

{{site.data.keyword.iot_full}} enables you to query and monitor the connection status of clients. You can know in real time whether a client is connected, and you can troubleshoot any connection issues.
{:shortdesc}

For example, you can see when a client was last active and why it became disconnected, or you can view all clients that did not connect in the past day so that you can address any issues.

**Important:** The monitoring client connectivity feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Querying client connection status
{: #query_status}

The Client Connection State API enables you to retrieve and query the connection status for any clients that have connected to {{site.data.keyword.iot_short_notm}}.

When you query the connection status, you can view the following information:

 - the connectivity status (either connected or disconnected)
 - the time of the last connection, in ISO 8601 format
 - the time of the last disconnection, in ISO 8601 format
 - the last activity event (such as MQTT publish or MQTT subscribe)
 - the time of the last activity, in ISO 8601 format
 - the entity that caused the disconnection, if the client is disconnected (client, server, or network)
 - the MQTT reason code for the disconnection, if the client is disconnected

**Examples**

To retrieve the client connection status for a single client:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

To retrieve the connection status of all clients:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

To retrieve all connected clients:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

To retrieve all clients that have been active within the past two days:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**Note:** There might be a brief delay between the time that a client connects and the time that the connection status from the API reflects the correct status.

For more information about the API, including all of the available query filters, see [the Client Connection State API Swagger document ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}.

## Monitoring client connection status
{: #monitor_status}

 All clients have a monitor topic that enables you to subscribe to receive live connectivity status updates for the client. For more information see [Subscribing to device status messages](../../applications/mqtt.html#subscribe_device_commands).

## Troubleshooting client connectivity issues

The Connection Logs API enables you to retrieve a list of connection log events for a device to diagnose connectivity issues. The entries record successful connections, unsuccessful connection attempts, intentional disconnections, and server-initiated disconnections.

For more information, see the [Connection Logs API Swagger document ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}.
