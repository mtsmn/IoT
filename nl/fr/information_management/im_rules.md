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

# Création de règles imbriquées (bêta)
{: #im_rules}

Après avoir correctement activé votre terminal jumeau ou votre actif jumeau et envoyé des données, vous devez vous assurer que les données fonctionnent pour vous en utilisant des règles.

**Important :** la fonction de règles imbriquées de {{site.data.keyword.iot_full}} pour la gestion des données est uniquement disponible dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## A propos des règles et des actions

Avec {{site.data.keyword.iot_full}}, vous pouvez configurer des règles qui se déclenchent lorsqu'un événement qui est reçu par {{site.data.keyword.iot_short_notm}} change l'état du terminal.

Les règles imbriquées sont des points de décisions basés sur des conditions qui font correspondre les données des terminaux en temps réel avec des valeurs seuil prédéfinies ou avec d'autres données de propriétés pour déclencher la règle si une condition est remplie.

Avec les règles imbriquées, vous spécifiez les conditions qui déclenchent une règle. Vous pouvez ensuite configurer une action en réponse au déclenchement, par exemple, vous pouvez envoyer une alerte au terminal d'un utilisateur et un courrier électronique à un administrateur lorsque la température de votre terminal atteint un pic.


## Connaissance des règles

Les règles sont associées à une [interface logique](../GA_information_management/ga_im_definitions.html#resources) et sont écrites en fonction de l'[état du terminal](../GA_information_management/ga_im_definitions.html#resources). Vous pouvez associer une ou plusieurs règles à une interface logique. 

Les règles sont évaluées lorsqu'un événement de terminal qui est reçu par {{site.data.keyword.iot_short_notm}} pourrait affecter l'état du terminal qui est défini par une interface logique. On l'appelle "event-driven evaluation".

Chaque règle doit avoir un paramètre **name** et **condition** défini. Le paramètre **condition** doit être conforme aux critères suivants :  
 - Le paramètre doit être une expression.  
 - Le paramètre doit donner la valeur booléenne **true** ou **false**. Si l'expression de condition donne la valeur **true**, un message MQTT est publié sur une rubrique MQTT.  

## Connaissance du langage d'expression

Définissez votre logique de règle à l'aide du langage d'expression pour spécifier des conditions indiquant quand une règle doit être déclenchée ou non. Une règle est déclenchée à chaque fois que ses conditions sont remplies. 

Le langage d'expression est un sous-ensemble de [JSONata ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/index.html){:new_window}. Pour en savoir plus sur l'utilisation du langage d'expression, reportez-vous à [Connaissance du langage d'expression de mappage](../GA_information_management/mapping_expression_language.html). 

Le langage d'expression de mappage a été étendu pour l'utiliser avec la fonctions des règles de gestion de données grâce à l'introduction des variables $state et $instance définir pour être utilisées dans les expressions. JSON est lié à ces variables avant que l'expression ne soit évaluée.

- La variable $state est utilisée pour sélectionner une propriété sur l'état du terminal à utiliser dans une expression, par exemple *$state.temperature*.
- La variable $instance est utilisée pour sélectionner un attribut de terminal à utiliser dans une expression, par exemple *$instance.metadata.tempThresholdMax*.

Vous pouvez utiliser ces variables pour ne pas avoir à coder les valeur en dur dans les expressions de vos conditions de règle, et vous pouvez définir une valeur de seuil différente pour chaque instance de terminal. Les variables $state et $instance sont utilisées dans l'exemple de la section ci-dessous.  


## Configuration des règles

Ce scénario vous montre comment créer deux règles et il est basé sur la configuration qui est créée dans le [guide détaillé 1](../GA_information_management/ga_im_index_scenario.html). 

Une règle appelée **tempRuleMax** est déclenchée lorsqu'un événement de température reçu par {{site.data.keyword.iot_short_notm}} fait dépasser de 44 degrés Celsius la propriété *temperature* de l'état du terminal. L'autre règle appelée **tempRuleMin** est déclenchée lorsqu'un événement de température reçu par {{site.data.keyword.iot_short_notm}} fait tomber la propriété *temperature* de l'état du terminal en dessous de 10 degrés Celsius. 

Pour créer les règles dans cet exemple, des informations sont requises sur les ressources suivantes qui sont configurées dans le cadre du guide détaillé 1 : 

- L'instance de terminal *tSensor*.   

Ce terminal publie des événements de température qui sont mesurés en degrés Celsius. Les métadonnées suivantes sont associées à l'instance de terminal *tSensor* :
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
Les variables et valeurs associées qui sont définies dans les métadonnées de l'instance de terminal sont requises pour que l'information puisse être utilisée dans l'expression de la condition de règle.  

**Conseil :** les métadonnées peuvent être ajoutées ou mises à jour dans une instance de terminal après avoir été créées à partir de la page *Informations sur le terminal* du tableau de bord. Pour obtenir des informations sur l'utilisation du tableau de bord, voir [Initiation à la gestion des données via l'interface Web](../GA_information_management/im_ui_flow.html).
 
