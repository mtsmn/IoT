---

copyright:
  years: 2015, 2017
lastupdated: "2017-03-29"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# HTTP REST API for applications
{: #api}

Use the {{site.data.keyword.iot_full}} HTTP REST API to build and customize applications that interact with your organization on {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

## Capabilities
{: #capabilities}

The {{site.data.keyword.iot_short_notm}} HTTP REST API supports the following capabilities and functions for applications:

- Organization information retrieval
- Bulk device operations (list, add, and remove)
- Device type operations (list, create, delete, view details, and update)
- Device operations (list, add, remove, view details, update, view location, and view management information)
- Device diagnostic operations (clear logs, retrieve logs, add log information, delete logs, get specific logs, clear error codes, get device error codes, and add error codes)
- Connection problem determination (list device connection log events)
- Last event cache (view the last event for a specific device)
- Device management request operations (list device management requests, initiate requests, clear request status, get details of a request, list all request statuses by device, and get the request status for a specific device)
- Usage management operations (retrieve the total amount of data used)
- Device event publishing (beta)
- Service status querying (retrieve service statuses by organization)

## Accessing the HTTP REST API documentation
{: #api_link}

To access the {{site.data.keyword.iot_short_notm}} HTTP REST API documentation and obtain more information about how to build and customize your applications, see [APIs](../reference/api.html).

## Authentication

All requests must include an authorization header. Basic authentication is the only method that is supported. Applications are authenticated by using API keys. When an application makes any request through the  {{site.data.keyword.iot_short_notm}} HTTP REST API, the following credentials are required:

```
username = API key (for example, a-orgId-a84ps90Ajs)
password = Authentication token
```

## Content-Type request headers

A `Content-Type` request header must be provided with the request. The following table shows how the supported types are mapped to the {{site.data.keyword.iot_short_notm}} internal formats.

|Content-Type header|Format in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"
