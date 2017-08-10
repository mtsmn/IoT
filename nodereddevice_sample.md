---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Creating and connecting a Node-RED device simulator
Use Node-RED to create a device simulator to send simulated device data to your {{site.data.keyword.iot_full}} organization.  
{:shortdesc}

## Overview

Node-RED is a tool that is used for wiring together hardware devices, APIs, and online services in new and interesting ways. For more information, see the [Node-RED ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://nodered.org/){: new_window} web site.  

You can run your Node-RED instance in your own environment or use it as a {{site.data.keyword.Bluemix_notm}} application. The following steps include instructions for {{site.data.keyword.Bluemix_notm}}.

Create and connect the Node-RED device simulator to send MQTT device messages to {{site.data.keyword.iot_short_notm}}. In the following example, the device simulator simulates sending data for a freight container to an MQTT broker, for example {{site.data.keyword.iot_short_notm}}.

## Steps

1. Create the Node-RED device simulator by completing the following steps:   
    1. Log in to {{site.data.keyword.Bluemix_notm}} at: https://console.ng.bluemix.net
    2. Select the **Catalog** tab.
    3. Locate the Boilerplates section of the service catalog and click  **Internet of Things Platform Starter**. **Tip:** Click  [here ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window} to go directly to the Internet of Things Platform Starter page.
    4. On the Internet of Things Platform Starter page, select the space where you want to deploy Node-RED, verify the Create an app selections, and click **Create** to add Node-RED to your Bluemix organization. For example:
    <ul>
     <li> Space: dev
     <li> App name: myDevice
     <li> Host name: myDevice  
    <ul>  
Leave the other options as the default values. After the application is deployed, the Start coding with Node-RED page is displayed.
**Note:** The staging process might take a few minutes.  

2. Click the Routes link to open Node-RED or Visit App URL, for example `http://myDevice.mybluemix.net`  
3. Complete the following the steps to **Secure your Node-RED editor**:
    1. Click **Next**
    2. Add a username and password.  
    **Note:** For the password you must meet the required complexity rules, otherwise the **Next** button stays greyed out.  
    3. Optional. Check **Allow anyone to view the editor, but not make any changes** or **Not recommended: Allow anyone to access the editor and make changes**
    4. Click **Finish** to complete the configuration.
4. Click  **Go to your Node-RED flow editor**
5. Enter the username and password that you created previously.  
The device simulator flow is made available to the flow editor. The flow simulates a **Thermostat** that sends its location, measured temperature and humidity data to {{site.data.keyword.iot_short_notm}}.  
6. Confirm that the device is connected by checking that a dot with connected message is displayed for the **Send to IBM IoT Platform** node. The check is also valid for the **IBM IoT App In** node for the **Temperature Monitor** subflow.  
7. Send and receive MQTT device messages by completing the following steps:  
    1. Click **Send Data** inject node to trigger data to be sent to {{site.data.keyword.iot_short_notm}}.
       **Note:** You can activate the **Debug output payload** debug node to see what data is sent and to check the **Device payload** function node to see the code that builds the payload. 
    2. After you click **Send Data**, the MQTT messages are sent to {{site.data.keyword.iot_short_notm}} and are received by the **IBM IoT App In** node. The **Temperature Monitor** subflow establishes whether the temperature is within defined limits and displays a message on the debug tab. 
       **Note:** Click the **temp thresh** switch node to check and change the threshold values
    3. Check the debug tab. A message is displayed, for example **Temperature (17) within safe limits**.
    
You have now connected your sample IoT device to {{site.data.keyword.iot_short_notm}} and can see device data.