- L'interface logique avec l'identificateur *5846ed076522050001db0e12*. 

L'identificateur est requis pour que les règles puissent être à la ressource d'interface logique correcte.
Une interface logique est utilisée pour définir la vue normalisée sur l'état du terminal, qui est mise à jour en réponse aux événements entrant du terminal. Dans notre exemple, l'interface logique définit la propriété d'état *temperature* dans la structure suivante :
```
{
"temperature" : <current temperature value in Celsius>
  }
```
La propriété *temperature* est utilisée dans l'expression de la condition de règle. 
 

### Effectuez les opérations suivantes pour configurer les deux règles :

1. Sélectionnez l'interface logique que vous souhaitez associer à votre règle. Dans ce scénario, nous utilisons l'interface logique avec l'identificateur *5846ed076522050001db0e12*. 
2. Affichez les règles en cours qui sont associées à l'interface logique à l'aide de l'API suivante :  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
Comme aucune règle n'est actuellement associée à l'interface logique *5846ed076522050001db0e12*, une liste vide est renvoyée.  
3. Ajoutez une règles appelée **tempRuleMax** à l'aide de l'API suivante :  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Incluez le nom de règle et un paramètre de condition dans le corps de la demande :   
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
L'exemple suivant montre la réponse à la méthode POST :  
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
La règle **tempRuleMax** est déclenchée si la propriété *temperature* de l'état du terminal définie sur l'interface logique *5846ed076522050001db0e12* dépasse la valeur de température spécifiée dans les métadonnées *tempThresholdMax* définies pour l'instance de terminal *tSensor*. Dans notre exemple, la valeur est de 44 degrés Celsius.  
4. Ajoutez une règle appelée **tempRuleMin** au moyen de l'API suivant :   
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Incluez le nom de règle et un paramètre de condition dans le corps de la demande :  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
L'exemple suivant montre la réponse à la méthode POST :  
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
La règle **tempRuleMin** est déclenché si la propriété *temperature* de l'état du terminal qui est définie sur l'interface logique *5846ed076522050001db0e12* tombe en-dessous de la valeur spécifiée dans les métadonnées *tempThresholdMin* définies pour l'instance de terminal *tSensor*. Dans notre exemple, la valeur est de 10 degrés Celsius.  
5. Validez et activez votre configuration à l'aide de l'opération **activate-configuration**. Vous devez activer votre configuration après avoir ajouté une règle car l'action d'ajouter la règle entraîne un changement au niveau du modèle provisoire. L'opération **activate-configuration** doit être réalisée sur la version provisoire du type de terminal.  
L'exemple ci-dessous illustre une demande PATCH où une opération **activate-configuration** est réalisée sur la version provisoire d'un type de terminal :  
```  
PATCH /draft/device/types/{typeId} 
```  
où le corps PATCH contient le contenu suivant :  
```  
{
  "operation": "activate-configuration"
}
```  
Pour en savoir plus sur l'activation et la désactivation de votre configuration, voir [Connaissance de la gestion des données](../GA_information_management/ga_im_definitions.html#draft_active_resources).  
6. Affichez les règles en cours sur l'interface logique active à l'aide de l'API suivante API :  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
L'exemple suivant illustre une réponse à la méthode GET :  
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

Pour en savoir plus sur l'utilisation de l'API, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window}.

## Génération d'une action
Lorsqu'une règle prend la valeur *true*, un message est publié dans la rubrique MQTT suivante :  
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
L'exemple suivant montre un exemple de corps de message :  
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
Vous pouvez ensuite configurer une action qui est initiée lorsqu'une règle est déclenchée. Par exemple, vous pouvez utiliser [Node-RED](../applications/dev_nodered.html) pour envoyer un message à un utilisateur spécifique lorsqu'une règle prend la valeur *true*. 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

Si une erreur est générée durant l'évaluation de la règle, un message d'erreur est publié sur la rubrique MQTT suivante :

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

L'exemple suivant montre un exemple de corps de message :

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
Le corps de message inclut des informations sur l'ID de l'interface logique, l'ID de règle, le terminal et l'état. Vous pouvez utiliser ces informations pour tenter de déboguer l'erreur.

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



