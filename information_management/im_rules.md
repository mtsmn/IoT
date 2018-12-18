---

copyright:
  years: 2016, 2018
lastupdated: "2018-12-18"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Creating embedded rules
{: #im_rules}

After you have successfully activated your device twin or asset twin and sent some data, it is time to make that data work for you by using rules.

## About rules and actions

With {{site.data.keyword.iot_full}} you can set up rules that trigger when an event that is received by {{site.data.keyword.iot_short_notm}} causes a change to the device state.

Embedded rules are condition-based decision points that match real-time device data with predefined threshold values or
other property data to trigger the rule if a condition is met.

With embedded rules, you specify the conditions that trigger a rule. You can then set up an action in response to the trigger, for example, sending an alert to a user's device and an email to an administrator, when the temperature of your device spikes.


## Understanding rules

Rules are associated with a [logical interface](../GA_information_management/ga_im_definitions.html#resources) and are written against the [device state](../GA_information_management/ga_im_definitions.html#resources). You can associate one or more rules with a logical interface.

Rules get evaluated when a device event that is received by {{site.data.keyword.iot_short_notm}} could affect the device state that is defined by a logical interface. This is called "event-driven evaluation".

Each rule must have a **name** and a **condition** parameter defined. The **condition** parameter must conform to the following criteria:  
 - The parameter must be an expression.  
 - The parameter must evaluate to a Boolean value of **true** or **false**. If the condition expression evaluates to **true** an MQTT message is published on an MQTT topic.  

## Understanding the expression language

Set up your rule logic by using the expression language to specify conditions for when a rule is or is not triggered. A rule is triggered every time its conditions are met.

The expression language is a subset of [JSONata ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/index.html){:new_window}. For more information about using the expression language, see [Understanding the mapping expression language](../GA_information_management/mapping_expression_language.html).

The mapping expression language has been extended for use with the data management rules feature through the introduction of $state and $instance variables which are defined for use in expressions. JSON is bound to these variables before the expression is evaluated.

- The $state variable is used to select a property on the device state to use in an expression, for example, *$state.temperature*.
- The $instance variable is used to select a device attribute to use in an expression, for example, *$instance.metadata.tempThresholdMax*.

You might want to use these variables so that you do not need to hard-code values in your rule condition expressions, and can set a different threshold value for each device instance. The $state and $instance variables are used in the example in the following section.  

## Configuring notifications
{: #notifications}

You can configure rules to set a notification strategy in which you define conditions determining the timing and frequency of notifications. This enables you to ensure that notifications are sent to alert users when data falls outside of normal ranges and conditions, while controlling the number of alerts that are triggered for a rule over a period of time. For example, you can set it up so that when the temperature of the device spikes for a specified amount of time, an alert is sent to the dashboard on a user's device, and an email is sent to the administrator.

Notifications can be sent every time the conditions specified in a rule are met, only the first time they are met, when they are met a certain number of times, or when they are met for a certain duration.

The following table shows the notification conditions that you can specify when you configure rules:

Condition name | Condition value
------------- | -------------
Default | The default notification condition triggers a notification event every time the rule is validated as true.
Becomes True | The rule is configured to trigger a notification the first time the rule is validated as true. Subsequent messages that are also validated as true do not trigger notifications. This is reset when the conditions are no longer met so that the rule is triggered the next time they are.
X in Y | This condition triggers a notification event if the rule is validated as true *x* number of times in *y* amount of time, measured in *days/hours/minutes/custom value*. For example, the rule can be configured to trigger only once if the conditions are met four times in 30 minutes. The device sends one new message every five minutes. At noon, the temperature initially exceeds 90 degrees, which meets the condition. The conditional trigger counter is started, but the rule is not yet triggered. After 15 minutes and three more messages that indicate that the temperature exceeded 90 degrees were received, the rule is triggered. The rule is then not triggered for another 15 minutes regardless of the temperature.
Persist | The rule is configured to trigger a notification event if the rule persists as true for the specified amount of time, such as 60 seconds or two days. The time interval starts when the conditions are initially met.

## Creating rules by using the API

To create a rule, use the {{site.data.keyword.iot_short_notm}} HTTP REST API. To use the HTTP REST API, you need to know the following information:
- Your organization ID.  
When you register with the Watson IoT Platform, you are given an organization ID which is a unique six character identifier for your account.  
- Your API Key.  
The API Key is set to *use-token-auth*.
- Your authorization token.  
The authentication token is either automatically generated or manually specified when a device is registered.

To create a rule, you must specify the logical interface identifier with which you want to associate the rule. To find a logical interface identifier, use the following HTTP REST API:

```
GET /logicalinterfaces
```

The following example shows how to use cURL to retrieve information about a logical interface:

```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/logicalinterfaces \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```  

The example authorization value ```TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n=``` consists of the following information: ```{API Key}:{authorization token}```, which is then Base64 encoded.

The following example shows a response to the GET method:

```
{
  "id": "5846ed076522050001db0e12",
  "name": "Thermometer Interface",
  "alias": "IThermometer",
  "description": "Thermometer Logical Interface",
  "schemaId": "5846ec826522050001db0e11",
  "version": "active",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil",
   "refs": {
        "schema": "/api/v0002/draft/schemas/5846ec826522050001db0e11"
           }
  }
```
In this example, the logical interface identifier is *5846ed076522050001db0e12*.


## Configuring rules - scenario

This scenario shows you how to create two rules and is based on the configuration that is created in [Step-by-step guide 1](../GA_information_management/ga_im_index_scenario.html).

One rule is called **tempRuleMax**, which is triggered when a temperature event is received by {{site.data.keyword.iot_short_notm}} that causes the *temperature* property of the device state to exceed 44 degrees Celsius. The other rule is called **tempRuleMin**, which is triggered when a temperature event is received by {{site.data.keyword.iot_short_notm}} that causes the *temperature* property of the device state to fall below 10 degrees Celsius.

To create the rules in this example, information about the following resources that are configured as part of Step-by-step guide 1 are required:

- The *tSensor* device instance.   

This device publishes temperature events that are measured in degrees Celsius. The following metadata is associated with the *tSensor* device instance:
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
A logical interface is used to define the normalized view onto the device state, which is updated in response to inbound device events. In our example, the logical interface defines the state property *temperature* in the following structure:
```
{
"temperature" : <current temperature value in Celsius>
}
```
The *temperature* property is used in the rule condition expression.


### Complete the following steps to configure the two rules:

1. Select the logical interface that you want to associate with your rule. In this scenario, we are using the logical interface with the identifier *5846ed076522050001db0e12*.
2. View the current rules that are associated with the logical interface by using the following API:  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
As no rules are currently associated with logical interface *5846ed076522050001db0e12*, an empty list is returned.  
3. Add a rule called **tempRuleMax** by using the following API:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Include the rule name and a condition parameter in the body of the request:  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
The following example shows the response to the POST method:  
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
The **tempRuleMax** rule is triggered if the device state *temperature* property that is defined on logical interface *5846ed076522050001db0e12*, exceeds the temperature value specified in the *tempThresholdMax* metadata that is defined for device instance *tSensor*. In our example, the value is 44 degrees Celsius.  
4. Add a rule called **tempRuleMin** by using the following API:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Include the rule name and a condition parameter in the body of the request:  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
The following example shows the response to the POST method:  
```  
{  
   "name": "tempRuleMin",  
   "id": "5a71991e59080100328710e10",  
   "logicalInterfaceId": "5846ed076522050001db0e12",  
   "condition": "$state.temperature < $instance.metadata.tempThresholdMin",  
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
The **tempRuleMin** rule is triggered if the device state *temperature* property that is defined on logical interface *5846ed076522050001db0e12* falls below the value specified in the *tempThresholdMin* metadata which is defined for device instance *tSensor*. In our example, the value is 10 degrees Celsius.  
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
       "condition": "$state.temperature < $instance.metadata.tempThresholdMin",
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
   "typeId" : "TSensor",
   "instanceId" : "tsensor",
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
You can then configure an action that is initiated when a rule is triggered. For example, you might use [Node-RED](../applications/dev_nodered.html) to send a message to a specified user when a rule evaluates to *true*.



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
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
 "typeId": "TSensor",
 "instanceId": "tSensor",
 "state": {
   "temperature": "string"
 },
 "timestamp": "2018-01-31T10:38:59Z",
 "updated": "2018-01-31T10:38:59Z"
}
```
The message body includes information about the logical interface ID, rule ID, device and state. You can use the information to help in debugging the error.

<!--## Next steps

The following section uses Node-RED to process the MQTT message that is generated when a rule evaluates to *true*.

Node-RED provides a browser-based flow editor that makes it easy to wire together devices, APIs, and online services by using the wide range of nodes in the palette. Flows can be then deployed to the Node.js runtime with a single click.

### IBM IoT App Node  
{: #watson_app_node}  

The *IBM IoT App Node* is a pair of nodes for connecting your applications to {{site.data.keyword.iot_short_notm}}. Applications can use the nodes to receive device events. The *IBM IoT App Node* has been extended to contain rules and device state. You can get the latest version of the *IBM IoT App Node* by navigating to *Manage Palettes* from the main menu of your Node-Red application or instance, and installing or updating the **node-red-contrib-scx-ibmiotapp** node to the latest version. If you want to use charts to visualize your device data or to send notifications on device state change, ensure that you also have the **node-red-dashboard** node installed.

For more information about getting started with Node-RED, see [Developing Watson IoT Platform by using Node-RED](../applications/dev_nodered.html).

The following example creates the following Node-RED flow:

![Sample Node_RED flow](images/Node-RED_flow.png "Sample Node_RED flow")

The flow results in a Slack message being generated and sent to a specified user if the temperature of the device state falls below 10 degrees Celsius or exceeds 44 degrees Celsius.

Complete the following steps to create the example Node-RED flow.

1. Complete the following steps to configure the *IBM IoT App Node* node:  
a. From your device’s Node-RED application or instance, drag and drop the **ibmiot** node from the *input* section into your flow and change the name of the node to *IBM IoT - Rule Trigger Input*.  
b. Double-click the node and set the following values:
 - Set the **Authentication** node property to *Bluemix Service* or *API Key*.  
 - Set the **Input Type** node property to *Rule Trigger*.
 - Select the **Logical Interface** and **Rule Id** check boxes.  
c. Click **Done**.  
2. Complete the following steps to configure the switch node:  
a. Drag and drop the **switch** node from the *function* section into your flow.  
b. Double-click the node and set the **Property** value to *msg.state.state.temperature*. This is the value that is specified in the body of the sample MQTT message that was generated earlier.  
c. Click **+add** and set the first value to *5a71991e59080100328710e9*. This is the rule ID that is associated with *tempRuleMax*. If an inbound device temperature event that is received by {{site.data.keyword.iot_short_notm}} causes the device state to exceed 44 degrees Celsius, the flow goes to the change node that is called **too hot** which sets **msg.payload** value to *true*. This change node is configured in Step 3.  
d. Click **+add** and set the second value to *5a71991e59080100328710e10*. This is the rule ID that is associated with *tempRuleMin*. If an inbound device temperature event that is received by {{site.data.keyword.iot_short_notm}} causes the device state to drop below 10 degrees Celsius, the flow goes to the change node that is called **too cold** which sets **msg.payload** value to *false*. This change node is configured in Step 3.
e. Click **Done**.  
3. Complete the following steps to configure the change nodes:  
a. Drag and drop a **change** node from the *function* section into your flow. Change the name to *too hot*.  
b. Double-click the node and set the **msg.payload** value to *true* in the *Rules* section. Setting the **msg.payload** value to *true* causes a notification to be generated by the code that is contained in the function node which is configured in Step 5.
c. Drag and drop another **change** node from the *function* section into your flow. Change the name to *too cold*.  
d. Double-click the node and set the **msg.payload** value to *false* in the *Rules* section.  
4. Complete the following steps to configure the rbe node:  
a. Drag and drop the **rbe** node from the *function* section into your flow.  
b. Double-click the node and set the **Mode** property value to *block unless value changes (ignore initial value)*. Selecting this value means that further notifications are sent only if the device state changes.  
c. Click **Done**.  
5. Complete the following steps to configure the function node:  
a. Drag and drop the **function** node from the *function* section into your flow.  
b. Double-click the node and add the following information to the *Function* section:  
```
var turnOn = msg.payload;
var msgVal = "It's too hot. Turn on the air conditioning!";
if (turnOn) {
    msgVal = "It's too cold. Turn the air conditioning off!";
}
var slackMsg = {
    "attachments" : [
        {
            "pretext" : "A Rule was triggered!",
            "mrkdwn_in" : ["pretext", "fields"],
            "fields" : [
                {"title" : "message", value: msgVal }
            ],
            "color" : "#3d86e5"
        }
    ]    
};
var newmsg = {
    "payload" : slackMsg
};
return newmsg;
```  
c. Click **Done**.  
5. Complete the following steps to configure the http request node:  
a. Drag and drop the **http request** node from the *function* section into your flow.  
b. Double-click the node and set the following properties to the values specified :  
  - Set **Method** to *POST*  
  - Set **URL** to the Slack channel to which you want the message sent.  
c. Click **Done**.  If the rule is triggered, a message is generated and is sent to the specified user in Slack.  
6. (Optional) Complete the following steps to configure the debug node:  
a. Drag and drop the **debug** node to your flow.  
7. Complete the following steps to configure and deploy your flow:  
a. Wire your nodes together. Ensure that each of the **switch** node flows goes to the correct **change** node.  
b. Click **Deploy**. -->
