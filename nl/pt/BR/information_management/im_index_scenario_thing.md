---

copyright:
years: 2016, 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Guia passo a passo 2: um exemplo detalhado sobre como trabalhar com Coisas por meio de uma interface comum (beta)
{: #scenario}

Este cenário é construído no [Guia passo a passo 1: um exemplo detalhado sobre como trabalhar com dispositivos por meio de uma interface comum](../GA_information_management/ga_im_index_scenario.html) anterior.

**Importante:** o recurso Coisas do {{site.data.keyword.iot_full}} para gerenciamento de dados está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Neste cenário, os dispositivos de temperatura e umidade publicam dados de ambiente que são coletados em duas salas de reunião - Sala de reunião 1 e Sala de reunião 2. Para cada sala de reunião, os dados do dispositivo de temperatura e umidade são mapeados separadamente para duas interfaces lógicas de tipo de dispositivo. 

Para a Sala de reunião 1, os dados do dispositivo de temperatura associados ao tipo de dispositivo *TSensor* são mapeados para a interface lógica *Thermometer Interface* e os dados do dispositivo de umidade associados ao tipo de dispositivo *HumiditySensor* são mapeados para a interface lógica *Hygrometer Interface*. 

Para a Sala de reunião 2, os dados do dispositivo de temperatura associados ao tipo de dispositivo TempSensor são mapeados para a interface lógica *Thermometer Interface* e os dados do dispositivo de umidade associados ao tipo de dispositivo *HumiditySensor* são mapeados para a interface lógica *Hygrometer Interface*. 

Um tipo de Coisa chamado *RoomType* é então criado, juntamente com duas instâncias de Coisa: *meetingroom1* e *meetingroom2*.

Este cenário configura uma composição que inclui as interfaces lógicas de termômetro e higrômetro e, em seguida, mapeia o dispositivo ambiental correto para cada uma das instâncias de sala, por exemplo, *tSensor* e *humiditySensor1* são mapeados para *meetingroom1*.

## Pré-requisitos
{: #pre_req}

Antes de continuar, certifique-se de:
- Usar a mesma instância de organização do {{site.data.keyword.iot_full}} e uma chave API ou token para essa organização que você usou no guia passo a passo 1. Para obter mais informações sobre chaves API e tokens, veja a seção Autenticação da documentação [API de REST HTTP para aplicativos](../applications/api.html#authentication).
- Ter duas interfaces lógicas configuradas, uma para um dispositivo de temperatura e uma para um dispositivo de umidade. Para obter informações sobre a configuração de uma interface lógica para um dispositivo de temperatura, veja a documentação [Guia passo a passo 1: um exemplo detalhado sobre como trabalhar com dispositivos por meio de uma interface comum](../GA_information_management/ga_im_index_scenario.html#step4). Para obter informações sobre a configuração de uma interface lógica para um dispositivo de umidade, veja [Informações adicionais para o Guia passo a passo 2 - configurando uma Interface Lógica para um dispositivo de umidade](im_hygrometer.html).

## Sobre essa Tarefa
{: #about}

No {{site.data.keyword.iot_short_notm}}, uma Coisa pode consistir em vários dispositivos e Coisas. Um tipo de Coisa define como as instâncias de uma Coisa são compostas. 

Uma interface lógica é associada com um tipo de Coisa. Essa associação define a estrutura do estado que é gerado para uma instância de tipo de Coisa. Os mapeamentos são usados para definir como as propriedades dos dispositivos e Coisas agregados são mapeadas para as propriedades em um estado de Coisa.

A interface lógica é usada para remover o requisito para que o aplicativo entenda como um dispositivo ou Coisa está configurado. Por exemplo, é possível medir a temperatura de uma sala usando um único
dispositivo ou é possível calcular a temperatura da sala fazendo a leitura média de um número de
dispositivos. O aplicativo requer informações sobre o estado de uma ou mais salas, um componente do qual uma
temperatura é propriedade. Não importa como o valor de temperatura que é recebido pelo aplicativo é
calculado.

Neste cenário, dois dispositivos de temperatura e dois dispositivos de umidade publicam eventos no {{site.data.keyword.iot_short_notm}}. Um dispositivo de temperatura e um dispositivo de umidade estão na Sala de reunião 1 de um bloco de escritório. Os outros dispositivos de temperatura e umidade estão na Sala de reunião 2. O diagrama a seguir ilustra a configuração para a Sala de reunião 1:

![Mapeamento entre a Coisa de temperatura e umidade e um aplicativo no {{site.data.keyword.iot_short_notm}}.](images/Information)

Um tipo de Coisa chamado *RoomType* é usado para definir como as instâncias de salas são compostas. Uma interface lógica está associada ao *RoomType* e define que os eventos de entrada são mapeados para uma única leitura que mostra tanto a temperatura quanto a umidade. Essa única leitura é o estado de Coisa. Os mapeamentos são usados para definir como as propriedades dos dispositivos de temperatura e umidade são mapeadas para esse estado de Coisa. Quando uma nova leitura é publicada por esses dispositivos, o valor da propriedade que está associado ao estado de Coisa é mudado.

A tabela a seguir mostra os quatro dispositivos que são usados em nosso exemplo, o tópico no qual cada dispositivo publica e uma carga útil de exemplo para cada dispositivo.

Dispositivo/Tipo | Evento | Propriedade/Carga útil do evento
------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**Nota:** os identificadores de evento *tevt*, *tempevt*, *humevt* são necessários quando você define mapeamentos para tornar uma propriedade que está associada a um evento de entrada desse tipo em uma propriedade na interface lógica. Neste cenário, duas propriedades são definidas na interface lógica - *temperature* e *humidity*.

Uma interface lógica também é configurada. A interface lógica representa o estado de Coisa na estrutura a seguir:
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

Use o cenário de exemplo a seguir para configurar seu próprio ambiente de interfaces. 

**Nota:** uma tabela listando os nomes, valores e identificadores de propriedade de recurso que são usados neste guia está documentada em [Propriedades e identificadores de recursos que são referenciados na documentação dos Guias passo a passo 1 e 2](im_id_reference.html).

## Se necessário, inclua um tipo de dispositivo e um dispositivo  
{: #add_device}  
Neste cenário, três tipos de dispositivo e quatro instâncias de dispositivo são usados. A instância de dispositivo *tSensor* está associada ao tipo de dispositivo *TSensor*. A instância de dispositivo *tempSensor* está associada ao tipo de dispositivo *TempSensor*. As instâncias de dispositivo *humiditySensor1* e *humiditySensor2* estão associadas ao tipo de dispositivo *HumiditySensor*.

É possível criar tipos de dispositivo e dispositivos usando o [painel do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://internetofthings.ibmcloud.com){: new_window} ou usando APIs de REST. 

Para obter mais informações sobre como usar o painel do {{site.data.keyword.iot_short_notm}} IoT Platform para incluir tipos de dispositivo e dispositivos, consulte a documentação de [Introdução ao gerenciamento de dados usando a interface da web](../GA_information_management/im_ui_flow.html).

Para obter informações sobre como usar APIs de REST para incluir dispositivos e tipos de dispositivo, veja a documentação [{{site.data.keyword.iot_short_notm}}API de REST HTTP ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window}.



## Etapa 1: criar um arquivo de esquema de tipo de Coisa.  
{: #crt_composition_file}  
Crie um arquivo de esquema de tipo de Coisa que referencie os identificadores de interface lógica do dispositivo para os tipos de dispositivo de temperatura e umidade.  

O exemplo a seguir mostra como criar um arquivo de esquema de tipo de Coisa chamado *roomTypeSchema*.   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON Schema that defines the structure of the Room Thing Type.",
   "properties" : {
        "termômetro": {
            "type": "object",
            "description": "The thermometer device",
            "$logicalInterfaceRef": "IThermometer"
        }, "hygrômetro": {, "hygrômetro": {
            "type": "object",
            "description": "The hygrometer device",
            "$logicalInterfaceRef": "IHygrometer"
        }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```
**Dica:** use o parâmetro **required** para marcar uma ou mais propriedades, conforme necessário. As propriedades necessárias devem ser incluídas em uma mensagem do dispositivo para o Watson IoT Platform para atualizar o estado do dispositivo com os dados do dispositivo. Uma mensagem que não inclui a propriedade necessária não é processada.   
**Nota:** o identificador de esquema que é gerado quando você cria um arquivo de esquema de tipo de Coisa deve ser especificado ao criar seu tipo de Coisa.  

## Etapa 2: criar um recurso de esquema de Coisa.  
{: #crt_composition_resource}  

Crie o recurso de esquema fazendo upload do arquivo de esquema de tipo de Coisa que foi gerado na etapa anterior.  
Faça upload do arquivo de esquema de tipo de Coisa usando a API a seguir:  
```
POST /draft/schemas
```  
O arquivo de definição de esquema é transmitido para o Watson IoT Platform dentro de um POST de várias partes (dados de formulário/partes múltiplas). O corpo do POST deve conter pelo menos duas partes:

  - Um com um nome de **schemaFile** que contém o conteúdo real do arquivo como o corpo da parte.
  - Um com um nome de **name** que contém uma sequência que define o nome do recurso de esquema no corpo da parte.


Para obter mais detalhes, veja a documentação [API de REST HTTP de {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).  

O exemplo a seguir mostra como usar o cURL para criar o recurso de esquema de tipo de Coisa:  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "name" : "roomTypeSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "roomType.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5a72ea48d60180002c4f5e58",
  "refs" : {
      "content": "/api/v0002/draft/schemas/5a72a72ea48d60180002c4f5e58/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
O identificador de esquema *5a72ea48d60180002c4f5e58* que é retornado em resposta ao método POST é necessário quando você cria um tipo de Coisa.


## Etapa 3: Criar um Tipo de Thing  
{: #crt_thing_type}  

Os tipos de Coisa são usados para modelar instâncias de Coisa. Um tipo de Coisa deve ser criado em uma organização antes que uma instância de Coisa possa ser criada. Para este cenário, crie um tipo de Coisa.  
O esquema que está associado a um tipo de Coisa define o tipo dos objetos que são agregados juntos para criar uma instância de uma Coisa. O tipo de Coisa deve referenciar o recurso de esquema de tipo de Coisa que você criou na etapa anterior.  
Crie um tipo de Coisa usando a API a seguir:
```
POST /draft/thing/types
```
Os parâmetros a seguir são necessários no corpo do método POST:  
<table>
<tr><th>Parâmetro</th><th>Descrição</th></tr>
<tr><td>id</td><td>Forneça um identificador para o tipo de Coisa que você está criando.</td></tr>
<tr><td>name</td><td>Forneça um nome para o tipo de Coisa que você está criando.</td></tr>
<tr><td>schemaId</td><td>O identificador criado para o recurso de esquema de composição.</td></tr>
</table>

Para obter mais detalhes, consulte a documentação de [API de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  

O exemplo a seguir mostra como usar o cURL para criar um tipo de Coisa chamado *RoomType*.
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

Resposta: 
```
{
 "name": "RoomType",
 "description": "Room Thing Type",
 "id": "RoomType",
 "schemaId": "5a72ea48d60180002c4f5e58",
 "metadata": {},
  "refs": {
    "schema": "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58",
    "mappings": "/api/v0002/draft/thing/types/RoomType/mappings",
    "logicalInterfaces": "/api/v0002/draft/thing/types/RoomType/logicalinterfaces"
   },
 "version": "draft",
 "created": "2018-02-01T10:22:43Z",
 "createdBy": "ANOther",
 "updated": "2018-02-01T10:22:43Z",
 "updatedBy": "ANOther"
}
```

## Etapa 4: criar um arquivo de esquema de interface lógica  
{: #crt_ai_schema_file}
Em sua interface lógica, é possível definir a estrutura dos dados que são armazenados como o estado de Coisa. Para este cenário, crie uma interface lógica que defina propriedades de temperatura e umidade. Associe a interface lógica ao tipo de Coisa *RoomType* referenciando o identificador de interface lógica que é gerado ao criar o recurso de interface lógica.  
O exemplo a seguir mostra como criar um arquivo de esquema de interface lógica chamado *roomSchema*.

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON Schema that defines the canonical room state structure",
    "properties" : {
        "temperature" : {
            "description" : "Temperature in degrees celsius",
           "type" : "number",
           "minimum" : -273.15,
           "default" : 0.0
        },
       "humidity" : {
            "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```  
## Etapa 5: criar um recurso de esquema da interface lógica.  
{: #crt_ai_schema_resource}  
Faça upload do arquivo de esquema de interface lógica criado na etapa anterior para criar um recurso de esquema de interface lógica para seu tipo de Coisa usando a API a seguir:  
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
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "roomSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "room.json",
  "version" : "draft",
  "refs" : {
    "content": "/api/v0002/draft/schemas/5a4b9847d60180002efce645/content"
  }, "id": "5a4b9847d60180002efce645"
}
```
Use o identificador de esquema *5a4b9847d60180002efce645* que é retornado na resposta para o método POST para incluir o recurso de esquema da interface lógica na interface lógica para seu tipo de Coisa.  


## Etapa 6: criar uma interface lógica para o tipo de Coisa.  
{: #crt_thing_ai}  
A interface lógica deve referenciar o recurso de esquema da interface lógica que você criou na etapa anterior.  
Para criar uma interface lógica, use a API a seguir:  
```
POST draft/logicalinterfaces
```  
Opcionalmente, é possível especificar um nome de alias significativo para sua interface lógica. O alias pode ser referenciado na chamada de API ou na assinatura de sequência de tópicos que é usada para recuperar o estado de uma Coisa, em vez de usar o identificador de interface lógica gerado automaticamente.

**Nota:** o nome do alias deve ter de 1 a 36 caracteres de comprimento e pode incluir caracteres alfanuméricos, de hífen, de ponto e sublinhado. O nome do alias não pode ser uma sequência hexadecimal de 24 caracteres.

Neste cenário, use o identificador de esquema *5a4b9847d60180002efce645* que foi retornado na resposta anterior para incluir o esquema de interface lógica na interface lógica.

O exemplo a seguir mostra como usar o cURL para criar uma interface lógica com o alias *IRoom*:
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
O exemplo a seguir mostra uma resposta para o método POST:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema": "/api/v0002/draft/draft/schemas/5a72ea48d60180002c4f5e58"
  },
  "schemaId" : "5a72ea48d60180002c4f5e58",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5a4b9847d60180002efce645",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "IRoom",
  "alias" : "IRoom",
  "version" : "draft"
}
```
Neste cenário, use o identificador de interface lógica *5a4b9847d60180002efce645* que é retornado na resposta para o método POST para incluir sua interface lógica em seu tipo de dispositivo. É possível também usar esse
identificador para mapear um evento de dispositivo de entrada para uma propriedade que é
definida pela interface lógica.

Para obter mais detalhes, consulte a documentação da
[API
de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).


## Etapa 7: incluir a interface lógica no tipo de Coisa.  
{: #add_thing_ai}  
Para incluir uma interface lógica em um tipo de Coisa, use a API a seguir:  
```
POST draft/thing/types/ { /logicalinterfaces
```  

Para obter mais detalhes, consulte a documentação de [API de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  
Neste cenário, a interface lógica está associada ao tipo de Coisa *RoomType*.

O exemplo a seguir mostra como usar o cURL para incluir a interface lógica de Coisa *IRoom* no tipo de Coisa *RoomType*:  
```
{   
  "id": "5a4b9847d60180002efce645"
}
```
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id": "5a4b9847d60180002efce645"}'
```

O exemplo a seguir mostra uma resposta para o método POST:
```
{
 "name": "Room Logical Interface",
 "description": "This is a Room logical interface",
 "id": "5a4b9847d60180002efce645",
 "schemaId": "5a4b9817d60180002efce644",
 "refs": {
   "schema": "/api/v0002/draft/schemas/5a4b9817d60180002efce644"
 },
 "version": "draft",
 "created": "2018-01-02T14:33:43Z",
 "createdBy": "ANOther",
 "updated": "2018-01-02T14:33:43Z",
 "updatedBy": "ANOther"
}
```

## Etapa 8: Definir mapeamentos
{: #define_Thing_type_mappings}

Defina os mapeamentos para o tipo de Coisa que descrevem como mapear propriedades do estado dos dispositivos agregados ou Coisas para as propriedades que são definidas na interface lógica do tipo de Coisa.

Para mapear eventos, use a API a seguir:  
```
Rascunhamento de POST t/thing/types/ { /mappings
```  

em que *thingtypeId* é o identificador retornado na resposta à solicitação de POST quando o tipo Coisa é criado. 

Os parâmetros a seguir são necessários no corpo da solicitação POST:  
<table>
<tr>
<th>	Parâmetro	</th><th>	Descrição	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	Uma estrutura JSON válida que mapeia propriedades definidas para a interface lógica com as propriedades da carga útil do evento do dispositivo.	</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	O identificador de interface lógica é necessário no corpo da carga útil.	</td>
</tr>
</table>  

Para obter mais detalhes, consulte a documentação de [API de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).

O exemplo a seguir mostra como usar o cURL para incluir um mapeamento no tipo de Coisa *RoomType*:

```
{
  "logicalInterfaceId": "5a4b9847d60180002efce645",
  "notificationStrategy": "on-state-change",
  "propertyMappings": {
       "termômetro": {
         "temperatura": "$event.temperature"
       }, "hygrômetro": {, "hygrômetro": {
         "humidity": "$event.humidity"
       }
   },
}
```  
O dispositivo *thermometer* é definido em [roomTypeSchema](#crt_composition_file). A propriedade *$event.temperature* é definida no esquema da interface lógica com o identificador *5846ed076522050001db0e12* e alias *IThermometer*.  
O dispositivo *hygrometer* é definido no [roomTypeSchema](#crt_composition_file). A propriedade *$event.humidity* é definida no esquema de interface lógica com o identificador *5846cd7c6522050001db0e24* e alias *IHygrometer*.


## Etapa 9: validar e ativar a configuração
{: #activate}

Valide e ative a configuração que está relacionada à atualização de estado de Coisa para cada tipo de Coisa. Essa configuração inclui seus esquemas, interfaces lógicas e mapeamentos. 

Para validar e ativar sua configuração de tipo de Coisa, use a API a seguir:
```
PATCH /draft/thing/types/ {
```
em que *thingTypeId* é o identificador de tipo de Coisa. 

O exemplo a seguir mostra como usar o cURL para validar e ativar a configuração que está associada ao tipo de Coisa *RoomType*:
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

O exemplo a seguir mostra uma resposta para o método PATCH:
```
{
 "message": "CUDIM0300I: State update configuration for Thing Type 'Room Thing Type' has been successfully submitted for  activation.",
"details": {
   "id": "CUDIM0300I",
   "properties": [
     "Thing Type",
     "Room Thing Type"
   ]
 },
 "failures": []
}
```

## Etapa 10: criar uma instância de um tipo de Coisa
{: #create_Thing_instances}

Uma Coisa é uma instância de um tipo de Coisa. Uma Coisa permite que você agregue uma ou mais instâncias de um dispositivo ou Coisa juntas em um único objeto.

Para criar uma Coisa, use a API a seguir:

```
POST /thing/types/{thingTypeId}/things
```
em que *thingtypeId* é o identificador retornado na resposta à solicitação de POST quando o tipo Coisa é criado. 

Para obter mais detalhes, consulte a documentação [{{site.data.keyword.iot_short_notm}}API de REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things).

Neste cenário, precisamos criar duas instâncias de Coisa que são do tipo de Coisa *RoomType*. Uma instância de Coisa é chamada *meetingroom1* e a outra instância de Coisa é chamada *meetingroom2*.

O exemplo a seguir mostra como usar o cURL para criar uma instância de Coisa chamada *meetingroom1*. A instância de Coisa *meetingroom1* está associada às instâncias de dispositivo *tSensor* e *humiditySensor1*.

```
thingId = "meetingroom1"
 meetingroom1AggregatedObjects = {
   "termômetro": {
     "type": "device",
     "typeId": "TSensor",
     "id": "tSensor"
   }, "hygrômetro": {, "hygrômetro": {
     "type": "device",
     "typeId": "HumiditySensor",
     "id": "humiditySensor1"
   }
 }
``` 

O identificador de Coisa criado é usado na URL do método POST que é chamado para incluir um evento de temperatura na interface lógica de Coisa.

O exemplo a seguir mostra como usar o cURL para criar uma instância de Coisa chamada *meetingroom2*. *meetingroom2* está associado às instâncias de dispositivo *tempSensor* e *humiditySensor2*.

```
thingId = "meetingroom2"
   meetingroom2AggregatedObjects = {
    "termômetro": {
      "type": "device",
      "typeId": "TempSensor",
      "id": "tempSensor"
    }, "hygrômetro": {, "hygrômetro": {
      "type": "device",
      "typeId": "HumiditySensor",
      "id": "humiditySensor2"
    }
  }

``` 

## Etapa 11: verificar se os eventos de dispositivo mapeados foram publicados na interface lógica  
{: #publish_event}  
Publique os eventos a seguir para os dispositivos que são agregados na Coisa *meetingroom1 *:  
 - um evento de temperatura de *tSensor* no tópico `iot-2/evt/tevt/fmt/json`  
 - um evento de umidade de *humiditySensor1* no tópico `iot-2/evt/humevt/fmt/json`  
 
Publique os eventos a seguir para os dispositivos que são agregados na Coisa *meetingroom2*:  
 - um evento de temperatura de *tempSensor* no tópico `iot-2/evt/tempevt/fmt/json`  
 - um evento de umidade no *humiditySensor2* no tópico
`iot-2/evt/humevt/fmt/json`  
 
Para obter informações sobre como publicar um evento de entrada por meio de um dispositivo, veja [Conectividade MQTT para aplicativos](../applications/mqtt.html#publishing_device_events).  

## Etapa 12: verificar se o estado da Coisa foi mudado.  
{: #verify_Thing_state}  

É possível recuperar o estado da Coisa usando APIs de REST HTTP ou assinando um tópico.

Se você tiver um aplicativo cliente MQTT, poderá assinar a sequência de tópicos a seguir:
```
iot-2/type/ ${ /id/ $thingId/intf/ ${ /evt/state
``` 

Como alternativa, é possível recuperar o estado de Coisa mais recente usando a API de REST HTTP a seguir:

```
GET /thing/types/ { /things/ { /state/ {
```  

Os seguintes parâmetros são obrigatórios:  
<table>
<tr>
<th>Parâmetro	</th><th>	Descrição</th>
</tr>
<tr>
<td>thingTypeId	</td><td>O identificador de tipo de Encadeamento.</td>
</tr>
<tr>
<td>thingId	</td><td>	O identificador de Coisa.</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>O identificador criado para a interface lógica.</td>
</tr>
</table>  

Para obter mais detalhes, consulte a documentação de [API de REST HTTP do {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  


## Próximas Eta

Crie regras que poderão ser usadas para iniciar uma ação quando um evento que for recebido pelo {{site.data.keyword.iot_short_notm}} causar uma mudança no estado do dispositivo ou da Coisa. Para obter informações sobre a criação de regras, veja [Criando regras integradas (beta)](im_rules.html).
