---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Informações adicionais para o Guia passo a passo 2 - configurando uma interface lógica para um dispositivo de umidade

Use as informações a seguir para criar um cenário no qual os sensores em dois dispositivos de umidade publicam eventos no IBM Watson™ IoT Platform. As leituras dos dispositivos de umidade são mapeadas para uma única leitura de umidade. Um dispositivo está localizado na sala de reunião 1 e o outro dispositivo está localizado na sala de reunião 2.


## Sobre essa Tarefa

Use a mesma instância de organização do {{site.data.keyword.iot_short_notm}} e uma chave de API ou token para essa organização que você usou no [Guia passo a passo 1](../GA_information_management/ga_im_index_scenario.html). 

Essa configuração é um pré-requisito para o tutorial descrito na documentação [Guia passo a passo 2](im_index_scenario_thing.html).

Neste cenário, um tipo de dispositivo e duas instâncias de dispositivo são criados.

As instâncias de dispositivo são chamadas *humiditySensor1* e *humiditySensor2*. Os dispositivos publicam eventos de umidade. O evento de umidade tem a carga útil de exemplo a seguir:
```
{
  "hum": 35
}
```
Os eventos de umidade são publicados no tópico `iot-2/evt/humevt/fmt/fmt/json`. 

** Nota: **  O identificador de evento é  * humevt *. Esse identificador é necessário quando você inclui um evento de umidade desses tipos na interface física e ao definir mapeamentos para mapear uma propriedade associada a um evento de entrada desse tipo para uma propriedade em sua interface lógica. Neste cenário, a propriedade definida na interface lógica é chamada **humidity**. Essa interface lógica representa o estado dos dispositivos desse tipo na estrutura a seguir:
```
{
  "humidity" : <current humidity value in Celsius>
}
```

Use o cenário de exemplo a seguir para configurar seu próprio ambiente de interfaces.

**Nota importante:** deve-se salvar os IDs gerados nas respostas curl, pois os IDs são necessários para concluir outras tarefas.

