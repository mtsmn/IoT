---

copyright:
  years: 2017
lastupdated: "2017-11-08"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guide 4: Simulating a large number of devices
In the first guide, you set up a basic device simulator to manually simulate one or more conveyor belts. In this guide, we expand on this simulation by adding large numbers of self-running simulators to your environment to test analytics and monitoring in a more realistic, multi-device environment.
{:shortdesc}

**Important:** The application requires 512 MB of memory, which is more than is allocated by default and which also exceeds the amount that is available to free trial accounts, including the {{site.data.keyword.Bluemix}} Trial Account and Standard Account. Subscription and Pay-As-You-Go account holders can increase the allocated memory to 512 MB. Free trial account holders need to upgrade to a Subscription or Pay-As-You-Go account. For more information about {{site.data.keyword.Bluemix_notm}} account types, see [Account types](/docs/pricing/index.html#pricing).

**Note:** Bluemix is now IBM Cloud. Check out the [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![External link icon](../../../icons/launch-glyph.svg "External link icon") for more details.

## Overview and goal
{: #overview}

In this guide, you will set up an {{site.data.keyword.Bluemix_notm}} application to simulate multiple conveyor belt devices using a  [Node-RED ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://nodered.org){: new_window} "back end".

The application contains three flows that:   
1. Create multiple devices.   
2. Simulate concurrent event publishing.
3. Delete the devices.   

![Device creation and deletion](images/device_creation.png "Device creation and deletion")

![Device event simulation](images/device_event_simulation.png "Device event simulation")

By completing the instructions that are in this guide, you will:
- Use Cloud Foundry to deploy a Node-RED based and webhook enabled device simulator application.
- Use API calls to create and register devices, publish device events, and delete devices.

**Important:** Simulating large numbers of devices that concurrently send device data to {{site.data.keyword.iot_short_notm}} might use up a large amount of data. You can use the {{site.data.keyword.iot_short_notm}} *Usage* dashboard to monitor how much data your devices and applications are using. The metrics are refreshed on a 2-hourly interval.

## Prerequisites
{: #prereqs}  
You will need the following accounts and tools:

* [{{site.data.keyword.Bluemix_notm}} account](https://console.ng.bluemix.net/registration/) with    
 - More than 512 MB RAM   
 - More than 1 GB disk quota  
 - Two services available
* [Cloud Foundry Command Line Interface (cf CLI) ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Use the cf CLI to deploy and manage your {{site.data.keyword.Bluemix_notm}} applications.
* Optional: [Git ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://git-scm.com/downloads){: new_window}  
If you choose to use Git to download the code samples you must also have a [GitHub.com account ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com){: new_window}. You can also download the code as a compressed file without a GitHub.com account.

For any of the REST calls in the steps that follow, you can either use cURL or the REST client add-on plugin that is available in Mozilla.  
{: tip}

## Step 1 - Deploy the application to {{site.data.keyword.Bluemix_notm}}
{: #step1}

Follow the steps below to create and deploy your app manually.   

1. Clone the *Lesson4* sample app GitHub repository.  
Use your favorite git tool to clone the following repository:  
https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
In Git Shell, use the following command:
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
```
3. Configure the application for your environment by editing the manifest.yml file.  
What to edit:
 - To use an existing {{site.data.keyword.iot_short_notm}} service, update all instances of `lesson4-simulate-iotf-service` to reflect that service name. For example, if you are using the {{site.data.keyword.iot_short_notm}} service from guide 1, use `iotp-for-conveyor` for the service name.    
 - Set the device name and host.   
In the applications section, change the `name` and `host` entries to something that is unique, such as `YOUR_NAME-lesson4-simulate`.   
**Tip:** The ROUTES URL that you use to access the app is created from the `host` entry, for example: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plan: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
1. From the command line, set your API endpoint by running the cf api command.   
Replace the `API-ENDPOINT` value with the API endpoint for your region.
   ```
cf api API-ENDPOINT
   ```
Example: `cf api https://api.ng.bluemix.net`  
<table>
<tr>
<th>Region</th>
<th>API Endpoint</th>
</tr>
<tr>
<td>US South</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>United Kingdom</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. Log into your {{site.data.keyword.Bluemix_notm}} account.
  ```
cf login
  ```
If prompted, select the organization and space where you want to deploy {{site.data.keyword.iot_short_notm}} and the sample app.
6. Create the required services in {{site.data.keyword.Bluemix_notm}}.   
 1. Use the following command to create the cloudantNoSQLDB Lite service:
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. Use the following command to create the {{site.data.keyword.iot_short_notm}} service.
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**Important:** If you are using an existing {{site.data.keyword.iot_short_notm}} service, and you updated the manifest.yml file accordingly, make sure that you use the existing service name. For example, from guide 1, use  `iotp-for-conveyor` instead of `lesson4-simulate-iotf-service` in the cf call.
7. Run the `cf push` command to build the project and push to your organization.  
Your sample application is deployed on {{site.data.keyword.Bluemix_notm}}.  
When deployment is completed, a message is displayed to indicate that your app is running.   
Example:  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. Get the service credentials for your app.
 1. Log in to {{site.data.keyword.Bluemix_notm}} at:  
 [https://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net){: new_window}.
 2. Select the account and space where you deployed the app.
 3. From the menu, select **Apps** and then select **Dashboard**.
 4. Under Cloud Foundry Apps, click the name of the application that you just deployed.
 4. Select **Connections**.
 5. Locate the iotf-service that you are using with your app and click **View credentials**.  
 6. On the service credentials page, locate `apiKey` and `apiToken`.   
The API key and authentication token are used as credentials when you use the app API to create and run your simulated devices.

## Step 2 - Secure your Node-RED flow
{: #step2}

Although this step is optional, it's good practice to secure your Node-RED flow.

1. Secure your editor so that only authorized users can access it.
2. To secure the Node-RED flow, go to the {{site.data.keyword.Bluemix_notm}} dashboard and select the application that you previously deployed.
3. Click **Runtime** and then click **Environment variables**.
4. Add the following user-defined environment variables and their respective values:
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. Save the flow.

When your Node-RED flow is secure, if you plan to use REST API commands to create, delete, or simulate multiple devices, then you must provide the Node-RED user name and password credentials during basic authentication.

## Step 3 - Create and connect devices
{: #step3}

You can use the Node-RED flow interface or the application rest API to complete the following tasks.

### Node-RED  
To register multiple devices:  
1. Log in to {{site.data.keyword.Bluemix_notm}} at:  
[https://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net){: new_window}.
2. Select the account and space where you deployed the app.
3. From the menu, select **Apps** and then select **Dashboard**.
4. Under Cloud Foundry Apps, click the **ROUTE** URL of the application that you just deployed.  
The ROUTE_URL is built from the `host` entry that you used in the manifest.yml file: `HOST.mybluemix.net`  
Example: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
The Node-RED interface opens.
5. Click **Go to your Node-RED flow editor**.
6. In the flow editor, select the **Device Type and Instance** tab.
7. To register five devices, click the inject node labeled **Register 5 motorController devices**.
8. Verify that your devices are registered.
 1. In your {{site.data.keyword.iot_short_notm}} dashboard, from the menu, select **Boards**.
 3. Select the **Device Centric Analytics** board.
 4. Locate the **Devices I Care About** card.  
The device names are displayed.

### REST API  
To register multiple devices:  

1. Make an HTTP POST request to the following URL: `ROUTE_URL/rest/devices`  
Example: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - Set 'Content-Type' and 'Accept' to 'application/json'  
 - Use the following JSON payload:  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
  Where:  
    - numberDevices is the number of devices to create and register.
    - Optional: typeId is the device type that the devices will be registered as. If typeId is not provided, it defaults to "iotp-for-conveyor". **Tip:** You can enter any device type name, but the other guides in the series expect devices of device type `iotp-for-conveyor`. If you use a different device type, you must modify the settings in the guides accordingly.
    - Optional: authToken is the authorization token the devices will register with.
    - Optional: If chunkSize is not provided, it is set to 500 by default. The 'chunksize' must be less than and a factor of numberDevices.
    - deviceName is the pattern for the deviceID for the created devices.
2. Verify that your devices are registered.
 1. In your {{site.data.keyword.iot_short_notm}} dashboard, from the menu, select **Boards**.
 3. Select the **Device Centric Analytics** board.
 4. Locate the **Devices I Care About** card.  
The device names are displayed.

## Step 4 - Simulate device events
{: #step4}

Because the simulated devices are registered with {{site.data.keyword.iot_short_notm}}, you can now run the simulator to start sending device events.

### Node-RED  
To send device events:  
1. In the Node-RED flow editor, select the **Simulate multiple devices** tab.
7. To simulate five devices, click the inject node that is labeled **Simulate 5 devices**.
8. Verify that your devices are sending data.
 1. In your {{site.data.keyword.iot_short_notm}} dashboard, from the menu, select **Boards**.
 3. Select the **Device Centric Analytics** board.
 4. Locate the **Devices I Care About** card.  
 5. Select one of the devices and verify that updated device data points that correspond to the published message are displayed in the **Device Properties** card.  


### Rest API  
To send device events:

1. Make an HTTP POST request to the following URL: `ROUTE_URL/rest/runtest`  
Example: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - Set 'Content-Type' and 'Accept' to 'application/json'  
 - Use the following JSON payload:   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
Where:
    - numberDevices is the number of devices that you want to simulate data for.
    - numberEvents is the number of events that each simulated device sends.
    - timeInterval is the spacing of the events in milliseconds.
    - deviceType is the device type that you created simulated devices for.
    - deviceName is the pattern for the deviceID for the created devices.
8. Verify that your devices are sending data.
 1. In your {{site.data.keyword.iot_short_notm}} dashboard, from the menu, select **Boards**.
 3. Select the **Device Centric Analytics** board.
 4. Locate the **Devices I Care About** card.  
 5. Select one of the devices and verify that updated device data points that correspond to the published message are displayed in the **Device Properties** card.   

## Step 5 - Deleting devices
{: #deleting}

### Node-RED  
To delete devices:  
1. In the Node-RED flow editor, select the **Device Type and Instance** tab.
2. To delete five devices, click the inject node that is labeled **Delete 5 devices**.
3. Verify that your devices are deleted.
 1. In your {{site.data.keyword.iot_short_notm}} dashboard, from the menu, select **Boards**.
 3. Select the **Device Centric Analytics** board.
 4. Locate the **Devices I Care About** card.  
 5. Verify that devices are no longer listed.  


### Rest API  

1. Make an HTTP POST request to the following URL: `ROUTE_URL/rest/deleteDevices`  
Example: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - Set 'Content-Type' and 'Accept' to  'application/json'  
 - Use the following JSON payload:      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName": "belt"  
}</code></pre>
2. Verify that your devices are deleted.
 1. In your {{site.data.keyword.iot_short_notm}} dashboard, from the menu, select **Boards**.
 3. Select the **Device Centric Analytics** board.
 4. Locate the **Devices I Care About** card.  
 5. Verify that devices are no longer listed.  


## What's next?
{: @whats_next}  
Jump to another topic that interests you:
- [Guide 2: Using basic real-time rules and actions](getting-started-iot-rules.html)  
Now that you successfully set up your conveyor belt, connected it to {{site.data.keyword.iot_short_notm}}, and sent some data, it is time to make that data work for you by using rules and actions.
- [Guide 3: Monitoring your device data](getting-started-iot-monitoring.html)  
Now that you connected one or more devices and started making good use of the device data, it is time to start monitoring a collection of devices.
- [Learn more about {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Learn more about {{site.data.keyword.iot_short_notm}} APIs](/docs/services/IoT/reference/api.html){:new_window}
