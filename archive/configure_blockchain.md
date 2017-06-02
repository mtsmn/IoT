---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} blockchain integration: Getting started
{: #gettingstartedtemplate}
Last updated: 29 June 2016

{{site.data.keyword.iot_short_notm}} with blockchain enables IoT devices to provide data to blockchain transactions which will store the data in the blockchain's immutable ledger and use it in the business rules implemented in the blockchain's smart contracts. The platform takes device data in its native format, maps it to the data format required by the blockchain's smart contract, and passes it to an organization's {{site.data.keyword.iot_short_notm}} fabric to invoke a transaction.
{:shortdesc}

The simple pre-deployed smart contract lets you map device data points to certain contract attributes to store the data point values in the blockchain ledger. When you have gone through these initial steps and have tested the simple contract, you can try a number of more advanced sample contracts and write contracts of your own.

**Important**  Blockchain integration support is currently available as part of a beta program. Future updates might include changes incompatible with the current version of this feature. To enable {{site.data.keyword.iot_short_notm}} blockchain integration, see [Extending ]

The IoT Blockchain architecture diagram below illustrates the IoT Blockchain beta getting started set up which consists of the following {{site.data.keyword.Bluemix_notm}} components:
- Beta user {{site.data.keyword.Bluemix_notm}} organization:
 - {{site.data.keyword.iot_short_notm}} service with IoT Blockchain enabled
 - Node-RED application running IoT device simulator  
 **Note:** The device simulator can also be deployed in a local Node-RED environment.
- Beta user local environment:
 - Node.js environment
 - IoT Blockchain simple UI
- IBM provided:
 - {{site.data.keyword.iot_short_notm}} fabric with a pre-deployed simple smart contract.

![The IoT Blockchain architecture.](images/architecture.svg "IoT Blockchain architecture")

## Before you begin
{: #byb}

- Request access to the beta.
- Get an overview of what {{site.data.keyword.iot_short_notm}} is, how it relates to the general blockchain concept, and what it can do for you at [{{site.data.keyword.iot_short_notm}}](http://www.ibm.com/blockchain/) on IBM.com.

## Getting started
{: #getting_started}
To enable blockchain integration with {{site.data.keyword.iot_short_notm}}, follow these steps:


1. Enable IoT Blockchain in {{site.data.keyword.iot_short_notm}}.  


2. Link your {{site.data.keyword.iot_short_notm}} service with the IBM-provided {{site.data.keyword.iot_short_notm}} service.  
   To write to the blockchain from {{site.data.keyword.iot_short_notm}} you must first link the services.
 1. From the {{site.data.keyword.iot_short_notm}} dashboard, click ![Settings.](images/platform_settings.png "Settings") in the menu side bar, and then scroll down to the **Extensions** section.
 2. Click the Blockchain switch to enable Blockchain for {{site.data.keyword.iot_short_notm}}.  
 4. Click **Add Fabric**.  
 5. Enter the following information to connect to the IBM-provided fabric:
   <table>
<thead>
<tr>
<th>Parameter</th>
<th>Value</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr>
<td>Fabric Name</td>
<td><i>A name</i></td>
<td>Enter a name to identify the fabric in {{site.data.keyword.iot_short_notm}}.</td>
</tr>
<tr>
<td>Peer Host</td>
<td>530a8196-0f15-4724-ba18-cac76beaee1e_vp1-api.blockchain.ibm.com</td>
<td>The `api_host` address for the Validating Peer 1 server</td>
</tr>
<tr>
<td>Port Number</td>
<td><ul><li>80 (TLS not used)</li><li>443 (TLS used)</li></td>
<td>The `api_port` number</td>
</tr>
<tr>
<td>USE TLS?</td>
<td>On or Off</td>
<td>Use Transport Layer Security to encrypt the communication between {{site.data.keyword.iot_short_notm}} and the contract in the fabric. The default port numbers are set by the deployed {{site.data.keyword.iot_short_notm}} instance that you are connecting to.
</td>
</tr>
<tr>
<td>User ID</td>
<td>user_type0_1bd80d264c</td>
<td>The `username` string for the user that was used to register the smart contract with the blockchain. You will also use this user ID when you configure the Simple UI later on. For the getting started {{site.data.keyword.iot_short_notm}} environment, this user is listed in the Credentials section of the *IoT Blockchain Beta Connections*  document with the comment *Registration with VP1*.</td>
</tr>
<tr>
<td>Secret Key</td>
<td>2e73357def</td>
<td>The `secret` string for the user</td>
</tr>
</tbody>
</table>
**Tip:** Detailed information about the fabric authorization details are provided in the Simple Contract Setup for Production section of the *IoT Blockchain Beta Connections* topic.

 6. Click **Save**  
 **Note:** If the fabric is not created, you might have entered some incorrect information. If needed, click the Blockchain switch to re-enable Blockchain proxy and then create the fabric again.  
 6. Click **Confirm all changes**  
  The fabric table is populated with the new entry.

3. Create the Node-RED device simulator
   Use the blockchain device simulator to send MQTT device messages to {{site.data.keyword.iot_short_notm}}. The device simulator simulated sending data for a freight container to an MQTT broker such as {{site.data.keyword.iot_short_notm}}.
    1. Log in to {{site.data.keyword.Bluemix_notm}} at: https://console.ng.bluemix.net
    2. Select the **Catalog** tab.
    3. Locate the Boilerplates section of the service catalog and click  **Node-RED Starter Community BETA**. **Tip:** Click  [here](https://console.ng.bluemix.net/catalog/starters/node-red-starter/) to go directly to the Node-RED Starter page.
    4. On the Node-RED Starter page, select the space where you want to deploy Node-RED, verify the Create an app selections, and click **Create** to add Node-RED to your {{site.data.keyword.Bluemix_notm}} organization.  
    For example:  
     - Space: dev
     - Name: blockchainDevice
     - Host: blockchainDevice

    Leave the rest of the options at their default values. After the app is deployed, you are brought to the Start coding with Node-RED page.  
    **Note:** The staging process might take a few minutes.

    3. Click the app link at the top of the page to open Node-RED.  
    Example: `http://blockchainDevice.mybluemix.net`
    4. Click **Go to your Node-RED flow editor** to open the editor.
    5. Open the *Node-RED device simulator nodes.txt* file that is available for download in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81).
    6. Copy the Node-RED flow data that you find in the document to your clipboard.
    5. In the Node-RED flow editor, click the menu in the upper right corner and select **Import > Clipboard**.  
    6. Paste the clipboard into the import nodes input field and and click **Ok**.
    The device simulator flow is imported to the flow editor.

4. Connect your Blockchain device to {{site.data.keyword.iot_short_notm}}  
Follow the instructions in [iotplatform_task.html](../iotplatform_task.html) steps to connect the the Node-RED sample device.  
You can use the following data:
- Name

 1. In {{site.data.keyword.Bluemix_notm}}, go to Dashboard
 2. Select the space in which you deployed {{site.data.keyword.iot_short_notm}}.
 3. Click the **{{site.data.keyword.iot_short_notm}}** tile.
 4. Click **Launch dashboard** to open the {{site.data.keyword.iot_short_notm}} dashboard.
 5. Select **Devices**
 6. Click **Add Device**
 7. Click **Create device type**.
 9. Enter a descriptive name and description for the device type, such as `blockchain_device`.
 10. Optional: Enter device type attributes.
 11. Optional: Enter device type meta data.
 12. Click **Create** to add the new device type.
 13. Click **Next** to add your device.
 14. Enter a device ID such as `Device001`.
 15. Optional: Enter device meta data.
 16. Click **Next** to add a device connection with an auto-generated authentication token.
 17. Verify that the summary information is correct, and then click **Add** to add the connection.
 18. In the device information page that opens, copy and save the device information:  
  <ul>
  <li> Organization ID
  <li> Device Type
  <li> Device ID
  <li> Authentication Method
  <li> Authentication Token
  </ul>
  **Tip:** You will need *Organization ID*, *Authentication Token*, *Device Type*, and *Device ID* in the next few steps to finalize the configuration of the Node-RED application to complete the connection.
 19. Open the Node-RED flow editor.
 20. Double-click the Publish to IoT node.
 21. Verify that the topic entry is: `iot-2/evt/obc/fmt/json` **Tip:** The /obc part of the topic sets the event name for the published events. The event name is used when mapping device data to blockchain contracts.
 21. Click the edit icon next to the Server field.
 22. Update the Server name to correspond to your {{site.data.keyword.iot_short_notm}} organization.  
 ```
 {organization_ID}.messaging.internetofthings.ibmcloud.com
 ```  
 Where {organization_ID} is the *Organization ID* that you saved earlier.
 23. Update the  Client ID with your organization ID, device type, and device ID:
 ```
 d:{organization_ID}:{device_type}:{device_ID}
   ```  
   Where {organization_ID}, {device_type}, and {device_ID} where the values that you saved earlier.
 22. Click the **Security** tab.
 23. Verify that the Username field has the value `use-token-auth`.
 23. Update the Password field with the *Authentication Token* that you saved earlier.
 23. Click **Update** and then **OK**.
 23. Optionally, double-click the Asset payload node and update the `var asset = "CON123";` entry with a different asset name. The asset name is included in the blockchain to identify each asset. In this beta environment, chances are that more than one asset will use the default name. Giving your asset a unique name will make it easier to locate the asset in the blockchain.
 23. In the upper right corner of the Node-RED flow editor, click **Deploy**.
 23. Verify that the Publish to IoT node connection status displays *connected*.  **Tip:** If the connection is not made, double check the settings that you entered. Even a small cut and paste error will cause the connection to fail.
 24. In another browser tab or window, open the {{site.data.keyword.iot_short_notm}} dashboard.
 25. Select **Devices** and click **Device001** or the name of the device that you added, if different.  
 The device information page opens. This view lets you see the connection status for your device. The device should show as disconnected at this stage.   
 26. Back in your Node-RED flow editor, click the button on the CON123 node to generate an asset payload.  
 The payload contains the following data points:  
 ```
 { d:{
                 "assetID":CON123,
                 "location":
                    {
                    "longitude": longitude,
    	            "latitude": latitude
                    },
                 "temperature":temperature,
    	         "carrier": carrier
    	    }
        }
 ```
 27. In the debug tab of the right pane, verify that messages are created.  
 28. Back in the {{site.data.keyword.iot_short_notm}} device information page, the device should now be connected. Verify that you see the same data points received from the device.  
 ```
 Event	Datapoint	 Value	Time Received
 obc	d.assetID	CON123	Mar 16, 2016 2:22:18 PM
 obc	d.location.longitude	-87,90	Mar 16, 2016 2:22:18 PM
 obc	d.location.latitude	43,03	Mar 16, 2016 2:22:18 PM
 obc	d.temperature	68	Mar 16, 2016 2:22:18 PM
 obc	d.carrier	C-ABC	Mar 16, 2016 2:22:18 PM
 ```
 You have now connected your sample IoT device to {{site.data.keyword.iot_short_notm}} and can see device data.  

5. Map the device data to the simple smart contract  
Specify the {{site.data.keyword.iot_short_notm}} contract to receive device info.
To start writing device data to the blockchain smart contracts you must first map device data to contracts.
 1. Open the {{site.data.keyword.iot_short_notm}} dashboard.
 2. Select **Blockchain**  by clicking ![Blockchain.](images/platform_blockchain.png "Blockchain") in the menu side bar.
 3. Click **Map Device Data**.
 4. Select the device type for which you want to store device data in the blockchain.
 5. Enter the event name for the events that you want to store.  
 **Tip:** The default event name for the blockchain device is obc. To find the event types for a device, go to the **Devices** page and click the device name to open the device details page. Scroll down to the **Sensor Information** section to see a list of the available events and data points for the device. You can change the event name that the Node-RED device publishes by updating the Topic field in the Publish to IoT mqtt out node.  

 6. Click **Next**.
 6. Select the Fabric Instance that you created earlier.
 7. Enter the contract ID and a contract name to identify the IBM-provided simple contract.  
    <table>
<thead>
<tr>
<th>Parameter</th>
<th>Value</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr>
<td>Contract ID</td>
<td>f9765d5fbb580ef920bdfb95ab8c943a2794a41acc81a28200db72934f01ec53f870f31bae76f0116bd2ac205fa83a219a06ddd940a1c15291793a2c388e5e42</td>
<td>The unique 128-character ID of the mapped contract.</td>
</tr>
<tr>
<td>Contract name</td>
<td><i>A name</i></td>
<td>A name to identify the contract in {{site.data.keyword.iot_short_notm}}.</td>
</tr>
</tbody>
</table>
 **Tip:** The simple contract ID is a 128-character alphanumeric string and is listed in the *Simple Contract (iot_blockchain_example_01) Version 1.0* section of the *IoT Blockchain Beta Connections* topic.

 8. Create a route to map device properties to contract parameters.  
 The parameters that are available in the contract are imported. For each parameter, enter a corresponding event data point.  
 For example:  
 <ul>
  <li>assetID - `assetID` (Required)
  <li>carrier - `carrier`
  <li>temperature - `temperature`
  <li>Location
  <ul>
  <li>location.longitude - `location.longitude`
  <li>location.latitude - `location.latitude`
  </ul>
 </ul>  
 **Important:** Do not include the `d.` that prepends the data point in the device message.
 9. In the summary page, verify that all information was entered correctly.
 10. The device data to contract mapping shows up in the Blockchain page.

Congratulations, you are now up and running!

## Test drive the sample smart contract
{: #test_simple}

Now that all the components are installed, configured, and tied together you can test the end-to-end data flow from device to blockchain ledger. Use the IoT Blockchain monitoring UI to view the blockchain activity and data for your assets.
1. Verify that Node.js is installed on the client computer on which you will run the IoT Blockchain simple UI.  
The simple UI requires Node.js. You can install it from https://nodejs.org/.  
1. Download the IoT Blockchain monitoring UI project.  
  Use `git clone` in the terminal to clone the https://github.com/ibm-watson-iot/blockchain-samples project.  
  The monitoring UI component is in the monitoring_ui folder.  
  **Tip:** You can also download a compressed file of the project by clicking **Download ZIP** from the project page.  
2. Install the monitoring UI.  
 Run the following command from the `monitoring_ui/` directory of the project: `npm install`  
3. Run and access the monitoring UI.
You can run the monitoring UI directly from the file system or by running the webpack-dev-server.  
 - Through the filesystem  
  <ol>
  <li>Run the `npm run build` command from the `monitoring_ui/` directory to generate a bundle.js file in the `monitoring_ui/public` directory.   
   <li>Go to the `monitoring_ui/public` directory and open index.html in a browser.
   </ol>
 - Using the webpack-dev-server.  
  **Note:** This method is not suitable for a production environment.   
  <ol>
 <li>Run the `npm run dev-server` command to load the project on a webpack-dev-server.  
 <li>Open the following URL in a Google Chrome browser: `http://localhost:8081/`  
**Tip:** If you run into an issue with the port already being used, set the PORT environment variable to the port you'd like to use. Note that hot reload is enabled for the webpack-dev-server, so saving changes to the source will be immediately reflected on the UI. No need to reload manually.
</ol>
5. Configure the monitoring UI to connect to {{site.data.keyword.iot_short_notm}}
 The configuration can be accessed by clicking on the **CONFIGURATION** button located near the top right of the screen. This will bring you to the configuration form, which allows you to configure the UI.  
 The connection details are provided in the Simple Contract (iot_blockchain_example_01) Version 1.0 section of the *IoT Blockchain Beta Connections* document that is available for download in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81).
 Use the following information to connect to the IBM-provided simple contract:
 <table>
<thead>
<tr>
<th>Parameter</th>
<th>Value</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr>
<td>API Host and Port</td>
<td>`http://530a8196-0f15-4724-ba18-cac76beaee1e_vp1-api.blockchain.ibm.com:80`</td>
<td>The host and port for the {{site.data.keyword.iot_short_notm}} REST API prepended with `http://`. Use the  `api_host` address and `api_port` number. </td>
</tr>
<tr>
<td>Chaincode ID</td>
<td>f9765d5fbb580ef920bdfb95ab8c943a2794a41acc81a28200db72934f01ec53f870f31bae76f0116bd2ac205fa83a219a06ddd940a1c15291793a2c388e5e42</td>
<td>The simple contract ID is a 128-character alphanumeric string that corresponds to the Contract ID entry.  
**Important:** As you cut-and-paste the chaincode ID, make sure that no spaces are included in the ID. If the ID is incorrectly entered, the UI will display the blockchain ledger entries, but the asset search function will not work.
</td>
</tr>
<tr>
<td>Secure Context</td>
<td>user_type0_1bd80d264c</td>
<td>This is required for connecting to {{site.data.keyword.iot_short_notm}} instances on Bluemix. Use thesecureContextentry.  
**Important:** The secureContext should be the user ID that you used to configure the fabric.
</td>
</tr>
</tbody>
</table>
2. In the Node-RED flow editor, click the button on the CON123 node to inject device data, have it sent as a message to {{site.data.keyword.iot_short_notm}}, and written to the {{site.data.keyword.iot_short_notm}} ledger by the simple contract.   
**Tip:** To get a continual data flow, double-click the inject node, set the Repeat parameter to `interval`, and configure an appropriate interval such as every 1 minutes.
3. In the simple UI, verify that device data shows up as expected in the blockchain blocks.  
  1. Verify that blocks are added to the chain when you inject data from the device.  
  **Important:** Do not use the browser refresh button to refresh the UI. The UI automatically refreshes every few seconds. Using the browser refresh resets the UI settings to their default values, and you must reconfigure the UI to see your contract block chain.
  2. To see the latest ledger information for your asset, in the Asset ID search field, enter the ID of your asset, and click **SUBMIT**. Example: `CON123`  
  To see blockchain data for more than one asset with the same contract, enter that asset name and click **SUBMIT**. Click **RESET** to start over.  
> **Tip:** The default assetID for the blockchain device is "CON123". If you have modified the device message or updated the assetID in the Node-RED device simulator you can look up the assetID in {{site.data.keyword.iot_short_notm}}. Go to the **Devices** page and click your device to open the device details page. Scroll down to the **Sensor Information** section to see a list of the data points for the device. Use the value for the `d.assetID` data point as your assetID.

## Next steps  
{: #next_steps}
You have now installed and configured a basic IoT Blockchain integrated {{site.data.keyword.iot_short_notm}} environment. In this minimal scenario the smart contract lets you write device data to the blockchain ledger to create an indelible device data history. Now that you have gone through these initial steps and have tested the simple contract, you can try a number of more advanced sample contracts and write contracts of your own.    

Instructions for these more advanced steps are provided in the *IoT Blockchain Beta Exploring smart contracts* document that is available for download in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81).
