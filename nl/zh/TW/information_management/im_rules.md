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

# 建立內嵌規則（測試版）
{: #im_rules}

在您順利啟動裝置對應項或資產對應項並傳送一些資料之後，即可使用規則，讓該資料為您所適用。

**重要事項：**資料管理的 {{site.data.keyword.iot_full}} 內嵌規則特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## 關於規則及動作

使用 {{site.data.keyword.iot_full}}，您可以設定規則，以在 {{site.data.keyword.iot_short_notm}} 接收到的事件導致裝置狀態變更時觸發。

內嵌規則是條件型決策點，用來比對即時裝置資料與預先定義的臨界值或其他內容資料，以在符合條件時觸發規則。

使用內嵌規則，您可以指定觸發規則的條件。然後，您可以設定動作以回應觸發程式，例如，在裝置溫度驟升時，將警示傳送給使用者的裝置，並將電子郵件傳送給管理者。


## 瞭解規則

規則與[邏輯介面](../GA_information_management/ga_im_definitions.html#resources)相關聯，並且是根據[裝置狀態](../GA_information_management/ga_im_definitions.html#resources)撰寫而成。您可以將一個以上的規則與一個邏輯介面產生關聯。 

{{site.data.keyword.iot_short_notm}} 接收到的裝置事件可能會影響邏輯介面所定義的裝置狀態時，即會評估規則。這稱為「事件驅動評估」。

每個規則都必須已定義 **name** 及 **condition** 參數。**condition** 參數必須符合下列準則：  
 - 此參數必須是表示式。  
 - 此參數必須評估為布林值 **true** 或 **false**。如果條件表示式評估為 **true**，則會針對 MQTT 主題發佈 MQTT 訊息。  

## 瞭解表示式語言

使用表示式語言指定觸發或未觸發規則時的條件，來設定規則邏輯。每次符合規則的條件時，即會觸發規則。 

表示式語言是 [JSONata ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/index.html){:new_window} 的子集。如需使用表示式語言的相關資訊，請參閱[瞭解對映表示式語言](../GA_information_management/mapping_expression_language.html)。 

已擴充對映表示式語言，透過引進定義用於表示式的 $state 及 $instance 變數以與資料管理規則特性搭配使用。評估表示式之前，JSON 會連結至這些變數。

- $state 變數是用來選取要在表示式中使用之裝置狀態的內容，例如，*$state.temperature*。
- $instance 變數是用來選取要在表示式中使用的裝置屬性，例如，*$instance.metadata.tempThresholdMax*。

您可能要使用這些變數，讓您不需要將值寫在規則條件表示式中，而且可以為每個裝置實例設定不同的臨界值。$state 及 $instance 變數會用於下一節的範例中。  


## 配置規則

此情境顯示如何建立兩個規則，並以[逐步手冊 1](../GA_information_management/ga_im_index_scenario.html) 中所建立的配置為基礎。 

其中一個規則稱為 **tempRuleMax**，觸發時機為 {{site.data.keyword.iot_short_notm}} 接收到導致裝置狀態的 *temperature* 內容超出攝氏 44 度的溫度事件時。另一個規則稱為 **tempRuleMin**，觸發時機為 {{site.data.keyword.iot_short_notm}} 接收到導致裝置狀態的 *temperature* 內容低於攝氏 10 度的溫度事件時。 

若要在此範例中建立規則，需要配置為「逐步手冊 1」一部分的下列資源的相關資訊： 

- *tSensor* 裝置實例。   

此裝置會發佈以攝氏度數測量的溫度事件。下列 meta 資料與 *tSensor* 裝置實例相關聯：
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
需要裝置實例 meta 資料中所定義的變數及關聯值，才能將這項資訊用於規則條件表示式中。  

**提示：**從儀表板的*裝置資訊* 頁面建立裝置實例之後，即可新增或更新該裝置實例的 meta 資料。如需使用儀表板的相關資訊，請參閱[使用 Web 介面開始使用資料管理](../GA_information_management/im_ui_flow.html)。
 
- ID 為 *5846ed076522050001db0e12* 的邏輯介面。 

需要提供 ID，規則才能與正確的邏輯介面資源產生關聯。邏輯介面用來將正規化視圖定義為裝置狀態，而此狀態會更新，以回應入埠裝置事件。在我們的範例中，邏輯介面定義下列結構的狀態內容 *temperature*：
```
{
"temperature" : <current temperature value in Celsius>
  }
```
*temperature* 內容用於規則條件表示式中。 
 

### 請完成下列步驟來配置這兩個規則：

1. 選取您要與規則產生關聯的邏輯介面。在此情境下，將會使用 ID 為 *5846ed076522050001db0e12* 的邏輯介面。 
2. 使用下列 API，來檢視與邏輯介面相關聯的現行規則：  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
因為目前沒有任何規則與邏輯介面 *5846ed076522050001db0e12* 相關聯，所以會傳回空清單。  
3. 使用下列 API，以新增稱為 **tempRuleMax** 的規則：  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
在要求內文中包括規則名稱及 condition 參數：  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
下列範例顯示 POST 方法的回應：  
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
如果邏輯介面 *5846ed076522050001db0e12* 上定義的裝置狀態 *temperature* 內容超出針對裝置實例 *tSensor* 所定義之 *tempThresholdMax* meta 資料中指定的溫度值，則會觸發 **tempRuleMax** 規則。在我們的範例中，此值是攝氏 44 度。  
4. 使用下列 API，以新增稱為 **tempRuleMin** 的規則：  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
在要求內文中包括規則名稱及 condition 參數：  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
下列範例顯示 POST 方法的回應：  
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
如果邏輯介面 *5846ed076522050001db0e12* 上所定義的裝置狀態 *temperature* 內容低於針對裝置實例 *tSensor* 所定義之 *tempThresholdMin* meta 資料中指定的值，則會觸發 **tempRuleMin** 規則。在我們的範例中，此值是攝氏 10 度。  
5. 使用 **activate-configuration** 作業，以驗證及啟動您的配置。您必須在新增規則之後啟動配置，因為新增規則的動作會導致草稿模型變更。必須對裝置類型的初版執行 **activate-configuration** 作業。  
下列範例顯示對裝置類型的初版執行 **activate-configuration** 作業的 PATCH 要求：  
```  
PATCH /draft/device/types/{typeId}
```  
其中，PATCH 主體的有效負載包含下列內容：
  
```  
{
  "operation": "activate-configuration"
  }
```  
如需啟動及取消啟動配置的相關資訊，請參閱[瞭解資料管理](../GA_information_management/ga_im_definitions.html#draft_active_resources)。  
6. 使用下列 API，來檢視作用中邏輯介面上的現行規則：  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
下列範例顯示 GET 方法的回應：  
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

如需使用 API 的詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window} 文件。

## 產生動作
規則評估為 *true* 時，會將訊息發佈至下列 MQTT 主題：  
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
下列範例顯示範例訊息內文：  
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
然後，您可以配置在觸發規則時起始的動作。例如，規則評估為 *true* 時，您可以使用 [Node-RED](../applications/dev_nodered.html) 將訊息傳送給指定的使用者。 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

如果在規則評估期間產生錯誤，則會針對下列 MQTT 主題發佈錯誤訊息：

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

下列範例顯示範例訊息內文：

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
訊息內文包括邏輯介面 ID、規則 ID、裝置及狀態的相關資訊。您可以使用此資訊，協助您對錯誤進行除錯。

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



