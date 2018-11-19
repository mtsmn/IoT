---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-27"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:important: .important}

# Getting started tutorial
{: #getting-started-with-iotp}

<p>The {{site.data.keyword.iot_short_notm}} pricing plans were updated on November 27, 2018.   
For more information, including upgrade information, see [{{site.data.keyword.iot_short_notm}} service plans](plans_overview.html). The contents of this [IBM Cloud documentation collection](https://console.bluemix.net/docs/services/IoT/) pertain to the {{site.data.keyword.iot_short_notm}} Lite plan, and to the previous Standard and Advanced Security plans. For documentation about the {{site.data.keyword.iot_short_notm}} Connection and Analytics Service plans, with their extended feature set, see the [{{site.data.keyword.iot_short_notm}} knowledge center ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/overview/overview.html).
</p>
{: important}

In this {{site.data.keyword.iot_full}} getting started tutorial, we connect an IoT device to {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

<div id="prerequisites"></div>

## Before you begin
{: #prereqs}

You'll need an [IBM Cloud account ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/registration/){: new_window},
an instance of the {{site.data.keyword.iot_short_notm}} service, and a device that meets the following requirements:

*	Your device must be able to communicate by using HTTP or MQTT protocols.

* The device messages must conform to the {{site.data.keyword.iot_short_notm}} message payload requirements.

For more information, see [Developing devices on Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Step 1: Register your device

You can add devices one at a time from the [{{site.data.keyword.iot_short_notm}} dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://internetofthings.ibmcloud.com){: new_window}, or you can use the [Watson IoT Platform API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} to add multiple devices.

To add a device from the {{site.data.keyword.iot_short_notm}} dashboard:

1. In the {{site.data.keyword.Bluemix_notm}} console, click **Launch** on the {{site.data.keyword.iot_short_notm}} service details page.

    The {{site.data.keyword.iot_short_notm}} web console opens in a new browser tab at the following URL:

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Where *org_id* is the ID of [your {{site.data.keyword.iot_short_notm}} organization](iotplatform_overview.html#organizations){: new_window}.

2. In the Overview dashboard, from the menu pane, select **Devices** and then click **Add Device**.

3. Create a device type for the device that you are adding.

    Each device that is connected to the {{site.data.keyword.iot_short_notm}} must be associated with a device type. Device types are groups of devices that share common characteristics.

    1. Click **Create device type**.
    2. Enter a device name, such as `my_device_type`, and a description for the device type.

        > **Note:** The device type name must be no more than 36 characters and can contain only alpha-numeric characters (a-z, A-Z, 0-9) and any of the following characters: `_`, `.`, and `-`.

    3. Optional: Enter device type attributes and metadata.

        You can add and edit attributes and metadata later.
        {: tip}

4. Click **Next** to begin the process of adding your device with the selected device type.

5. Enter a device ID, for example `my_first_device`.

    The device ID is used to identify the device in the {{site.data.keyword.iot_short_notm}} dashboard and is also a required parameter for connecting your device to {{site.data.keyword.iot_short_notm}}.

    > **Note:** The device type name must be no more than 36 characters and can contain only alpha-numeric characters (a-z, A-Z, 0-9) and any of the following characters: `_`, `.`, and `-`.

    For network connected devices, the device ID could be the device MAC address without any separating colons.

5. Click **Next** to complete the process.

6. Provide an authentication token, or accept an automatically generated token. If you choose to create your own token, make sure that it is between 8 and 36 characters long and consists only of alpha-numerical characters and the following characters: `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)`, and `-`.

    The token must not contain repeated character sequences, dictionary words, user names, or other predefined sequences.

7. Verify the summary information is correct, and then click **Add** to add the connection.

8. In the device information page, copy and save the following details:

    * Organization ID
    * Device Type
    * Device ID
    * Authentication Method
    * Authentication Token

    You'll need Organization ID, Device Type, Device ID, and Authentication Token configure your device to connect to {{site.data.keyword.iot_short_notm}}.

## Step 2: Connect your device to {{site.data.keyword.iot_short_notm}}

1. Set up your device for MQTT messaging and authenticate by using Organization ID, Device Type, Device ID, and Authentication Token.

2. Send device messages to your {{site.data.keyword.iot_short_notm}} organization by using the MQTT protocol.

    The following information is required when connecting your device:

    * URL: *org_id*.messaging.internetofthings.ibmcloud.com

      Where *org_id* is the ID of your {{site.data.keyword.iot_short_notm}} organization.

    * Port:
      * 1883
      * 8883 (encrypted)
      * 443 (websockets)
    * Device ID: d:_org_id:device_type:device_id_
    * User name: use-token-auth
    * Password: _Authentication token_
    * Event topic format: iot-2/evt/_event_id/fmt/format_string_

      Where _event_id_ specifies the event name that's shown in {{site.data.keyword.iot_short_notm}}, and _format_string_ is the format of the event, such as JSON.

    * Message format: JSON

  For more information, see [MQTT connectivity for devices](/docs/services/IoT/devices/mqtt.html).


## Next steps

Extend the data analytics features by creating and connecting your own apps to consume device data.

  * Check out the [client libraries](/docs/services/IoT/iot_platform_client_lib.html) for tools to build code for integrating and connecting your devices and apps.
