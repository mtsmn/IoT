---

copyright:
  years: 2015, 2017
lastupdated: "2016-10-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# HTTP REST API for devices
{: #api}

**Important:** The {{site.data.keyword.iot_full}} HTTP REST API for devices feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Accessing the HTTP REST API documentation
{: #api_link}

To access the {{site.data.keyword.iot_short_notm}} HTTP REST API documentation and obtain more information about how to integrate devices into your organization, see [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/orgAdmin.html){: new_window}.

The only version of the {{site.data.keyword.iot_short_notm}} HTTP REST API that is supported is version 2. Ensure that your {{site.data.keyword.iot_short_notm}} solutions are using version 2.

## Authentication

All requests must include an authorization header. Basic authentication is the only method that is supported. When a device makes an HTTP request through the {{site.data.keyword.iot_short_notm}} HTTP REST API, the following credentials are required:

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
To access the {{site.data.keyword.iot_short_notm}} HTTP REST API documentation, see [{{site.data.keyword.iot_short_notm}} Organization Information Management REST APIs ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html){: new_window}.
