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

# Criando regras integradas (Beta)
{: #im_rules}

Depois de ter ativado com êxito seu dispositivo gêmeo ou ativo gêmeo e enviado alguns dados, é hora de fazer com que os dados trabalhem por você usando regras.

**Importante:** o recurso de regras integradas do {{site.data.keyword.iot_full}} para gerenciamento de dados está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Sobre Regras e Ações

Com o {{site.data.keyword.iot_full}}, é possível configurar regras que são acionadas quando um evento recebido pelo {{site.data.keyword.iot_short_notm}} causa uma mudança no estado do dispositivo.

As regras integradas são pontos de decisão baseados em condição que correspondem aos dados do dispositivo de tempo real com valores de limite predefinidos ou
outros dados de propriedade para acionar a regra se uma condição for atendida.

Com regras integradas, você especifica as condições que acionam uma regra. Em seguida, é possível configurar uma ação em resposta ao acionador, por exemplo, enviar um alerta para o dispositivo de um usuário e um e-mail para um administrador, quando a temperatura de seu dispositivo aumenta.


## Entendendo Regras

As regras são associadas a uma [interface lógica](../GA_information_management/ga_im_definitions.html#resources) e são gravadas com relação ao [estado do dispositivo](../GA_information_management/ga_im_definitions.html#resources). É possível associar uma ou mais regras a uma interface lógica. 

As regras são avaliadas quando um evento de dispositivo recebido pelo {{site.data.keyword.iot_short_notm}} pode afetar o estado do dispositivo que é definido por uma interface lógica. Isso é chamado de "avaliação acionada por eventos".

Cada regra deve ter um parâmetro **name** e um **condition** definidos. O parâmetro **condition** deve se adequar aos critérios a seguir:  
 - O parâmetro deve ser uma expressão.  
 - O parâmetro deve ser avaliado para um valor booleano igual a **true** ou **false**. Se a expressão de condição for avaliada como **true**, uma mensagem do MQTT será publicada em um tópico do MQTT.  

## Entendendo o Idioma da Expressão

Configure sua lógica de regra usando a linguagem de expressão para especificar condições de quando uma regra é ou não é acionada. Uma regra é acionada toda vez que suas condições são atendidas. 

A linguagem de expressão é um subconjunto do [JSONata ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/index.html){:new_window}. Para obter mais informações sobre o uso da linguagem de expressão, veja [Entendendo a linguagem de expressão de mapeamento](../GA_information_management/mapping_expression_language.html). 

A linguagem de expressão de mapeamento foi estendida para uso com o recurso de regras de gerenciamento de dados por meio da introdução de variáveis $state e $instance que são definidas para uso em expressões. O
JSON é ligado a essas variáveis antes de a expressão ser avaliada.

- A variável $state é usada para selecionar uma propriedade no estado do dispositivo para usar em uma expressão, por exemplo, *$state.temperature*.
- A variável $instance é usada para selecionar um atributo de dispositivo para ser usado em uma expressão, por exemplo, *$instance.metadata.tempThresholdMax*.

Você pode desejar usar essas variáveis para que não precise codificar permanentemente os valores em suas expressões de condição de regra e possa configurar um valor de limite diferente para cada instância de dispositivo. As variáveis $state e $instance são usadas no exemplo na seção a seguir.  


## Configurando Regras

Este cenário mostra como criar duas regras e é baseado na configuração criada no [Guia passo a passo 1](../GA_information_management/ga_im_index_scenario.html). 

Uma regra é chamada **tempRuleMax**, que é acionada quando um evento de temperatura é recebido pelo {{site.data.keyword.iot_short_notm}} que faz com que a propriedade *temperature* do estado do dispositivo exceda 44 graus Celsius. A outra regra é chamada **tempRuleMin**, que é acionada quando um evento de temperatura é recebido pelo {{site.data.keyword.iot_short_notm}} que faz com que a propriedade *temperature* do estado do dispositivo caia abaixo de 10 graus Celsius. 

Para criar as regras neste exemplo, são necessárias informações sobre os recursos a seguir que estão configurados como parte do Guia passo a passo 1: 

- A instância do dispositivo  * tSensor * .   

Esse dispositivo publica eventos de temperatura que são medidos em graus Celsius. Os metadados a seguir estão associados à instância de dispositivo *tSensor*:
```
{
 tempThresholdMax: 44 tempThresholdMin: 10
}
```
As variáveis e os valores associados que estão definidos nos metadados da instância de dispositivo são necessários para que as informações possam ser usadas na expressão de condição da regra.  

**Dica:** os metadados podem ser incluídos ou atualizados em uma instância de dispositivo após ela ter sido criada na página *Informações sobre o dispositivo* do painel. Para obter informações sobre como usar o painel, veja [Introdução ao Data Management usando a interface da web](../GA_information_management/im_ui_flow.html).
 
- A interface lógica com o identificador *5846ed076522050001db0e12*. 

O identificador é necessário para que as regras possam ser associadas com o recurso de interface lógica correto.
Uma interface lógica é usada para definir a visualização normalizada para o estado do dispositivo, que é atualizado em resposta a eventos de dispositivo de entrada. Em nosso exemplo, a interface lógica define a propriedade de estado *temperature* na estrutura a seguir:
```
{
"temperature" : <current temperature value in Celsius>
  }
```
A propriedade *temperature* é usada na expressão de condição da regra. 
 

### Conclua as etapas a seguir para configurar as duas regras:

1. Selecione a interface lógica que você deseja associar à sua regra. Neste cenário, estamos usando a interface lógica com o identificador *5846ed076522050001db0e12*. 
2. Visualize as regras atuais que estão associadas à interface lógica usando a API a seguir:  
```
GET /draft/logicalinterfaces/5846ed076522050001db0e12/rules
```  
Como nenhuma regra está associada atualmente à interface lógica *5846ed076522050001db0e12*, uma lista vazia é retornada.  
3. Inclua uma regra chamada **tempRuleMax** usando a API a seguir:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Inclua o nome da regra e um parâmetro de condição no corpo da solicitação:  
```  
{  
    "name" : "tempRuleMax",
    "condition" : "$state.temperature > $instance.metadata.tempThresholdMax"
}  
```  
O exemplo a seguir mostra a resposta para o método POST:  
```  
{  
   "name": "tempRuleMax",  
   "id": "5a71991e59080100328710e9",  
   "logicalInterfaceId": "5846ed076522050001db0e12",  
   "condition": "$state.temperature > $instance.metadata.tempThresholdMax",  
   "refs": {  
         "logicalInterface": "/api/v0002/draft/logicalinterfaces/5846ed076522050001db0e12" }, "version": "draft", "created": "2018-01-31T10:23:26Z", "createdBy": "a-7p9t2v-zsrcacabpa", "updated": "2018-01-31T10:23:26Z", "updatedBy": "a-7p9t2v-zsrcacabpa" }  
```  
A regra **tempRuleMax** será acionada se a propriedade *temperature* do estado do dispositivo que está definida na interface lógica *5846ed076522050001db0e12* exceder o valor de temperatura especificado nos metadados *tempThresholdMax* definidos para a instância de dispositivo *tSensor*. Em nosso exemplo, o valor é 44 graus Celsius.  
4. Inclua uma regra chamada **tempRuleMin** usando a API a seguir:  
```  
POST /draft/logicalinterfaces/5846ed076522050001db0e12/rules  
```  
Inclua o nome da regra e um parâmetro de condição no corpo da solicitação:  
```  
{  
    "name" : "tempRuleMin",
    "condition" : "$state.temperature < $instance.metadata.tempThresholdMin"
}  
```  
O exemplo a seguir mostra a resposta para o método POST:  
```  
{  
   "name": "tempRuleMin",  
   "id": "5a71991e59080100328710e10",  
   "logicalInterfaceId": "5846ed076522050001db0e12",  
   "condition": "$state.temperature < $instance.metadata.tempThresholdMin",  
   "refs": {  
        "logicalInterface": "/api/v0002/draft/logicalinterfaces/5846ed076522050001db0e12" }, "version": "draft", "created": "2018-01-31T10:23:26Z", "createdBy": "a-7p9t2v-zsrcacabpa", "updated": "2018-01-31T10:23:26Z", "updatedBy": "a-7p9t2v-zsrcacabpa" }  
```  
A regra **tempRuleMin** será acionada se a propriedade *temperature* do estado do dispositivo que está definida na interface lógica *5846ed076522050001db0e12* cair abaixo do valor especificado nos metadados *tempThresholdMin* definidos para a instância de dispositivo *tSensor*. Em nosso exemplo, o valor é 10 graus Celsius.  
5. Valide e ative sua configuração usando a operação **activate-configuration**. Deve-se ativar sua configuração depois de incluir uma regra, porque a ação de incluir a regra resulta em uma mudança no modelo de rascunho. A operação **activate-configuration** deve ser executada na versão de rascunho do tipo de dispositivo.  
O exemplo a seguir mostra uma solicitação de PATCH em que uma operação **ativar-configuração** é executada em uma versão de rascunho de um tipo de dispositivo:  
```  
PATCH /draft/device/types/{typeId} 
```  
em que a carga útil do corpo PATCH possui o conteúdo a seguir:  
```  
{
  "operation" : "activate-configuration"
}
```  
Para obter mais informações sobre a ativação e a desativação da configuração, veja [Entendendo o gerenciamento de dados](../GA_information_management/ga_im_definitions.html#draft_active_resources).  
6. Visualize as regras atuais na interface lógica ativa usando a API a seguir:  
```  
GET /logicalinterfaces/5846ed076522050001db0e12/rules  
```  
O exemplo a seguir mostra uma resposta para o método GET:  
```  
   {
       "name": "tempRuleMax",  
       "id": "5a71991e59080100328710e9",
       "logicalInterfaceId": "5846ed076522050001db0e12",
       "condition": "$state.temperature > $instance.metadata.tempThresholdMax",
       "refs": {
           "logicalInterface": "/api/v0002/logicalinterfaces/5846ed076522050001db0e12"
       }, "version": "active", "created": "2018-01-31T10:23:26Z", "createdBy": "a-7p9t2v-zsrcacabpa", "updated": "2018-01-31T10:23:26Z", "updatedBy": "a-7p9t2v-zsrcacabpa"
   }   
   {
       "name": "tempRuleMin",  
       "id": "5a71561e59055500328710e10",
       "logicalInterfaceId": "5846ed076522050001db0e12",
       "condition": "$state.temperature < $instance.metadata.tempThresholdMin",
       "refs": {
           "logicalInterface": "/api/v0002/logicalinterfaces/5846ed076522050001db0e12"
       }, "version": "active", "created": "2018-01-31T10:23:26Z", "createdBy": "a-7p9t2v-zsrcacabpa", "updated": "2018-01-31T10:23:26Z", "updatedBy": "a-7p9t2v-zsrcacabpa"
   }   
```  

Para obter mais detalhes sobre o uso da API, veja a documentação [{{site.data.keyword.iot_short_notm}}API de REST HTTP ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window}.

## Gerando uma Ação
Quando uma regra é avaliada como *true*, uma mensagem é publicada no tópico do MQTT a seguir:  
```
iot-2/intf/ < logicalInterfaceId> /rule/ < ruleId> /evt/trigger
```  
O exemplo a seguir mostra um corpo da mensagem de amostra:  
```
{
   "type" : "device",
   "typeId" : "TSensor",
   "instanceId" : "tsensor",
   "logicalInterfaceId": "5846ed076522050001db0e12",
   "ruleId" : "5a71991e59080100328710e9",
   "state" : {
       "state": {
           "temperatura": 45
       },
       "timestamp": "2018-01-31T10:29:28Z",
       "updated": "2018-01-31T10:29:10Z"
   }
}
```
É possível, então, configurar uma ação que é iniciada quando uma regra é acionada. Por exemplo, você poderá usar [Node-RED](../applications/dev_nodered.html) para enviar uma mensagem para um usuário especificado quando uma regra for avaliada como *true*. 



<!-- In our example from [Step-by-step guide 1](../information_management/im_index_scenario_device.html#step11) step 11, the notification setting is set to **on-state-change** in the mappings resource, and rule notification is active. The
following table shows the notifications that are sent when temperature events are received by {{site.data.keyword.iot_short_notm}}.
Temperature in the received event | State change notification sent | Rule notification sent
10 degrees Celsius | Yes | No
45 degrees Celsius | Yes | Yes
45 degrees Celsius | No | Yes
The first received event changes the state but does not exceed the value of the temperature threshold variable, so no rule notification is sent but a state change notification is sent. The second received event changes the state and exceeds the threshold value, so a rule and state change notification is sent. The third received event does not change the state but does exceed the threshold value, so only a rule notification is sent. -->

Se um erro for gerado durante a avaliação de regra, uma mensagem de erro será publicada no tópico do MQTT a seguir:

```
iot-2/intf/ < logicalInterfaceId> /rule/ < ruleId> /err/data
```

O exemplo a seguir mostra um corpo da mensagem de exemplo:

```
{
 "message": "The result of expression evaluation is not of type boolean.",
 "logicalInterfaceId": "5846ed076522050001db0e12",
 "ruleId": "5i8t306d59087777329f1c56",
 "type": "device",
 "typeId": "TSensor",
 "instanceId": "tSensor",
 "state": {
   "temperatura": "cadeia"
 },
 "timestamp": "2018-01-31T10:38:59Z",
 "updated": "2018-01-31T10:38:59Z"
}
```
O corpo da mensagem inclui informações sobre o ID da interface lógica, ID de regra, dispositivo e estado. É possível usar as informações para ajudar a depurar o erro.

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



