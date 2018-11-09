---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 创建嵌入式规则 (Beta)
{: #im_rules}

成功激活双联设备或双联资产并发送一些数据后，是时候使用规则，使数据得以运作了。

**重要信息：**用于数据管理的 {{site.data.keyword.iot_full}} 嵌入式规则功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## 关于规则和操作

通过 {{site.data.keyword.iot_full}}，可以设置规则，以用于在 {{site.data.keyword.iot_short_notm}} 收到的事件导致更改设备状态时触发。

嵌入式规则是基于条件的决策点，用于使实时设备数据与预定义的阈值或其他属性数据相匹配，以在满足条件时触发该规则。

使用嵌入式规则时，可指定用于触发规则的条件。然后，可以设置操作以响应触发器，例如在设备温度达到峰值时，向用户的设备发送警报并向管理员发送电子邮件。


## 了解规则

规则与[逻辑接口](../GA_information_management/ga_im_definitions.html#resources)相关联，并且是根据[设备状态](../GA_information_management/ga_im_definitions.html#resources)编写的。可以将一个或多个规则与一个逻辑接口相关联。 

{{site.data.keyword.iot_short_notm}} 收到的设备事件可能会影响逻辑接口定义的设备状态时，会对规则求值。这称为“事件驱动的评估”。

每个规则都必须定义 **name** 和 **condition** 参数。**condition** 参数必须符合以下条件：  
 - 该参数必须为表达式。  
 - 该参数必须求值为布尔值 **true** 或 **false**。如果条件表达式求值为 **true**，那么将在 MQTT 主题上发布 MQTT 消息。  

## 了解表达式语言

使用表达式语言来设置规则逻辑，以指定触发或不触发规则的条件。每次满足规则条件时，就触发规则。 

表达式语言是 [JSONata ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/index.html){:new_window} 的子集。有关使用表达式语言的更多信息，请参阅[了解映射表达式语言](../GA_information_management/mapping_expression_language.html)。 

通过引入定义用于表达式的 $state 和 $instance 变量，扩展了映射表达式语言，以便与数据管理规则功能配合使用。在对表达式求值之前，JSON 会绑定到这些变量。

- $state 变量用于选择要在表达式中使用的设备状态的属性，例如 *$state.temperature*。
- $instance 变量用于选择要在表达式中使用的设备属性，例如 *$instance.metadata.tempThresholdMax*。

您可能希望使用这些变量，这样就不需要在规则条件表达式中使用硬编码值，并且可为每个设备实例设置不同的阈值。以下部分中的示例使用了 $state 和 $instance 变量。  


## 配置规则

此场景显示如何创建两个规则，并且此场景基于在[逐步指南 1](../GA_information_management/ga_im_index_scenario.html) 中创建的配置。 

一个规则名为 **tempRuleMax**，用于在 {{site.data.keyword.iot_short_notm}} 收到导致设备状态的 *temperature* 属性超过摄氏 44 度的温度事件时触发。另一个规则名为 **tempRuleMin**，用于在 {{site.data.keyword.iot_short_notm}} 收到导致设备状态的 *temperature* 属性低于摄氏 10 度的温度事件时触发。 

要创建此示例中的规则，需要有关在逐步指南 1 中配置的以下资源的信息： 

- *tSensor* 设备实例。   

此设备将发布以摄氏度为单位测量的温度事件。以下元数据与 *tSensor* 设备实例相关联：
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
设备实例元数据中定义的变量和关联值是必需的，以便可以在规则条件表达式中使用这些信息。  

**提示：**在仪表板的*设备信息*页面中创建设备实例后，可以向其添加或更新元数据。有关使用仪表板的信息，请参阅[使用 Web 界面开始进行数据管理](../GA_information_management/im_ui_flow.html)。
 
- 标识为 *5846ed076522050001db0e12* 的逻辑接口。 

标识是必需的，以便规则可以与正确的逻辑接口资源相关联。逻辑接口用于将规范化视图定义到设备状态，设备状态将根据入站设备事件进行更新。在本示例中，逻辑接口按以下结构定义状态属性 *temperature*：
```
{
"temperature" : <current temperature value in Celsius>
  }
```
在规则条件表达式中使用 *temperature* 属性。 
 

### 完成以下步骤以配置这两个规则：

1. 选择要与规则关联的逻辑接口。在此场景中，使用的是标识为 *5846ed076522050001db0e12* 的逻辑接口。 
2. 使用以下 API 查看与逻辑接口关联的当前规则：  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
由于当前没有规则与逻辑接口 *5846ed076522050001db0e12* 相关联，因此将返回空列表。  
3. 使用以下 API 添加名为 **tempRuleMax** 的规则：  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
在请求主体中包含规则名称和条件参数：  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
以下示例显示了对 POST 方法的响应：  
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
如果在逻辑接口 *5846ed076522050001db0e12* 上定义的设备状态 *temperature* 属性超过为设备实例 *tSensor* 定义的 *tempThresholdMax* 元数据中指定的值，那么会触发 **tempRuleMax** 规则。在本示例中，值为摄氏 44 度。  
4. 使用以下 API 添加名为 **tempRuleMin** 的规则：  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
在请求主体中包含规则名称和条件参数：  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
以下示例显示了对 POST 方法的响应：  
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
如果在逻辑接口 *5846ed076522050001db0e12* 上定义的设备状态 *temperature* 属性低于为设备实例 *tSensor* 定义的 *tempThresholdMin* 元数据中指定的温度值，那么会触发 **tempRuleMin** 规则。在本示例中，值为摄氏 10 度。  
5. 使用 **activate-configuration** 操作验证并激活配置。添加规则后必须激活配置，因为添加规则的操作会导致对草稿模型进行更改。**activate-configuration** 操作必须对设备类型的草稿版本执行。  
以下示例显示了 PATCH 请求，其中对设备类型的草稿版本执行 **activate-configuration** 操作：  
```  
PATCH /draft/device/types/{typeId}
```  
其中，PATCH 主体的有效内容包含以下内容：
  
```  
{
  "operation": "activate-configuration"
  }
```  
有关激活和停用配置的更多信息，请参阅[了解数据管理](../GA_information_management/ga_im_definitions.html#draft_active_resources)。  
6. 使用以下 API 查看活动逻辑接口上的当前规则：  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
以下示例显示了对 GET 方法的响应：  
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

有关使用该 API 的更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window} 文档。


## 生成操作
当规则求值为 *true* 时，将向以下 MQTT 主题发布消息：  
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
以下示例显示了样本消息体：  
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
然后，可以配置在触发规则时启动的操作。例如，当规则求值为 *true* 时，可以使用 [Node-RED](../applications/dev_nodered.html) 将消息发送给指定的用户。 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

如果在规则评估期间生成错误，那么将在以下 MQTT 主题上发布错误消息：

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

以下示例显示了示例消息体：

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
该消息体包含有关逻辑接口标识、规则标识、设备和状态的信息。可以使用这些信息来帮助调试错误。

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



