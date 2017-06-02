---

copyright:
years: 2016, 2017
lastupdated: "2017-05-15"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-level worklow
{: #im_workflow}

Use the following steps to help you to configure the resources that you need to start mapping your device data by using interfaces.
{:shortdesc}

For details about the API, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html){:new_window} documentation.

**Tip:** For more detailed information about each of the steps, see the example scenarios or use the links to go directly to a specific step in the example scenario. The example documented in [Scenario](ga_im_index_scenario.html#scenario) walks you through the steps to create a device type logical interface for heterogeneous thermometer devices.

### Before you begin
To create a logical interface associated with a device type you must have [at least one device registered with {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) and sending events with state properties.  

### Steps

1. 	Define the incoming state properties.  
First define the incoming state properties that you want your logical interface to make available to your applications.  
Depending on the logical interface that you are creating, do one of two things:
<dl>
<dt>Device type: Create a draft physical interface.</dt>
<dd>
<ol>
<li>[Create a draft event schema file](ga_im_index_scenario.html#step1). The event schema file is a local .JSON file that defines the structure and format of an inbound event.
<li>[Create a draft event schema resource for your event type](ga_im_index_scenario.html#step2). The event schema resource is a programmatic construct that is used by {{site.data.keyword.iot_short_notm}}.
<li>[Create a draft event type that references the event schema](ga_im_index_scenario.html#step3). The event type is used by {{site.data.keyword.iot_short_notm}} to map one or more event schema resources to a physical interface.
<li>[Create a draft physical interface](ga_im_index_scenario.html#step7).
<li>[Add the draft event type to the draft physical interface](ga_im_index_scenario.html#step8).
<li>[Add your draft physical interface to your draft device type](ga_im_index_scenario.html#step9).
</ol>
</dd>
</dl>
4. 	Create the draft logical interface.
 1. 	Create a draft logical interface schema file for the draft [device type](ga_im_index_scenario.html#step4).  
A logical interface schema file is a local .JSON file that defines the device state that is made available to your applications.
 2. 	Create a draft logical interface schema resource for the draft [device type](ga_im_index_scenario.html#step5).
 3.	Create a draft logical interface for the draft [device type](ga_im_index_scenario.html#step6).
 4.	Add the draft logical interface to the draft [device type](ga_im_index_scenario.html#step10).
5. 	Define the draft mappings for the draft [device type](ga_im_index_scenario.html#step11).   
Mappings to map inbound properties to properties in the logical interface.
6. 	Validate and activate the configuration that is associated with the draft [device type](ga_im_index_scenario.html#step15).
7. 	Check that the state of the active [device](ga_im_index_scenario.html#step13) updates.  
Verify that your subscriptions show the updated device data or that updated device data is returned by using a REST-call or by subscribing to topic `iot-2/%s/type/%s/id/%s/intf/%s/evt/state`.
