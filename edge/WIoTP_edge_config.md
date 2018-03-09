---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configuring {{site.data.keyword.iot_short_notm}} Edge (Preview)
{: #edge_configure}

**Important:** The {{site.data.keyword.iot_full}} Edge feature is available only as part of a limited preview program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Before you can begin receiving data from devices that are connected to your Edge node, you must connect the gateway to {{site.data.keyword.iot_full}}. Connecting a gateway to {{site.data.keyword.iot_short_notm}} involves creating a gateway device type and registering the gateway with {{site.data.keyword.iot_short_notm}}. You can then use the registration information to connect the gateway device to {{site.data.keyword.iot_short_notm}}.

## High-level configuration flow

1. [Create an organization](#edge_org) within the {{site.data.keyword.iot_short_notm}} Edge preview test environment.
2. [Enable the {{site.data.keyword.iot_short_notm}} preview](#edge_enable).
3. Configure your organization to use Edge gateway nodes with {{site.data.keyword.iot_short_notm}} by [creating a gateway type with Edge enabled](#edge_gw_type), [adding gateway devices](#edge_gw), and selecting the Edge services to be run on your Edge nodes. You can [create custom services](WIoTP_edge_dev.html#create_service) if necessary.
4. [Configure the Edge services](#config_service).
5. [Install Edge on your devices](#edge_install_device).
6. [Manage and monitor the Edge services](#monitor_service).

## Create an organization
{: #edge_org}

Create an organization within the {{site.data.keyword.iot_short_notm}} Edge preview test environment by completing the following steps:
1. Go to http://YOUR_ORGANIZATION.internetofthings.ibmcloud.com.
2. Complete the information to provision the IBM Cloud organization. **Important:** For the {{site.data.keyword.iot_short_notm}} Edge preview, you must create your IBM Cloud organization in the US region.
3. Click **Provision Service Instance**.
4. In the {{site.data.keyword.iot_short_notm}} instance that opens, click **Launch**.
A new WIoTP organization is created. The org ID is shown at the start of the URL and in the dashboard under your user name. You can now enable {{site.data.keyword.iot_short_notm}} Edge for the organization.

## Enable the {{site.data.keyword.iot_short_notm}} Edge preview
{: #edge_enable}

The {{site.data.keyword.iot_short_notm}} Edge preview is turned off by default. To enable {{site.data.keyword.iot_short_notm}} Edge:

1. From the {{site.data.keyword.iot_full}} dashboard for your organization, select **Settings**.
2. In the **Experimental Features** section, activate the experimental features, including the {{site.data.keyword.iot_short_notm}} Edge preview.
2. Refresh your browser to view the new features and shortcuts in the dashboard navigation menu.

The flag is added to the local browser storage.

## Create a gateway type with Edge capabilities enabled
{: #edge_gw_type}

Each gateway that is connected to the {{site.data.keyword.iot_short_notm}} must be associated with a gateway type. Gateway types are groups of devices that share common characteristics.

1. From the {{site.data.keyword.iot_short_notm}} dashboard main navigation menu, select **Devices**.
2. On the **Device Types** tab, click **Add Device Type**.
3. On the **Identity** tab of the **Add Type** page, select the type **Gateway** and enter a gateway type name such as `my_gateway_type` and a description for the gateway type.
**Important:** The gateway type name must be no more than 36 characters and can contain only:
<ul>
 <li>Alpha-numeric characters (a-z, A-Z, 0-9)</li>
 <li>Hypens (-)</li>
 <li>Underscores (&lowbar;)</li>
 <li>Periods (.)</li>
 </ul>
3. Optional: Enter gateway type attributes and metadata.
4. Turn on the **Edge Capabilities** button.
5. Select the architecture type for the gateway type. The architecture type must match the architecture of your device. For example, an Odroid device runs on the ARM64 architecture.
6. Click **Next**.
7. Optional: On the **Device Information** tab, enter additional details and metadata related to the gateway device type.
8. Select the Edge services to be added to your Edge devices. The Core IoT service is added to all devices, but you can add other services from the catalog by completing the following steps:
 1. Click **Add Services**.
 2. In the **Add Edge Services** window, select the appropriate version of the services that you want to add, and then click **Done**.
9. In the **Add Gateway Type** window Click **Done**.
You can proceed to [add gateway devices to this gateway type](#edge_gw).

## Adding and gateway devices
{: #edge_gw}

1. From the {{site.data.keyword.iot_short_notm}} dashboard main navigation menu, select **Devices**.
2. Click **Add Device**.
3. On the **Identity** tab, select the gateway device type that you are adding devices to and enter a unique ID for the gateway device that you are adding.
The device ID is used to identify the gateway device in the {{site.data.keyword.iot_short_notm}} dashboard and is also a required parameter for connecting your gateway device to {{site.data.keyword.iot_short_notm}}.
**Important:** The device ID must be no more than 36 characters and can contain only:
 <ul>
 <li>Alpha-numeric characters (a-z, A-Z, 0-9)</li>
 <li>Hyphens (-)</li>
 <li>Underscores (&lowbar;)</li>
 <li>Periods (.)</li>
 </ul>
 **Tip:** For network connected devices, the device ID could for example be the device MAC address without any separating colons.
4. Click **Next**.
5. Optional: On the **Device Information** tab, enter additional details and metadata related to the gateway device.
6. Click **Next**.
7. On the **Security** tab of the **Add Device** page, auto-generate an authentication token, or provide your own authentication token for this device. If you choose to create your own token, make sure that it is between 8 and 36 characters long, contains a mix of lower and upper case letters, numbers, and hyphen, underscore, or period. The token must not contain repeated character sequences, dictionary words, user names, or other predefined sequences.
9. Click **Next**.
10. Click **Done**.
11. On the device information page, copy and save the following device information:
 - Organization ID, such as `tubo8x`
 - Device Type, such as `my_gateway_type`
 - Device ID. **Tip:** For network connected devices, this could for example be the MAC address without any separating colons.
 - Authentication Method, such as `token`
 - Authentication Token, such as `PtBVriRqIg4uh)_-Kl`
  Use this information to connect the gateway to {{site.data.keyword.iot_short_notm}} and start receiving data from devices that are connected to the gateway.

## Configuring Edge services
{: #config_service}

After you create a new device type with Edge capabilities enabled, you can select different services to be installed in your Edge node.

1. In the {{site.data.keyword.iot_short_notm}} dashboard main navigation menu, select **Devices**.
2. Click **Device Types**.
3. Select the Edge device type that you want to configure.
4. On the  **Edge Services** tab, select the pencil icon to edit the record.
6. Click **Add Edge Services** to see a list of services that were uploaded for this organization.
7. In the **Add Edge Services** window, select the appropriate version of the services that you want to add, and then click **Done**.
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## Managing and monitoring Edge services
{: #monitor_service}

After Edge nodes are installed, {{site.data.keyword.iot_short_notm}} allows you to monitor status of services that are running on Edge nodes.

1.	In the {{site.data.keyword.iot_short_notm}} dashboard main navigation menu, select **Devices**.
2.	Select device corresponding to your Edge node
3.	Click **Edge Services** to see the status of the services that are installed on Edge node.

## Finding more information
{: #more_info}

For information about connecting your gateway to {{site.data.keyword.iot_short_notm}}, see [MQTT connectivity for gateways](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt).

For more detailed information on the {{site.data.keyword.iot_short_notm}}, see [Getting started with Watson IoT Platform](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp).

For more information about {{site.data.keyword.iot_short_notm}} Edge, see the following topics:
- [{{site.data.keyword.iot_short_notm}} Edge overview](WIoTP_edge.html#edge_overview)
- [Installing {{site.data.keyword.iot_short_notm}} Edge on edge devices](WIoTP_edge_install.html#edge_install_device)
- [Developing for {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
