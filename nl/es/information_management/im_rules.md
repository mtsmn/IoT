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

# Creación de reglas incorporadas (Beta)
{: #im_rules}

Una vez que haya activado correctamente el duplicado de dispositivo o el duplicado de activo y enviado algunos datos, es hora de hacer que esos datos trabajen para usted utilizando reglas.

**Importante:** La característica de reglas incorporadas de {{site.data.keyword.iot_full}} para la gestión de datos solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Acerca de las reglas y acciones

Con {{site.data.keyword.iot_full}}, puede configurar reglas que desencadenan cuándo causa un suceso recibido por {{site.data.keyword.iot_short_notm}} un cambio en el estado del dispositivo.

Las reglas incorporadas son puntos de decisión basados en condiciones que coinciden con los datos de dispositivo en tiempo real con valores de umbral predefinidos u
otros datos de propiedad para desencadenar la regla si se cumple una condición.

Con reglas incluidas, especifica las condiciones que desencadenan una regla. A continuación, puede configurar una acción en respuesta al desencadenante, por ejemplo, enviando una alerta al dispositivo de un usuario y un correo electrónico a un administrador, cuando la temperatura de su dispositivo se repunte.


## Reglas de comprensión

Las reglas están asociadas con una [interfaz lógica](../GA_information_management/ga_im_definitions.html#resources) y se escriben en el [estado del dispositivo](../GA_information_management/ga_im_definitions.html#resources). Puede asociar una o varias reglas con una interfaz lógica. 

Las reglas se evalúan cuando un suceso de dispositivo que recibe {{site.data.keyword.iot_short_notm}} puede afectar al estado del dispositivo que está definido por una interfaz lógica. Esto se denomina "evaluación gestionada por sucesos".

Cada regla debe tener un parámetro **name** y una **condición** definido. El parámetro **condition** debe ajustarse a los criterios siguientes:  
 - El parámetro debe ser una expresión.  
 - El parámetro debe evaluarse en un valor booleano de **true** o **false**. Si la expresión de condición se evalúa como **true**, se publica un mensaje MQTT en un tema MQTT.  

## Comprensión del lenguaje de expresión

Configure la lógica de reglas utilizando el lenguaje de expresiones para especificar condiciones para cuando una regla se desencadena o no. Se desencadena una regla cada vez que se cumplen sus condiciones. 

El lenguaje de expresiones es un subconjunto de [JSONata ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/index.html){:new_window}. Para obtener más información sobre el uso del lenguaje de expresiones, consulte [Comprensión del lenguaje de expresión de correspondencia](../GA_information_management/mapping_expression_language.html). 

El lenguaje de expresión de correlación se ha ampliado para utilizarlo con la característica de reglas de gestión de datos a través de la introducción de las variables $state e $instance que están definidas para su uso en expresiones. El JSON se limita a estas variables antes de evaluar la expresión.

- La variable $state se utiliza para seleccionar una propiedad en el estado de dispositivo que se utilizará en una expresión, por ejemplo, *$state.temperature*.
- La variable $instance se utiliza para seleccionar un atributo de dispositivo que se debe utilizar en una expresión, por ejemplo, *$instance.metadata.tempThresholdMax*.

Es posible que desee utilizar estas variables para que no sea necesario codificar valores en las expresiones de condición de regla, y puede establecer un valor de umbral distinto para cada instancia de dispositivo. Las variables $state e $instance se utilizan en el ejemplo en la sección siguiente.  


## Configuración de reglas

En este caso de ejemplo se muestra cómo crear dos reglas y se basa en la configuración que se crea en la [Guía paso a paso 1](../GA_information_management/ga_im_index_scenario.html). 

Una regla se denomina **tempRuleMax**, que se desencadena cuando {{site.data.keyword.iot_short_notm}} recibe un suceso de temperatura que hace que la propiedad *temperature* del estado del dispositivo supere los 44 grados Celsius. La otra regla se denomina **tempRuleMin**, que se desencadena cuando {{site.data.keyword.iot_short_notm}} recibe un suceso de temperatura que hace que la propiedad *temperature* del estado del dispositivo caiga por debajo de los 10 grados Celsius. 

Para crear las reglas en este ejemplo, se necesita información acerca de los recursos siguientes que están configurados como parte de la Guía paso a paso 1: 

- La instancia de dispositivo *tSensor*.   

Este dispositivo publica los sucesos de temperatura que se miden en grados centígrados. Los metadatos siguientes están asociados a la instancia de dispositivo *tSensor*:
```
{
 tempThresholdMax: 44
 tempThresholdMin: 10
}
```
Las variables y los valores asociados que están definidos en los metadatos de instancia de dispositivo son necesarios para que la información se pueda utilizar en la expresión de condición de regla.  

**Sugerencia:** Los metadatos se pueden añadir o actualizar a una instancia de dispositivo después de que se haya creado a partir de la página *Información de dispositivo* del panel de control. Para obtener información sobre la utilización del panel de control, consulte [Cómo empezar con la Gestión de datos utilizando la interfaz web](../GA_information_management/im_ui_flow.html).
 
- La interfaz lógica con el identificador *5846ed076522050001db0e12*. 

El identificador es necesario para que las reglas se puedan asociar con el recurso de interfaz lógica correcto.
Se utiliza una interfaz lógica para definir la vista normalizada en el estado del dispositivo, que se actualiza en respuesta a sucesos de dispositivo de entrada. En nuestro ejemplo, la interfaz lógica define la propiedad de estado *temperature* en la estructura siguiente:
```
{
"temperature" : <valor temperatura actual en grados centígrados>
  }
```
La propiedad *temperature* se utiliza en la expresión de condición de regla. 
 

### Complete los pasos siguientes para configurar las dos reglas:

1. Seleccione la interfaz lógica que desea asociar a la regla. En este caso de ejemplo, estamos utilizando la interfaz lógica con el identificador *5846ed076522050001db0e12*. 
2. Vea las reglas actuales que están asociadas con la interfaz lógica utilizando la API siguiente:  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
Puesto que no hay reglas asociadas actualmente con la interfaz lógica *5846ed076522050001db0e12*, se devuelve una lista vacía.  
3. Añada una regla denominada **tempRuleMax** utilizando la API siguiente:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Incluya el nombre de regla y un parámetro de condición en el cuerpo de la solicitud:  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
En el ejemplo siguiente se muestra la respuesta al método POST:  
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
La regla **tempRuleMax** se desencadena si la propiedad *temperature* de estado de dispositivo que se ha definido en la interfaz lógica *5846ed076522050001db0e12* supera el valor de temperatura especificado en los metadatos *tempThresholdMax* que se ha definido para la instancia de dispositivo *tSensor*. En nuestro ejemplo, el valor es 44 grados Celsius.  
4. Añada una regla denominada **tempRuleMin** utilizando la API siguiente:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Incluya el nombre de regla y un parámetro de condición en el cuerpo de la solicitud:  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
En el ejemplo siguiente se muestra la respuesta al método POST:  
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
La regla **tempRuleMin** se desencadena si la propiedad *temperature* de estado de dispositivo que se ha definido en la interfaz lógica *5846ed076522050001db0e12* está por debajo del valor especificado en los metadatos *tempThresholdMin* que se define para la instancia de dispositivo *tSensor*. En nuestro ejemplo, el valor es 10 grados Celsius.  
5. Valide y active la configuración utilizando la operación **activate-configuration**. Debe activar la configuración después de añadir una regla, ya que la acción de añadir la regla da como resultado un cambio en el modelo de borrador. La operación **activate-configuration** se debe realizar en la versión de borrador del tipo de dispositivo.  
El siguiente ejemplo muestra una solicitud PATCH donde se realiza una operación **activate-configuration** en una versión borrador de un tipo de dispositivo:  
```  
PATCH /draft/device/types/{typeId} 
```  
donde la carga útil del cuerpo PATCH contiene el siguiente contenido:  
```  
{
  "operation": "activate-configuration"
}
```  
Para obtener más información sobre la activación y la desactivación de la configuración, consulte [Visión general de la gestión de datos](../GA_information_management/ga_im_definitions.html#draft_active_resources).  
6. Vea las reglas actuales en la interfaz lógica activa utilizando la API siguiente:  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
El ejemplo siguiente muestra una respuesta al método GET:  
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

Para obtener más detalles sobre el uso de la API, consulte la documentación [API REST HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window}.

## Generación de una acción
Cuando una regla se evalúa como *true*, se publica un mensaje en el siguiente tema MQTT:  
```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/evt/trigger
```  
En el ejemplo siguiente se muestra un cuerpo de mensaje de ejemplo:  
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
A continuación, puede configurar una acción que se inicia cuando se desencadena una regla. Por ejemplo, es posible que utilice [Node-RED](../applications/dev_nodered.html) para enviar un mensaje a un usuario especificado cuando una regla se evalúe en *true*. 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

Si se genera un error durante la evaluación de regla, se publica un mensaje de error en el siguiente tema de MQTT:

```
iot-2/intf/<logicalInterfaceId>/rule/<ruleId>/err/data
```

En el ejemplo siguiente se muestra un cuerpo del mensaje de ejemplo:

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
El cuerpo del mensaje incluye información sobre el ID de interfaz lógica, el ID de regla, el dispositivo y el estado. Puede utilizar la información para ayudar a depurar el error.

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



