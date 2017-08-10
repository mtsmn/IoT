---

copyright:
years: 2016, 2017
lastupdated: "2017-08-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configuring devices for use with the Data Management feature by using the Web interface (Beta)
{: #im_config_devices}

**Important:** The {{site.data.keyword.iot_full}} Data Management Web interface feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.


## Adding a device type
{: #create_device_type}
1. From the {{site.data.keyword.iot_full}} main navigation menu, select **Devices**.
2. On the **Device Types** tab, click **Add Device Type**.
3. On the **Identity** tab of the **Add Type** page, select the type Device and enter a name and description of the device type you are creating.
4. Click **Next**.
5. Optional: On the **Device Information** tab, enter the details of the device type.
6. Optional: Add metadata. 
7. Click **Done**.
You can proceed to [register devices to this device type] and [create interfaces] to normalize and transform data for devices of this type.

## Adding a device
{: #add_device}
1. From the {{site.data.keyword.iot_short_notm}} main navigation menu, select **Devices**.
2. Click **Add Device**.
3. On the **Identity** tab, select the device type and enter a unique ID for the device you are creating.
4. Click **Next**.
5. Optional: On the **Device Information** tab, enter the details of the device.
6. Optional: Add metadata. 
7. Click **Next**.
8. On the **Security** tab of the **Add Device** page, autogenerate an authentication token, or provide your own authentication token for this device. 
9. Click **Next**.
10. Click **Done**.