## Se necessário, inclua um Tipo de dispositivo e um Dispositivo
{: #step14}

Neste cenário, um tipo de dispositivo e duas instâncias de dispositivo são assumidos. As instâncias de dispositivo *humiditySensor1* e *humiditySensor2* estão associadas ao tipo de dispositivo *HumiditySensor*. 

É possível criar tipos de dispositivo e dispositivos usando o [painel do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://internetofthings.ibmcloud.com){: new_window} ou usando APIs de REST. Para obter mais informações sobre como usar o painel do {{site.data.keyword.iot_short_notm}} IoT Platform para incluir tipos de dispositivo e dispositivos, consulte a documentação de [Introdução ao gerenciamento de dados usando a interface da web](../GA_information_management/im_ui_flow.html).

O exemplo a seguir mostra como criar um tipo de dispositivo chamado *HumiditySensor* usando a API de REST:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**Nota importante:** é possível incluir metadados quando você cria um dispositivo e um tipo de dispositivo. Neste cenário, os metadados a seguir são incluídos no tipo de dispositivo *HumiditySensor*:
```
{
    "maxHumidityThreshold": 70
}
```
Esses metadados podem ser usados ao [criar regras](im_rules.html) que são acionadas se um evento de umidade que faz com que a propriedade *humidity* do estado do dispositivo exceda um valor de 70% é recebido pelo {{site.data.keyword.iot_short_notm}} de *humiditySensor1* ou *humiditySensor2*. 

## Registre os dispositivos

O exemplo a seguir mostra como registrar uma instância de dispositivo chamada *humiditySensor1*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
O exemplo a seguir mostra como registrar uma instância de dispositivo chamada *humiditySensor2*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**Nota:** quando um dispositivo faz uma solicitação de HTTP por meio da API de REST HTTP do Watson IoT Platform, um nome de usuário e uma senha são necessários. A senha é o valor do token de autenticação que é gerado automaticamente ou especificado manualmente quando um dispositivo é registrado. Se você está usando um cliente MQTT, deve-se manter uma nota do token de autenticação de seu dispositivo, já que precisa do token para recuperar o estado do dispositivo ou da Coisa assinando uma sequência de tópicos do IoT.

## Etapa 1: Criar um arquivo de esquema de evento
{: #step1}

Para este cenário, crie um arquivo de esquema de evento para definir a estrutura dos eventos de umidade de entrada.

O exemplo a seguir mostra como criar um arquivo de esquema chamado *humEventSchema.json*. Esse arquivo define a estrutura de um evento de entrada de um dispositivo de umidade:

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "defines the structure of a humidity event",
    "properties" : {
        "umidade": {
            "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
      "humidade"
    ]
}
```
**Dica:** use o parâmetro **required** para marcar uma ou mais propriedades, conforme necessário. As
propriedades necessárias devem ser incluídas em uma mensagem do dispositivo para que o
{{site.data.keyword.iot_short_notm}} atualize o estado do dispositivo com
os dados do dispositivo. Uma mensagem que não inclui a propriedade necessária não é processada.   


## Etapa 2: Criar um recurso de esquema de evento para seu tipo de evento
{: #step2}

Para criar um recurso de esquema de evento, use a API a seguir:

```
POST /draft/schemas
```

O arquivo de definição de esquema é transmitido para o Watson IoT Platform dentro de um POST de várias partes (dados de formulário/partes múltiplas). O corpo do POST deve conter pelo menos duas partes:

- Um com um nome de **schemaFile** que contém o conteúdo real do arquivo como o corpo da parte.
- Um com um nome de **name** que contém uma sequência que define o nome do recurso de esquema no corpo da parte.

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

O exemplo a seguir mostra como usar o cURL para criar o recurso de esquema de evento *humEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**Nota:** o valor de autorização de exemplo `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` consiste nas informações a seguir:
`{API Key}:{authorization token}`, que é então codificado em Base64.

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "name" : "humEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "humEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e20",
  "refs" : {
      "content": "/api/v0002/draft/schemas/5846cd7c6522050001db0e20/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de esquema *5846cd7c6522050001db0e20* retornado em resposta ao método POST é necessário quando você inclui um esquema de evento em seu tipo de evento.



## Etapa 3: Criar um tipo de evento que referencie o esquema de evento
{: #step3}

Cada tipo de evento referencia o esquema de evento relevante que foi criado no exemplo anterior usando o identificador de esquema retornado na resposta para o método POST usado para criar o recurso de esquema de evento.

Para criar um tipo de evento, use a API a seguir:

```
POST /draft/event/types
```
O tipo de evento de rascunho deve referenciar a definição de esquema que define a estrutura do evento MQTT de entrada.


Para obter mais detalhes, consulte a documentação da
[API
de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


O exemplo a seguir mostra como usar o cURL para criar um tipo de evento para um evento de umidade:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

O identificador de esquema *5846cd7c6522050001db0e20* é usado para incluir o esquema de evento no tipo de evento. Esse identificador foi retornado em resposta ao método POST que foi usado para criar o recurso de esquema de evento *humEventSchema.json*

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e20",
  "refs" : {
    "schema": "/api/v0002/draft/draft/schemas/5846cd7c6522050001db0e20"
  },
  "name" : "humEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e21",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

O identificador de tipo de evento *5846cd7c6522050001db0e21* que é retornado em resposta ao método POST é usado para incluir um tipo de evento na interface física.


## Etapa 4: Criar uma interface física
{: #step7}

Para criar uma interface física, use a API a seguir:

```
POST /draft/physicalinterfaces
```

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Neste cenário, precisamos de uma interface física para o tipo de evento que criamos anteriormente.

O exemplo a seguir mostra como usar o cURL para criar a interface física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events": "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  }, "id" : "5846cd7c6522050001db0e22", "name" : "Hygrometer Physical Interface", "version" : "draft", "created" : "2016-12-07T09:09:51Z", "updated" : "2016-12-07T09:09:51Z", "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

O identificador de interface física *5846cd7c6522050001db0e22* retornado na resposta é usado na URL do método POST chamado para incluir um evento de temperatura medido em graus Celsius para a interface física.


## Etapa 5: Incluir o tipo de evento na interface física
{: #step8}

Para incluir um tipo de evento na interface física, use a API a seguir:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Neste cenário, o tipo de evento a seguir é incluído nas interfaces físicas especificadas:
- O evento de umidade *humevt* é incluído na interface física com o identificador *5846cd7c6522050001db0e22* usando o *eventId* do tópico e o *eventTypeId* da criação do recurso de esquema de evento.

O exemplo a seguir mostra como usar o cURL para incluir o evento de umidade *humevt* na interface física com o identificador *5846cd7c6522050001db0e22*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## Etapa 6: Atualizar o tipo de dispositivo para conectar a interface física
{: #step9}

Para atualizar um tipo de dispositivo, use a API a seguir:

```
POST /draft/device/types/{typeId}/physicalinterface
```

em que *typeId* é o identificador de tipo de dispositivo.


Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Neste cenário, o tipo de dispositivo *HumiditySensor* é atualizado para se conectar à interface física *5846cd7c6522050001db0e22*.

O exemplo a seguir mostra como usar o cURL para atualizar o tipo de dispositivo *HumiditySensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events": "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  }, "id" : "5846cd7c6522050001db0e22", "name" : "Hygrometer Physical Interface", "version" : "draft", "created" : "2016-12-07T09:09:51Z", "updated" : "2016-12-07T09:09:51Z", "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de dispositivo *HumiditySensor* é necessário quando você inclui sua interface física e sua interface lógica.
  


## Etapa 7: Criar um arquivo de esquema da interface lógica
{: #step4}

O exemplo a seguir mostra como criar um arquivo de esquema de interface lógica chamado *hygrometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema that defines the canonical interface for a hygrometer",
    "properties" : {
        "umidade": {
            "description" : "percentage humidity",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## Etapa 8: Criar um recurso de esquema da interface lógica
{: #step5}
O exemplo a seguir mostra como usar cURL para criar uma interface lógica:

```
curl --request POST \ --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \ --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \ --header 'content-type: application/json' \ --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema": "/api/v0002/draft/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometer",
  "version" : "draft"
}
```

## Etapa 9: Criar uma interface lógica que referencia um esquema de interface lógica
{: #step6}

Para criar uma interface lógica, use a API a seguir:

```
POST /draft/logicalinterfaces
```

Opcionalmente, é possível especificar um nome de alias significativo para sua interface lógica. O alias pode ser referenciado na chamada API ou na assinatura da sequência de tópicos usada para recuperar o estado de um dispositivo, em vez de usar o identificador de interface lógica gerado automaticamente.

**Nota:** o nome do alias deve ter de 1 a 36 caracteres de comprimento e pode incluir caracteres alfanuméricos, de hífen, de ponto e sublinhado. O nome do alias não pode ser uma sequência hexadecimal de 24 caracteres. 

Para obter mais detalhes, consulte a documentação da
[API
de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

Neste cenário, use o identificador de esquema *5846cd7c6522050001db0e23* que foi retornado na resposta anterior para incluir o esquema de interface lógica na interface lógica.

O exemplo a seguir mostra como usar cURL para criar uma interface lógica:

```
curl --request POST \ --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \ --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \ --header 'content-type: application/json' \ --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema": "/api/v0002/draft/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometermometer",
  "version" : "draft"
}
```
Neste cenário, use o identificador de interface lógica *5846cd7c6522050001db0e24* que é retornado na resposta para o método POST para incluir sua interface lógica em seu tipo de dispositivo. É possível também usar esse
identificador para mapear um evento de dispositivo de entrada para uma propriedade que é
definida pela interface lógica. É possível usar o alias da interface lógica *IHygrometer* para recuperar o estado do dispositivo, usando APIs de REST HTTP ou assinando uma sequência de tópicos.


## Etapa 10: incluir a interface lógica no tipo de dispositivo

O exemplo a seguir mostra como usar o cURL para incluir a interface lógica 5846cd7c6522050001db0e24 que referencia o identificador de esquema de interface lógica 5846cd7c6522050001db0e23 para o tipo de dispositivo HumiditySensor:
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "refs" : {
      "schema": "/api/v0002/draft/draft/schemas/5846cd7c6522050001db0e23"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Hygrometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846cd7c6522050001db0e24",
  "schemaId" : "5846cd7c6522050001db0e23"
}
```

## Etapa 11: incluir os mapeamentos no tipo de dispositivo

    
O exemplo a seguir mostra como usar o cURL para incluir um mapeamento no tipo de dispositivo *HumiditySensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "logicalInterfaceId": "5846cd7c6522050001db0e24", "propertyMappings": {
    "humevt": {
      "humidity": "$event.hum"
    }
  }, "notificationStrategy": "on-state-change", "version": "draft", "created": "2017-06-16T15:41:49Z", "createdBy": "a-8x7nmj-9iqt56kfil", "updated": "2017-06-16T15:41:49Z", "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## Etapa 12: ativar a configuração para o tipo de dispositivo


    
O exemplo a seguir mostra como usar o cURL para validar e ativar a sua configuração para o tipo de dispositivo *HumiditySensor*:
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
O exemplo a seguir mostra uma resposta para o método PATCH:
```
{
 "message": "CUDRS0520I: State update configuration for device type 'HumiditySensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["HumiditySensor"]
  },
 "failures": []
}
```
