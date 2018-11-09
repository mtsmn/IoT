---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Guia passo a passo 1: um exemplo detalhado sobre como trabalhar com dispositivos por meio de uma interface comum
{: #scenario}

Use as informações a seguir para criar um cenário no qual dois dispositivos de temperatura publicam eventos no {{site.data.keyword.iot_full}}. Um dispositivo mede a temperatura em graus Celsius. O outro dispositivo mede a temperatura em graus Fahrenheit. Essas leituras são mapeadas para uma única leitura de temperatura que está em graus Celsius. Quando uma nova leitura de temperatura é publicada por esses dispositivos, o valor da propriedade associada ao estado do dispositivo muda.
{: shortdesc}

## Antes de iniciar

Ao criar um [recurso](ga_im_definitions.html#definitions_resources), ele é criado como versão de rascunho. A versão de rascunho é uma cópia funcional de seu recurso que é possível consultar, atualizar e excluir diretamente usando APIs. Ao usar as APIs de REST, o prefixo **draft/** é usado para identificar recursos que estão em um estado de rascunho. 

Para obter mais informações sobre versões de rascunho e ativas de recursos, veja [Entendendo o gerenciamento de dados](ga_im_definitions.html).

## Pré-requisitos

Deve-se ter uma [instância de organização](../iotplatform_overview.html#organizations) do {{site.data.keyword.iot_short_notm}} e uma chave API e token de autenticação para essa organização para autenticar solicitações. 

Para obter informações sobre a geração de uma chave API, veja a documentação da [API](../reference/api.html). Para obter mais informações sobre a geração de um token da API, veja a documentação [Tutorial de introdução](../getting-started.html).


## Sobre essa Tarefa

Neste cenário, dois dispositivos são criados.

Um dispositivo é chamado  * tSensor *. Esse dispositivo publica eventos de temperatura que são medidos em graus Celsius. O evento de temperatura é publicado no tópico `iot-2/evt/tevt/fmt/json` e possui a carga útil de exemplo a seguir:
```
{
  "t" : 34.5
}
```

**Nota:** o identificador de evento é *tevt*. Este identificador é necessário ao incluir um evento de temperatura deste tipo na interface física e ao definir mapeamentos para mapear uma propriedade associada a um evento de entrada deste tipo para uma propriedade em sua interface lógica. Neste cenário, a propriedade definida na interface lógica é chamada **temperatura**.

O outro dispositivo é chamado  * tempSensor *. Esse dispositivo publica eventos de temperatura que são medidos em graus Fahrenheit. O evento de temperatura é publicado no tópico `iot-2/evt/tempevt/fmt/json` e possui a carga útil de exemplo a seguir:
```
{
  "temp" : 72.55
}
```

**Nota:** o identificador de evento é *tempevt*. Este identificador é necessário ao incluir um evento de temperatura deste tipo na interface física e ao definir mapeamentos para mapear uma propriedade associada a um evento de entrada deste tipo para uma propriedade em sua interface lógica. Neste cenário, a propriedade definida na interface lógica é chamada **temperatura**.

Uma interface lógica também é configurada. Essa interface lógica representa o estado dos dispositivos desse tipo na estrutura a seguir:
```
{
  "temperature" : <current temperature value in Celsius>
  }
```
Essa configuração significa que é possível configurar seu aplicativo para processar o valor que está associado a **temperatura**, em vez de configurar o aplicativo para processar o valor que está associado a **t** e processar o valor que está associado a **temp** depois de converter esse valor em graus Celsius.

![Mapeando entre os dispositivos de sensor de temperatura e um aplicativo em {{site.data.keyword.iot_short_notm}}.](../information_management/images/Information)  

Use o cenário de exemplo a seguir para configurar seu próprio ambiente de interfaces.

**Nota importante:** deve-se salvar os IDs gerados nas respostas curl, pois os IDs são necessários para concluir outras tarefas.
Uma tabela que lista os nomes, valores e identificadores da propriedade do recurso que são usados neste guia está documentada em [Informações adicionais para os guias passo a passo 1 e 2 - nomes e identificadores de recurso](../information_management/im_id_reference.html).

## Se necessário, inclua um Tipo de dispositivo e um Dispositivo
{: #step14}

Neste cenário, são assumidos dois tipos de dispositivo e duas instâncias de dispositivo. A instância de dispositivo *tSensor* está associada ao tipo de dispositivo *TSensor*. A instância de dispositivo *tempSensor* está associada ao tipo de dispositivo *TempSensor*. 

É possível criar tipos de dispositivo e dispositivos usando o [painel do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://internetofthings.ibmcloud.com){: new_window} ou usando APIs de REST. Para obter mais informações sobre como usar o painel do {{site.data.keyword.iot_short_notm}} para incluir dispositivos e tipos de dispositivo, veja a documentação [Introdução ao gerenciamento de dados usando a interface da web](im_ui_flow.html).

O exemplo a seguir mostra como criar um tipo de dispositivo chamado *TSensor* usando a API de REST:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TSensor", "description" : "The Celsius sensor device type", "metadata": {"tempThresholdMax": 44,
    "tempThresholdMin": 10}}' \
 ```
 
 O exemplo a seguir mostra como criar um tipo de dispositivo chamado *TempSensor* usando a API de REST:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TempSensor", "description" : "The Fahrenheit sensor device type"}' \
 ```

**Nota:** é possível incluir metadados quando você cria um dispositivo e um tipo de dispositivo. Neste cenário, os metadados a seguir são incluídos no tipo de dispositivo *TSensor*:
```
{
    "tempThresholdMax": 44,
    "tempThresholdMin": 10 
}
```
Esses metadados são usados ao criar [regras](../information_management/im_rules.html) que são acionadas quando um evento de temperatura que faz com que a propriedade *temperature* do estado do dispositivo exceda 44 graus Celsius é recebido pelo {{site.data.keyword.iot_short_notm}} do dispositivo *tSensor*. 


Em seguida, é necessário registrar uma instância de dispositivo que está associada a um tipo de dispositivo. O exemplo a seguir mostra como registrar uma instância de dispositivo chamada *tSensor* que está associada ao tipo de dispositivo *TSensor* usando a API de REST:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tSensor", "authToken": "password"}' \
```

O exemplo a seguir mostra como registrar uma instância de dispositivo chamada *tempSensor* que está associada ao tipo de dispositivo *TempSensor* usando a API de REST:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tempSensor", "authToken": "password"}' \
```

Para obter informações sobre como usar APIs de REST para incluir dispositivos e tipos de dispositivo, veja a documentação [{{site.data.keyword.iot_short_notm}}API de REST HTTP ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

**Nota:** quando um dispositivo faz uma solicitação de HTTP por meio da API de REST HTTP do Watson IoT Platform, um nome de usuário e uma senha são necessários. A senha é o valor do token de autenticação que é gerado automaticamente ou especificado manualmente quando um dispositivo é registrado. Se você está usando um cliente MQTT, deve-se manter uma nota do token de autenticação de seu dispositivo, já que você precisa do token para recuperar o estado do dispositivo ou coisa assinando uma sequência de tópicos.

## Etapa 1: Criar um arquivo de esquema de evento
{: #step1}

Para este cenário, crie dois arquivos de esquema de evento para definir a estrutura de cada um dos eventos de temperatura de entrada.

O exemplo a seguir mostra como criar um arquivo de esquema chamado *tEventSchema.json*. Esse arquivo define a estrutura de um evento de entrada por meio de um dispositivo de temperatura que mede a temperatura em graus Celsius:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tEventSchema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
      "description" : "temperature in degrees Celsius",
      "type" : "number",
      "minimum" : -273.15,
      "default" : 0.0
    }
  },
  "required" : ["t"]
}
  ```
**Dica:** use o parâmetro **required** para marcar uma ou mais propriedades, conforme necessário. As
propriedades necessárias devem ser incluídas em uma mensagem do dispositivo para que o
{{site.data.keyword.iot_short_notm}} atualize o estado do dispositivo com
os dados do dispositivo. Uma mensagem que não inclui a propriedade necessária não é processada.   

O nome do arquivo de esquema *tEventSchema* é usado quando você cria um recurso de esquema de evento para seu tipo de evento.

O exemplo a seguir mostra como criar um arquivo de esquema chamado *tempEventSchema.json*. Esse arquivo define a estrutura de um evento de entrada por meio de um dispositivo de temperatura que mede a temperatura em graus Fahrenheit:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tempEventSchema",
  "description" : "defines the structure of a temperature event in degrees Fahrenheit",
  "properties" : {
    "temp" : {
      "description" : "temperature in degrees Fahrenheit",
      "type" : "number",
      "minimum" : -459.67,
      "default" : 0.0
    }
  },
  "required" : ["temp"]
}
  ```
O nome do arquivo de esquema *tempEventSchema* é usado quando você cria um recurso de esquema de evento para seu tipo de evento.   

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

O exemplo a seguir mostra como usar cURL para criar o recurso de esquema de evento *tEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

**Dica:** o valor de autorização de exemplo `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` consiste nas informações a seguir:
`{API Key}:{authorization token}`, que é então codificado em Base64.

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "name" : "tEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "tEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e0d",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de esquema *5846cd7c6522050001db0e0d* retornado em resposta ao método POST é necessário quando você inclui um esquema de evento em seu tipo de evento.

O exemplo a seguir mostra como usar cURL para criar o recurso de esquema de evento *tempEventSchema.json*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "schemaType" : "json-schema",
  "schemaFileName" : "tempEventSchema.json",
  "updated" : "2016-12-06T14:44:51Z",
  "name" : "tempEventSchema",
  "version" : "draft",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "created" : "2016-12-06T14:44:51Z",
  "id" : "5846cee36522050001db0e0e",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e/content"
  },
  "contentType" : "application/octet-stream",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de esquema *5846cee36522050001db0e0e* retornado em resposta ao método POST é necessário quando você inclui um esquema de evento em seu tipo de evento.

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


O exemplo a seguir mostra como usar cURL para criar um tipo de evento para um evento de temperatura que é medido em graus Celsius:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

O identificador de esquema *5846cd7c6522050001db0e0d* é usado para incluir o esquema de evento no tipo de evento. Esse identificador foi retornado em resposta ao método POST usado para criar o recurso de esquema de evento *tEventSchema.json*

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e0d",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d"
  },
  "name" : "tEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846d0fd6522050001db0e0f",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

