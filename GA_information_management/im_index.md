---

copyright:
years: 2016, 2017
lastupdated: "2017-07-20"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Using interfaces to map device data (Beta)
{: #im_index}

******************************************************************************************************************************************
**Important:** Please note that the information management Beta is now closed as we work towards the delivery of an updated and generally available version of this functionality.

The device state functionality of this Beta is now generally available as part of the Data Management feature.  For more information, see [Introduction to data management](../GA_information_management/ga_im_device_twin.html). Please note that there are changes to the HTTP REST APIs and the configuration steps.

The device aggregation functionality of this Beta is not part of the current release but will be generally available soon in a future update.
******************************************************************************************************************************************

Application interfaces extend the [device type](#resources) concept to better control the data that flows through {{site.data.keyword.iot_short_notm}} and to provide a device-agnostic view of IoT data.


## Overview
{: #overview}

Use application interfaces to create shared abstractions of devices and things to improve reuse and maintenance and to manage the complexities of an IoT ecosystem while keeping the application insulated from data change. Application interfaces are decoupled from the variability in the message data that devices communicate to {{site.data.keyword.iot_short_notm}}.

Through application interfaces applications can access the currect state of devices and things. The state consists of a set of state properties that are defined by the application interface. As devices send state change events, the most recent values of these properties are stored in {{site.data.keyword.iot_short_notm}} and are made available to the application on request by using an HTTP API.

By using application interfaces, you can:
- Map state properties to event message data
- Define the data structure that you prefer
- Define more than one representation or view of the device state
- Subscribe to device states or query them at any time through an HTTP API

Some common use cases for application interfaces include:
- Providing your application developers consistent interfaces to access event-driven device data in a REST-like manner.
