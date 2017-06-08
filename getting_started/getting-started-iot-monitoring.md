---

copyright:
  years: 2017
lastupdated: "2017-06-08"

---

{:shortdesc: .shortdesc}
{:new_window: target="\_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guide 3: Monitoring your device data
Now that you have one or more devices connected, it is time to start monitoring device data in real time.
{:shortdesc}

## Overview and goal
{: #overview}  

In this guide, you will deploy a monitoring application on {{site.data.keyword.Bluemix_notm}} to view data from your devices.

As in guide 1, you can follow one or both of the following paths:
- Path A: [Step 1A - Deploy and connect the monitoring web application](#deploy_app)  
By following path, A you will deploy a ready-made app that monitors the conveyor belt devices that you are running in your organization.
- Path B: [Step 1B - Create a monitoring user interface using the widget library](#widget-library)
The slightly more complex path B introduces the widget library and leads you through some basic user interface creation.

## Prerequisites
{: #prereqs}  
Before you begin, make sure that the following requirements are fulfilled.

### Local environment
- [Node.js ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://nodejs.org){: new_window} version 6.x or higher.
- Path A: [Angular CLI ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/angular/angular-cli){: new_window} version 1.x or higher.  
- [Cloud Foundry CLI ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Use the cf CLI to deploy apps and services to {{site.data.keyword.Bluemix_notm}}. For more information, see the [Cloud Foundry CLI documentation![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
- Optional: [Git ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://git-scm.com/downloads){: new_window}  
If you choose to use Git to download the code samples you must also have a [GitHub.com account ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com){: new_window}. You can also download the code as a compressed file without a GitHub.com account.

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
For more information about device events and messaging format, see [Publishing events](/docs/services/IoT/devices/mqtt.html#publishing_events).  
If you completed [Guide 1: Getting started with {{site.data.keyword.iot_short_notm}} and a simulated conveyor belt](getting-started-iot-conveyor.html), you are all set.  
{: tip}

## Step 1A - Deploy and connect the monitoring web application
{: #deploy_app}

The Plant Floor Monitoring sample app lists all iot-conveyor-belt type devices that are connected to your {{site.data.keyword.iot_short_notm}} organization together with a subset of the event data such as RPM, last updated, and device ID.

The sample app is built using the Node.js client libraries at: [https://github.com/ibm-watson-iot/iot-nodejs ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![Node.js based monitoring app](images/app_monitor.png "Node.js based monitoring app")

As part of this step, you will:
- Deploy a GitHub-sourced sample monitoring web application by using Cloud Foundry.
- Configure the sample app to connect to {{site.data.keyword.iot_short_notm}} by using an API key and authentication token.
- Use the web application to monitor your connected conveyor belt devices.  

### Detailed steps
The following steps guide you through creating and deploying the app on {{site.data.keyword.Bluemix_notm}}. For information about running the app locally, see the README file in GitHub.
1. Clone the Node.js *Plant Floor Monitoring* sample app GitHub repository.  
Use your favorite git tool to clone the following repository:  
https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-angular   
In Git shell, use the following command:
  ```bash
git clone https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-angular
  ```
2. Create an API key and authentication token combination for your app.  
You will need these when you configure the app to connect to your organization. For more information about registering devices, see [Connecting applications](/docs/services/IoT/platform_authorization.html).  
 1. Open the {{site.data.keyword.iot_short_notm}} dashboard.
 2. Select **Apps**.
 3. Click **Generate API Key**
 4. Copy the API key and authentication token values that are listed.
 5. Select **Visualization Application** as the API role.  
**Tip:** If you add more features to the application, you'll need to elevate it to a higher role.
 6. Add a comment so that you can easily identify this API key and authentication token combination.
 7. Click **Generate**.
3. Configure your app to connect to {{site.data.keyword.Bluemix_notm}}.
Navigate to the root of the *iot-guide-conveyor-ui-angular* repository and create a `basicConfig.json` file that contains the following content:
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
Replace the parameter values with the corresponding values for your {{site.data.keyword.Bluemix_notm}} organization: orgID, API key, and authentication token that you just created.  
Example:
```
 {   
  "org": "3v5whr",    
  "apiKey": "a-3v5whr-jhkmsghlqv",  
  "apiToken": "-q0MkPN2cNYB6+?ISk"  
}
```
4. Log into your {{site.data.keyword.Bluemix_notm}} account by using the cloudfoundry CLI.  
For more information, see the [Cloud Foundry CLI documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
From the command line, enter the following command:  
  ```
cf login
  ```
If prompted, select the organization and space where you want to deploy the monitoring sample app.
5. If necessary, set your API endpoint by running the cf api command.   
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
6. Change the directory to the directory in which the sample app is located.  
  ```
cd iot-guide-conveyor-ui-angular
  ```
7. Run `npm install -g @angular/cli` to install the Angular CLI.
8. Run `npm install`.
9. Run `npm run push` to build the project and push to your organization.  
Your sample web application is deployed on {{site.data.keyword.Bluemix_notm}}.  
When deployment is completed, a message is displayed to indicate that your app is running.   
Example:  
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
10. Open the web app.  
From the {{site.data.keyword.Bluemix_notm}} apps dashboard, click **Visit App URL** to open the web application.  
Access and manage the application URL by clicking **Routes**.   
The default URL is similar to:  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## Step 1B - Create a monitoring user interface by using the widget library
{: #widget-library}

The widget library based sample app includes a motor speed gauge, an accelerometer data gauge, and a motor speed diagram that displays the data for a single iot-conveyor-belt type devices that is connected to your {{site.data.keyword.iot_short_notm}} organization. You can use the sample code to build a full front end application for your {{site.data.keyword.iot_short_notm}} connected devices.

![Widget library based monitoring app](images/app_monitor_b.png "Widget library based monitoring app")

As part of this step, you will:
- Deploy a GitHub-sourced sample web application by using Cloud Foundry.
- Configure the sample app to connect to {{site.data.keyword.iot_short_notm}} by using an API key and authentication token.
- Configure three user interface widgets to display device data as gauges and graphs.
- Use the web application to monitor your connected conveyor belt device.  

### Detailed steps
The following steps guide you through creating and deploying the app on {{site.data.keyword.Bluemix_notm}}. For information about running the app locally, see the README file in GitHub.
1. Clone the *Widget Library Monitoring* sample app GitHub repository.  
Use your favorite git tool to clone the following repository:  
https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-html
In Git Shell, use the following command:
```
git clone https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-html
```
2. Install the application dependencies.  
Navigate to the root of the *iot-guide-conveyor-ui-html* repository and run the following command:
```
npm install
```
3. Construct the user interface.  
To build the application user interface, you must add the widgets as JavaScript code in the application index.html file for each user interface component.  
Each widget uses the following JavaScript parameters:  
`WIoTPWidget.CreateGauge("name","event name", "orgID", "deviceID", "property" , ADDITIONAL_WIDGET_SETTINGS)`
<ul>
<li>name - The name of the widget, which appears in the application.
<li>event name - The device event name that includes the property to display.
<li>orgID - The ID of [your {{site.data.keyword.iot_short_notm}} organization](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}.
<li>deviceID - The ID of the device that supplies the data to display.
<li>property - The device message payload property to display.
<li>ADDITIONAL_WIDGET_SETTINGS -  One or more additional parameters for the widget, see examples.
</ul>
For details on each widget type, see the examples that follow and the documentation in the [IoT Widgets GitHub repository ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-widgets){: new_page}.
 1. Add an RPM gauge.  
This gauge displays the conveyor belt rpm as a gauge that has a minimum of 0 and a maximum of 5 rpm.
    1. Open the following template for editing: `public/index.html`  
    2. Locate the rpm gauge placeholder: `<!--- place holder for rpm gauge  -->`
    3. Add the following div element with a unique id as shown:
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. Locate the JavaScript placeholder: `/// Add your scripts here`
    4. Add the rpm JavaScript code.  
Example:  
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
 2. Add an acceleration gauge.  
This gauge displays the accelerometer reading as a gauge that has readings between -1 and 1.
    1. Locate the accelerometer gauge placeholder: `<!--- place holder for accelerometer gauge  -->`
    2. Add the following div element with an unique id as shown:
 ```html
 <div id="aygauge" ></div>
 ```  
    3. Locate the javascript placeholder: `/// Add your scripts here`
    4. Add the accelerometer JavaScript code.  
Example:   
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
 3. Add a motor speed chart.  
This chart displays the motor speed as a line diagram.
    1. Locate the motor speed gauge placeholder: `<!--- place holder for motor speed gauge  -->`
    2. Add the following div element with an unique id as shown:
 ```html
 <div id="speedchart" ></div>
 ```  
    3. Locate the JavaScript placeholder: `/// Add your scripts here`
    4. Add the speedchart JavaScript code.  
Example:  
 ```javascript
 WIoTPWidget.CreateGauge("speedchart ","sensorData", "iotp-for-conveyor", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. Deploy the application to {{site.data.keyword.Bluemix_notm}}  
 1. Update the manifest.yml file with your {{site.data.keyword.iot_short_notm}} service name.  
For example, if you deployed a {{site.data.keyword.iot_short_notm}} service as part of [Guide 1: Connecting a conveyor belt device](getting-started-iot-monitoring.html) then YOUR_PLATFORM_NAME is `iotp-for-conveyor`.
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. Log into your {{site.data.keyword.Bluemix_notm}} account by using the cloudfoundry CLI.  
 For more information, see the [Cloud Foundry CLI documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
 From the command line, enter the following command:  
   ```
 cf login
   ```
 If prompted, select the organization and space where you want to deploy the monitoring sample app.
 5. If necessary, set your API endpoint by running the cf api command.   
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
 6. Change the directory to the directory in which the sample app is located.  
   ```
 cd iot-guide-conveyor-ui-html
   ```
 2. Run the cf push command to push the application into {{site.data.keyword.Bluemix_notm}}:  
Give your application a unique name.
```
cf push YOUR_APP_NAME
```
5. Open the widget-library-based web app.  
From the {{site.data.keyword.Bluemix_notm}} apps dashboard, click **Visit App URL** to open the web application.  
Access and manage the application URL by clicking **Routes**.   
The default URL is similar to:  
`https://YOUR_APP_NAME.mybluemix.net`

## Step 2 - View your connected devices
{: #view_devices}

Now that the web console is up and running, you can view your connected conveyor belt devices.
1. From the web console **Devices** section, verify that your conveyor belts are listed and that the correct RPM and Running state is displayed.
2. Change the RPM value for your conveyor belt device and verify that the value are updated as expected on the monitoring app.


## What's next?
{: @whats_next}  
Continue with the next guide or jump to another topic that interests you:
- Path A: Modify the monitoring app to suit your needs.  
For technical details, see:
 - [https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-angular/blob/master/README.md ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-angular/blob/master/README.md){: new_window}
 - [Node.js clienmt libraries ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- Path B: Modify the widget library app to suit your needs.  
For technical details, see:
 - [https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-html/blob/master/README.md ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-guide-conveyor-ui-html/blob/master/README.md){: new_window}
- [Guide 4: Simulating a large number of devices](getting-started-iot-large-scale-simulation.html)  
Expand on the basic simulation by adding large numbers of self-running simulators to your environment. This expansion will let you test drive the basic analytics and monitoring from the previous guides in a more realistic, multi-device environment.
- [Learn more about {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Learn more about {{site.data.keyword.iot_short_notm}} APIs](/docs/services/IoT/reference/api.html){:new_window}