O identificador de tipo de evento *5846d0fd6522050001db0e0f* retornado em resposta ao método POST é usado para incluir um tipo de evento na interface física.

O exemplo a seguir mostra como usar cURL para criar um tipo de evento para um evento de temperatura que é medido em graus Fahrenheit:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
O identificador de esquema *5846cee36522050001db0e0e* é usado para incluir o esquema de evento no tipo de evento. Esse identificador foi retornado em resposta ao método POST usado para criar o recurso de esquema de evento *tempEventSchema.json*

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "schemaId" : "5846cee36522050001db0e0e",
  "created" : "2016-12-06T15:00:20Z",
  "id" : "5846d2846522050001db0e10",
  "updated" : "2016-12-06T15:00:20Z",
  "name" : "tempEvent",
  "version" : "draft",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e"
  },
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de tipo de evento *5846d2846522050001db0e10* retornado em resposta ao método POST é usado para incluir um tipo de evento na interface física.

## Etapa 4: Criar uma interface física
{: #step7}

Para criar uma interface física, use a API a seguir:

```
POST /draft/physicalinterfaces
```

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Neste cenário, precisamos de duas interfaces físicas, uma para cada tipo de evento.

O exemplo a seguir mostra como usar cURL para criar a primeira interface física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TSensor Physical Interface"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

O identificador de interface física *5847d1df6522050001db0e1a* retornado na resposta é usado na URL do método POST que é chamado para incluir um evento de temperatura que é medido em graus Celsius para a interface física.

O exemplo a seguir mostra como usar cURL para criar a segunda interface física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TempSensor Physical Interface"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

O identificador de interface física *5847d1df6522050001db0e1b* retornado na resposta é usado na URL do método POST que é chamado para incluir um evento de temperatura que é medido em graus Fahrenheit para a interface física.   

## Etapa 5: Incluir o tipo de evento na interface física
{: #step8}

Para incluir um tipo de evento na interface física, use a API a seguir:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Neste cenário, os tipos de eventos a seguir são incluídos nas interfaces físicas especificadas:
- o evento de temperatura Celsius *tevt* é incluído na interface física com o identificador *5847d1df6522050001db0e1a* usando o *eventId* do tópico e o *eventTypeId* da criação do recurso de esquema de evento.
- o evento de temperatura Fahrenheit *tempevt* é incluído na interface física com o identificador *5847d1df6522050001db0e1b* usando o *eventId* do tópico e o *eventTypeId* da criação do recurso de esquema de evento.


O exemplo a seguir mostra como usar cURL para incluir o evento de temperatura *tevt* na interface física com o identificador *5847d1df6522050001db0e1a*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

O exemplo a seguir mostra como usar cURL para incluir o evento de temperatura *tempevt* na interface física com o identificador *5847d1df6522050001db0e1b*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
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

Neste cenário, o tipo de dispositivo *TSensor* é atualizado para se conectar à interface física *5847d1df6522050001db0e1b* e o tipo de dispositivo *TempSensor * é atualizado para se conectar à interface física *5847d1df6522050001db0e1a*.


O exemplo a seguir mostra como usar o cURL para atualizar o tipo de dispositivo *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de dispositivo *TSensor* é necessário quando você inclui sua interface física e sua interface lógica.

O exemplo a seguir mostra como usar o cURL para atualizar o tipo de dispositivo *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de dispositivo *TempSensor* é necessário quando você inclui sua interface física e sua interface lógica.


## Etapa 7: Criar um arquivo de esquema da interface lógica
{: #step4}

O exemplo a seguir mostra como criar um arquivo de esquema de interface lógica chamado *thermometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "thermometerSchema",
    "description" : "Schema that defines the canonical interface for a thermometer",
    "properties" : {
        "temperature" : {
            "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## Etapa 8: Criar um recurso de esquema da interface lógica
{: #step5}

Para criar um recurso de esquema de interface lógica, use a API a seguir:

```
POST /draft/schemas
```

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

O exemplo a seguir mostra como usar cURL para criar o esquema de interface lógica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=thermometerSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/thermometer.json"'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "thermometerSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "thermometer.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
Use o identificador de esquema *5846ec826522050001db0e11* que é retornado na
resposta ao método POST para incluir o esquema de interface lógica na interface lógica.

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

Neste cenário, use o identificador de esquema
*5846ec826522050001db0e11* que foi retornado na resposta anterior para
incluir o esquema de interface lógica na interface lógica.

O exemplo a seguir mostra como usar cURL para criar uma interface lógica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Thermometer Interface", "alias" : "IThermometer", "schemaId" : "5846ec826522050001db0e11"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "schemaId" : "5846ec826522050001db0e11",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846ed076522050001db0e12",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Thermometer Interface",
  "alias" : "IThermometer",
  "version" : "draft"
}
```
Neste cenário, use o identificador de interface lógica
*5846ed076522050001db0e12* que é retornado na resposta ao método POST para
incluir sua interface lógica no seu tipo de dispositivo. É possível também usar esse
identificador para mapear um evento de dispositivo de entrada para uma propriedade que é
definida pela interface lógica. É possível usar o alias da interface lógica *IThermometer* para [recuperar o estado do dispositivo](##step13), usando APIs de REST HTTP ou assinando uma sequência de tópicos.

## Etapa 10: incluir a interface lógica em um tipo de dispositivo
{: #step10}

Para incluir uma interface lógica em um tipo de dispositivo, use a API a seguir:

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

em que *typeId* é o nome do tipo de dispositivo. 

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).  
**Nota:** neste cenário, a mesma interface lógica *5846ed076522050001db0e12* está associada ao *TSensor* e *TempSensor*.

O exemplo a seguir mostra como usar o cURL para incluir a interface lógica *5846ed076522050001db0e12* associada ao identificador de esquema lógico *5846ec826522050001db0e11* para o tipo de dispositivo *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  }, "updated" : "2016-12-06T16:53:27Z", "updatedBy" : "a-8x7nmj-9iqt56kfil", "createdBy" : "a-8x7nmj-9iqt56kfil", "name" : "Thermometer Interface", "version" : "draft", "created" : "2016-12-06T16:53:27Z", "id" : "5846ed076522050001db0e12", "schemaId" : "5846ec826522050001db0e11"
}
```

O exemplo a seguir mostra como usar o cURL para incluir a interface lógica *5846ed076522050001db0e12* que referencia o identificador de esquema de interface lógica *5846ec826522050001db0e11* para o tipo de dispositivo *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  }, "updated" : "2016-12-06T16:53:27Z", "updatedBy" : "a-8x7nmj-9iqt56kfil", "createdBy" : "a-8x7nmj-9iqt56kfil", "name" : "Thermometer Interface", "version" : "draft", "created" : "2016-12-06T16:53:27Z", "id" : "5846ed076522050001db0e12", "schemaId" : "5846ec826522050001db0e11"
}
```

## Etapa 11: Definir mapeamentos para mapear propriedades no evento de entrada para propriedades na interface lógica
{: #step11}  
**Importante:** Se você estiver usando um aplicativo cliente MQTT e
desejar assinar o recebimento de notificações sobre atualizações de estado do
dispositivo, será necessário configurar definições de notificação quando você definir os
mapeamentos. Ao configurar sua estratégia de notificação dessa maneira, será possível
reutilizar uma interface lógica entre vários tipos de dispositivo, com cada tipo de
dispositivo configurado para receber notificações de mudança de estado da maneira
escolhida. A estratégia de notificação pode ser definida independentemente de se um
cliente MQTT assina a sequência de tópicos especificada.  
  
As seguintes definições de notificação podem ser configuradas:  

Parâmetro	|	Descrição
------	|	-----
never	|	Nenhuma notificação é enviada - essa configuração é o padrão
on-every-event	|	Notificações são enviadas em cada evento, independentemente de se o estado do dispositivo é mudado
on-state-change	|	Notificações são enviadas somente quando um evento resulta em uma mudança de estado do dispositivo  

**Nota:** Se você selecionar uma configuração de notificação
*nunca*, seu aplicativo nunca receberá uma notificação sobre uma mudança no
estado do dispositivo. Portanto, convém escolher uma estratégia notificação *a cada
evento* ou *na mudança de estado*.  

Para mapear eventos, use a API a seguir:

```
POST /draft/device/types/{typeId}/mappings
```

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Neste cenário, definimos mapeamentos para o tipo de dispositivo *TSensor* para mapear a propriedade **t** no evento de entrada *tevt* para a propriedade **temperature** na interface lógica. Também definimos os mapeamentos para o tipo de dispositivo *TempSensor* para mapear a propriedade **temp** no evento de entrada *tempevt* para a propriedade **temperature** na interface lógica. As configurações de notificação são definidas como "na mudança de estado". 

O exemplo a seguir mostra como usar o cURL para incluir um mapeamento no tipo de dispositivo *TSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Importante:** para cargas úteis de evento [JSON ![ícone de Link externo](../../../icons/launch-glyph.svg "ícone de Link externo")](http://json-schema.org/){:new_window}, assegure-se de que a entrada $event.*property* corresponda corretamente à estrutura JSON de suas propriedades. Por exemplo, se a carga útil for `{"d":{"t":<temp>}}` use `$event.d.t`

Especifique o identificador de interface lógica *5846ed076522050001db0e12* que é retornado na resposta para o método POST usado para criar a interface lógica e o tipo de dispositivo *TSensor*.

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12", "propertyMappings": {
    "tevt" : {
      "temperature" : "$event.t"
              }
  }, "notificationStrategy": "on-state-change", "version": "draft", "created": "2017-06-16T15:41:49Z", "createdBy": "a-8x7nmj-9iqt56kfil", "updated": "2017-06-16T15:41:49Z", "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```
O exemplo a seguir mostra como usar o cURL para incluir um mapeamento no tipo de dispositivo *TempSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Especifique o identificador de interface lógica *5846ed076522050001db0e12* que é retornado na resposta para o método POST usado para criar a interface lógica e o tipo de dispositivo *TempSensor*.
É aplicada uma conversão para mudar o valor de uma medida em graus Fahrenheit para uma medida em graus Celsius.


O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12", "propertyMappings": {
     "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
    }
  }, "notificationStrategy": "on-state-change", "version": "draft", "created": "2017-06-16T15:41:49Z", "createdBy": "a-8x7nmj-9iqt56kfil", "updated": "2017-06-16T15:41:49Z", "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## Etapa 12: Validar e ativar a configuração
{: #step15}

Valide e ative a configuração que está relacionada à atualização de estado do dispositivo para cada tipo de dispositivo. Essa configuração inclui seus esquemas, tipos de eventos, interfaces físicas, interfaces lógicas e mapeamentos.

Para validar e ativar sua configuração de tipo de dispositivo, use a API a seguir:

```
PATCH /draft/device/types/{typeId}
```

em que *typeId* é o identificador de tipo de dispositivo.

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Neste cenário, precisamos validar e ativar a configuração para dois tipos de dispositivo.

O exemplo a seguir mostra como usar o cURL para validar e ativar sua configuração para o tipo de dispositivo *TSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
O exemplo a seguir mostra uma resposta para o método PATCH:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TSensor"]
  },
 "failures": []
}
```

O exemplo a seguir mostra como usar o cURL para validar e ativar sua configuração para o tipo de dispositivo *TempSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

O exemplo a seguir mostra uma resposta para o método PATCH:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TempSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TempSensor"]
  },
 "failures": []
}
```

