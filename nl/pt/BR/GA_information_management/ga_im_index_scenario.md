---

copyright:
years: 2016, 2017
lastupdated: "2017-09-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Guia passo a passo: um exemplo detalhado sobre como trabalhar com dispositivos por meio de uma interface comum
{: #scenario}

Use as informações a seguir para criar um cenário no qual dois sensores de temperatura publiquem eventos no {{site.data.keyword.iot_full}}. Um sensor mede a temperatura em graus Celsius. O outro sensor mede a temperatura em graus Fahrenheit. Essas leituras são mapeadas para uma única leitura de temperatura que está em graus Celsius. Quando uma nova leitura de temperatura é publicada por esses dispositivos, o valor da propriedade associada ao estado do dispositivo muda.
{: shortdesc}

## Pré-requisitos

Deve-se ter uma instância de organização do {{site.data.keyword.iot_short_notm}} e uma chave ou um token API para essa organização.

## Sobre essa Tarefa

Neste cenário, dois dispositivos estão configurados.

Um dispositivo chama-se *TemperatureSensor1*. Esse dispositivo publica eventos de temperatura que são medidos em graus Celsius. O evento de temperatura é publicado no tópico `iot-2/evt/tevt/fmt/json` e possui a carga útil de exemplo a seguir:
```
{
  "t" : 34.5
}
```

**Nota:** o identificador de evento é *tevt*. Este identificador é necessário ao incluir um evento de temperatura deste tipo na interface física e ao definir mapeamentos para mapear uma propriedade associada a um evento de entrada deste tipo para uma propriedade em sua interface lógica. Neste cenário, a propriedade definida na interface lógica é chamada **temperatura**.

O outro dispositivo chama-se *TemperatureSensor2*. Esse dispositivo publica eventos de temperatura que são medidos em graus Fahrenheit. O evento de temperatura é publicado no tópico `iot-2/evt/tempevt/fmt/json` e possui a carga útil de exemplo a seguir:
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

![Mapeando entre os dispositivos de sensor de temperatura e um aplicativo em {{site.data.keyword.iot_short_notm}}.](images/Information)  

Use o cenário de exemplo a seguir para configurar seu próprio ambiente de interfaces.

## Se necessário, inclua um Tipo de dispositivo e um Dispositivo
{: #step14}

Neste cenário, são assumidos dois tipos de dispositivo e duas instâncias de dispositivo. A instância de dispositivo *TemperatureSensor1* é associada ao tipo de dispositivo *EnvSensor1*. A instância de dispositivo *TemperatureSensor2* é associada ao tipo de dispositivo *EnvSensor2*.

Para obter informações sobre como usar APIs de REST para incluir um tipo de dispositivo, consulte a documentação da [API de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html).

## Etapa 1: Criar um arquivo de esquema de evento de rascunho
{: #step1}

Para este cenário, crie dois arquivos de esquema de evento de rascunho para definir
a estrutura de cada um dos eventos de temperatura de entrada.

O exemplo a seguir mostra como criar um arquivo de esquema de rascunho que é chamado *tEventSchema.json*. Esse arquivo define a estrutura de um evento de entrada de um sensor de temperatura que mede temperatura em graus Celsius:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor1 tEvent Schema",
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
**Dica:** use o parâmetro “required” para marcar uma ou mais propriedades conforme necessário. As
propriedades necessárias devem ser incluídas em uma mensagem do dispositivo para que o
{{site.data.keyword.iot_short_notm}} atualize o estado do dispositivo com
os dados do dispositivo. Uma mensagem que não inclui a propriedade necessária não é processada.   

O nome do arquivo de esquema *tEventSchema* é usado quando você cria um recurso de esquema de evento para seu tipo de evento.

O exemplo a seguir mostra como criar um arquivo de esquema de rascunho chamado *tempEventSchema.json*. Esse arquivo define a estrutura de um evento de entrada de um sensor de temperatura que mede a temperatura em graus Fahrenheit:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor2 tempEvent Schema",
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

## Etapa 2: Criar um recurso de esquema de evento de rascunho para seu tipo de evento
{: #step2}

Para criar um recurso de esquema de evento, use a API a seguir:

```
POST /draft/schemas
```

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição  
------	|	-----  
name	|	Forneça um nome para o esquema de evento que você está criando.  
schemaFile	|	Caminho para o arquivo JSON do esquema de evento local.  



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

## Etapa 3: Criar um tipo de evento de rascunho que referencie o esquema de evento
{: #step3}

Cada tipo de evento referencia o esquema de evento relevante que foi criado no exemplo anterior usando o identificador de esquema retornado na resposta para o método POST usado para criar o recurso de esquema de evento.

Para criar um tipo de evento de rascunho, use a API a seguir:

```
POST /draft/event/types
```

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição
------	|	-----
name	|	Forneça um nome para o tipo de evento que você está criando.
schemaId	|	O identificador criado para o recurso de esquema de evento.


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

## Etapa 4: Criar uma interface física de rascunho
{: #step7}

Para criar uma interface física de rascunho, use a API a seguir:

```
POST /draft/physicalinterfaces
```

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição
------	|	-----
name	|	Forneça um nome para a interface física que você está criando.


Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Neste cenário, precisamos de duas interfaces físicas, uma para cada tipo de evento.

O exemplo a seguir mostra como usar cURL para criar a primeira interface física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Env sensor physical interface 1"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  }, "id" : "5847d1df6522050001db0e1a", "name" : "Env sensor physical interface 1", "version" : "draft", "created" : "2016-12-07T09:09:51Z", "updated" : "2016-12-07T09:09:51Z", "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

O identificador de interface física *5847d1df6522050001db0e1a* retornado na resposta é usado na URL do método POST que é chamado para incluir um evento de temperatura que é medido em graus Celsius para a interface física.

O exemplo a seguir mostra como usar cURL para criar a segunda interface física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Env sensor physical interface 2"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  }, "id" : "5847d1df6522050001db0e1b", "name" : "Env sensor physical interface 2", "version" : "draft", "created" : "2016-12-07T09:19:51Z", "updated" : "2016-12-07T09:19:51Z", "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

O identificador de interface física *5847d1df6522050001db0e1b* retornado na resposta é usado na URL do método POST que é chamado para incluir um evento de temperatura que é medido em graus Fahrenheit para a interface física.   

## Etapa 5: Incluir o tipo de evento na interface física de rascunho
{: #step8}

Para incluir um tipo de evento na interface física, use a API a seguir:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição
------	|	-----
eventId	|	Insira o nome do evento da sua carga útil do evento de dispositivo.
eventTypeId	|	O identificador criado para o tipo de evento.


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

## Etapa 6: Atualizar o tipo de dispositivo de rascunho para conectar a interface física de rascunho
{: #step9}

Para atualizar um tipo de dispositivo de rascunho, use a API a seguir:

```
POST /draft/device/types/{typeId}/physicalinterface
```

em que *typeId* é o identificador de tipo de dispositivo.


Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Neste cenário, o tipo de dispositivo *EnvSensor1* é atualizado para se conectar à interface física *5847d1df6522050001db0e1a* e o tipo de dispositivo *EnvSensor2* é atualizado para se conectar à interface física *5847d1df6522050001db0e1b*.

O exemplo a seguir mostra como usar cURL para atualizar o tipo de dispositivo *EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/physicalinterface \
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
  }, "id" : "5847d1df6522050001db0e1a", "name" : "Env sensor physical interface 1", "version" : "draft", "created" : "2016-12-07T09:09:51Z", "updated" : "2016-12-07T09:09:51Z", "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de dispositivo *EnvSensor1* é necessário quando você inclui sua interface física e sua interface lógica.
O exemplo a seguir mostra como usar cURL para atualizar o tipo de dispositivo *EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/physicalinterface \
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
  }, "id" : "5847d1df6522050001db0e1b", "name" : "Env sensor physical interface 2", "version" : "draft", "created" : "2016-12-07T09:19:51Z", "updated" : "2016-12-07T09:19:51Z", "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de dispositivo *EnvSensor2* é necessário quando você inclui
sua interface física e sua interface lógica.
## Etapa 7: Criar um arquivo de esquema de interface lógica de rascunho
{: #step4}

O exemplo a seguir mostra como criar um arquivo de esquema de interface lógica de
rascunho chamado *envSensor.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Environment Sensor Schema",
    "description" : "Schema to represent a canonical environment sensor device",
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

## Etapa 8: Criar um recurso de esquema de interface lógica de rascunho
{: #step5}

Para criar um recurso de esquema de interface lógica de rascunho, use a API a seguir:

```
POST /draft/schemas
```

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição
------	|	-----
name	|	Forneça um nome para o esquema de interface lógica que você está criando.
schemaFile	|	Caminho para o arquivo JSON do esquema de interface lógica local.


Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

O exemplo a seguir mostra como usar cURL para criar o esquema de interface lógica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=temperatureEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/envSensor.json"'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "temperatureEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "envSensor.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
Use o identificador de esquema *5846ec826522050001db0e11* que é retornado na
resposta ao método POST para incluir o esquema de interface lógica na interface lógica.
## Etapa 9: Criar uma interface lógica de rascunho que referencie um esquema de interface lógica de rascunho
{: #step6}

Para criar uma interface lógica rascunho, use a API a seguir:

```
POST /draft/logicalinterfaces
```

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição
------	|	-----
name	|	Forneça um nome para a interface lógica que você está criando.
schemaId	|	O identificador criado para o recurso de esquema de interface lógica.


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
  --data '{"name" : "environment sensor interface", "schemaId" : "5846ec826522050001db0e11"}'
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
  "name" : "environment sensor interface",
  "version" : "draft"
}
```
Neste cenário, use o identificador de interface lógica
*5846ed076522050001db0e12* que é retornado na resposta ao método POST para
incluir sua interface lógica no seu tipo de dispositivo. É possível também usar esse
identificador para mapear um evento de dispositivo de entrada para uma propriedade que é
definida pela interface lógica.
## Etapa 10: Incluir a interface lógica de rascunho em um tipo de dispositivo
{: #step10}

Para incluir uma interface lógica de rascunho em um tipo de dispositivo, use a API a seguir:

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

em que *typeId* é o nome do tipo de dispositivo. 

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).  
**Nota:** Neste cenário, a mesma interface lógica
*5846ed076522050001db0e12* está associada a *EnvSensor1* e
*EnvSensor2*.

O exemplo a seguir mostra como usar cURL para incluir a interface lógica
*5846ed076522050001db0e12* que referencia o identificador de esquema de
interface lógica *5846ec826522050001db0e11* no tipo de dispositivo
*EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  }, "updated" : "2016-12-06T16:53:27Z", "updatedBy" : "a-8x7nmj-9iqt56kfil", "createdBy" : "a-8x7nmj-9iqt56kfil", "name" : "environment sensor interface", "version" : "draft", "created" : "2016-12-06T16:53:27Z", "id" : "5846ed076522050001db0e12", "schemaId" : "5846ec826522050001db0e11"
}
```

O exemplo a seguir mostra como usar cURL para incluir a interface lógica
*5846ed076522050001db0e12* associada ao identificador de esquema lógico
*5846ec826522050001db0e11* no tipo de dispositivo
*EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

O exemplo a seguir mostra uma resposta para o método POST:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  }, "updated" : "2016-12-06T16:53:27Z", "updatedBy" : "a-8x7nmj-9iqt56kfil", "createdBy" : "a-8x7nmj-9iqt56kfil", "name" : "environment sensor interface", "version" : "draft", "created" : "2016-12-06T16:53:27Z", "id" : "5846ed076522050001db0e12", "schemaId" : "5846ec826522050001db0e11"
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

Os seguintes parâmetros são obrigatórios:  

Parâmetro	|	Descrição
------	|	-----
logicalInterfaceId	|	O identificador que é criado para a interface lógica.
propertyMappings	|	Uma estrutura JSON válida que mapeia as propriedades definidas para a
interface lógica com as propriedades da carga útil de evento de dispositivo.	
Você tem a opção de configurar o parâmetro *notificationStrategy*.  

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Neste cenário, definimos os mapeamentos do tipo de dispositivo
*EnvSensor1* para mapear a propriedade **t** no evento
de entrada *tevt* para a propriedade **temperature** na
interface lógica. Também definimos os mapeamentos do tipo de dispositivo
*EnvSensor2* para mapear a propriedade **temp** no evento
de entrada *tempevt* para a propriedade **temperature**
na interface lógica. As configurações de notificação são definidas como "na mudança de estado". 

O exemplo a seguir mostra como usar cURL para incluir um mapeamento no tipo de dispositivo *EnvSensor1*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Importante:** para cargas úteis de evento [JSON ![ícone de Link externo](../../../icons/launch-glyph.svg "ícone de Link externo")](http://json-schema.org/){:new_window}, assegure-se de que a entrada $event.*property* corresponda corretamente à estrutura JSON de suas propriedades. 
Por exemplo, se a carga útil for `{"d":{"t":<temp>}}` use `$event.d.t`

Especifique o identificador de interface lógica
*5846ed076522050001db0e12* que é retornado na resposta ao método POST usado
para criar a interface lógica e o tipo de dispositivo *EnvSensor1*.

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
O exemplo a seguir mostra como usar cURL para incluir um mapeamento no tipo de dispositivo *EnvSensor2*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Especifique o identificador de interface lógica
*5846ed076522050001db0e12* que é retornado na resposta ao método POST usado
para criar a interface lógica e o tipo de dispositivo *EnvSensor2*.
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

Valide e ative a configuração que está relacionada à atualização de estado do
dispositivo para cada tipo de dispositivo de rascunho. Esta configuração inclui seus esquemas de rascunho, tipos de eventos, interfaces físicas, interfaces lógicas e mapeamentos.

Para validar e ativar a configuração do tipo de dispositivo de rascunho, use a API a seguir:

```
PATCH /draft/device/types/{typeId}
```

em que *typeId* é o identificador de tipo de dispositivo.

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Neste cenário, precisamos validar e ativar a configuração de dois tipos de dispositivo de rascunho.

O exemplo a seguir mostra como usar cURL para validar e ativar sua configuração para o tipo de dispositivo de rascunho *EnvSensor1*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

O exemplo a seguir mostra uma resposta para o método PATCH:

```
{
 "message": "CUDRS0520I: A configuração de atualização de estado do tipo de dispositivo 'EnvSensor1' foi submetida com êxito para ativação",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor1"]
  },
 "failures": []
}
```
O exemplo a seguir mostra como usar cURL para validar e ativar sua configuração do tipo de dispositivo de rascunho *EnvSensor2*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
O exemplo a seguir mostra uma resposta para o método PATCH:

```
{
 "message": "CUDRS0520I: A configuração de atualização de estado do tipo de dispositivo 'EnvSensor2' foi submetida com sucesso para ativação",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor2"]
  },
 "failures": []
}
```

## Etapa 13: Publicar um evento de dispositivo de entrada
{: #step12}

Publique um evento de temperatura de *TemperatureSensor1* no tópico `iot-2/evt/tevt/fmt/json` com a carga útil de exemplo a seguir:
```
{
  "t" : 34.5
}
```
Publique um evento de temperatura de *TemperatureSensor2* no tópico `iot-2/evt/tempevt/fmt/json` com a carga útil de exemplo a seguir:
```
{
  "temp" : 72.55
}
```

Para obter informações sobre como publicar um evento de entrada por meio de um dispositivo, veja [Conectividade MQTT para aplicativos](../applications/mqtt.html#publishing_device_events).


## Etapa 14: Recuperar o estado do dispositivo
{: #step13}

É possível recuperar o estado do dispositivo usando APIs de REST HTTP ou assinando um tópico.

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
logicalInterfaceId	|	O identificador criado para a interface lógica.

Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

O exemplo a seguir mostra como usar cURL para recuperar o estado atual de *TemperatureSensor1* referenciando o identificador da interface lógica que foi criada:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor1/devices/TemperatureSensor1/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

O identificador de interface lógica *5846ed076522050001db0e12* é usado no método GET. Este identificador é retornado na resposta para o método POST que foi usado para criar a interface lógica.
O exemplo a seguir mostra uma resposta para o método GET:
```
{
  "timestamp": "2017-07-03T12:15:50Z", "updated": "2017-07-03T12:15:50Z", "state": {
    "temperature":34.5
  }
}
```
O exemplo a seguir mostra como usar cURL para recuperar o estado atual de *TemperatureSensor2* referenciando o identificador da interface lógica que foi criada:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor2/devices/TemperatureSensor2/state/5846ed076522050001db0e12 \
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
Observe que a leitura de temperatura retornada está em graus Celsius e não em graus Fahrenheit.

Seu aplicativo pode processar esses dados normalizados sem requerer configuração para entender ou converter as diferentes escalas de temperatura.


