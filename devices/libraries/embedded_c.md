---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Embedded C for device developers
{: #embedded_c}

You can use Embedded C to build and customize devices that interact with your organization on {{site.data.keyword.iot_full}}. Use the information and examples that are provided to start developing your devices by using Embedded C.
{:shortdesc}

## Downloading the Embedded C client and resources
{: #embeddedc_client_download}

To access the Embedded C client libraries and samples for {{site.data.keyword.iot_short_notm}}, go to the [iotf-embeddedc ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/iot-embeddedc){: new_window} repository in GitHub and complete the installation instructions.


## Dependencies
{: #dependencies}

|Dependency |Description|
|:---|:---|
|[Eclipse Paho Embedded C library ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](http://git.eclipse.org/c/paho/org.eclipse.paho.mqtt.embedded-c.git){: new_window} |Provides an MQTT C client library. For more information, see [MQTT Client Package -  C for embedded devices ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](http://www.eclipse.org/paho/clients/c/embedded/){: new_window}.|
|[mbed TLS 2.4.1 ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://github.com/ARMmbed/mbedtls/archive/mbedtls-2.4.1.tar.gz){: new_window} | Provides SSL Library to enable TLS Support and Client Side Certificates based authentication. For more information, check [SSL Library from mbed TLS ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://tls.mbed.org/ssl-library){: new_window}.|



## Installation
{: #installation}

To install the {{site.data.keyword.iot_short_notm}} client library for Embedded C, complete the following instructions:

1. Get the source from github repository:
```
  git clone https://github.com/ibm-watson-iot/iot-embeddedc.git
```
2. Set the environment variable IOT_EMBDC_HOME and run the script to download required dependencies:
```
    export IOT_EMBDC_HOME=$HOME/iot-embeddedc/
    cd $IOT_EMBDC_HOME
    ./setup.sh
```
3. Create directory build within $IOT_EMBDC_HOME Path: 
```  
   mkdir $IOT_EMBDC_HOME/build
```
4. Install CMake Utility if it's not already installed referring to the link - https://cmake.org/

5. Create build directory within $IOT_EMBDC_HOME Path and run CMake and make Utility to build:
```
   mkdir $IOT_EMBDC_HOME/build
   cd $IOT_EMBDC_HOME/build
   cmake ..
   make
```

The iotfclient is client for the IBM Watson Internet of Things Platform service can be connected either as device client or gateway client. At the time of initialization, we need to specify the client type - device client or gateway client. We can use device client to connect to the Watson IoT platform, publish events and subscribe to commands.

## Initializing the client library
{: #initialize_client_library}

After the client library is downloaded, it must be initialized and connected to the {{site.data.keyword.iot_short_notm}}. You can initialize the {{site.data.keyword.iot_short_notm}} client library for Embedded C by passing parameters or by using a configuration file.

### Passing parameters

The `initialize` function uses the following parameters to connect to the {{site.data.keyword.iot_short_notm}} service:

|Definition |Description |
|:---|:---|
|`client`|A pointer to the *iotfclient*.|
|`org`|Your organization ID.|
|`type` |The type of your device.|
|`id` |The device ID.|
|`auth-method` |The method of authentication to be used. The only value that is currently supported is `token`.|
|`auth-token`|An authentication token to securely connect your device to Watson IoT Platform.|
|`serverCertPath` | Custom CA path used to sign the server certificate if there is one otherwise leave blank.|
|`useClientCertificates` | Set to 1 to use client certificates and 0 for not to use client certificates.|
|`rootCACertPath` | CA Certificate(s) path if useClientCertificates=1 otherwise leave blank.|
|`clientCertPath` | Client Certificate Path if useClientCertificates=1 otherwise leave blank.|
|`clientKeyPath` | Client Private Key Path if useClientCertificates=1 otherwise leave blank.|
|`clientType` | 0 for device client and 1 for gateway client.|


```
   #include "deviceclient.h"
   ....
   ....

   iotfclient client;

   if(!useCerts)
	rc = initialize(&client,orgId,"internetofthings.ibmcloud.com",devType,            
	                devId,"token",devToken,NULL,useCerts,NULL,NULL,NULL,0);
   else
	rc = initialize(&client,orgId,"internetofthings.ibmcloud.com",devType,
			devId,"token",devToken,NULL,useCerts,rootCACertPath,clientCertPath,clientKeyPath,0);
   ....

```

### Using a configuration file

You can also use a configuration file to initialize the Embedded C client library. The `initialize_configfile` function takes the configuration file path as a parameter.

```
   #include "deviceclient.h"
   ....
   ....
   char* configFilePath="./device.cfg";
   iotfclient client;

   rc = initialize_configfile(&client, configFilePath,0);
   free(configFilePath);

   ....
```

The configuration file must use the following format:

```
	org=$orgId
	domain=$domainName
	type=$myDeviceType
	id=$myDeviceId
	auth-method=token
	auth-token=$token
	serverCertPath=$customServerCertificateCAPath
	useClientCertificates=0 or 1
	rootCACertPath=$rootCACertPath if useClientCertificates=1 otherwise leave blank
	clientCertPath=$clientCertPath if useClientCertificates=1 otherwise leave blank
	clientKeyPath=$clientKeyPath if useClientCertificates=1 otherwise leave blank
```

## Connecting to the service
{: #connecting_service}

After you initialize the {{site.data.keyword.iot_short_notm}} Embedded C client library, you can connect to the {{site.data.keyword.iot_short_notm}} by calling the `connectiotf` function.

```
   #include "deviceclient.h"
   ....
   ....
   char* configFilePath="./device.cfg";
   iotfclient client;

   rc = initialize_configfile(&client, configFilePath,0);
   free(configFilePath);

   if(rc != SUCCESS){
       printf("initialize failed and returned rc = %d.\n Quitting..", rc);
       return 0;
   }

   rc = connectiotf(&client);

   if(rc != SUCCESS){
       printf("Connection failed and returned rc = %d.\n Quitting..", rc);
       return 0;
   }
   ....
```

## Handling commands
{: #handling_commands}

When the device client connects, it automatically subscribes to any command for this device. To process specific commands, you need to register a command callback function by calling the `setCommandHandler` function. The callback function has the following properties:

|Property |Description|
|:---|:---|
|`commandName`  |The name of the command that was invoked. |  
|`format`  |The format of the event. The format can be any string, for example JSON.|
|`payload`  |The data for the command payload. Maximum length is 131072 bytes. |


```
	#include "deviceclient.h"
	....
	....

	void myCallback (char* commandName, char* format, void* payload)
	{
		printf("------------------------------------\n" );
		printf("The command received :: %s\n", commandName);
		printf("format : %s\n", format);
		printf("Payload is : %s\n", (char *)payload);

		printf("------------------------------------\n" );
	}
	 ...
	 ...
	 char* configFilePath="./device.cfg";
	 iotfclient client;
	 ....

	 rc = connectiotf(&client);

	 if(rc != SUCCESS){
	    printf("Connection failed and returned rc = %d.\n Quitting..", rc);
	    return 0;
 	}
	subscribeCommands(&client);
 	setCommandHandler(&client, myCallback);
 	yield(&client,1000);
 	....

```
**Note:** The ``yield()`` function enables the device to receive commands from the Watson IoT Platform and keeps the connection alive. If the ``yield()`` function is not called within the time frame that is specified by the keepAlive interval, then any commands that are sent from the platform will not be received by the device. The value that is assigned to the ``yield()`` function specifies length of time (in milliseconds) that data can be read from the socket before control is returned to the application.

## Publishing events
{: #publishing_events}

Events can be published with the following properties:

|Property |Description|
|:---|:---|
|eventType  |The type of event that is published, for example status or  gps. |  
|eventFormat  |The format can be any string, for example `json`. |
|data  |The data for the payload. Maximum length is 131072 bytes. |
|QoS  |The Quality of Service level for the publish event. Supported values are `0`, `1`, `2`.|


```
	#include "deviceclient.h"
 	....
	rc = connectiotf (&client);
 	char *payload = {\"d\" : {\"temp\" : 34 }};

 	rc= publishEvent("status","json", "{\"d\" : {\"temp\" : 34 }}", QOS0);
 	....
```

## Disconnecting the client
{: #disconnect_client}

To disconnect the client and release the connections, run the following code snippet:

```
	#include "deviceclient.h"
 	....
 	rc = connectiotf (org, type, id , authmethod, authtoken);
 	char *payload = {\"d\" : {\"temp\" : 34 }};

 	rc= publishEvent("status","json", payload , QOS0);
 	...
 	rc = disconnect(&client);
 	....
```

## Samples
{: #samples}

There are couple of sample programs available in the samples directory under $IOT_EMBDC_HOME directory. Before running them,

1. Update the configuration files as described in the above sections.
2. To enable logging, set the envrionment variable IOT_EMBDC_LOGGING - `export IOT_EMBDC_LOGGING=ON`
3. If IOT_EMBDC_HOME is set, then $IOT_EMBDC_HOME/iotclient.log is used for logging otherwise ./iotclient.log is used.
4. Run helloWorld sample being in the path $IOT_EMBDC_HOME/build:
```
	./samples/helloWorld orgID deviceType deviceId token useCerts caCertsPath clientCertPath clientKeyPath
```
5. Run Device Sample being in the path $IOT_EMBDC_HOME/build:
```
	./samples/sampleDevice
```
