---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-27"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:important: .important}

# {{site.data.keyword.iot_short_notm}} Feature Overview
{: #feature_overview}

<p>The {{site.data.keyword.iot_short_notm}} pricing plans were updated on November 27, 2018.   
For more information, including upgrade information, see [{{site.data.keyword.iot_short_notm}} service plans](plans_overview.html). The contents of this [IBM Cloud documentation collection](https://console.bluemix.net/docs/services/IoT/) pertain to the {{site.data.keyword.iot_short_notm}} Lite plan, and to the previous Standard and Advanced Security plans. For documentation about the {{site.data.keyword.iot_short_notm}} Connection and Analytics Service plans, with their extended feature set, see the [{{site.data.keyword.iot_short_notm}} knowledge center ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/overview/overview.html).
</p>
{: important}

The {{site.data.keyword.iot_full}} is built on the following key areas:

  1. Connect - Connect devices and develop applications.
  2. Information Management - Store, normalize, transform, and review device data and integrate your {{site.data.keyword.iot_short_notm}} with other services.
  3. Analytics - Specify rule conditions that are based on real-time device data to trigger alerts and actions.
  4. Risk Management - Configure secure connectivity and architecture with access control for users and applications.

## Connect
{: #connect}

{{site.data.keyword.iot_short_notm}} Connect is the starting point for any {{site.data.keyword.iot_short_notm}} service. Connecting devices, creating applications, controlling your devices, and interacting with third-party services are all available under {{site.data.keyword.iot_short_notm}} Connect.

### Gateway devices

By using a gateway, you can connect devices to the {{site.data.keyword.iot_short_notm}} that otherwise cannot connect to the Internet. Gateways have all the functions of a device, but can also publish and subscribe on behalf of the devices connected to them. Gateway devices allow groups of sensors which cannot connect to the Internet to connect to your {{site.data.keyword.iot_short_notm}} by sending their data to a gateway. For more information, see [developing for gateways](https://console.ng.bluemix.net/docs/services/IoT/gateways/gw_dev_index.html).

### Device management

Device management capabilities are provided through a device management API and a device management agent that is installed on devices. Devices with a device management agent can perform device management actions, which can be triggered through the main {{site.data.keyword.iot_short_notm}} dashboard or the device management API. Device management actions include reboot, download and install firmware updates, and reset devices to factory settings. Device management can also be extended to include custom device management actions. For more information, see the [device management documentation](https://console.ng.bluemix.net/docs/services/IoT/devices/device_mgmt/index.html).

### Extensions and service integrations

Extensions and service integration enable both external services and user-defined extensions of core services to be added to an instance of {{site.data.keyword.iot_short_notm}}. External services which can be integrated with the {{site.data.keyword.iot_short_notm}} include The Weather Company weather location services, allowing you to find the current weather at a device location and Jasper SIM data. For more information on third-party service integrations and extensions, see [integrating external services](https://console.ng.bluemix.net/docs/services/IoT/reference/extensions/index.html).

---

## Information Management
{: #information_management}

{{site.data.keyword.iot_short_notm}} Information Management controls the data that is sent by devices after it reaches your {{site.data.keyword.iot_short_notm}} service. Information management includes data storage and transformation.

### Device last event cache

By using the {{site.data.keyword.iot_short_notm}} Last Event Cache API, you can retrieve the last event that was sent by a device. This works whether the device is online or offline, which allows you to retrieve device status regardless of the device's physical location or use status. Last event data of a device can be retrieved for any specific event that occurred up to 45 days ago.

### Device event data storage

Device event data from your {{site.data.keyword.iot_short_notm}} service can be stored for later use. Data storage is an essential first step towards performing insightful analytics to gain insights from that data.  For example, you can track changes over longer periods of time and store sets of data for use with powerful analytics tools, including use with Watson APIs and cognitive computing. For more information, see [connecting a {{site.data.keyword.cloudant_short_notm}} historian service](https://console.ng.bluemix.net/docs/services/IoT/cloudant_connector.html), or [connecting a {{site.data.keyword.messagehub}} historian service](https://console.ng.bluemix.net/docs/services/IoT/message_hub.html).

### Data management

Different makes and models of devices publish data in different formats. The data management feature lets you transform and normalize this data into a single, logical view called *device state*, which can be understood and processed by applications. Using the data management feature greatly simplifies application development because the application no longer needs to understand the different formats of the event data that is sent from each device. When devices publish events to {{site.data.keyword.iot_short_notm}}, the content of the events can be mapped to user-defined state properties by using mappings. If the inbound event results in a change to the state of the device, the values of the device state properties are updated and stored in {{site.data.keyword.iot_short_notm}}. The values are made available to the application on request by using an HTTP API, or by subscribing to a topic.

For more information about using this feature, see [Introduction to data management](GA_information_management/ga_im_device_twin.html).

---

## Analytics
{: #analytics}

### Edge and cloud analytics

By using {{site.data.keyword.iot_short_notm}} cloud analytics, you specify rule conditions that are based on real-time device data and that trigger alerts and optional actions when met. For example, you might create a rule to ensure that when the device is dropped or when the temperature of the device spikes, an alert is sent to the dashboard on a user's device, and an email is sent to the administrator. For more information, see the [cloud analytics documentation](https://console.ng.bluemix.net/docs/services/IoT/cloud_analytics.html).

With edge analytics, you move the analytics rule-triggering process from the cloud to an edge analytics enabled gateway that might dramatically reduce the amount of device data traffic to the cloud by doing the analytics processing close to the device. For more information, see the [edge analytics documentation](https://console.ng.bluemix.net/docs/services/IoT/edge_analytics.html).

---

## Risk Management
{: #risk_management}

### Secure connectivity and architecture

The architecture of the {{site.data.keyword.iot_short_notm}} is designed to prevent devices from impersonating other devices, which maintains the integrity of your device data. Devices connect to the {{site.data.keyword.iot_short_notm}} by using a combination of a client ID and authentication token, which only you know. After the devices are registered or the API keys are generated, the authentication token is salted and hashed to maintain the security of your credentials.

The Risk and Security Management add-on enables you to enhance {{site.data.keyword.iot_short_notm}} security to ensure that all points of connection between the server and devices are authenticated with proven credentials. With this add-on, certificates and transport layer security (TLS) authentication are used, on top of the {{site.data.keyword.iot_short_notm}} use of user IDs and tokens. During communication between devices and the server, any devices that do not have valid certificates with server access, as configured in the Risk and Security Management add-on, are denied access, even if they use valid user IDs and passwords.

---
