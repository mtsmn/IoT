---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# MQTT connectivity for applications
{: #mqtt}

MQTT is the primary protocol that devices and applications use to communicate with the {{site.data.keyword.iot_full}}. Client libraries, information, and samples are provided to help you to connect and integrate your {{site.data.keyword.iot_short_notm}} applications.
{:shortdesc}

## Client connections
{: #client_connections}

For information about client security and how to connect MQTT clients to applications in  {{site.data.keyword.iot_short_notm}}, see [Connecting applications, devices, and gateways to {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

## MQTT authentication
{: #mqtt_authentication}

{{site.data.keyword.iot_short_notm}} applications require an API key to connect to an organization. When an API key is registered, an authentication token is generated, which must be used with that API key.

The following example shows a typical API key:

<pre class="pre">a-<var class="keyword varname">orgId</var>-a84ps90Ajs</pre>
{: codeblock}


The following example shows a typical authentication token:

```
MP$08VKz!8rXwnR-Q*
```

When you make an MQTT connection by using an API key, ensure that the following guidelines are applied:

- The MQTT client ID is in the format: a:*orgId*:*appId*
- The MQTT user name is the API key (for example, a-*orgId*-a84ps90Ajs)
- The MQTT password is the authentication token (for example, MP$08VKz!8rXwnR-Q*)


## Publishing device events
{: #publishing_device_events}

An application can publish events as if they came from any registered device, for example:

-  Publish to topic iot-2/type/*device_type*/id/*device_id*/evt/*event_id*/fmt/*format_string*

To send existing data from a device into {{site.data.keyword.iot_short_notm}}, you can create an application to process the data and publish it into {{site.data.keyword.iot_short_notm}}.

**Important:** The message payload is limited to a maximum of 131072 bytes.  Messages that exceed the limit are rejected.

### Retained messages
{{site.data.keyword.iot_short_notm}} organizations are not authorized to publish retained MQTT messages. If an application, gateway, or device sends a retained message, the {{site.data.keyword.iot_short_notm}} service overrides the retained message flag when it is set to true and processes the message as if the flag is set to false.

## Publishing device commands
{: #publishing_device_commands}

An application can publish a command to any registered device, for example:

-  Publish to topic iot-2/type/*device_type*/id/*device_id*/cmd/*command_id*/fmt/*format_string*

## Subscribing to device events
{: #subscribe_device_events}

An application can subscribe to events from one or more devices, for example:

-  Subscribe to topic iot-2/type/*device_type*/id/*device_id*/evt/*event_id*/fmt/*format_string*

**Note:**  To subscribe to more than one type of event or to events from more than a single device, use the MQTT "any" wildcard character (+) for any of the following components:

- device_type
- device_id
- event_id
- format_string

## Subscribing to device commands
{: #subscribe_device_commands}

An application can subscribe to commands that are being sent to one or more devices, for example:

-  Subscribe to topic iot-2/type/*device_type*/id/*device_id*/cmd/*command_id*/fmt/*format_string*

**Note:** To subscribe to more than one type of event or to events from more than a single device, use the MQTT "any" wildcard character (+) for any of the following components:

-  device_type
-  device_id
-  cmd_id
-  format_string

## Subscribing to device status messages
{: #subscribe_device_status}

An application can subscribe to monitor the status of one or more devices, for example:

-  Subscribe to topic iot-2/type/*device_type*/id/*device_id*/mon

**Note:** To subscribe to updates from more than one device, use the MQTT "any" wildcard character (+) for any of the following components:

- device_type
- device_id

## Subscribing to application status messages
{: #subscribe_app_status}

An application can subscribe to monitor status of one or more applications, for example:

- Subscribe to topic iot-2/app/*appId*/mon

**Note:** To subscribe to updates for all applications, use the MQTT "any" wildcard character (+) for the *appId* component.

## Monitoring messages
{: #monitoring_messages}

{{site.data.keyword.iot_full}} sends monitoring messages when a device or application connects, disconnects, or fails to connect.  These monitoring messages can be subscribed to by gateways and applications.  Use monitoring messages to keep track of connection status and to debug connection issues.  All monitoring messages are sent as QoS=0.  

When a device or application connects successfully, a retained message of type Action=Connect is sent.

When a device or application fails to connect, a non-retained message of type Action=FailedConnect is sent.  A failed connection might be due to an authorization error, or because the client disconnects between the server receiving the first packet and the server completing authorization.

When a device disconnects, a retained message of type Action=Disconnect is sent.  These messages are also sent during server recovery for devices which were connection at the time of failure.

When a application disconnects, a non-retained message of type Action=Disconnect is sent. {{site.data.keyword.iot_full}} also clears all existing retained messages associated with this disconnected application.

For gateways, subscribe to topic *iot-2/type/{type}/id/{id}/mon* to receive monitoring messages for devices that are in the resource group belonging to that gateway.

For applications, subscribe to topic *iot-2/type/{type}/id/{id}/mon* to see monitoring messages for devices that are in the resource group belonging to that application, or in the application's organization if the device does not have a group. To receive monitoring messages for other applications in the same organization, subscribe to topic *iot-2/app/{appid}/mon*.

The monitoring message is a JSON object which contains the following fields:

|Name|Data type|Optional|Description|
|:---|:---|:---|:---|
|``Action``|`Connect`, `FailedConnect`, `Disconnect`|No|The type of message|
|``Time``|ISO timestamp|No|The time the monitoring message is sent|
|``ClientAddr``|String|No|The IP address of the client as known to the server|
|``ClientID``|String|No|The clientID of the client|
|``Port``|Integer|No|The port number on the server|
|``Secure``|Boolean|No|Whether the connection uses TLS|
|``Protocol``|`mqtt3`, `mqtt4`, `mqtt5`, `mqtt3-ws`, `mqtt4-ws`, `mqttv5-ws`|No|The protocol used.  The `-ws` suffix indicates that it uses websockets.|
|``User``|String|Yes|The user name, if specified|
|``Certname``|String|Yes|The client certificate common name, if specified|
|``ConnectTime``|ISO timestamp|Yes|The time of connection|
|``CloseCode``|Integer|Yes|The closing code of the connection.  This is not used by `Connect` or if the code is 0 (good)|
|``Reason``|String|Yes|The closing reason string.  This is not used by `Connect` or by if the code is 0 (good)|
|``ReadBytes``|Integer|No|The bytes sent from client to server|
|``ReadMsg``|Integer|No|The messages sent from client to server|
|``WriteBytes``|Integer|No|The bytes sent from server to client|
|``WriteMsg``|Integer|No|The messages sent from server to client|


## {{site.data.keyword.iot_short_notm}} messaging connection closing codes
{: #connection_closing_codes}

When a messaging connection to {{site.data.keyword.iot_short_notm}} closes, the reason for the closure is logged.  The reason is made up of a Reason Code and a Reason String. The Reason Code and Reason String information is included in the monitoring messages that are sent when a connection is closed. 

The Reason Code is an integer value and can be used to parse log or monitoring messages. The Reason String can vary for the same Reason Code, so is not suitable for parsing or monitoring messages.

The Reason String is a human readable string which provides the reason for the disconnect. This reason can vary for the same Reason Code, to provide additional information. Therefore, use the Reason Code for automated parsing. The extra information in the Reason String usually follows a colon (:) and might contain labels for the data.

The following table shows the most common closing reasons:

|Reason Code|Reason String|Description|
|:---|:---|:---|
|0|The connection has completed normally|The client sent a disconnect action to the server. Other connection closes indicate that the close is not normal.|
|91|The connection was closed by the client|The connection was closed by something other than the server.  The connection was closed by something other than the server, for example because an error was received on the socket.  The Reason String may include more information on the error.|
|92|The connection was closed by the server|The connection was closed because a problem was found by the server. The client can retry the connection but if the problem might reoccur if a fix is not implemented.|
|93|The connection was closed because the server was shutdown|When a server is shut down, all connections are closed. A server might be shut down when the server ID is updated. The server can consist of multiple physical servers, so connection can close when one of the servers is shut down. You can retry the connection, but it might take some time before the server is available.|
|94|The connection was closed by an administrator|An explicit action was taken by an administrator to close one connection or a set of connections, for example because an organization is disabled.  The administrator might need to take an administrative action to allow a new connection.|
|160|The connection timed out|The client has not sent a packet in the time negotiated between the client and server as a keep alive.|
|180|The operation is not authorized|The connection is closed because the connection or an action that is taken in the connection is not authorized. The problem must be fixed or authorization added before retrying.|
|287|The message size is too large for this endpoint|A message larger than that allowed has been sent by the client causing the connection to be closed.|
|288|The clientID was reused|A second connection was made using the same ClientID.  The initial connection is closed with this reason.  This is normal in the case that a client dropped a connection but the server was not notified.  It indicates an error if two devices are trying to use the same ClientID.|

The following table shows less common closing reasons:

|Reason Code|Reason String|Description|
|:---|:---|:---|
|104|The sever capacity is reached|The server is unable to make or continue with this connection due to server constraints. Try reconnecting at a later time.|
|105|The data from the client is not valid|The MQTT data stream is not valid. This might be due to a problem with the client implementation or a network error.|
|154|Too many producers or consumers in a single connection|The limit on the number of subscriptions in an MQTT connection is exceeded.|
|163|Closed during TLS handshake|The connection was closed before the secure connection completed. This reason is usually not used when the server detects a problem in the credentials.|
|164|No data was received on a connection|A connection was made but no data was sent on the connection within the required time.|
|165|The initial packet is too large|The initial packet exceeds the maximum size.  This can also happen when too many bytes are sent before the connection is authorized.  This might indicate a denial-of-service attack, or a problem with one of the clients.|
|166|The ClientID is not valid|The connection is not allowed because the ClientID is invalid.|
|167|Server not available|The server is temporarily not available.  This reason is returned during the short period of time that the server is up but not accepting connections.  A retry should work.|
|169|Certificate missing|A client certificate is required and is not specified.|
|170|Certificate not valid|A client certificate is returned but is not valid.|
|173|Too many connections for an organizaton|The number of connections exceeds the number allowed for this organization.|
|175|The HTTP Authorization header cannot be changed in a connection|An HTTP messaging connection is commonly only authorized once and subsequent requests should use the same authorization header.|
|176|Authorization request is in delay or too many authentication requests|There are too many authentication requests in the queue so it might take some time for the connection to be authorized. Retry the connection later.|
|271|The length of the message is not correct|The MQTT data is not valid and the length of the message does not match the packet size. This might indicate an error in the client.|
|276|The topic is not valid|The connection is closed because it used a topic name or topic filter which is not valid. Fix the problem before retrying the connection.|

## Tracking connection status using monitoring messages
{: #tracking_connection_status}

To track the connection status of devices by using monitoring messages, subscribe to the monitoring topic. All retained messages are received on subscribing the to the topic. For devices, not including Quickstart, a message type of either Action=Connect or Action=Disconnect is received. If a message is not received for a device on subscribing to the topic, the device state is unknown. To find the state of a specific device, subscribe to the monitoring message for that device.

Keep the subscription open to receive monitoring messages when the connection status changes.  A Connect message shows that the device is connected to {{site.data.keyword.iot_short_notm}}. A Disconnect message means that the device is disconnected from {{site.data.keyword.iot_short_notm}},  unless the CloseCode is set to 288. A 288 CloseCode indicates that the client dropped a connection and a then reconnected by using the same client ID.

## Quickstart restrictions
{: #quickstart_restrictions}
If you are planning to create application code for use with the Quickstart service, the following features are not supported in Quickstart:

- Publishing commands
- Subscribing to commands
- Using the MQTT "any" wildcard character (+) in **device_id** or **appId** components
- MQTT connection over Transport Layer Security (TLS)
- Scalable applications

## Scalable applications
{: #scalable_apps}

By adjusting the way that your applications connect, you can make your {{site.data.keyword.iot_short_notm}} applications more scalable by balancing the load handling of event messages across multiple instances of an application.

The number of clients that are required for optimum load balancing and scalability varies by deployment. To identify the optimum number of clients, you need to stress test your system.

Scalable applications are defined as either non-durable subscriptions or mixed-durability shared subscriptions (Beta).

### Non-durable subscriptions
{: #shared_sub_non_durable}

To enable load balancing, ensure that the application subscription is non-durable and that the client ID in the subscription matches the following format:

<pre class="pre">A:<var class="keyword varname">orgId</var>:<var class="keyword varname">appId</var></pre>
{: codeblock}

Where:
-  The character **A** indicates that the client is a scalable application.
-  *orgId* is the unique six-character organization ID that was generated when you registered the service.
-  *appId* is a user-defined unique string identifier for the client. The string can contain only alphanumeric characters (a-z, A-Z, 0-9) and the dash (-), underscore (_), and dot (.) special characters.


**Important:**
- The client ID must start with an uppercase **A** character to be correctly designated as a scalable application by {{site.data.keyword.iot_short_notm}}.
- Other clients that are part of the scalable application must use the same client ID.
- The clean session value must be set to false (0) for non-durable subscriptions.

### Mixed-durability shared subscriptions
{: #shared_sub_mixed}

The {{site.data.keyword.iot_short_notm}} service extends the MQTT V3.1.1 messaging protocol specification to support mixed-durability shared subscriptions. Shared subscriptions provide load balancing capabilities for applications. A shared subscription might be needed if a back-end enterprise application cannot process the volume of messages that are being published to a specific topic space. For example, when many devices publish messages that are being processed by a single application, it might be necessary to use the load balancing capability of a shared subscription.

For mixed-durability shared subscriptions, ensure that the client ID in the subscription matches the following format:

<pre class="pre">A:<var class="keyword varname">org</var>:<var class="keyword varname">appId</var>:<var class="keyword varname">instanceId</var></pre>
{: codeblock}

Where:
- The character **A**, *orgId*, and *appId* in the client ID are defined in the same way as for [non-durable subscriptions](#shared_sub_non_durable).
- *instanceID* is a string that must be no more than 36 characters and can contain only the following characters:
   - Alpha-numeric characters (a-z, A-Z, 0-9)
   - Dashes ( - )
   - Underscores ( _ )
   - Dots ( . )

**Important:**
- The clean session value can be set to either true (1) or false (0) in mixed-durability shared subscriptions.
- Clients that connect with the instanceId use different subscriptions than clients that connect without the instanceId. Therefore, if you would like multiple clients to connect on a mixed-durability shared subscription, you need to specify the instanceID in all subscriptions.

**Example scenario**

The following scenario is one example of how a non-durable scalable application works in {{site.data.keyword.iot_short_notm}}:

- Client one connects as **A:abc123:myApplication** and subscribes to all device events, which means that client one receives 100% of the device events that are published.
- Client two connects as **A:abc123:myApplication** and also subscribes to all device events, which means that client one and client two both share all of the events that are published. The processing load is shared between client one and client two.
- Client three connects as **A:abc123:myApplication** and also subscribes to all device events, which means that client one, client two, and client three all share the processing load for events.
- Client two and client three then unsubscribe from all device events, which means that only client one receives all device events that are published. While client two and client three are still connected to the service, client one receives 100% of the device events that are published.
- When two or more applications share a subscription, messages might not arrive in the order in which they were sent. For example, message B might arrive at client one before message A arrives at client two, even though message A was sent first.
