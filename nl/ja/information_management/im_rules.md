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

# 組み込みルールの作成 (ベータ)
{: #im_rules}

デバイス・ツインまたはアセット・ツインのアクティブ化とデータの送信に成功したら、ルールを使用してそのデータを活用できるようにします。

**重要:** データ管理用の {{site.data.keyword.iot_full}} 組み込みルール機能は、限定されたベータ・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## ルールとアクションについて

{{site.data.keyword.iot_full}} では、{{site.data.keyword.iot_short_notm}} が受信するイベントが原因でデバイス状態が変更される場合にトリガーされるルールをセットアップできます。

組み込みルールは条件に基づく決定点であり、リアルタイム・デバイス・データを事前定義しきい値または他の
プロパティー・データと突き合わせ、条件が満たされた場合にルールがトリガーされます。

組み込みルールでは、ルールをトリガーする条件を指定します。次に、トリガーに対する応答としてアクションをセットアップできます。例えば、デバイスの温度が急上昇したときに、ユーザーのデバイスにアラートを送信したり、管理者に E メールを送信したりすることができます。


## ルールの解説

ルールは、[論理インターフェース](../GA_information_management/ga_im_definitions.html#resources)に関連付けられ、[デバイス状態](../GA_information_management/ga_im_definitions.html#resources)に対して作成されます。1 つの論理インターフェースに 1 つ以上のルールを関連付けることができます。 

ルールは、{{site.data.keyword.iot_short_notm}} が受信するデバイス・イベントにより、論理インターフェースで定義されたデバイス状態が影響を受ける可能性がある場合に評価されます。これは、「イベント・ドリブン評価」と呼ばれます。

各ルールには、**name** と **condition** のパラメーターが定義されている必要があります。**condition** パラメーターは、以下の基準に準拠している必要があります。  
 - パラメーターは式でなければなりません。  
 - パラメーターはブール値 **true** または **false** に評価されなければなりません。条件式が **true** に評価された場合、MQTT メッセージが MQTT トピックにパブリッシュされます。  

## 式言語の解説

式言語を使用してルール・ロジックをセットアップし、ルールがトリガーされる場合またはされない場合の条件を指定します。条件が満たされるたびに、ルールはトリガーされます。 

式言語は、[JSONata ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/index.html){:new_window} のサブセットです。式言語の使用について詳しくは、[マッピング式言語の解説](../GA_information_management/mapping_expression_language.html)を参照してください。 

マッピング式言語は、式で使用するために定義される $state および $instance 変数の導入により、データ管理ルール機能で使用できるように拡張されています。JSON は、式が評価される前にこれらの変数にバインドされます。

- $state 変数は、式で使用するデバイス状態のプロパティーを選択するために使用されます (例えば、*$state.temperature*)。
- $instance 変数は、式で使用するデバイス属性を選択するために使用されます (例えば、*$instance.metadata.tempThresholdMax*)。

これらの変数を使用すると、ルール条件式で値をハードコーディングする必要がなくなり、デバイス・インスタンスごとに異なるしきい値を設定できます。以下のセクションの例では、$state および $instance 変数を使用しています。  


## ルールの構成

このシナリオでは、[ステップバイステップ・ガイド 1](../GA_information_management/ga_im_index_scenario.html) で作成される構成に基づいて 2 つのルールを作成する方法を示します。 

ルールの 1 つは **tempRuleMax** という名前で、デバイス状態の *temperature* プロパティーが摂氏 44 度を超える温度イベントが {{site.data.keyword.iot_short_notm}} によって受信された場合にトリガーされます。もう 1 つのルールは **tempRuleMin** という名前で、デバイス状態の *temperature* プロパティーが摂氏 10 度を下回る温度イベントが {{site.data.keyword.iot_short_notm}} によって受信された場合にトリガーされます。 

この例でルールを作成するには、ステップバイステップ・ガイド 1 の一部として構成される以下のリソースに関する情報が必要です。 

- *tSensor* デバイス・インスタンス。   

このデバイスは、摂氏で測定した温度イベントをパブリッシュします。 以下のメタデータが *tSensor* デバイス・インスタンスに関連付けられています。
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
デバイス・インスタンス・メタデータに定義されている変数および関連値は、ルール条件式で情報を使用するため必須です。  

**ヒント:** メタデータは、ダッシュボードの*「デバイス情報」* ページから作成された後、デバイス・インスタンスに追加または更新できます。ダッシュボードの使用について詳しくは、[Web インターフェースを使用したデータ管理の概説](../GA_information_management/im_ui_flow.html)を参照してください。
 
- ID が *5846ed076522050001db0e12* の論理インターフェース。 

ID は、ルールが正しい論理インターフェース・リソースに関連付けられるようにするため必須です。
論理インターフェースは、デバイス状態に関する正規化ビューを定義するために使用され、インバウンド・デバイス・イベントに応答して更新されます。この例では、論理インターフェースは、状態プロパティー *temperature* を以下の構造で定義します。
```
{
"temperature" : <current temperature value in Celsius>
  }
```
*temperature* プロパティーは、ルール条件式で使用されます。 
 

### 2 つのルールを構成する手順:

1. ルールに関連付ける論理インターフェースを選択します。このシナリオでは、ID が *5846ed076522050001db0e12* の論理インターフェースを使用します。 
2. 以下の API を使用して、論理インターフェースに関連付けられている現行ルールを表示します。  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
現在、論理インターフェース *5846ed076522050001db0e12* に関連付けられているルールがないため、空のリストが返されます。  
3. 以下の API を使用して、**tempRuleMax** というルールを追加します。  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
要求の本体に、ルール名と条件パラメーターを含めます。  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
この POST メソッドに対する応答の例を以下に示します。  
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
**tempRuleMax** ルールは、論理インターフェース *5846ed076522050001db0e12* で定義されたデバイス状態 *temperature* プロパティーが、デバイス・インスタンス *tSensor* に定義された *tempThresholdMax* メタデータに指定された温度値を超える場合にトリガーされます。この例では、値は摂氏 44 度です。  
4. 以下の API を使用して、**tempRuleMin** というルールを追加します。  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
要求の本体に、ルール名と条件パラメーターを含めます。  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
この POST メソッドに対する応答の例を以下に示します。  
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
**tempRuleMin** ルールは、論理インターフェース *5846ed076522050001db0e12* で定義されたデバイス状態 *temperature* プロパティーが、デバイス・インスタンス *tSensor* に定義された *tempThresholdMin* メタデータに指定された温度値を下回る場合にトリガーされます。この例では、値は摂氏 10 度です。  
5. **activate-configuration** 操作を使用して、構成を検証およびアクティブ化します。ルールを追加するアクションの結果としてドラフト・モデルが変更されるため、ルールを追加した後に構成をアクティブ化する必要があります。**activate-configuration** 操作は、デバイス・タイプのドラフト・バージョンに対して実行する必要があります。  
以下の例では、デバイス・タイプのドラフト・バージョンに対して **activate-configuration** 操作を実行する PATCH 要求を示します。  
```  
PATCH /draft/device/types/{typeId} 
```  
PATCH 本文のペイロードの内容は以下のとおりです。  
```  
{
  "operation" : "activate-configuration"
}
```  
構成のアクティブ化および非アクティブ化について詳しくは、[データ管理の解説](../GA_information_management/ga_im_definitions.html#draft_active_resources)を参照してください。  
6. 以下の API を使用して、アクティブな論理インターフェースの現行ルールを表示します。  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
この GET メソッドに対する応答の例を以下に示します。  
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

API の使用について詳しくは、[{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window} の資料を参照してください。

## アクションの生成
ルールが *true* に評価されると、以下の MQTT トピックにメッセージがパブリッシュされます。  
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
以下の例は、サンプル・メッセージ本体を示しています。  
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
次に、ルールのトリガー時に開始されるアクションを構成できます。例えば、[Node-RED](../applications/dev_nodered.html) を使用して、ルールが *true* に評価されたときに、指定されたユーザーにメッセージを送信できます。 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

ルールの評価中にエラーが生成された場合、以下の MQTT トピックにエラー・メッセージがパブリッシュされます。

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

以下の例は、サンプル・メッセージ本体を示しています。

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
メッセージ本体には、論理インターフェース ID、ルール ID、デバイス、および状態に関する情報が含まれています。この情報は、エラーのデバッグ時に役立ちます。

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



