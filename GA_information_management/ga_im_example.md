---

copyright:
years: 2016, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with data management
{: #im_example}

Use the following steps to help you to configure the resources that you need to start using the data management feature.

For details about the API, see the [{{site.data.keyword.iot_full}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} documentation.

**Tip:** For more detailed information about each of the steps, see the example scenarios or use the links to go directly to a specific step in the step-by-step guide. [Step-by-step guide: A detailed example about how to work with devices through a common interface](ga_im_index_scenario.html#scenario) walks you through the steps to create a device type logical interface for heterogeneous thermometer devices.


## Before you begin
To start using the data management feature, you must have at least one [device registered with](ga_im_index_scenario.html#step14) and sending data to {{site.data.keyword.iot_short_notm}}.  

The following diagram shows a logical view of how the resources that you need to configure fit together:

![Topographic resource view in {{site.data.keyword.iot_short_notm}}.](images/ga_im_resource_view1.svg "Resource topography in {{site.data.keyword.iot_short_notm}}")

## Steps

1. 	Define the incoming state properties.  
Define the incoming state properties that you want your logical interface to make available to your applications.  
<dl>
<dd>
<ol>
<li>[Create a draft event schema file](ga_im_index_scenario.html#step1). The event schema file is a local .JSON file that defines the structure and format of an inbound event.
<li>[Create a draft event schema resource for your event type](ga_im_index_scenario.html#step2). The event schema resource is a programmatic construct that is used by {{site.data.keyword.iot_short_notm}}.
<li>[Create a draft event type that references the event schema](ga_im_index_scenario.html#step3). The event type is used by {{site.data.keyword.iot_short_notm}} to map one or more event schema resources to a physical interface.
<li>[Create a draft physical interface](ga_im_index_scenario.html#step7).
<li>[Add the event type to the draft physical interface](ga_im_index_scenario.html#step8).
<li>[Update the draft device type to connect the draft physical interface](ga_im_index_scenario.html#step9).
</ol>
</dd>
</dl>
4. 	Create the draft logical interface.
 1. 	[Create a draft logical interface schema file](ga_im_index_scenario.html#step4) for the draft device type.  
A logical interface schema file is a local .JSON file that defines the device state that is made available to your applications.
 2. [Create a draft logical interface schema resource](ga_im_index_scenario.html#step5) for the draft device type.
 3.	[Create a draft logical interface](ga_im_index_scenario.html#step6) for the draft device type.
 4.	[Add the draft logical interface to the draft device type](ga_im_index_scenario.html#step10).
5. 	[Define the draft mappings](ga_im_index_scenario.html#step11) for the draft device type.   
Mappings are used to map inbound properties to properties in the logical interface.
6. 	[Validate and activate the configuration](ga_im_index_scenario.html#step15) that is associated with the draft device type.
7. 	[Retrieve the state of the active device](ga_im_index_scenario.html#step13).  
Verify that your subscriptions show the updated device data or that updated device data is returned by using a REST-call or by subscribing to a topic.