## Etapa 13: Publicar um evento de dispositivo de entrada
{: #step12}

Publique um evento de temperatura de *tSensor* no tópico `iot-2/evt/tevt/fmt/json` com a carga útil de exemplo a seguir:
```
{
  "t" : 34.5
}
```
Publique um evento de temperatura de *tempSensor* no tópico `iot-2/evt/tempevt/fmt/json` com a carga útil de exemplo a seguir:
```
{
  "temp" : 72.55
}
```

Para obter informações sobre como publicar um evento de entrada por meio de um dispositivo, veja [Conectividade MQTT para aplicativos](../applications/mqtt.html#publishing_device_events).


## Etapa 14: Recuperar o estado do dispositivo
{: #step13}

É possível recuperar o estado do dispositivo usando APIs de REST HTTP ou assinando uma sequência de tópicos. Se você especificou um nome de alias quando criou sua interface lógica, é possível usar o nome de alias em vez do parâmetro *logicalInterfaceId* para recuperar o estado do dispositivo. 

- Se você tiver um aplicativo cliente MQTT, poderá assinar a sequência de tópicos a seguir:  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- Como alternativa, é possível recuperar o último estado do dispositivo usando a API de REST HTTP a seguir:  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição
------	|	-----
typeId	|	O identificador de tipo de dispositivo.
deviceId	|	O identificador de dispositivo.
logicalInterfaceId ou alias|	O identificador criado para a interface lógica ou o nome de alias especificado pelo usuário. 


Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

O exemplo a seguir mostra como usar o cURL para recuperar o estado atual de *tSensor*, referenciando o identificador da interface lógica que foi criada:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/tSensor/devices/tSensor/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

O identificador de interface lógica *5846ed076522050001db0e12* é usado no método GET. Este identificador é retornado na resposta para o método POST que foi usado para criar a interface lógica.
O exemplo a seguir mostra uma resposta para o método GET:
```
{
  "timestamp": "2017-07-03T12:15:50Z", "updated": "2017-07-03T12:15:50Z", "state": {
    "temperature":22.5
  }
}
```

O exemplo a seguir mostra como usar o cURL para recuperar o estado atual de *tempSensor* referenciando o alias da interface lógica que foi criada:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices/tempSensor/state/IThermometer \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

O alias da interface lógica *IThermometer* é usado no método GET. Este identificador é retornado na resposta para o método POST que foi usado para criar a interface lógica.
O exemplo a seguir mostra uma resposta para o método GET:
```
{
  "timestamp": "2017-07-03T12:15:50Z", "updated": "2017-07-03T12:15:50Z", "state": {
    "temperature":34.5
  }
}
```

Observe que a leitura de temperatura retornada está em graus Celsius e não em graus Fahrenheit.

Seu aplicativo pode processar esses dados normalizados sem requerer configuração para entender ou converter as diferentes escalas de temperatura.

## Próximas Eta

Você pode desejar construir este cenário para utilizar o recurso ativo gêmeo de gerenciamento de dados, configurando um tipo de Coisa e a instância da Coisa. Para obter informações sobre como configurar Coisas, veja [Guia passo a passo 2: um exemplo detalhado sobre como trabalhar com Coisas por meio de uma interface comum (beta)](../information_management/im_index_scenario_thing.html).

Também é possível criar regras que podem ser usadas para acionar uma ação quando um evento que é recebido pelo {{site.data.keyword.iot_short_notm}} causa uma mudança no estado do dispositivo ou da Coisa. Para obter informações sobre a criação de regras, veja [Criando regras integradas (beta)](../information_management/im_rules.html).

