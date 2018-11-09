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

# 임베디드 규칙 작성(베타)
{: #im_rules}

쌍둥이 디바이스 또는 쌍둥이 자산을 성공적으로 활성화하고 일부 데이터를 전송했으므로, 이제는 규칙을 사용하여 해당 데이터를 작동시켜야 합니다. 

**중요:** 데이터 관리를 위한 {{site.data.keyword.iot_full}} 임베디드 규칙 기능은 제한된 베타 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

## 규칙 및 조치 정보

{{site.data.keyword.iot_full}}에서 사용자는 {{site.data.keyword.iot_short_notm}}에서 수신한 이벤트로 인해 디바이스 상태가 변경될 때 트리거되는 규칙을 설정할 수 있습니다. 

임베디드 규칙은 실시간 디바이스 데이터를 사전 정의된 임계값이나 기타 특성 데이터와 일치시켜서 조건이 충족되면 규칙을 트리거하는 조건 기반의 의사결정 지점입니다. 

임베디드 규칙을 사용하여 사용자는 규칙을 트리거하는 조건을 지정합니다. 그리고 트리거에 대한 응답으로 조치를 설정할 수 있습니다(예: 디바이스의 온도가 치솟으면 사용자 디바이스에 경보를 보내고 관리자에게 이메일을 발송함). 


## 규칙 이해

규칙은 [논리 인터페이스](../GA_information_management/ga_im_definitions.html#resources)와 연관되어 있으며 [디바이스 상태](../GA_information_management/ga_im_definitions.html#resources)에 대해 작성됩니다. 하나 이상의 규칙을 논리 인터페이스와 연관시킬 수 있습니다. 

규칙은 {{site.data.keyword.iot_short_notm}}에서 수신한 디바이스 이벤트가 논리 인터페이스에서 정의한 디바이스 상태에 영향을 줄 수 있을 때 평가됩니다. 이를 "이벤트 구동 평가" 라고 합니다. 

각 규칙에는 **name** 및 **condition** 매개변수가 정의되어 있어야 합니다. **condition** 매개변수는 다음 기준을 준수해야 합니다.   
 - 매개변수는 표현식이어야 합니다.  
 - 매개변수는 **true** 또는 **false**의 부울 값으로 평가되어야 합니다. 조건식이 **true**로 평가되면 MQTT 메시지가 MQTT 주제에 공개됩니다.   

## 표현식 언어 이해

규칙이 트리거되거나 트리거되지 않는 경우에 대한 조건을 지정하도록 표현식 언어를 사용하여 규칙 로직을 설정하십시오. 규칙은 해당 조건이 충족될 때마다 트리거됩니다. 

표현식 언어는 [JSONata ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/index.html){:new_window}의 서브세트입니다. 표현식 언어의 사용에 대한 자세한 정보는 [맵핑 표현식 언어 이해](../GA_information_management/mapping_expression_language.html)를 참조하십시오.  

맵핑 표현식 언어는 표현식에서 사용하기 위해 정의된 $state 및 $instance 변수의 도입을 통해 데이터 관리 규칙 기능에서 사용할 수 있도록 확장되었습니다. JSON은 표현식 평가 전에 이러한 변수에 바인드됩니다.

- $state 변수는 표현식에서 사용할 디바이스 상태의 특성을 선택하는 데 사용됩니다(예: *$state.temperature*).
- $instance 변수는 표현식에서 사용할 디바이스 속성을 선택하는 데 사용됩니다(예: *$instance.metadata.tempThresholdMax*).

규칙 조건 표현식의 값을 하드 코드화할 필요가 없으며 각 디바이스 인스턴스에 대해 다른 임계값을 설정할 수 있도록 이러한 변수를 사용하고자 할 수 있습니다. $state 및 $instance 변수는 다음 섹션의 예제에서 사용됩니다.   


## 규칙 구성

이 시나리오에서는 두 개의 규칙을 작성하는 방법을 보여주며, 이는 [단계별 안내서 1](../GA_information_management/ga_im_index_scenario.html)에서 작성된 구성을 기반으로 합니다.  

하나의 규칙은 **tempRuleMax**라고 하며, 이는 디바이스 상태의 *temperature* 특성이 섭씨 44도를 초과하도록 하는 온도 이벤트를 {{site.data.keyword.iot_short_notm}}에서 수신할 때 트리거됩니다. 또 하나의 규칙은 **tempRuleMin**이라고 하며, 이는 디바이스 상태의 *temperature* 특성이 섭씨 10도 미만으로 떨어지도록 하는 온도 이벤트를 {{site.data.keyword.iot_short_notm}}에서 수신할 때 트리거됩니다.  

이 예제에서 규칙을 작성하려면 단계별 안내서 1의 일부로서 구성된 다음 리소스에 대한 정보가 필요합니다.  

- *tSensor* 디바이스 인스턴스.   

이 디바이스는 섭씨로 측정되는 온도 이벤트를 공개합니다. 다음의 메타데이터는 *tSensor* 디바이스 인스턴스와 연관되어 있습니다.
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
해당 정보가 규칙 조건 표현식에서 사용될 수 있도록, 디바이스 인스턴스 메타데이터에 정의된 변수 및 연관 값이 필요합니다.   

**팁:** 대시보드의 *디바이스 정보* 페이지에서 작성된 후에는 메타데이터를 디바이스 인스턴스에 추가하거나 업데이트할 수 있습니다. 대시보드 사용에 대한 정보는 [웹 인터페이스를 사용하여 데이터 관리 시작하기](../GA_information_management/im_ui_flow.html)를 참조하십시오. 
 
- ID *5846ed076522050001db0e12*의 논리 인터페이스.  

ID는 규칙이 올바른 논리 인터페이스 리소스와 연관될 수 있도록 하기 위해 필요합니다.
논리 인터페이스는 디바이스 상태에 대한 정규화된 보기를 정의하는 데 사용되며, 이는 인바운드 디바이스 이벤트에 대한 응답으로 업데이트됩니다. 이 예에서 논리 인터페이스는 다음 구조에서 상태 특성 *temperature*를 정의합니다.
```
{
"temperature" : <current temperature value in Celsius>
}
```
*temperature* 특성은 규칙 조건식에서 사용됩니다.  
 

### 다음 단계를 완료하여 2개의 규칙을 구성하십시오.

1. 규칙과 연관시킬 논리 인터페이스를 선택하십시오. 이 시나리오에서는 ID *5846ed076522050001db0e12*의 논리 인터페이스를 사용합니다.  
2. 다음 API를 사용하여 논리 인터페이스와 연관된 현재 규칙을 보십시오.   
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
현재는 논리 인터페이스 *5846ed076522050001db0e12*와 연관된 규칙이 없으므로 비어 있는 목록이 리턴됩니다.   
3. 다음 API를 사용하여 **tempRuleMax**라고 하는 규칙을 추가하십시오.   
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
요청의 본문에 규칙 이름 및 조건 매개변수를 포함시키십시오.  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
다음 예제는 POST 메소드에 대한 응답을 표시합니다.   
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
**tempRuleMax** 규칙은 논리 인터페이스 *5846ed076522050001db0e12*에 정의된 디바이스 상태 *temperature* 특성이 디바이스 인스턴스 *tSensor*에 대해 정의된 *tempThresholdMax* 메타데이터에 지정된 온도 값을 초과하는 경우에 트리거됩니다. 이 예제에서 값은 섭씨 44도입니다.   
4. 다음 API를 사용하여 **tempRuleMin**이라고 하는 규칙을 추가하십시오.   
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
요청의 본문에 규칙 이름 및 조건 매개변수를 포함시키십시오.  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
다음 예제는 POST 메소드에 대한 응답을 표시합니다.   
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
**tempRuleMin** 규칙은 논리 인터페이스 *5846ed076522050001db0e12*에 정의된 디바이스 상태 *temperature* 특성이 디바이스 인스턴스 *tSensor*에 대해 정의된 *tempThresholdMin* 메타데이터에 지정된 값 미만으로 떨어지는 경우에 트리거됩니다. 이 예제에서 값은 섭씨 10도입니다.   
5. **activate-configuration** 오퍼레이션을 사용하여 구성을 유효성 검증하고 활성화하십시오. 규칙을 추가하는 조치의 결과로 드래프트 모델이 변경되므로, 규칙을 추가한 후에는 구성을 활성화해야 합니다. **activate-configuration** 오퍼레이션은 디바이스 유형의 드래프트 버전에서 수행되어야 합니다.   
다음 예제는 **activate-configuration** 오퍼레이션이 디바이스 유형의 드래프트 버전에서 수행되는 PATCH 요청을 표시합니다.  
```  
PATCH /draft/device/types/{typeId} 
```  
여기서 PATCH 본문의 페이로드에 다음 컨텐츠가 포함됩니다.  
```  
{
  "operation": "activate-configuration"
}
```  
구성의 활성화 및 비활성화에 대한 자세한 정보는 [데이터 관리 이해](../GA_information_management/ga_im_definitions.html#draft_active_resources)를 참조하십시오.   
6. 다음 API를 사용하여 활성 논리 인터페이스의 현재 규칙을 보십시오.   
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
다음 예제는 GET 메소드에 대한 응답을 표시합니다.  
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

API 사용에 대한 세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window} 문서를 참조하십시오. 

## 조치 생성
규칙이 *true*로 평가되면 메시지가 다음 MQTT 주제에 공개됩니다.   
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
다음 예제는 샘플 메시지 본문을 표시합니다.   
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
그 다음에는 규칙이 트리거될 때 시작되는 조치를 구성할 수 있습니다. 예를 들어, [Node-RED](../applications/dev_nodered.html)를 사용하여 규칙이 *true*로 평가될 때 지정된 사용자에게 메시지를 전송할 수 있습니다.  



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

규칙 평가 중에 오류가 생성되면 오류 메시지가 다음 MQTT 주제에서 공개됩니다. 

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

다음 예제는 예제 메시지 본문을 표시합니다. 

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
메시지 본문에는 논리 인터페이스 ID, 규칙 ID, 디바이스 및 상태에 대한 정보가 포함됩니다. 해당 정보를 사용하면 오류를 디버깅하는 데 도움이 됩니다. 

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



