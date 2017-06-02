---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} blockchain integration: Exploring smart contracts
<!-- {: #iotblockchain_link} -->
Last updated: 28 June 2016

In this second step of the beta program you will use a Hyperledger development environment to create and test your own smart contracts based on the sample contracts that IBM provides. Once familiar with these, you can expand on them to map your own use cases into deployable chain code.  
{:shortdesc}

Instead of using the pre-deployed simple smart contract in an IBM&reg;-provided IBM Blockchain environment, you will set up your own IBM Blockchain environment to explore the sample contracts. This includes deploying your own Bluemix&trade; IBM Blockchain fabric and installing a Hyperledger development environment.

**Important**  
Blockchain integration support is currently available as part of a limited beta program. Future updates might include changes incompatible with the current version of this feature. Try it out and let us know what you think.

With Watson&trade; {{site.data.keyword.iot_short_notm}} blockchain integration you can upload smart contracts in the form of chain code executables to the IBM Blockchain to run business logic on the device data that is written to the blockchain. The smart contract chain code is developed in GoLang.

The IoT Architecture diagram illustrates the IoT Blockchain beta smart contract exploration set up which consists of the following components:
- Beta user Bluemix organization:
 - {{site.data.keyword.iot_short_notm}} service with IoT Blockchain enabled
 - IBM Blockchain fabric
 - Node-RED application running IoT device simulator  
 **Note:** You can also use a locally deployed Node-RED environment to run the simulator.
- Beta user local environment:
 - Hyperledger development environment to develop and test smart contract chain code. The environment includes GoLang.
 - IoT Blockchain Simple UI
- GitHub environment:
 - IBM-provided GitHub repository for sample smart contracts
 - Beta user GitHub repository for deploying smart contracts to beta user IBM Blockchain fabric

![The IoT Blockchain architecture.](images/architecture_contracts.svg "IoT Blockchain architecture")

## Before you begin
{: #byb}

- Complete the getting started tasks that are provided in the *IoT Blockchain Beta Getting started*   document that is available for download in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81).
- Get an overview of what IBM Blockchain is, how it relates to the general blockchain concept, and what it can do for you:
 * [IBM Blockchain](http://www.ibm.com/blockchain/) on IBM.com and in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81).
 * [IBM Blockchain DOCS](https://console.ng.bluemix.net/docs/services/blockchain/index.html) -  Getting started with IBM Blockchain service. **Note:** The document that you are currently reading contains all necessary instructions to get up and running. There is no need to deploy IBM Blockchain separately from the instructions in the linked IBM Blockchain DOCS documentation.
 * [IBM Blockchain API](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/) -  Get an understanding of the IBM Blockchain API. In this beta, the majority of the steps will be done using IBM tools, and there is not initially any need to use API commands. As your blockchain project evolves however, the need might arise.
 * [IBM Blockchain for Developers](http://www.ibm.com/blockchain/for_developers.html)  - Get an overview of how blockchain fits into your development environment, and get started with some walk-throughs, all of which feature live demos and code that is deployable to run on IBM Bluemix.
 * Get a quick overview of the GoLang produced smart contracts and how they are created and deployed to the blockchain in the *Understand IBM Blockchain smart contract chain code* document that is available for download in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81) and in the sample contracts in the IBM Blockchain contracts GitHub repository at:  https://github.com/ibm-watson-iot/blockchain-samples.  


## Configure your own IBM Blockchain environment
{: #configure_environment}
Before you can begin deploying and testing smart contracts you must set up your own blockchain environment.

1. Log in to your Bluemix organization.  
The Bluemix organization is the container for the services and applications that you will use. Select the **Dashboard** tab to be brought to your organization and default `dev` space.  

2. Create and configure your IBM Blockchain fabric.  
{{site.data.keyword.iot_short_notm}} blockchain integration requires the IBM Blockchain fabric to manage the blockchain ledger, smart contracts, and the general blockchain infrastructure. IoT Blockchain uses IBM Blockchain to manage the chains. If you have access to an existing IBM Blockchain environment you can use it. If not, you must create an instance of IBM Blockchain from [here](https://console.ng.bluemix.net/catalog/services/blockchain/).

 1. From your Bluemix account Dashboard, click **Use services or APIs**.
 2. Locate the experimental section of the service catalog and select **Blockchain**.  
   **Tip:** Click [here](https://console.ng.bluemix.net/catalog/services/blockchain/) to go directly to the IBM Blockchain experimental service page.
 3. On the IBM Blockchain service page, verify the Add Service selections:  
    - Space - If you have more that the default `dev` space, verify that you are deploying the service in the intended space.
    - App - Leave unbound.
    - Service name - Optionally change the service name to something that is easy to remember. This name is displayed in the IBM Blockchain tile in the Bluemix dashboard.
    - Selected plan - For the IoT Blockchain beta select the Free plan. The free plan provides two validating peers and one certificate authority.
  4. Click **Create** to deploy IBM Blockchain to your Bluemix services.  
  The blockchain is deployed with two peer nodes initially. You can add more nodes as needed.

4. Link {{site.data.keyword.iot_short_notm}} with your IBM Blockchain service  
    To write to the blockchain from {{site.data.keyword.iot_short_notm}} you must first link the services.
     1. In Bluemix, go to Dashboard
     2. Select the space in which you deployed IBM Blockchain.
     3. Click the **Blockchain** tile.
     4. In the left pane, click **Service credentials**.
     5. Click **Add credentials** to create a new set of service credentials and give them a descriptive name such as "IoT-Platform-integration."
     6. In the JSON formatted service credentials, select one peer and locate and copy `api_host` and `api_port` local. Then select one user of type 1 (client) and copy `username` and `secret` local.  
     ```
     {
      "credentials": {
      "peers": [
      {
       "discovery_host": "169.44.63.203",
       "discovery_port": "32904",
       "api_host": "169.44.63.203",
       "api_port": "32905",
       "type": "peer",
       "network_id": "f621cde2-bdec-4897-b737-da4df144c41f",
       "id": "f621cde2-bdec-4897-b737-da4df144c41f_vp1",
       "api_url": "http://169.44.63.203:32905"
      },
       ...
      ],
      "users": [
      {
       "username": "user_type1_fa8e6ef0dc",
       "secret": "33401036a9"
      },
       ...
       ]
      }
     }
     ```  
     **Important:** The user you select should not have been previously registered with a peer other than the peer you selected.
     7. Click **Back to Dashboard** to return to your Bluemix dashboard.
     8. Select the space in which you deployed {{site.data.keyword.iot_short_notm}}.
     9. Click the **{{site.data.keyword.iot_short_notm}}** tile.
     10. Click **Launch** to open the {{site.data.keyword.iot_short_notm}} dashboard.
     11. From the {{site.data.keyword.iot_short_notm}} dashboard select **Settings > Connections** by clicking ![Settings.](images/platform_settings.png "Settings") in the menu side bar.
     12. Click the Blockchain switch to enable Blockchain proxy.  
    Fabric connection fields will automatically appear in the page, replacing the table.
     14. Enter the following information to connect to the fabric:
      - Fabric Name - Enter a name to identify the fabric in {{site.data.keyword.iot_short_notm}}.
      - Peer Address - Enter the `api_host` address
      - Port Number - Enter the `api_port` number
      - User ID - Enter the `username` string
      - User Secret - Enter the `secret` string
     15. Click **Confirm all changes**  
  The fabric table is populated with the new entry.  

## Create, test, and deploy your own smart contracts
{: #test_contracts}

You can now create your own smart contract chain code in GoLang, test it in the sand box environment and finally deploy and test it on your own IBM Blockchain fabric.
**Tip:** As a starting point you can use a number of sample IBM-created smart contracts that are available in the IBM Blockchain contracts GitHub repository.   

1. Create a GitHub project to store your smart contract chain code.  
The smart contracts that you want to deploy must be in a public GitHub repository. For more information, see https://github.com/.
2.  Set up a local Hyperledger development and testing environment  
To develop and test your own chain code before deploying it to IBM Blockchain you must set up a local development environment. This environment includes GoLang which is used to write the chain code for your contracts.
 1. Set up the development environment.  
 The development environment includes the tools that you need to develop your smart contracts using chain code build in GoLang. For more information, go here:  
 https://github.com/hyperledger/fabric/blob/master/docs/dev-setup/devenv.md
 2. Install a chain code debugging environment.   
 The debugging environment provides you with the tools that you need to test and debug your smart contracts before you deploy them to IBM Blockchain. For more information, go here:   https://github.com/hyperledger/fabric/blob/master/docs/API/SandboxSetup.md
 3. Set up a network for development.   
 The network for development provides you with a stricter, production-like, environment for final testing of your smart contracts.  Use this environment for final testing of your tested and debugged contracts before you deploy them to IBM Blockchain. For more information, go here:  
 https://github.com/hyperledger/fabric/blob/master/docs/dev-setup/devnet-setup.md

3. Download the IBM-provided sample smart contracts.  
IBM provides a number of smart contracts that you can download and use directly as-is, or modify to suit your organization goals.  
To download the sample contracts:
 1. Go to the IBM Blockchain contracts GitHub repository at: https://github.com/ibm-watson-iot/blockchain-samples/  
 The simple_contract and trade_lane_contract folders contain the simple and beta contracts respectively.
 3. Use `git clone` in the terminal to clone the https://github.com/ibm-watson-iot/blockchain-samples project.  
 **Tip:** You can also download a compressed file of the project by clicking **Download ZIP** from the project page.

6. Create and test a smart contract   
 With {{site.data.keyword.iot_short_notm}} blockchain integration you can upload smart contracts in the form of chain code executables to the IBM Blockchain to run business logic on the device data that is written to the blockchain. The smart contract chain code is developed in GoLang.  
 The sample beta contract is focused on wiring IoT device data for events of interest into a blockchain for sharing with third parties or to create tamper resistant log entries. For an overview of smart contract architecture, see the *Understand IBM Blockchain smart contract chain code* document that is available for download in the [Watson IoT blockchain Beta Program Community](https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=cf921b26-ee7f-4c10-a445-89d51d432fa1#fullpageWidgetId=W6b3a7a280b13_46e4_94a7_cabb83baaa81) .
2. Create the contract executables.  
  The contract code needs to be converted to an executable for it to be deployed on the blockchain.  
  For the sample contracts, do the following:
   1. Open your command line and navigate to the contract folder.
   2. Run the `go generate` command.  
   This will cause any ‘go generate’ commands that exist in the code to be executed.  Go generate is a go program  tool that allows pre-build code generation. In our example contracts, we use go generate to create the schemas.go and sample.go files.  These expose the schema of the contract to the consumer of the contract and an example of the same, respectively.
   2. Run the `go build` command.  
   This is the build command. It creates an executable in the folder in the same name as the folder name. This is the contract executable that will be deployed on the blockchain.

6. Test the smart contract in the Hyperledger sand box  
  Before you deploy your finished smart contract to IBM Blockchain, you can test and debug the chain code in the Hyperledger sand box that you installed as part of your development environment.  

6. Deploy the smart contract chain code to IBM Blockchain  
 When you have tested and verified your contract locally you can deploy it to your IBM Blockchain fabric to test.
  1. Upload your contract to your public GitHub repository.  
  For example, upload the sample.go file to:  
  `http://github.com/{my organization}/{my project}/`
  2. Register the contract with the peer that you connected to earlier.  
  Use a REST client like CURL or Postman to submit the register call. For more information about the register call, see the [POST registrar API documentation](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Registrar/registerUser). Use the following information when registering:
  <ul>
  <li>URL: `http://api_host:api_port/registrar`
  <li>Type: POST
  <li>Header: `Content type: application/x-www-form-urlencoded`
  <li>Payload:  
  ```
   {
        "enrollId": "`username`"    
        "enrollSecret": "`secret`"  
   }
   ```
  </ul>
  3. Deploy the contract to the peer  
  For more information about the deploy call see the [POST devops/deploy API documentation](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Devops/chaincodeDeploy).  
  Use the following information when deploying:  
  <ul>
  <li>URL: `http://api_host:api_port/devops/deploy`
  <li>Type: POST
  <li>Header: `Content type: application/x-www-form-urlencoded`
  <li>Payload:  
  ```
  {
      "type": "GOLANG",   
      "chaincodeID": {  
      "path": "http://github.com/{my organization}/{my project}/sample.go",
      "name": "string"
    },
    "ctorMsg": {  
      "function": "init",  
      "args": [
        "{\"version\":\"1.0\}"}"
      ]
    },
    "secureContext": "'username'",
    "confidentialityLevel": "PUBLIC"
  }
  ```  
  </ul>  
  Your contract is deployed to the fabric.  
  **Important:** Copy local the returned contract ID in the form of a 128-character alphanumeric string. You will need the contract ID to map devices to the contract.  

10. Map the device data to the new smart contract  
  To start writing device data to the new blockchain smart contracts you must first map device data to the contracts.  
   1. In Bluemix, go to Dashboard
   2. Select the space in which you deployed {{site.data.keyword.iot_short_notm}}.
   3. Click the **{{site.data.keyword.iot_short_notm}}** tile.
   4. Click **Launch** to open the {{site.data.keyword.iot_short_notm}} dashboard.
   5. Select **Blockchain**  by clicking ![Blockchain.](images/platform_blockchain.png "Blockchain") in the menu side bar.
   6. Click **Link Contract**.
   6. Select the fabric name for the fabric that you created earlier.
   7. Enter the following information:  
     - Contract ID - Paste in the 128-character contract ID that you saved when you deployed the contract.
     - Contract Name - Enter a name to identify the contract in {{site.data.keyword.iot_short_notm}}.
     - Select the device type for which you want to store device data in the blockchain.
     - Select the event name for the events that you want to store.  
     > **Tip:** The default event name for the Node-RED blockchain device is obc. To find the event types for a device, go to the **Devices** page and click the device name to open the device details page. Scroll down to the **Sensor Information** section to see a list of the available events and data points for the device. You can change the event name that the Node-RED device publishes by updating the Topic field in the Publish to IoT mqtt out node.

   11. Map the available device datapoints to contract parameters.   
   **Important:** Do not include the `d.` that prepends the data point in the device message. Also verify that the data type for each data point that you map corresponds to the data type that is required by the contract parameter that you map it to. For example, a contract property such as Asset ID of the type string must be mapped to a data point of type string. The contract parameter requirements are defined in the `type` definitions in the contract go-code.  
   For example, the simple IBM-provided contract has the following contract parameter requirements:  
    <ul>
    <li> Latitude - float64
    <li>  Longitude - float64
    <li>  AssetID - string
    <li>  Location - Geolocation  
    <li>  Temperature - float64  
    <li>  Carrier - string   
    </ul>
   12. In the summary page, verify that the information is correct.
   13. The device data to contract mapping shows up in the Blockchain page.

7. Test your smart contract in IBM Blockchain.  
In order to test your smart contract, perform an end-to-end test, similar to the initial simple contract test by creating a device in {{site.data.keyword.iot_short_notm}}, connecting your device to {{site.data.keyword.iot_short_notm}}, configure IoT Blockchain to connect to your blockchain fabric, configure {{site.data.keyword.iot_short_notm}} to map and store your device messages in the blockchain, with the IBM Blockchain console you can view the blockchain to see the device data in the ledger. If your contract supports readAsset(), you should be able to use the simple UI to view your blockchain. You can also <a href="#monitoringUI">install the more advanced Monitoring UI</a> to further explore how the IoT Blockchain integration works. With this technique, you will be able to view your own device data from your own scenario being stored indelibly in a blockchain.

### Install the Monitoring UI
{: #monitoringUI}

Use the IoT Blockchain monitoring UI to view the blockchain activity and data for your assets.
1. Verify that Node.js is installed on the client computer on which you will run the IoT Blockchain monitoring UI.  
The monitoring UI requires Node.js. You can install it from https://nodejs.org/.  
2. Download the IoT Blockchain monitoring UI project.  
  Use `git clone` in the terminal to clone the https://github.com/ibm-watson-iot/blockchain-samples project.  
  The monitoring UI component is in the monitoring_ui folder.  
  **Tip:** You can also download a compressed file of the project by clicking **Download ZIP** from the project page.  
3. Install the monitoring UI.  
 Run the following command from the `monitoring_ui/` directory of the project: `npm install`  
4. Run and access the monitoring UI.
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

5. Configure the monitoring UI to connect to IBM Blockchain
 The configuration can be accessed by clicking on the **CONFIGURATION** button located near the top right of the screen. This will bring you to the configuration form, which allows you to configure the UI.  
 Use the following information to connect to a contract:
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
<td>`http://peer_URL:port`</td>
<td>The host and port for the IBM Blockchain REST API prepended with `http://`. Use the  `api_host` address and `api_port` number. </td>
</tr>
<tr>
<td>Chaincode ID</td>
<td>The contract ID that was returned when you registered the contract.</td>
<td>The contract ID is a 128-character alphanumeric string that corresponds to the Contract ID entry.  
**Important:** As you cut-and-paste the contract ID, make sure that no spaces are included in the ID. If the ID is incorrectly entered, the UI will display the blockchain ledger entries, but the asset search function will not work.
</td>
</tr>
<tr>
<td>Secure Context</td>
<td>Your fabric user</td>
<td>This is required for connecting to IBM Blockchain instances on Bluemix. Use thesecureContextentry.  
**Important:** The secureContext should be the `username` that you used to configure the fabric.
</td>
</tr>
<tr>
<td>Number of blocks to display</td>
<td>A positive integer. Default: 10</td>
<td>The number of blockchain blocks to display.
</td>
</tr>
</tbody>
</table>

3. In the monitoring UI, verify that your setup is working as expected by interacting with the blockchain with the monitoring UI components:  
 - Chaincode operations  
 Verify that the contract-specific chaincode operations can be run as expected. For example, verify that running a `createAsset` function results in an asset being added to the blockchain.
 - Response Payloads  
 Verify that peer request responses show up as expected when you submit REST requests with the Chaincode Operations tab.
 - Blockchain  
Verify that blocks are added to the chain when you inject data from a linked device or when you use the Chaincode Operations component.    

## Next steps
{: #next_steps}

You have now deployed and explored the sample IBM-provided smart contracts. However, the simple- and beta contracts are only scraping the surface of the sea of possibilities that well designed smart contract chain code opens up. Now is the time to experiment and map your business scenarios to chain code contracts in IBM Blockchain, and then use {{site.data.keyword.iot_short_notm}} with IoT Blockchain integration to write device data to the blockchain ledger and execute business logic stored as smart contracts in response to the data.     

To get you started and provide you with even more examples, take a look at the
[IoT Trade Lane Demo Contract](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/trade_lane_contract) that is available in the IBM-provided GitHub repository at:   https://github.com/ibm-watson-iot/blockchain-samples/.

Happy block chaining!
