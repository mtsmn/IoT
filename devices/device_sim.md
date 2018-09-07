---

copyright:
  years: 2017, 2018
lastupdated: "2018-09-06"
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
3. In the **Data and Devices** section, activate the device simulator.
4. From the main navigation pane, select **Devices**. A message in the lower right corner of the screen indicates that no simulations are running.
5. Click on the "0 Simulations running" message.
6. In the **Simulations** pop-up window, click **Creat a Simulation**.
7. Select an existing device type that you want to simulate data for, or create a new device type by typing the device type name.
8. Click **Import/Export Simulation** to use an existing configuration, or manually configure the simulation details for the device type:
   1. Click **+ New Event Type** to add an event type to the device type.
   2. Enter a name for the event type.
   3. Under **Schedule**, set the frequency of the event.
   3. Edit the event type details in JSON format and save the updated event type.
   
   <p> You can use the following special variables when configuring events:  
        - `range`:  This variable is replaced with a random number that is between the two provided values. For example: `{"random": range(300, 100)}`  
        - `$counter`: This variable is replaced with the number of devices that are added in the simulator for the current type. For example: `{"total": $counter}`  
        - `increment`: This variable indicates the starting number and the increment by which it increases. For example, `{"myNumber": increment(10, 1)}`. In this case, `myNumber` starts at 10 and increases by 1 each time a paylad is sent (10, 11, 12, and so on).</p>
   {: tip}

9. Select a registered device for the simulation.
10. To create more simulations, click **+ New Simulation** and repeat the configuration steps for each device that you want to apply custom settings to. The message in the lower right of the screen shows the number of active simulations.
11. **Optional:** After you configure simulations for devices, export the simulation details so that they can be reused or shared:
    1. In the **Simulations** pop-up window, click **Import/Export**.
    2. Select the **Export** tab.
    3. Copy the simulation details to the clipboard or download them in a JSON file.
12. On the **Devices** page, browse to one of the simulated devices and select the **Recent Events** tab to verify that the simulated events are working.
