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

# Creazione delle regole integrate (Beta)
{: #im_rules}

Dopo aver attivato correttamente il tuo dispositivo gemello o asset gemello ed inviato alcuni dati, è il momento di rendere utili tali dati utilizzando le regole.

**Importante:** la funzione delle regole integrate di {{site.data.keyword.iot_full}} per la gestione dei dati è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Informazioni su regole e azioni

Con {{site.data.keyword.iot_full}} puoi impostare delle regole che vengono attivate quando un evento ricevuto da {{site.data.keyword.iot_short_notm}} provoca una modifica allo stato del dispositivo.

Le regole integrate sono punti di decisione basati sulle condizioni che mettono in corrispondenza i dati del dispositivo in tempo reale con valori di soglia predefiniti o
altri dati di proprietà per attivare la regola se viene soddisfatta una condizione.

Con le regole integrate, specifichi le condizioni che attivano una regola. Puoi quindi impostare un'azione in risposta al trigger, ad esempio, l'invio di un avviso al dispositivo dell'utente e un'e-mail a un amministratore, quando la temperatura del dispositivo raggiunge dei picchi.


## Informazioni sulle regole

Le regole sono associate a un'[interfaccia logica](../GA_information_management/ga_im_definitions.html#resources) e vengono scritte sullo [stato del dispositivo](../GA_information_management/ga_im_definitions.html#resources). Puoi associare una o più regole a un'interfaccia logica. 

Le regole vengono valutate quando un evento del dispositivo ricevuto da {{site.data.keyword.iot_short_notm}} potrebbe influire sullo stato del dispositivo definito da un'interfaccia logica. Questo è chiamato "valutazione guidata dagli eventi".

In ogni regola deve essere definito un parametro di **nome** e **condizione**. Il parametro **condizione** deve essere conforme ai seguenti criteri:  
 - Il parametro deve essere un'espressione.  
 - Il parametro deve restituire un valore booleano **true** o **false**. Se l'espressione della condizione viene valutata come **true**, un messaggio MQTT viene pubblicato su un argomento MQTT.  

## Informazioni sul linguaggio delle espressioni

Imposta la logica della regola utilizzando il linguaggio delle espressioni per specificare le condizioni per cui una regola viene o non viene attivata. Una regola viene attivata ogni volta che vengono soddisfatte le sue condizioni. 

Il linguaggio delle espressioni è un sottoinsieme di [JSONata ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/index.html){:new_window}. Per ulteriori informazioni sull'utilizzo del linguaggio delle espressioni, vedi [Informazioni sul linguaggio dell'espressione di associazione](../GA_information_management/mapping_expression_language.html). 

Il linguaggio delle espressioni di associazione è stato esteso per essere utilizzato con la funzione delle regole di gestione dei dati attraverso l'introduzione delle variabili $state e $instance definite per l'utilizzo nelle espressioni. JSON viene associato a queste variabili prima che venga valutata l'espressione.

- La variabile $state viene utilizzata per selezionare una proprietà sullo stato del dispositivo da utilizzare in un'espressione, ad esempio *$state.temperature*.
- La variabile $instance viene utilizzata per selezionare un attributo di dispositivo da utilizzare in un'espressione, ad esempio *$instance.metadata.tempThresholdMax*.

Potresti voler utilizzare queste variabili in modo da non dover preimpostare i valori nelle espressioni di condizione della regola e puoi impostare un valore di soglia diverso per ogni istanza del dispositivo. Le variabili $state e $instance sono utilizzate nell'esempio riportato nella seguente sezione.  


## Configurazione delle regole

Questo scenario ti mostra come creare due regole e si basa sulla configurazione creata in [Guida passo a passo 1](../GA_information_management/ga_im_index_scenario.html). 

Una regola è denominata **tempRuleMax**, che viene attivata quando {{site.data.keyword.iot_short_notm}} riceve un evento di temperatura che fa sì che la proprietà *temperature* dello stato del dispositivo superi i 44 gradi Celsius. L'altra regola è denominata **tempRuleMin**, che viene attivata quando {{site.data.keyword.iot_short_notm}} riceve un evento di temperatura che fa sì che la proprietà *temperature* dello stato del dispositivo scenda al di sotto di 10 gradi Celsius. 

Per creare le regole in questo esempio, sono necessarie informazioni sulle seguenti risorse configurate come parte della guida passo a passo 1: 

- L'istanza del dispositivo *tSensor*.   

Questo dispositivo pubblica gli eventi di temperatura misurati in gradi Celsius. All'istanza del dispositivo *tSensor* sono associati i seguenti metadati:
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
Le variabili e i valori associati definiti nei metadati dell'istanza del dispositivo sono obbligatori affinché le informazioni possano essere utilizzate nell'espressione della condizione della regola.   

