---

copyright:
years: 2016, 2018
lastupdated: "2018-05-14"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with Data Management by using the Web interface
{: #gs_web}

{{site.data.keyword.iot_full}} provides online tools to help you normalize and transform data as part of the Data Management feature.

## Overview
{: #web_overview}

In addition to the [Watson IoT Platform Data Management API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} that is provided, you can use the Simple Interface Creator or the Advanced Interface Creator to configure resources and start mapping your device data. The online interface creators guide you through the steps in the user interface.

A physical interface is used to model the interface between a physical device and Watson IoT Platform. Event types can be associated with physical interface. A logical interface is used to define the normalized view onto the device state in Watson IoT Platform and is usually consumed by applications. A logical interface must be associated with a logical interface schema. The state is updated in response to inbound device events.

For more information about the Data Management feature and its benefits, see the [Introduction to Data Management](../GA_information_management/ga_im_device_twin.html#device_twins).

For information about the resources that you need to need to configure, see [Understanding Data Management](../GA_information_management/ga_im_definitions.html#definitions_resource).

With the Simple Interface Creator, you add one physical interface and then one logical interface is automatically created and the two are mapped together. With the Advanced Interface Creator, you have more control over the configuration. You add one physical interface and one or more logical interfaces, and then add mappings between them.

The interfaces and mappings are added in draft format. After you have added and validated them, you must activate them. For more information about draft and active resources, see [Creating, updating, activating, and deactivating your resources](../GA_information_management/ga_im_definitions.html#draft_active_resources).

The interface libraries hold template physical and logical interfaces that you can reuse.

## High-level flow
{: #interface_flow}

Use the following steps to help you to configure the resources that you need to start using the data management feature.

### Before you begin

This procedure assumes that you have a device type with at least one registered and connected device. For more information about these prerequisites and for instructions on how to create a device type and register a device, see [Connecting devices](../iotplatform_task.html#iotplatform_task).

### Steps

1. Select either the Simple Interface Creator or the Advanced Interface Creator:
  - **Simple Interface Creator flow**
    1. [Create a draft physical interface](#create_physical_interface_simple).
  - **Advanced Interface Creator flow**
    1. [Create a draft physical interface](#create_physical_interface_advanced).
    2. [Create one or more draft logical interfaces](#create_logical_interface). (This step is completed automatically when you use the simple flow.)
    3. [Map the physical interface to the logical interfaces](#create_interface_mappings).
2. [Set notification preferences](#set_notifications).
3. [Activate the configuration](#validate_activate).

You can also configure the Data Management feature by using the APIs. For detailed information about configuring the Data Management feature to normalize and transform your device data by using the APIs, see [Getting stared with Data Management](ga_im_example.html#im_example). [Step-by-step guide: A detailed example about how to work with devices through a common interface](../GA_information_management/ga_im_index_scenario.html#scenario) walks you through the steps to create a device type logical interface for heterogeneous thermometer devices. For details about the API, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} documentation.
{: tip}


## Creating a draft physical interface (Simple flow)
{: #create_physical_interface_simple}

1. From the main navigation menu, click **Devices**.
2. Click **Device Types** and select the device type that you want to create an interface for. You can also create a new device type. See [Connecting devices](../iotplatform_task.html#iotplatform_task) for more information.
3. View the device type information and click **Interface**.
4. Click **Simple Flow** to open the Simple Interface Creator.
5. Click **Create Interface**.
6. Click **Add Property** to start adding events and properties to the physical interface.
   a. In the **Add Properties to the Interface** window, select the events that you want to add to the interface. The system listens for active events for connected devices of the selected device type. You can select the device ID of the last cached event, or select another event from the list.
   b. Select the properties from the event and click **Add**.
   c. Add more properties as necessary.
7. Click **Done**. The physical interface is created. If you want to add more details, you can [upgrade to an advanced interface](#create_physical_interface_advanced) by selecting the **Use Advanced Interface Creator** link.


## Creating a draft physical interface (Advanced flow)
{: #create_physical_interface_advanced}

1. From the main navigation menu, click **Devices**.
2. Click **Device Types** and select the device type that you want to create an interface for. You can also create a new device type. See [Connecting devices](../iotplatform_task.html#iotplatform_task) for more information.
2. View the device type information and click **Interface**.
3. Click **Advanced Flow** to open the Advanced Interface Creator.
4. Choose one of the following options:
 - To create a new physical interface, click **Create Interface**.
 - To use an interface template from the library, click **Add From Library**, select the interface, and then go to [Creating a draft logical interface](#create_logic_interface).
5. On the **Identity** tab of the **Create Physical Interface** page, type a name and description of the physical interface.
6. Optional: Turn on the Edge switch if you are using Edge-enabled devices with the interface. For more information about Edge, see [Edge analytics](../edge_analytics.html#edge_analytics).
7. Click **Next**.
8. Define the physical interface by adding and modifying event types and payloads:
   a. Select an event type from the list to modify and existing event type, or click **Create Event Type**.
   b. Select the last cached event, select an event from the list, or add an event manually and then click **Add**.
9. Click **Done**. The physical interface is created in draft format. You can proceed to add one or more logical interfaces.

## Creating draft logical interfaces (Advanced flow)
{: #create_logical_interface}

1. After you have created the draft physical interface, choose one of the following options:
 - Click **Add Logical Interface** to create a new logical interface.
 - Click **Add From Library** to use an interface template from the interface library. Select the interface and then go to step 7.
2. On the **Identity** tab of the **Create Logical Interface** page, type a name and description of the logical interface.
3. Click **Next**.
4. Click **Add Property** to start defining the properties of the logical interface.
5. In the **Create Property** window select the properties from the physical interface that you want to add to the logical interface and then save the changes.
6. Click **Add Object**.
7. Add additional logical interfaces as necessary and then click **Next**.
8. {: #set_notifications}Set up the notification preferences for the interface. Select one of the following options to determine when you want device state notifications to be sent:
 - No Event Notifiers - No notifications are sent. You can use the REST API to retrieve device state change information.
 - On State Change - You are notified when the device state changes.
 - On Every Event - You are notified every time the platform processes an event for the device, even if the event does not result in a state change.
8. Click **Done**. The interfaces are created and ![Draft status icon](images/draft_icon.png) indicates the draft status. You can proceed to modify or activate the interfaces.

## Activating the configuration
{: #validate_activate}

After you have created the draft physical and logical interfaces, you must activate the configuration.

- Click **Activate** to activate the configuration. The configuration status is set to activated.
![Active deployment](images/active_deployment.png) If you make changes to the interfaces, the interfaces become drafts again and you must reactivate them.
