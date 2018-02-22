---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Creating embedded rules (Beta)
{: #im_rules}

After you have successfully activated your device twin and sent some data, it is time to make that data work for you by using rules and actions.

**Important:** The {{site.data.keyword.iot_full}} embedded rules feature for data management is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## About rules and actions

With {{site.data.keyword.iot_full}} you can set up rules and actions that trigger from an event that causes a change
in device state.

Embedded rules are condition-based decision points that match real-time device data with predefined threshold values or
other property data to trigger an alert if a condition is met.

With embedded rules, you specify the conditions that trigger actions. For example, you might create a rule to ensure
that an alert is sent to a user's device, and that an email is sent to the administrator, when the temperature of your device spikes.


## Understanding rules

Rules are associated with a logical interface and are written against the device state. You can associate one or more rules with a logical interface.

Rules get evaluated when a device event that is received by {{site.data.keyword.iot_short_notm}} could affect the device state that is defined by a logical interface. This is called "event-driven evaluation".

Each rule must have a **name** and a **condition** parameter defined. 

The **condition** parameter must conform to the following criteria:  
 - The parameter must be an expression.  
 - The parameter must evaluate to a Boolean value of **true** or **false**. If the condition expression evaluates to **true** an MQTT message is published on an MQTT topic.  
 - A description can optionally be included on the POST method.  

## Understanding the expression language

Set up your rule logic by using the expression language to specify conditions for when a rule is or is not triggered. A rule is triggered every time its conditions are met. 

The expression language is a subset of [JSONata ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/index.html){:new_window}. For more information about using the expression language, see [Understanding mapping expression language](../GA_information_management/mapping_expression_language.html). 

The mapping expression language has been extended for use with the data management rules feature through the introduction of $state and $instance variables which are defined for use in expressions. JSON is bound to these variables before the expression is evaluated.

- The $state variable is used to select a property on the device state to use in an expression, for example, *$state.temperature*.
- The $instance variable is used to select a device attribute to use in an expression, for example, *$instance.metadata.tempThresholdMax*.

You might want to use these variables so that you do not need to hard-code values in your rule condition expressions, and can set a different threshold value for each device instance. The $state and $instance variables are used in the example in the following section.  


## Configuring rules

This scenario shows you how to create two rules and is based on the configuration that is created in [Step-by-step guide 1](../GA_information_management/ga_im_index_scenario.html). 

One rule is called **tempRuleMax**, which is triggered when a temperature event is received by {{site.data.keyword.iot_short_notm}} that causes the *temperature* property of the device state to exceed 44 degrees Celsius. The other rule is called **tempRuleMin**, which is triggered when a temperature event is received by {{site.data.keyword.iot_short_notm}} that causes the *temperature* property of the device state to fall below 10 degrees Celsius. 

To create the rules in this example, information about the following resources that are configured as part of Step-by-step guide 1 are required: 

- The device called *TemperatureSensor1*. This device publishes temperature events that are measured in degrees Celsius. The following metadata is associated with *TemperatureSensor1*:
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
The variables and associated values that are defined in the device instance metadata are required so that the information can be used in the rule condition expression.  

**Tip:** Metadata can be added or updated to a device instance after it has been created from the *Device Information* page of the dashboard. For information about using the dashboard, see [Getting started with Data Management by using the Web interface](../GA_information_management/im_ui_flow.html).
 
- The logical interface with the identifier *5846ed076522050001db0e12*. 
The identifier is required so that the rules can be associated with the correct logical interface resource.
A logical interface is also used to define the normalized view onto the device state, which is updated in response to inbound device events. In our example, the logical interface defines the state property *temperature* in the following structure:
```
{
"temperature" : <current temperature value in Celsius>
}
```
The *temperature* property is used in the rule condition expression. 
 

### Complete the following steps to configure the two rules:

1. Select the logical interface that you want to associate with your rule. In this scenario, we are using logical interface *5846ed076522050001db0e12*.  
2. View the current rules that are associated with the logical interface by using the following API:  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
As no rules are currently associated with logical interface *5846ed076522050001db0e12*, an empty list is returned.  
3. Add a rule called **tempRuleMax** by using the following API: 
```
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
Include the rule name and a condition parameter in the body of the request. 
```
{
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}
```  
The **tempRuleMax** rule is triggered if the device state *temperature* property that is defined on logical interface *5846ed076522050001db0e12* exceeds the temperature value specified in the *tempThresholdMax* metadata that is defined for device instance *EnvSensor1*. In our example, the value is 44 degrees Celsius.  
4. Add a rule called **tempRuleMin** by using the following API:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Include the rule name and a condition parameter in the body of the request.  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
The **tempRuleMin** rule is triggered if the device state *temperature* property that is defined on logical interface *5846ed076522050001db0e12* falls below the value specified in the *tempThresholdMin* metadata which is defined for device instance *EnvSensor1*. In our example, the value is 10 degrees Celsius.  
The following examples show the responses to the POST methods:  
```  
{  
   "name": "tempRuleMax",  
   "id": "5a71991e59080100328710e9",  
   "logicalInterfaceId": "5846ed076522050001db0e12",  
   "condition": "$state.temperature > $instance.metadata.tempThresholdMax",  
   "refs": {  
         "logicalInterface": "/api/v0002/draft/logicalinterfaces/5846ed076522050001db0e12"  
           },
   "version": "draft",  
   "created": "2018-01-31T10:23:26Z",  
   "createdBy": "a-7p9t2v-zsrcacabpa",  
   "updated": "2018-01-31T10:23:26Z",
   "updatedBy": "a-7p9t2v-zsrcacabpa"  
}
```  
```  
{  
   "name": "tempRuleMin",  
   "id": "5a71991e59080100328710e10",  
   "logicalInterfaceId": "5846ed076522050001db0e12",  
   "condition": "$state.temperature > $instance.metadata.tempThresholdMin",  
   "refs": {  
        "logicalInterface": "/api/v0002/draft/logicalinterfaces/5846ed076522050001db0e12"  
   },  
   "version": "draft",  
   "created": "2018-01-31T10:23:26Z",  
   "createdBy": "a-7p9t2v-zsrcacabpa",  
   "updated": "2018-01-31T10:23:26Z",  
   "updatedBy": "a-7p9t2v-zsrcacabpa"  
}  
```  
5. Validate and activate your configuration by using the **activate-configuration** operation. You must activate your configuration after adding a rule, because the action of adding the rule results in a change to the draft model. The **activate-configuration** operation must be performed on the draft version of the device type. 
The following example shows a PATCH request where an **activate-configuration** operation is performed on a draft version of a device type:  
```  
PATCH /draft/device/types/{typeId}  
```  
where the payload of the PATCH body contains the following content:  
```
{
  "operation": "activate-configuration"
}
```  
For more information about activating and deactivating your configuration, see [Understanding data management](../GA_information_management/ga_im_definitions.html#draft_active_resources).  
6. View the current rules on the active logical interface by using the following API:  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
The following example shows a response to the GET method:  
```  
   {
       "name": "tempRuleMax",  
       "id": "5a71991e59080100328710e9",
       "logicalInterfaceId": "5846ed076522050001db0e12",
       "condition": "$state.temperature > $instance.metadata.tempThresholdMax",
       "refs": {
           "logicalInterface": "/api/v0002/logicalinterfaces/5846ed076522050001db0e12"
       },
       "version": "active",
       "created": "2018-01-31T10:23:26Z",
       "createdBy": "a-7p9t2v-zsrcacabpa",
       "updated": "2018-01-31T10:23:26Z",
       "updatedBy": "a-7p9t2v-zsrcacabpa"
   }   
   {
       "name": "tempRuleMin",  
       "id": "5a71561e59055500328710e10",
       "logicalInterfaceId": "5846ed076522050001db0e12",
       "condition": "$state.temperature > $instance.metadata.tempThresholdMin",
       "refs": {
           "logicalInterface": "/api/v0002/logicalinterfaces/5846ed076522050001db0e12"
       },
       "version": "active",
       "created": "2018-01-31T10:23:26Z",
       "createdBy": "a-7p9t2v-zsrcacabpa",
       "updated": "2018-01-31T10:23:26Z",
       "updatedBy": "a-7p9t2v-zsrcacabpa"
   }   
```  

For more details about using the API, see the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window} documentation.

## Generating an action
When a rule evaluates to *true*, a message is published to the following MQTT topic:  
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
The following example shows a sample message body:  
```
{
   "type" : "device",
   "typeId" : "TempSensor",
   "instanceId" : "ts001",
   "logicalInterfaceId": "5846ed076522050001db0e12",
   "ruleId" : "5a71991e59080100328710e9",
   "state" : {
       "state": {
           "temperature": 45
       },
       "timestamp": "2018-01-31T10:29:28Z",
       "updated": "2018-01-31T10:29:10Z"
   }
}
```

<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
------------------- |------------- | --------
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

If an error is generated during rule evaluation, an error message is published on the following MQTT topic:

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

The following example shows an example message body:

```
{
 "message": "The result of expression evaluation is not of type boolean.",
 "logicalInterfaceId": "5846ed076522050001db0e12",
 "ruleId": "5i8t306d59087777329f1c56",
 "type": "device",
 "typeId": "EnvSensor",
 "instanceId": "EnvSensor1",
 "state": {
   "temperature": "string"
 },
 "timestamp": "2018-01-31T10:38:59Z",
 "updated": "2018-01-31T10:38:59Z"
}
```
The message body includes information about the logical interface ID, rule ID, device and state. You can use the information to help in debugging the error.