**Suggerimento:** è possibile aggiungere o aggiornare i metadati nell'istanza del dispositivo dopo che è stata creata utilizzando la pagina *Device Information* del dashboard. Per informazioni sull'utilizzo del dashboard, consulta [Introduzione alla gestione dei dati utilizzando l'interfaccia web](../GA_information_management/im_ui_flow.html).
 
- L'interfaccia logica con l'identificativo *5846ed076522050001db0e12*. 

L'identificativo è obbligatorio affinché le regole possano essere associate alla risorsa dell'interfaccia logica corretta.
Un'interfaccia logica viene utilizzata per definire la vista normalizzata sullo stato del dispositivo, che viene aggiornato in risposta agli eventi del dispositivo in entrata. Nel nostro esempio, l'interfaccia logica definisce la proprietà dello stato *temperature* nella seguente struttura:
```
{
"temperature" : <current temperature value in Celsius>
  }
```
La proprietà *temperature* viene utilizzata nell'espressione della condizione della regola. 
 

### Completa la seguente procedura per configurare le due regole:

1. Seleziona l'interfaccia logica che vuoi associare alla regola. In questo scenario, utilizziamo l'interfaccia logica con l'identificativo *5846ed076522050001db0e12*. 
2. Visualizza le regole correnti associate all'interfaccia logica utilizzando la seguente API:  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
Poiché nessuna regola è attualmente associata all'interfaccia logica *5846ed076522050001db0e12*, viene restituito un elenco vuoto.  
3. Aggiungi una regola denominata **tempRuleMax** utilizzando la seguente API:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Includi il nome della regola e un parametro di condizione nel corpo della richiesta:  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
Il seguente esempio mostra la risposta al metodo POST:  
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
La regola **tempRuleMax** viene attivata se la proprietà *temperature* dello stato del dispositivo definita sull'interfaccia logica *5846ed076522050001db0e12* supera il valore di temperatura specificato nei metadati *tempThresholdMax* definiti per l'istanza del dispositivo *tSensor*. Nel nostro esempio, il valore è 44 gradi Celsius.  
4. Aggiungi una regola denominata **tempRuleMin** utilizzando la seguente API:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Includi il nome della regola e un parametro di condizione nel corpo della richiesta:  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
Il seguente esempio mostra la risposta al metodo POST:  
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
La regola **tempRuleMin** viene attivata se la proprietà *temperature* dello stato del dispositivo definita sull'interfaccia logica *5846ed076522050001db0e12* scende al di sotto del valore specificato nei metadati *tempThresholdMin* definiti per l'istanza del dispositivo *tSensor*. Nel nostro esempio, il valore è 10 gradi Celsius.  
5. Convalida e attiva la tua configurazione utilizzando l'operazione **activate-configuration**. Devi attivare la configurazione dopo aver aggiunto una regola, poiché l'azione di aggiunta della regola comporta una modifica al modello di bozza. L'operazione **activate-configuration** deve essere eseguita nella versione di bozza del tipo di dispositivo.  
Il seguente esempio mostra un richiesta PATCH in cui l'operazione **activate-configuration** viene eseguita in una versione di bozza di un tipo di dispositivo:  
```  
PATCH /draft/device/types/{typeId} 
```  
dove il payload del corpo PATCH contiene il seguente contenuto:  
```  
{
  "operation": "activate-configuration"
}
```  
Per ulteriori informazioni sull'attivazione e disattivazione della tua configurazione, consulta [Informazioni sulla gestione dei dati](../GA_information_management/ga_im_definitions.html#draft_active_resources).  
6. Visualizza le regole correnti sull'interfaccia logica attiva utilizzando la seguente API:  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Il seguente esempio mostra una risposta al metodo GET:  
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

Per ulteriori dettagli sull'utilizzo dell'API, consulta la documentazione [API REST HTTP {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window}.

## Generazione di un'azione
Se una regola viene valutata come *true*, viene pubblicato un messaggio nel seguente argomento MQTT:  
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
Di seguito è riportato un corpo del messaggio di esempio:  
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
Puoi quindi configurare un'azione che viene avviata quando viene attivata una regola. Ad esempio, potresti utilizzare [Node-RED](../applications/dev_nodered.html) per inviare un messaggio a un utente specificato quando una regola viene valutata come *true*. 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

Se durante la valutazione della regola viene generato un errore, viene pubblicato un messaggio di errore nel seguente argomento MQTT:

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

Di seguito è riportato un corpo del messaggio di esempio:

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
Il corpo del messaggio include informazioni sull'ID dell'interfaccia logica, l'ID della regola, il dispositivo e lo stato. Puoi utilizzare le informazioni per facilitare il debug dell'errore.

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



