---

copyright:
  years: 2016

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

# Creating applications with Node-RED

Creating applications by using Node-RED allows you to process real-time events from devices in your {{site.data.keyword.iot_full}} organization. The Node-RED application can be extended to respond to real-time event data by sending commands to devices.
{:shortdesc}

## Prerequisites

Before you create an application with Node-RED, complete the following tasks:

- Deploy an instance of {{site.data.keyword.iot_short}}.
- Add devices to your {{site.data.keyword.iot_short}} which are sending data to the service.

## Creating a Node-RED application for {{site.data.keyword.iot_short}}

1. Create a Node-RED instance by:
  a. From your {{site.data.keyword.bluemix_notm}} dashboard, click **Create App** then click **Web**.
  b. Select **Browse Boilerplates**.
  c. Click on the **Internet of Things Platform Starter**.
  d. Choose an app name and a host name and take note of them, then click **Create**.
2. Bind an instance of the {{site.data.keyword.iot_short}} to your Node-RED application.
  a. From your {{site.data.keyword.bluemix_notm}} dashboard, click on your Node-RED application.
  b. Click on **Bind a Service or API**.
  c. Select your {{site.data.keyword.iot_short}} service from the list and click **Add**.
3. After your Node-RED application has restaged, enter the following URL in a browser, replacing `host_name` with your Node-RED host name:
```
http://host_name.mybluemix.net/
```
You will see the Node-RED landing page. Click **Go to your Node-RED flow editor**. The editor will open with a sample flow already loaded.
4. Double-click the **IoT App In** node. The configuration menu will open.
5. Configure the node with the following parameters:
```
Authentication: Bluemix Service
Input Type: Device Event
Device Type: ALL
Device ID: ALL
Event: ALL
Format: ALL
```
The **IoT App In** node can be configured to subscribe to events from specific devices.
6. Click **Deploy** in the upper right of the screen. Look at the debug pane to see events and messages received from devices registered to your {{site.data.keyword.iot_short}} service.

## Customizing the Node-RED Application

The **IoT App Out** node can be used to send commands to a device connected to your {{site.data.keyword.iot_short}} service.

Node-RED has a number of nodes which can be applied to create 'triggers' for other interactions. For example, a Node-RED application could be built to send a command to a device if data from a device sensor reached a specified threshold.
