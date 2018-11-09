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

# Eingebettete Regeln erstellen (Beta)
{: #im_rules}

Nachdem Sie den Gerätezwilling oder Assetzwilling erfolgreich aktiviert und einige Daten gesendet haben, sollten Sie mit der Nutzung der Daten beginnen, indem Sie Regeln verwenden.

**Wichtig:** Die {{site.data.keyword.iot_full}}-Funktion für eingebettete Regeln für das Datenmanagement steht nur im Rahmen eines eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Informationen zu Regeln und Aktionen

Mit {{site.data.keyword.iot_full}} können Sie Regeln einrichten, die ausgelöst werden, wenn ein Ereignis, das von {{site.data.keyword.iot_short_notm}} empfangen wird, eine Änderung des Gerätestatus bewirkt.

Eingebettete Regeln sind bedingungsbasierte Entscheidungspunkte, die gerätebezogene Echtzeitdaten und vordefinierte Schwellenwerte oder andere Eigenschaftsdaten abgleichen,
um die Regel auszulösen, wenn eine Bedingung erfüllt wird.

Mit eingebetteten Regeln geben Sie Bedingungen an, durch die eine Regel ausgelöst wird. Sie können dann eine Aktion als Antwort auf den Auslöser einrichten, indem Sie beispielsweise einen Alert an das Gerät eines Benutzers und eine E-Mail an einen Administrator senden, wenn die Temperatur Ihres Geräts Spitzenwerte erreicht.


## Erklärung der Regeln

Regeln sind einer [logischen Schnittstelle](../GA_information_management/ga_im_definitions.html#resources) zugeordnet und werden für den [Gerätestatus](../GA_information_management/ga_im_definitions.html#resources) geschrieben. Sie können eine oder mehrere Regeln einer logischen Schnittstelle zuordnen. 

Regeln werden ausgewertet, wenn ein Geräteereignis, das von {{site.data.keyword.iot_short_notm}} empfangen wird, sich auf den Gerätestatus auswirkt, der durch eine logische Schnittstelle definiert ist. Dies wird als 'ereignisgesteuerte Auswertung' bezeichnet.

Für jede Regel muss ein **Name** und ein Bedingungsparameter (**condition**) angegeben sein. Der Parameter **condition** muss den folgenden Kriterien entsprechen:  
 - Der Parameter muss ein Ausdruck sein.  
 - Der Parameter muss mit dem booleschen Wert **true** oder **false** bewertet werden. Wenn der Bedingungsausdruck als **true** bewertet wird, wird eine MQTT-Nachricht in einem MQTT-Topic publiziert.  

## Erklärung der Ausdruckssprache

Richten Sie mithilfe der Ausdruckssprache eine Regellogik ein, um Bedingungen anzugeben, unter denen eine Regel ausgelöst wird (oder auch nicht). Eine Regel wird jedes Mal ausgelöst, wenn ihre Bedingungen erfüllt sind. 

Die Ausdruckssprache stellt eine Untergruppe von [JSONata ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/index.html){:new_window} dar. Weitere Informationen zur Verwendung der Ausdruckssprache finden Sie in [Erklärung der Zuordnungsausdruckssprache](../GA_information_management/mapping_expression_language.html). 

Die Zuordnungsausdruckssprache wurde durch die Einführung der Variablen '$state' und '$instance', die für die Verwendung in Ausdrücken definiert wurden, ergänzt und kann nur zusammen mit der Funktion für Regeln für das Datenmanagement verwendet werden. JSON wird an diese Variablen gebunden, bevor der Ausdruck ausgewertet wird.

- Die Variable '$state' wird verwendet, um eine Eigenschaft für den Gerätestatus auszuwählen, die in einem Ausdruck verwendet werden soll, z. B. *$state.temperature*.
- Die Variable '$instance' wird verwendet, um ein Geräteattribut auszuwählen, das in einem Ausdruck verwendet werden soll, z. B. *$instance.metadata.tempThresholdMax*.

Sie können diese Variablen verwenden, damit Sie in Ihren Regelbedingungsausdrücken keine fest codierten Werte benötigen und für die einzelnen Geräteinstanzen einen anderen Schwellenwert festlegen können. Die Variablen '$state' und '$instance' werden in dem Beispiel im folgenden Abschnitt verwendet.  


## Regeln konfigurieren

In diesem Szenario wird gezeigt, wie Sie zwei Regeln erstellen können. Das Szenario basiert auf der Konfiguration, die in [Schrittweise Anleitung 1](../GA_information_management/ga_im_index_scenario.html) erstellt wurde. 

Eine Regel heißt **tempRuleMax**, die ausgelöst wird, wenn ein Temperaturereignis von {{site.data.keyword.iot_short_notm}}  empfangen wird, das dazu führt, dass die Eigenschaft *temperature* des Gerätestatus 44 Grad Celsius überschreitet. Die andere Regel heißt **tempRuleMin**, die ausgelöst wird, wenn ein Temperaturereignis von {{site.data.keyword.iot_short_notm}} empfangen wird, das bewirkt, dass die Eigenschaft *temperature* des Gerätestatus unter 10 Grad Celsius fällt. 

Um die Regeln in diesem Beispiel zu erstellen, sind Informationen zu den folgenden Ressourcen erforderlich, die als Teil der schrittweisen Anleitung 1 konfiguriert sind: 

- Die Geräteinstanz *tSensor*.   

Dieses Gerät publiziert Temperaturereignisse, die in Grad Celsius gemessen werden. Die folgenden Metadaten sind der Geräteinstanz *tSensor* zugeordnet:
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
Die Variablen und zugehörigen Werte, die in den Metadaten der Geräteinstanz definiert sind, sind erforderlich, damit die Informationen im Ausdruck für die Regelbedingung verwendet werden können.  

**Tipp:** Metadaten können nach dem Erstellen über die Seite *Geräteinformationen* des Dashboards einer Geräteinstanz hinzugefügt oder auf diese aktualisiert werden. Informationen zur Verwendung des Dashboards finden Sie in [Einführung zum Datenmanagement mithilfe der Webschnittstelle](../GA_information_management/im_ui_flow.html).
 
- Die logische Schnittstelle mit der ID *5846ed076522050001db0e12*. 

Die ID ist erforderlich, damit die Regeln mit der richtigen logischen Schnittstellenressource verknüpft werden können.
Eine logische Schnittstelle wird verwendet, um die normalisierte Ansicht für den Gerätestatus zu definieren, der als Antwort auf eingehende Geräteereignisse aktualisiert wird. In diesem Beispiel definiert die logische Schnittstelle die Statuseigenschaft *temperature* in der folgenden Struktur:
```
{
"temperature" : <aktueller Temperaturwert in Celsius>
  }
```
Die Eigenschaft *temperature* wird im Ausdruck für die Regelbedingung verwendet. 
 

### Führen Sie die folgenden Schritte aus, um die beiden Regeln zu konfigurieren:

1. Wählen Sie die logische Schnittstelle aus, die Ihrer Regel zugeordnet werden soll. In diesem Szenario wird die logische Schnittstelle mit der ID *5846ed076522050001db0e12* verwendet. 
2. Zeigen Sie die aktuellen Regeln an, die der logischen Schnittstelle zugeordnet sind, indem Sie die folgende API verwenden:  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
Da der logischen Schnittstelle *5846ed076522050001db0e12* momentan keine Regeln zugeordnet sind, wird eine leere Liste zurückgegeben.  
3. Fügen Sie eine Regel namens **tempRuleMax** mit der folgenden API hinzu:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Schließen Sie den Regelnamen und den Bedingungsparameter in den Hauptteil der Anforderung ein:  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
Das folgende Beispiel zeigt die Antwort auf die POST-Methode:  
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
Die Regel **tempRuleMax** wird ausgelöst, wenn die in der logischen Schnittstelle *5846ed0765220522050001db0e12* definierte Eigenschaft *temperature* des Gerätestatus den in den Metadaten für *tempThresholdMax* angegebenen Temperaturwert überschreitet, der für die Geräteinstanz *tSensor* definiert ist. In diesem Beispiel beträgt der Wert 44 Grad Celsius.  
4. Fügen Sie eine Regel namens **tempRuleMin** mit der folgenden API hinzu:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Schließen Sie den Regelnamen und den Bedingungsparameter in den Hauptteil der Anforderung ein:  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
Das folgende Beispiel zeigt die Antwort auf die POST-Methode:  
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
Die Regel **tempRuleMin** wird ausgelöst, wenn die in der logischen Schnittstelle *5846ed0765220522050001db0e12* definierte Eigenschaft *temperature* des Gerätestatus unter den in den Metadaten für *tempThresholdMin* angegebenen Wert fällt, der für die Geräteinstanz *tSensor* definiert ist. In diesem Beispiel beträgt der Wert 10 Grad Celsius.  
5. Überprüfen und aktivieren Sie die Konfiguration mithilfe der Operation **activate-configuration**. Sie müssen Ihre Konfiguration aktivieren, nachdem Sie eine Regel hinzugefügt haben, weil die Aktion zum Hinzufügen der Regel zu einer Änderung am Entwurfsmodell führt. Die Operation **activate-configuration** muss für die Entwurfsversion des Gerätetyps ausgeführt werden.  
Im folgenden Beispiel wird die PATCH-Anforderung dargestellt, für die eine Operation **activate-configuration** für eine Entwurfsversion eines Gerätetyps ausgeführt wird:  
```  
PATCH /draft/device/types/{typeId} 
```  
Hierbei enthalten die Nutzdaten des PATCH-Hauptteils den folgenden Inhalt:  
```  
{
  "operation": "activate-configuration"
}
```  
Weitere Informationen zum Aktivieren und Inaktivieren Ihrer Konfiguration finden Sie in [Erklärung des Datenmanagements](../GA_information_management/ga_im_definitions.html#draft_active_resources).  
6. Zeigen Sie die aktuellen Regeln für die aktive logische Schnittstelle an, indem Sie die folgende API verwenden:  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Das folgende Beispiel zeigt eine Antwort auf die GET-Methode:  
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

Weitere Informationen zur Verwendung der API finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window}.

## Aktion generieren
Wenn eine Regel die Bewertung *true* ausgibt, wird eine Nachricht im folgenden MQTT-Topic publiziert:  
```
iot-2/intf/<logicalInterfaceId>/rule/<regel-id>/evt/trigger
```  
Das folgende Beispiel zeigt einen Nachrichtenhauptteil:  
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
Anschließend können Sie eine Aktion konfigurieren, die initiiert wird, wenn eine Regel ausgelöst wird. Sie können beispielsweise [Node-RED](../applications/dev_nodered.html) verwenden, um eine Nachricht an einen bestimmten Benutzer zu senden, wenn eine Regel die Bewertung *true* ausgibt. 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

Wenn während der Regelauswertung ein Fehler generiert wird, wird eine Fehlernachricht im folgenden MQTT-Topic publiziert:

```
iot-2/intf/<logicalInterfaceId>/rule/<regel-id>/err/data
```

Das folgende Beispiel zeigt einen Nachrichtenhauptteil:

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
Der Nachrichtenhauptteil enthält Informationen zur ID der logischen Schnittstelle, zur Regel-ID, zum Gerät und zum Status. Sie können diese Informationen für die Fehlerbehebung verwenden.

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



