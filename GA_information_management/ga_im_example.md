---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with data management by using REST APIs
{: #im_example}

Use the following steps to help you to configure the resources that you need to start using the device and asset twin features of the {{site.data.keyword.iot_full}} data management component.

For details about the API, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} documentation.


## High-level workflow
{: #workflow}

Use the following steps to help you to configure the resources that you need to start mapping your device or Thing data by using the device or asset twin feature.

**Tip:** For more detailed information about each of the steps, see the example scenarios or use the links to go directly to a specific step in the example scenario. [Step-by-step guide 1](ga_im_index_scenario.html#scenario) walks you through the steps to create a device type logical interface for heterogeneous thermometer devices and [Step-by-step guide 2](../information_management/im_index_scenario_thing.html#scenario) grows the scenario by describing how to build a logical interface that lets your application consume data from two different climate device types conflated into a room type Thing.

The process for creating and consuming logical interfaces differ somewhat depending on whether you are creating a logical interface that is associated with a device type or with a Thing type.

### Before you begin
To create a logical interface that is associated with a device type or Thing type you must have [at least one device registered with {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) and sending events with state properties.  


### Steps

1. 	Define the incoming state properties.  
First define the incoming state properties that you want your logical interface to make available to your applications.  
Depending on the logical interface that you are creating, do one of two things:
<dl>
<dt>Device type: Create a physical interface.</dt>
<dd>
<ol>
<li>[Create an event schema file](ga_im_index_scenario.html#step1). The event schema file is a local .JSON file that defines the structure and format of an inbound event.
<li>[Create an event schema resource for your event type](ga_im_index_scenario.html#step2). The event schema resource is a programmatic construct that is used by {{site.data.keyword.iot_short_notm}}.
<li>[Create an event type that references the event schema](ga_im_index_scenario.html#step3). The event type is used by {{site.data.keyword.iot_short_notm}} to map one or more event schema resources to a physical interface.
<li>[Create a physical interface](ga_im_index_scenario.html#step7).
<li>[Add the event type to the physical interface](ga_im_index_scenario.html#step8).
<li>[Add your physical interface to your device type](ga_im_index_scenario.html#step9).
</ol>
</dd>
<dt>Thing type: Define a Thing type.</dt>
<dd>
<ol>
<li>[Create a Thing type schema file](../information_management/im_index_scenario_thing.html#crt_composition_file).  
A Thing type schema file is a local .JSON file that defines the composition of the thing type by pointing to existing logical interfaces.
<li>[Create the Thing type schema resource](../information_management/im_index_scenario_thing.html#crt_composition_resource).  
Upload the local .JSON file to {{site.data.keyword.iot_short_notm}}.
<li>[Create a Thing type](../information_management/im_index_scenario_thing.html#crt_thing_type). A Thing type serves the same purpose as a device type in that it represents a class of Things.
</ol>
</dd>
</dl>
4. 	Create the logical interface.
 1. 	Create a logical interface schema file for the [device type](ga_im_index_scenario.html#step4) or [Thing type](../information_management/im_index_scenario_thing.html#crt_ai_schema_file).  
A logical interface schema file is a local .JSON file that defines the device state that is made available to your applications.
 2. 	Create a logical interface schema resource for the [device type](ga_im_index_scenario.html#step5) or [Thing type](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource).
 3.	Create a logical interface for the [device type](ga_im_index_scenario.html#step6) or [Thing type](../information_management/im_index_scenario_thing.html#crt_thing_ai).
 4.	Add the logical interface to the [device type](ga_im_index_scenario.html#step10) or [Thing type](../information_management/im_index_scenario_thing.html#add_thing_ai).
5. 	Define the mappings for the [device type](ga_im_index_scenario.html#step11) or [Thing type](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings).   
Mappings are used to map inbound properties to properties in the logical interface.  
6. 	Validate and activate the configuration that is associated with the draft [device type](ga_im_index_scenario.html#step15) or [Thing_type](../information_management/im_index_scenario_thing.html#activate).
7. 	Retrieve the state of the [device](ga_im_index_scenario.html#step13) or [Thing](../information_management/im_index_scenario_thing.html##verify_Thing_state).  
Verify that your subscriptions show the updated device data or that updated device data is returned by using a REST-call or by subscribing to a topic string.  



