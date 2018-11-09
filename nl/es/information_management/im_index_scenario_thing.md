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

# Guía paso a paso 2: Un ejemplo detallado sobre cómo trabajar con las cosas a través de una interfaz común (Beta)
{: #scenario}

Este caso de ejemplo se basa en la [Guía paso a paso 1: Un ejemplo detallado sobre cómo trabajar con dispositivos a través de una interfaz común](../GA_information_management/ga_im_index_scenario.html) anterior.

**Importante:** La característica cosas de {{site.data.keyword.iot_full}} para la gestión de datos solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

En este caso de ejemplo, los dispositivos de temperatura y humedad publican datos ambientales que se recopilan en dos salas de reuniones: Sala de reuniones 1 y Sala de reuniones 2. Para cada sala de reuniones, los datos del dispositivo sobre temperatura y humedad se correlacionan por separado con dos interfaces lógicas de tipo de dispositivo. 

Para la Sala de reuniones 1, los datos de dispositivo de temperatura asociados con el tipo de dispositivo *TSensor* se correlacionan con la interfaz lógica *Interfaz de termómetro* y los datos de dispositivo de humedad que están asociados con el tipo de dispositivo *HumiditySensor* se correlacionan con la interfaz lógica *Interfaz de higrómetro*. 

Para la Sala de reuniones 2, los datos de dispositivo de temperatura asociados con el tipo de dispositivo TempSensor se correlacionan con la interfaz lógica *Interfaz de termómetro* y los datos de dispositivo de humedad que están asociados con el tipo de dispositivo *HumiditySensor* se correlacionan con la interfaz lógica *Interfaz de higrómetro*. 

A continuación, se crea un tipo de cosa llamado *RoomType*, junto con dos instancias de cosas de la sala: *meetingroom1* y *meetingroom2*.

Este caso de ejemplo establece una composición que incluye las interfaces lógicas de termómetro e higrómetro y, a continuación, correlaciona el dispositivo de entorno correcto con cada una de las instancias de la sala, por ejemplo, *tSensor* y *humiditySensor1* se correlacionan con *meetingroom1*.

## Requisitos previos
{: #pre_req}

Antes de continuar, asegúrese de que:
- Utilice la misma instancia de organización de {{site.data.keyword.iot_full}} y una clave o señal de API para esa organización que ha utilizado en la Guía paso a paso 1. Para obtener más información sobre las claves y las señales de API, consulte la sección Autenticación de la documentación de [API REST de HTTP para aplicaciones](../applications/api.html#authentication).
- Tenga configuradas dos interfaces lógicas, una para un dispositivo de temperatura y otra para un dispositivo de humedad. Para obtener información sobre cómo configurar una interfaz lógica para un dispositivo de temperatura, consulte la documentación [Guía paso a paso 1: Un ejemplo detallado sobre cómo trabajar con dispositivos a través de una interfaz común](../GA_information_management/ga_im_index_scenario.html#step4). Para obtener información sobre cómo configurar una interfaz lógica para un dispositivo de humedad, consulte [Información adicional para la Guía paso a paso 2: Configuración de una interfaz lógica para un dispositivo de humedad](im_hygrometer.html).

## Acerca de esta tarea
{: #about}

En {{site.data.keyword.iot_short_notm}}, una cosa puede constar de un número de dispositivos y cosas. Un tipo de cosa define cómo se componen las instancias de una cosa. 

Una interfaz lógica está asociada con un tipo de cosa. Esta asociación define la estructura del estado que se genera para una instancia de tipo de cosa. Las correlaciones se utilizan para definir cómo se correlacionan las propiedades de los dispositivos agregados y las cosas con las propiedades en un estado de cosa.

La interfaz lógica se utiliza para eliminar el requisito de la aplicación para que entienda cómo se configura un dispositivo o una cosa. Por ejemplo, supongamos que desea medir la temperatura de una sala utilizando un único dispositivo, o que quiera calcular la temperatura de la sala tomando la lectura media de varios dispositivos. La aplicación necesita información sobre el estado de una sala o salas, uno de cuyos componentes es la propiedad temperatura. No importa cómo se ha calculado el valor de temperatura que recibe la aplicación.

En este caso de ejemplo, dos dispositivos de temperatura y dos dispositivos de humedad publican sucesos en {{site.data.keyword.iot_short_notm}}. Un dispositivo de temperatura y un dispositivo de humedad están en la Sala de reuniones 1 de un bloque de oficinas. Los otros dispositivos de temperatura y humedad se encuentran en la Sala de reuniones 2. El diagrama siguiente ilustra la configuración para la Sala de reuniones 1:

![Correlación entre temperatura y cosa de humedad y una aplicación en {{site.data.keyword.iot_short_notm}}.](images/Information)

Se utiliza un tipo de cosa denominado *RoomType* para definir cómo se componen las instancias de las salas. Una interfaz lógica está asociada con el *RoomType* y define que los sucesos de entrada se correlacionan con una sola lectura que muestra la temperatura y la humedad. Esta sola lectura es el estado de cosa. Las correlaciones se utilizan para definir cómo se correlacionan las propiedades de los dispositivos de temperatura y humedad con este estado de cosa. Cuando estos dispositivos publican una nueva lectura, se cambia el valor de la propiedad que está asociada con el estado de cosa.

La tabla siguiente muestra los cuatro dispositivos que se utilizan en nuestro ejemplo, el tema en el que publica cada dispositivo, y una carga útil de ejemplo para cada dispositivo.

Dispositivo/Tipo | Suceso | Carga de trabajo/Propiedad del suceso
------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**Nota:** Los identificadores de sucesos *tevt*, *tempevt*, *humevt* son necesarios cuando se definen correlaciones para crear una propiedad asociada con un suceso de entrada de ese tipo a una propiedad en la interfaz lógica. En este caso de ejemplo, se definen dos propiedades en la interfaz lógica: *temperatura* y *humedad*.

También se configura una interfaz lógica. La interfaz lógica representa el estado de cosa en la estructura siguiente:
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

Utilice el siguiente caso de ejemplo para configurar su propio entorno de interfaces. 

**Nota:** Una tabla que muestra los nombres de propiedades de recursos, los valores y los identificadores que se utilizan en esta guía se documenta en [Propiedades e identificadores de recursos a los que se hace referencia en la documentación de las Guías paso a paso 1 y 2](im_id_reference.html).

## Si es necesario, añada un tipo de dispositivo y un dispositivo  
{: #add_device}  
En este caso de ejemplo, se utilizan tres tipos de dispositivos y cuatro instancias de dispositivos. La instancia de dispositivo *tSensor* está asociada con el tipo de dispositivo *TSensor*. La instancia de dispositivo *tempSensor* está asociada con el tipo de dispositivo *TempSensor*. Las instancias de dispositivo *humiditySensor1* y *humiditySensor2* están asociadas con el tipo de dispositivo *HumiditySensor*.

Puede crear tipos de dispositivos y dispositivos utilizando el [panel de control de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://internetofthings.ibmcloud.com){: new_window}, o utilizando API REST. 

Para obtener más información sobre el uso del panel de control de {{site.data.keyword.iot_short_notm}} IoT Platform para añadir tipos de dispositivos y dispositivos, consulte la documentación [Cómo empezar con la Gestión de datos utilizando la interfaz web](../GA_information_management/im_ui_flow.html).

Para obtener información sobre el uso de las API REST para añadir tipos de dispositivos y dispositivos, consulte la documentación de la [API REST de HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window}.



## Paso 1: Crear un archivo de esquema de tipo cosa.  
{: #crt_composition_file}  
Cree un archivo de esquema de tipo cosa que haga referencia a los identificadores de interfaz lógica de dispositivo para los tipos de dispositivos de temperatura y de humedad.  

En el ejemplo siguiente se muestra cómo crear un archivo de esquema de tipo cosa que se denomina *roomTypeSchema*.   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON Schema that defines the structure of the Room Thing Type.",
   "properties" : {
        "thermometer": {
            "type": "object",
            "description": "The thermometer device",
            "$logicalInterfaceRef": "IThermometer"
        },
        "hygrometer": {
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
**Sugerencia:** Utilice el parámetro **necesario** para marcar una o más propiedades según sea necesario. Las propiedades obligatorias se deben incluir en un mensaje de dispositivo para que Watson IoT Platform actualice el estado del dispositivo con los datos del dispositivo. Un mensaje que no incluya la propiedad obligatoria no se procesa.   
**Nota:** El identificador de esquema que se genera al crear un archivo de esquema de tipo cosa debe especificarse cuando se crea el tipo de cosa.  

## Paso 2: Crear un recurso de esquema de cosa.  
{: #crt_composition_resource}  

Cree el recurso de esquema cargando el archivo de esquema de tipo cosa que se ha generado en el paso anterior.  
Cargue el archivo de esquema de tipo cosa utilizando la siguiente API:  
```
POST /draft/schemas
```  
El archivo definición de esquema se pasa a la Watson IoT Platform dentro de un POST de varias partes (multipart/form-data). El cuerpo de la POST debe contener al menos dos partes:

  - Una con un nombre de **schemaFile** que contiene el contenido real del archivo como el cuerpo de la parte.
  - Una con un nombre de **name** que contiene una serie que define el nombre del recurso de esquema en el cuerpo de la parte.


Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).  

En el ejemplo siguiente se muestra cómo utilizar cURL para crear el recurso de esquema de tipo cosa:  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
El ejemplo siguiente muestra una respuesta al método POST:
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
      "content" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
El identificador de esquema *5a72ea48d60180002c4f5e58* que se devuelve en respuesta al método POST es necesario cuando se crea un tipo de cosa.


## Paso 3: Crear un tipo de cosa  
{: #crt_thing_type}  

Los tipos de cosas se utilizan para modelar las instancias de cosas. Se debe crear un tipo de cosa en una organización antes de que se pueda crear una instancia de cosa. Para este caso de ejemplo, cree un tipo de cosa.  
El esquema que está asociado a un tipo de cosa define el tipo de objetos que se agregan juntos para crear una instancia de una cosa. El tipo de cosa debe hacer referencia al recurso de esquema de tipo de cosa que ha creado en el paso anterior.  
Cree un tipo de cosa utilizando la API siguiente:
```
POST /draft/thing/types
```
Los parámetros siguientes son necesarios en el cuerpo del método POST:  
<table>
<tr><th>Parámetro</th><th>Descripción</th></tr>
<tr><td>id</td><td>Proporcione un identificador para el tipo de cosa que está creando.</td></tr>
<tr><td>name</td><td>Proporcione un nombre para el tipo de cosa que está creando.</td></tr>
<tr><td>schemaId</td><td>El identificador creado para el recurso del esquema de composición.</td></tr>
</table>

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  

El ejemplo siguiente muestra cómo utilizar cURL para crear un tipo de cosa que se denomina *RoomType*.
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

Respuesta: 
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

## Paso 4: Crear un archivo de esquema de interfaz lógica  
{: #crt_ai_schema_file}
En la interfaz lógica, puede definir la estructura de los datos que se almacenan como el estado de cosa. Para este caso de ejemplo, cree una interfaz lógica que defina las propiedades de temperatura y humedad. Asocie la interfaz lógica con el tipo de cosa *RoomType* haciendo referencia al identificador de interfaz lógica que se genera al crear el recurso de interfaz lógica.  
En el ejemplo siguiente se muestra cómo crear un archivo de esquema de interfaz lógica denominado *roomSchema*.

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON Schema that defines the canonical room state structure",
    "properties" : {
        "temperature" : {
            "description" : "Temperatura en grados centígrados",
           "type" : "number",
           "minimum" : -273.15,
           "default" : 0.0
        },
       "humidity" : {
            "description" : "Porcentaje de humedad",
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
## Paso 5: Crear un recurso de esquema de interfaz lógica.  
{: #crt_ai_schema_resource}  
Cargue el archivo de esquema de la interfaz lógica que ha creado en el paso anterior para crear un recurso de esquema de interfaz lógica para el tipo de cosa utilizando la API siguiente:  
```
POST /draft/schemas
```  

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).  

En el siguiente ejemplo se muestra cómo utilizar cURL para crear el esquema de interfaz lógica:
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
El ejemplo siguiente muestra una respuesta al método POST:
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
    "content" : "/api/v0002/draft/schemas/5a4b9847d60180002efce645/content"
  },
  "id" : "5a4b9847d60180002efce645"
}
```
Utilice el identificador de esquema *5a4b9847d60180002efce645* que se devuelve en la respuesta al método POST para añadir el recurso de esquema de interfaz lógica a la interfaz lógica para el tipo de cosa.  


## Paso 6: Crear una interfaz lógica para el tipo de cosa.  
{: #crt_thing_ai}  
La interfaz lógica debe hacer referencia al recurso de esquema de interfaz lógica que ha creado en el paso anterior.  
Para crear una interfaz lógica, utilice la API siguiente:  
```
POST draft/logicalinterfaces
```  
Opcionalmente, puede especificar un nombre de alias significativo para la interfaz lógica. Se puede hacer referencia al alias en la llamada a la API o a la suscripción de serie de tema que se utiliza para recuperar el estado de una cosa, en lugar de utilizar el identificador de interfaz lógica generado automáticamente.

**Nota:** El nombre de alias debe tener entre 1 y 36 caracteres de longitud y puede incluir caracteres alfanuméricos, de guión, de punto y de subrayado. El nombre de alias no puede ser una serie hexadecimal de 24 caracteres.

En este caso de ejemplo, utilice el identificador de esquema *5a4b9847d60180002efce645* que se ha devuelto en la respuesta anterior para añadir el esquema de interfaz lógica a la interfaz lógica.

En el ejemplo siguiente se muestra cómo utilizar cURL para crear una interfaz lógica con el alias *IRoom*:
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
El ejemplo siguiente muestra una respuesta al método POST:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58"
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
En este caso de ejemplo, utilice el identificador de interfaz lógica *5a4b9847d60180002efce645* que se devuelve en la respuesta al método POST para añadir la interfaz lógica a su tipo de dispositivo. También utilizará este identificador para correlacionar un suceso de dispositivo de entrada a una propiedad definida por la interfaz lógica.

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).  


## Paso 7: Añadir la interfaz lógica al tipo de cosa.  
{: #add_thing_ai}  
Para añadir una interfaz lógica a un tipo de cosa, utilice la API siguiente:  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  
En este caso de ejemplo, la interfaz lógica está asociada con el tipo de cosa *RoomType*.

En el ejemplo siguiente se muestra cómo utilizar cURL para añadir la interfaz lógica de cosa *IRoom* al tipo de cosa *RoomType*:  
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

El ejemplo siguiente muestra una respuesta al método POST:
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

## Paso 8: Defina correlaciones
{: #define_Thing_type_mappings}

Defina las correlaciones para el tipo de cosa que describen cómo correlacionar las propiedades del estado de los dispositivos agregados o de las cosas con las propiedades definidas en la interfaz lógica del tipo de cosa.

Para correlacionar sucesos, utilice la siguiente API:  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

donde *thingtypeId* es el identificador que se devuelve en la respuesta a la solicitud POST cuando se crea el tipo de cosa. 

Los parámetros siguientes son necesarios en el cuerpo de la solicitud POST:  
<table>
<tr>
<th>	Parámetro	</th><th>	Descripción	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	Una estructura JSON válida que correlaciona las propiedades definidas para la interfaz lógica con las propiedades de la carga útil de sucesos de dispositivo.	</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	El identificador de interfaz lógica es necesario en el cuerpo de la carga útil.	</td>
</tr>
</table>  

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).

En el ejemplo siguiente se muestra cómo utilizar cURL para añadir una correlación al tipo de cosa *RoomType*:

```
{
  "logicalInterfaceId": "5a4b9847d60180002efce645",
  "notificationStrategy": "on-state-change",
  "propertyMappings": {
       "thermometer": {
         "temperature": "$event.temperature"
       },
       "hygrometer": {
         "humidity": "$event.humidity"
       }
   },
}
```  
El dispositivo *termómetro* se define en [roomTypeSchema](#crt_composition_file). La propiedad *$event.temperature* se define en el esquema de interfaz lógica con el identificador *5846ed076522050001db0e12* y el alias *IThermometer*.  
El dispositivo *higrómetro* se define en [roomTypeSchema](#crt_composition_file). La propiedad *$event.humidity* se define en el esquema de interfaz lógica con el identificador *5846cd7c6522050001db0e24* y el alias *IHygrometer*.


## Paso 9: Validar y activar la configuración
{: #activate}

Valide y active la configuración que está relacionada con la actualización del estado de cosa para cada tipo de cosa. Esta configuración incluye los esquemas, las interfaces lógicas y las correlaciones. 

Para validar y activar la configuración del tipo de cosa, utilice la API siguiente:
```
PATCH /draft/thing/types/{thingTypeId}
```
donde *thingTypeId* es el identificador de tipo de cosa. 

En el ejemplo siguiente se muestra cómo utilizar cURL para validar y activar la configuración que está asociada con el tipo de cosa *RoomType*:
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

El ejemplo siguiente muestra una respuesta al método PATCH:
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

## Paso 10: Crear una instancia de un tipo de cosa
{: #create_Thing_instances}

Una cosa es una instancia de un tipo de cosa. Una cosa le permite agregar una o varias instancias de un dispositivo o de una cosa juntas en un solo objeto.

Para crear una cosa, utilice la API siguiente:

```
POST /thing/types/{thingTypeId}/things
```
donde *thingtypeId* es el identificador que se devuelve en la respuesta a la solicitud POST cuando se crea el tipo de cosa. 

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things).

En este caso de ejemplo, tenemos que crear dos instancias de cosas que son del tipo de cosa *RoomType*. Una instancia de cosa se denomina *meetingroom1* y la otra instancia de cosa se denomina *meetingroom2*.

El ejemplo siguiente muestra cómo utilizar cURL para crear una instancia de cosa que se denomina *meetingroom1*. La instancia de cosa *meetingroom1* está asociada con las instancias de dispositivo *tSensor* y *humiditySensor1*.

```
thingId = "meetingroom1"
 meetingroom1AggregatedObjects = {
   "thermometer": {
     "type": "device",
     "typeId": "TSensor",
     "id": "tSensor"
   },
   "hygrometer": {
     "type": "device",
     "typeId": "HumiditySensor",
     "id": "humiditySensor1"
   }
 }
``` 

El identificador de cosa que se crea se utiliza en el URL del método POST que se llama para añadir un suceso de temperatura a la interfaz lógica de cosa.

El ejemplo siguiente muestra cómo utilizar cURL para crear una instancia de cosa que se denomina *meetingroom2*. *meetingroom2* está asociado con las instancias de dispositivo *tempSensor* y *humiditySensor2*.

```
thingId = "meetingroom2"
   meetingroom2AggregatedObjects = {
    "thermometer": {
      "type": "device",
      "typeId": "TempSensor",
      "id": "tempSensor"
    },
    "hygrometer": {
      "type": "device",
      "typeId": "HumiditySensor",
      "id": "humiditySensor2"
    }
  }

``` 

## Paso 11: Compruebe que los sucesos del dispositivo correlacionado se publican en la interfaz lógica  
{: #publish_event}  
Publique los siguientes sucesos para los dispositivos agregados en la cosa *meetingroom1*:  
 - un suceso de temperatura de *tSensor* en el tema `iot-2/evt/tevt/fmt/json`  
 - un suceso de humedad de *humiditySensor1* en el tema `iot-2/evt/humevt/fmt/json`  
 
Publique los siguientes sucesos para los dispositivos agregados en la cosa *meetingroom2*:  
 - un suceso de temperatura de *tempSensor* en el tema `iot-2/evt/tempevt/fmt/json`  
 - un suceso de humedad de *humiditySensor2* en el tema `iot-2/evt/humevt/fmt/json`  
 
Para obtener información sobre cómo publicar un suceso de entrada procedente de un dispositivo, consulte [Conectividad de MQTT para las aplicaciones](../applications/mqtt.html#publishing_device_events).  

## Paso 12: Comprobar que el estado de la cosa ha cambiado.  
{: #verify_Thing_state}  

Puede recuperar el estado de la cosa utilizando las API REST de HTTP, o bien mediante la suscripción a un tema.

Si tiene una aplicación cliente MQTT, puede suscribirse a la siguiente serie de tema:
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

De forma alternativa, puede recuperar el último estado de cosa utilizando la siguiente API REST de HTTP:

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

Los parámetros siguientes son obligatorios:  
<table>
<tr>
<th>Parámetro	</th><th>	Descripción</th>
</tr>
<tr>
<td>thingTypeId	</td><td>El identificador de tipo de cosa.</td>
</tr>
<tr>
<td>thingId	</td><td>	El identificador de cosa.</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>El identificador creado para la interfaz lógica.</td>
</tr>
</table>  

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  


## Pasos siguientes

Cree reglas que pueda utilizar para iniciar una acción cuando un suceso recibido por {{site.data.keyword.iot_short_notm}} provoca un cambio en el estado del dispositivo o de la cosa. Para obtener información sobre la creación de reglas, consulte [Creación de reglas incorporadas (Beta)](im_rules.html).
