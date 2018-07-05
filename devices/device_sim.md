---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-07"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Simulating device data 
{: #sim_device_data}

By using the {{site.data.keyword.iot_full}} device simulator, you can set up simulated events for devices. You can use the simulated event data to learn about, test, and demonstrate fully functioning {{site.data.keyword.iot_short_notm}} features.
{: shortdesc}

You can use existing device types and devices and the simulator enables you to generate new devices for existing types. You can configure the event details for each device or set a default configuration that is applied to all devices. You can export a simulated event configuration so that it can be reused or shared to set up other simulations.

To simulate device data: 

1. Log in to {{site.data.keyword.iot_short_notm}}.
2. From the main navigation pane, select **Settings**.
3. In the **Experimental Features** section, activate the device simulator.
4. From the main navigation pane, select **Devices**. A message in the lower right corner of the screen indicates that no simulations are running.
5. Click on the "0 Simulations running" message.
6. In the **Simulations** pop-up window, click **Add First Simulation**.
7. Select the device type that you want to simulate data for.
8. Select one of the following options:
  - To add one or more new devices, select the quantity and click **New Device**. The new devices are listed.
  - To add an existing device, click **Use Registered Device** and select a device from the list.
  - To import an existing simulation, click **Import/Export**, select the **Import** tab, and import a JSON file or copy a previously exported configuration from the clipboard.
9. Click ![Settings icon](images/settings_icon.png) and configure the simulation details for a device type:
   1. Select the device type from the list and click **+ Event Type** to add an event type to the device type.
   2. Select an event type from the list and click **Schedule** to set the frequency of the event.
   3. Edit the event type details in JSON format and save the updated event type.
   
   <p> You can use the following two special variables when configuring events:  
        - `range`:  This variable is replaced with a random number that is between the two provided values. For example: `{ "random": range(300, 100) }`  
        - `$counter`: This variable is replaced with the number of devices that are added in the simulator for the current type. For example: `{"total": $counter}`</p>
   {: tip}
   
   4. Choose whether to use these settings as the default values for all devices of this device type or whether to use custom values for individual devices. 
   5. Repeat the configuration steps for each device that you want to apply custom settings to. The message in the lower right of the screen shows the number of active simulations.
10. **Optional:** After you configure simulations for devices, export the simulation details so that they can be reused or shared:
    1. In the **Simulations** pop-up window, click **Import/Export**.
    2. Select the **Export** tab.
    3. Copy the simulation details to the clipboard or download them in a JSON file.
11. On the **Devices** page, browse to one of the simulated devices and select the **Recent Events** tab to verify that the simulated events are working.
