---

copyright:
  years: 2016, 2018
lastupdated: "2017-03-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Real-Time IoT Data Analytics
{: #analytics}  

**Important:** We are launching a Beta for a new way to define rules on your IoT device data as part of a wider 
program of changes to improve the way {{site.data.keyword.iot_full}} delivers rules and actions.

To find out more, check out the blog post [An alternative approach to defining Rules on IoT data ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}.

To start defining your own rules, see the [Creating embedded rules (Beta)](information_management/im_rules.html) documentation.

## About Real-Time IoT Data Analytics

Use Watson {{site.data.keyword.iot_short_notm}} analytics to get the real-time analytical information that you need from the raw data that your devices produce.  
{: shortdesc}

By using [boards and cards](data_visualization.html), you can view graphics that represent data set values from one or more devices for a quick overview and understanding of the device data.

With analytics rules, you specify the conditions that trigger actions. For example, you might create a rule to ensure that an alert is sent to the dashboard on a user's device, and that an email is sent to the administrator, when the device is dropped, or when the temperature of the device spikes.

Use [cloud rules](cloud_analytics.html) to trigger rules for devices that are connected directly to {{site.data.keyword.iot_short_notm}} in the cloud, and [edge rules](edge_analytics.html) to trigger rules for devices that are connected to edge analytics enabled gateways.
