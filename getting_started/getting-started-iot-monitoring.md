---

copyright:
  years: 2017
lastupdated: "2017-06-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="\_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guide 3: Monitoring
Now that you have connected one or more devices and started making good use of the device data, it is time to start monitoring a collection of devices and the real-time data they are sending.
{:shortdesc}

## Overview and goal
{: #overview}  

In this guide you will deploy a monitoring application on {{site.data.keyword.Bluemix_notm}} to view data from your devices.

As in guide 1 you can follow one or both of two paths:
- Path A: [Step 1A - Deploy and connect the monitoring web application](#deploy_app)  
By following path A you will deploy a ready-made app that monitors the conveyor belt devices that you are running in your organization.
- Path B: [Step 1B - Create a monitoring user interface using the widget library](#widget-library)
This slightly more complex path introduces the widget library and leads you through some basic user interface creation.

## Prerequisites
{: #prereqs}  
Before you begin, make sure that the following requirements are fulfilled.

### Local environment
- [Node.js ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://nodejs.org){: new_window} version 6.x or higher.
- Path A: [Angular CLI ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/angular/angular-cli){: new_window} version 1.x or higher.  
- [Cloud Foundry CLI ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window}

### Other requirements
You must also have a connected device of device type `iot-conveyor-belt` that sends events with event name `sensorData` and with a message payload that includes the following properties:
```
{
	"d": {
		"id": "belt1",
		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
For more information about device events and messaging format, see [Publishing events](../devices/mqtt.html#publishing_events).  
If you have completed [Guide 1: Getting started with {{site.data.keyword.iot_short_notm}} and a simulated conveyor belt](getting-started-iot-conveyor.html) you are all set.  
{: tip}

## Step 1A - Deploy and connect the monitoring web application
{: #deploy_app}

The Plant Floor Monitoring sample app lists all iot-conveyor-belt type devices that are connected to your {{site.data.keyword.iot_short_notm}} organization together with a subset of the event data such as RPM, last updated, and device ID.

The sample app is built using the Node.js client libraries at: [https://github.com/ibm-watson-iot/iot-nodejs ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![Monitor app](images/app_monitor.png "Monitor app")

As part of this step you will:
- Deploy a GitHub-sourced sample monitoring web application using Cloud Foundry.
- Configure the sample app to connect to {{site.data.keyword.iot_short_notm}} using an API key and Authentication Token.
- Use the web application to monitor your connected conveyor belt devices.  

### Detailed steps

1. Clone the Node.js *Plant Floor Monitoring* sample app GitHub repository.  
Use your favorit git tool to clone the following repository:  
https://github.com/theiliad/IBM-IoTP-Monitoring-Control
2. Log into your {{site.data.keyword.Bluemix_notm}} account using the cloudfoundry CLI.  
For more information, see the [Cloud Foundry CLI documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
From the command line, enter the following command:  
  ```
cf login
  ```
If prompted, select the organization and space where you want to deploy the monitoring sample app.
3. If necessary, set your API endpoint by running the cf api command.   
Replace the `<API-endpoint>` value with the API endpoint for your region.
  ```
cf api <API-endpoint>
  ```
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
<tr>
<td>Sydney</td>
<td>https://api.au-syd.bluemix.net</td>
</tr>
</table>
3. Change the directory to the directory in which the sample app is located.  
  ```
cd IBM-IoTP-Monitoring-Control
  ```
4. Run `npm run push` to build the project and push to your organization.  
Your sample web application is deployed on {{site.data.keyword.Bluemix_notm}}.  
When deployment completes, a message displays to indicate that your app is running.   
For example:  
  ```
requested state: started
instances: 1/1
usage: 64M x 1 instances
urls: iotmonitoringcontrol-undertided-eng.mybluemix.net
last uploaded: Tue May 16 19:01:13 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     state     since                    cpu    memory     disk        details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
1. Create an API Key and Authentication Token combination for your app.  
You will need these when you configure the app to connect to your organization. For more information about registering devices, see [Connecting applications](../platform_authorization.html).
 1. Open the {{site.data.keyword.iot_short_notm}} dashboard.
 2. Select **Apps**.
 3. Click **Generate API Key**
 4. Copy the API Key and Authentication Token values that are listed.
 5. Select **Operations Application** as the API role.
 6. Add a comment so that you can easily identify this API Key and Authentication Token combination.
 7. Click **Generate**.
3. Configure your app to connect to {{site.data.keyword.Bluemix_notm}}.
 1. Log in to {{site.data.keyword.Bluemix_notm}} at:  
 [https://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net){: new_window}.
 2. Select the account and space where you deployed the monitoring sample app.
 3. From the menu, select **Apps** then **Dashboard**.
 4. Click the **IoTMonitoringControl** Cloud Foundry App name.
 4. Select **Runtime**.
 5. Select **Environment variables**.
 6. Scroll down to the **User defined** section.
 7. Click **Add** to add a new item.
 8. For *Name* enter `basicConfig`  
 9. For *Value* enter:
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
Replace the parameter values with the corresponding values for your {{site.data.keyword.Bluemix_notm}} organization: orgID, API Key, and Authentication Token that you just created.  
For more information about environment variables, see [Adding user-defined environment variables ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/manageapps/depapps.html#ud_env){: new_window} in the {{site.data.keyword.Bluemix_notm}} docs.
 10. Click **Save** to complete the creation of the user defined environment variable.
The app is now connected to {{site.data.keyword.Bluemix_notm}}.
3. Open the web app.  
From the {{site.data.keyword.Bluemix_notm}} apps dashboard click **Visit App URL** to open the web application.  
Access and manage the application URL by clicking **Routes**.   
The default URL is similar to:  
`https://iotmonitoringcontrol-{randomstring-eng}.mybluemix.net`

## Step 1B - Create a monitoring user interface using the widget library

{: #widget-library}
### Detailed steps

1. Clone the *Widget Library Monitoring* sample app GitHub repository.  
Use your favorit git tool to clone the following repository:  
https://github.ibm.com:wiotp-toolingdevx/lesson3b
In Git shell, use the following command:
```
git clone git@github.ibm.com:wiotp-toolingdevx/lesson3b.git
```
2. Install the application dependencies  
The application needs a number of dependent
Navigate to the root of the repository and run the following command:
```
npm install
```
3. Construct the user interface  

 1. Add an RPM gauge
    1. Open the public/index.html template
    2. Find the place holder for rpm gauge in the line no:17
    3. Added dev element with an unique id as given below
 ```html
 <div id="rpmgauge" ></div>
 ```  
    4. Update the javascript to create the widget
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iotp-for-conveyor", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. Adding gauge for Accelerometer
    1. Find the place holder for accelerometer gauge in the line no:26
    2. Added dev element with an unique id as given below
 ```html
 <div id="aygauge" ></div>
 ```  
    3. Update the javascript to creat the widget
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iotp-for-conveyor", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
          },
          show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. Adding gauge for motor speed chart
    1. Find the place holder for motor speed chart in the line no: 35
    2. Added dev element with an unique id as given below
 ```html
 <div id="speedchart" ></div>
 ```  
    3. Update the javascript to creat the widget
 ```javascript
 WIoTPWidget.CreateGauge("speedchart ","sensorData", "iotp-for-conveyor", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. Deploying it to bluemix  
 1. Update the Watson IoT platform service name that you have created in the previous lesson in the manifest.yml  
<pre><code>
declared-services:
  < your iot platform service name >:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- < your iot platform service name >  </br>
</pre></code>
 2. Run the following command to push the application into bluemix
```
cf push <appname>
```
**Note:**  Above steps will work when widget library is publicly available. You can run the application locally by following steps
 3. Add application credentials under config folder with the following json
``` javascript
config/default.json
{
  "api-key" : "Your app key",
  "auth-token" : "your auth-token"
}
```
 4. Start the application  
 `npm run start`
5. Open the widget library based web app.  
From the {{site.data.keyword.Bluemix_notm}} apps dashboard click **Visit App URL** to open the web application.  
Access and manage the application URL by clicking **Routes**.   
The default URL is similar to:  
`https://SOMETHING.mybluemix.net`

## Step 2 - View your connected devices
{: #view_devices}

With the web console up and running you can now view your connected conveyor belt devices.
1. From the web console **Devices** section, verify that your conveyor belts are listed and that the correct RPM and Running state is displayed.
2. Change the RPM value for your conveyor belt device and verify that the value updates as expected on the monitoring app.


## What's next?
{: @whats_next}
- Path A: Modify the monitoring app to suit your needs.  
For technical details, see:
 - [https://github.com/theiliad/IBM-IoTP-Monitoring-Control/blob/master/README.md ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/theiliad/IBM-IoTP-Monitoring-Control/blob/master/README.md){: new_window}
 - [Node.js clienmt libraries ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- Path B: Modify the widget library app to suit your needs.  
For technical details, see:
 - [https://github.ibm.com/wiotp-toolingdevx/lesson3b/blob/master/README.md ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.ibm.com/wiotp-toolingdevx/lesson3b/blob/master/README.md){: new_window}
- [Guide 4: Simulating lots of devices](getting-started-iot-large-scale-simulation.html)  
Expand on the basic simulation by adding large numbers of self running simulators to your environment. This will let you test drive the basic analytics and monitoring from the previous guides in a more realistic, multi-device environment.
- [Learn more about {{site.data.keyword.iot_short_notm}}](../../services/IoT/iotplatform_overview.html){:new_window}
- [Learn more about {{site.data.keyword.iot_short_notm}} APIs](../../services/IoT/reference/api.html){:new_window}
