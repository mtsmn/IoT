---

copyright:
years: 2017
lastupdated: "2017-08-10"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with Data Management by using the Web interface (Beta)
{: #gs_web}

**Important:** The {{site.data.keyword.iot_full}} Data Management Web interface feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} provides online tools to help you normalize and transform data as part of the Data Management feature. In addition to the [Watson IoT Platform Data Management API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} that is provided, you can use the Simple Interface Creator or the Advanced Interface Creator to configure resources and start mapping your device data. The online interface creators guide you through the steps in the user interface.

For information about the Data Management feature and its benefits, see the [Introduction to Data Management](../GA_information_management/ga_im_device_twin.html#device_twins).

For information about the resources that you need to need to configure, see [Understanding Data Management](../GA_information_management/ga_im_definitions.html#definitions_resource).

With the Simple Interface Creator, you add one physical interface and one logical interface is automatically created and the two are mappped together. With the Advanced Interface Creator, you have more control over the configuration. You add one physical interface and one or more logical interfaces, and then add mappings between them.

The interfaces and mappings are added in draft format. After you have added and validated them, you must activate them. For more information about draft and active resources, see [Creating, updating, activating, and deactivating your resources](../GA_information_management/ga_im_definitions.html#draft_active_resources).



## High-level flow
{: #interface_flow}

Use the following steps to help you to configure the resources that you need to start using the data management feature.

**Before you begin**
This procedure assumes that you have a device type with at least one conncected and registered device. For more information about these prerequisites and for instructions on how to create a device type and register a device, see [Configuring devices for use with the Data Management feature](im_config_devices.html).

1. Select the device type that you want to normalize and transform data for.
2. Select either the Simple Interface Creator or the Advance Interface Creator:

a. Simple flow  
   i. [Create a draft physical interface](#create_physical_interface_simple).  
   
b. Advanced flow  
   i. [Create a draft physical interface](#create_physical_interface_advanced).  
   ii. [Create one or more draft logical interfaces](#create_logical_interface). (This step is completed automatically when you use the simple flow.)  
   iii. [Map the physical interface to the logical interfaces](#create_interface_mappings).  
     
     
3. [Set notification preferences](#set_notifications).
4. [Activate the configuration](#validate_activate).

*Note:* You can also configure the Data Management feature by using the APIs. For detailed information about configuring the Data Management feature to normalise and transform your device data by using the APIs, see [Getting stared with Data Management]{ga_im_example.html#im_example}. [Step-by-step guide: A detailed example about how to work with devices through a common interface](../GA_information_management/ga_im_index_scenario.html#scenario) walks you through the steps to create a device type logical interface for heterogeneous thermometer devices.

For details about the API, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} documentation.


## Creating a draft physical interface (Simple flow)
{: #create_physical_interface_simple}

1. From the main navigation menu, select **Devices**.
2. Select **Device Types** and select the device type that you want to create an interface for. You can also [create a new device type].
3. View and update the device type information, as necessary, and save any changes.
4. Click **Interface**.
5. Click **Simple Flow** to open the Simple Interface Creator.
6. Click **Create Interface**.
7. Click **Add Property** to start adding events and properties to the phsycial interface.  
 a. In the **Add Properties to the Interface** window, select the events that you want to add to the interface. The system listens for active events for connected devices of the selected device type. You can select the device ID of the last cached event, or select another event from the list.  
  b. Select the properties from the event and click **Add**.  
  c. Add more properties as necessary.
8. Click **Done**. The physical interface is created. If you want to add more details, you can upgrade the interface by selecting the **Use Advanced Interface Creator** link.


## Creating a draft physical interface (Advanced flow)
{: #create_physical_interface_advanced}

1. From the main navigation menu, select **Devices**.
2. Select **Device Types** and select the device type that you want to create an interface for. You can also [create a new device type].
2. View the device type information and click **Interface**.
4. Click **Advanced Flow** to open the Advanced Interface Creator.
5. Click **Create Interface**
7. On the **Identity** tab of the **Create Physical Interface** page, type a name and description of the physical interface.
7. Optional: Turn on the Edge Analytics switch if you are using Edge-enabled devices with the interface.
8. Click **Next**.
9. Define the physical interface by adding event types and payloads:
 a. Click **Create Event Type**. 
 b. Select the last cached event, select an event from the list, or add an event manually.
10. Click **Done**. The physical interface is created in draft format. You can proceed to add one or more logical interfaces.

## Creating a draft logical interface
{: #create_logical_interface}

1. After creating the draft physical interface, click **Add Logical Interface**.
2. On the **Identity** tab of the **Create Logical Interface** page, type a name and description of the logical interface.
3. Click **Next**.
4. Click **Add Property** to start defining the properties of the logical interface.
5. In the **Create Property** window select the properties from the physical interface that you want to add to the logical interface and then save the changes.
6. Add additional logical interfaces as necessary and then click **Next**.
7. {: #set_notifications}Set up the notification preferences for the interface. Select one of the following options to determine when you want device state notifications to be sent:
 - No Event Notifiers - No notifications are sent. You can use the REST API to retrieve device state change information.
 - On State Change - You are notified when the device state changes.
 - On Every Event - You are notified every time the platform processes an event for the device, even if the event does not result in a state change.
8. Click **Done**. The interfaces are created in draft format.
9. {: #validate_activate} Click **Deploy** to validate the configuration.
10. Click **Deploy** to activiate the configuration. The configuration status is set to deployed. 

