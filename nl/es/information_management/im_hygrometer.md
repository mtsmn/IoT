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

# Información adicional para la Guía paso a paso 2: Configuración de una interfaz lógica para un dispositivo de humedad

Utilice la siguiente información para crear un caso de ejemplo en el que los sensores de dos dispositivos de humedad publiquen sucesos en IBM Watson™ IoT Platform.  Las lecturas de los dispositivos de humedad se correlacionan con una sola lectura de humedad. Un dispositivo está ubicado en la sala de reuniones 1, y el otro dispositivo se ubica en la sala de reuniones 2.


## Acerca de esta tarea

Utilice la misma instancia de organización de {{site.data.keyword.iot_short_notm}} y una clave o señal de API para dicha organización que ha utilizado en la [Guía paso a paso 1](../GA_information_management/ga_im_index_scenario.html). 

Esta configuración es un requisito previo a la guía de aprendizaje descrita en la documentación [Guía paso a paso 2](im_index_scenario_thing.html).

En este caso de ejemplo, se crea un tipo de dispositivo y dos instancias de dispositivo.

Las instancias de dispositivo se denominan *humiditySensor1* y *humiditySensor2*. Los dispositivos publican sucesos de humedad. El suceso de humedad tiene la carga útil de ejemplo siguiente:
```
{
  "hum" : 35
}
```
Los sucesos de humedad se publican en el tema `iot-2/evt/humevt/fmt/json`. 

**Nota:** El identificador de suceso es *humevt*. Este identificador es necesario cuando se añade un suceso de humedad de estos tipos a la interfaz física y cuando se definen correlaciones para correlacionar una propiedad asociada con un suceso de entrada de este tipo a una propiedad en la interfaz lógica. En este caso de ejemplo, la propiedad definida en la interfaz lógica se denomina **humidity**. Esta interfaz lógica representa el estado de los dispositivos de este tipo en la estructura siguiente:
```
{
  "humidity" : <current humidity value in Celsius>
}
```

Utilice el siguiente caso de ejemplo para configurar su propio entorno de interfaces.

**Nota importante:** Debe guardar los ID que se generan en las respuestas de curl, ya que los ID son necesarios para completar otras tareas.

## Si es necesario, añada un tipo de dispositivo y un dispositivo
{: #step14}

En este caso de ejemplo, se supone un tipo de dispositivo y dos instancias de dispositivo. Las instancias de dispositivo *humiditySensor1* y *humiditySensor2* están asociadas con el tipo de dispositivo *HumiditySensor*. 

Puede crear tipos de dispositivos y dispositivos utilizando el [panel de control de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://internetofthings.ibmcloud.com){: new_window}, o utilizando API REST. Para obtener más información sobre el uso del panel de control de {{site.data.keyword.iot_short_notm}} IoT Platform para añadir tipos de dispositivos y dispositivos, consulte la documentación [Cómo empezar con la Gestión de datos utilizando la interfaz web](../GA_information_management/im_ui_flow.html).

En el ejemplo siguiente se muestra cómo crear un tipo de dispositivo denominado *HumiditySensor* utilizando la API REST:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**Nota importante:** Puede añadir metadatos al crear un tipo de dispositivo y un dispositivo. En este caso de ejemplo, se añaden los metadatos siguientes al tipo de dispositivo *HumiditySensor*:
```
{
    "maxHumidityThreshold": 70
}
```
Estos metadatos se pueden utilizar cuando [crea reglas](im_rules.html) que activan si un suceso de humedad que hace que la propiedad *humidity* del estado del dispositivo supere un valor de 70% se recibe mediante {{site.data.keyword.iot_short_notm}} desde *humiditySensor1* o *humiditySensor2*. 

## Registrar los dispositivos

En el ejemplo siguiente se muestra cómo registrar una instancia de dispositivo llamada *humiditySensor1*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
En el ejemplo siguiente se muestra cómo registrar una instancia de dispositivo denominada *humiditySensor2*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**Nota:** Cuando un dispositivo realiza una solicitud HTTP a través de la API REST de HTTP de Watson IoT Platform, se necesita un nombre de usuario y una contraseña. La contraseña es el valor de la señal de autenticación que se genera automáticamente o que se especifica manualmente cuando se registra un dispositivo. Si utiliza un cliente MQTT, debe conservar una nota de la señal de autenticación del dispositivo, ya que necesita la señal para recuperar el dispositivo o el estado de cosa suscribiéndose a una serie de tema de IoT.

## Paso 1. Cree un archivo de esquema de suceso
{: #step1}

Para este caso de ejemplo, cree un archivo de esquema de sucesos para definir la estructura de los sucesos de humedad de entrada.

En el ejemplo siguiente se muestra cómo crear un archivo de esquema llamado *humEventSchema.json*. Este archivo define la estructura de un suceso de entrada desde un dispositivo de humedad:

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "defines the structure of a humidity event",
    "properties" : {
        "humidity" : {
            "description" : "Porcentaje de humedad",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
      "humidity"
    ]
}
```
**Sugerencia:** Utilice el parámetro **necesario** para marcar una o más propiedades según sea necesario. Las propiedades obligatorias se deben incluir en un mensaje de dispositivo para que {{site.data.keyword.iot_short_notm}} actualice el estado del dispositivo con los datos del dispositivo. Un mensaje que no incluya la propiedad obligatoria no se procesa.   


## Paso 2. Cree un recurso de esquema de suceso para el tipo de suceso
{: #step2}

Para crear un recurso de esquema de suceso, utilice la siguiente API:

```
POST /draft/schemas
```

El archivo definición de esquema se pasa a la Watson IoT Platform dentro de un POST de varias partes (multipart/form-data). El cuerpo de la POST debe contener al menos dos partes:

- Una con un nombre de **schemaFile** que contiene el contenido real del archivo como el cuerpo de la parte.
- Una con un nombre de **name** que contiene una serie que define el nombre del recurso de esquema en el cuerpo de la parte.

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

En el ejemplo siguiente se muestra cómo utilizar cURL para crear el recurso de esquema de sucesos *humEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**Nota:** El valor de autorización de ejemplo `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` consta de la información siguiente:
`{API Key}:{authorization token}`, que está codificada como Base64.

El ejemplo siguiente muestra una respuesta al método POST:

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
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
El identificador de esquema *5846cd7c6522050001db0e20* que se devuelve en respuesta al método POST es necesario cuando se añade un esquema de suceso al tipo de suceso.



## Paso 3. Cree un tipo de suceso que haga referencia al esquema de suceso
{: #step3}

Cada tipo de suceso hace referencia al esquema de suceso relevante que se ha creado en el ejemplo anterior utilizando el identificador de esquema devuelto como respuesta al método POST utilizado para crear el recurso de esquema de suceso.

Para crear un tipo de suceso, utilice la siguiente API:

```
POST /draft/event/types
```
El tipo de suceso de borrador debe hacer referencia a la definición de esquema que define la estructura del suceso MQTT de entrada.


Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


En el ejemplo siguiente se muestra cómo utilizar cURL para crear un tipo de suceso para un suceso de humedad:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

El identificador de esquema *5846cd7c6522050001db0e20* se utiliza para añadir el esquema de suceso al tipo de suceso. Este identificador se ha devuelto en respuesta al método POST que se ha utilizado para crear el recurso de esquema de sucesos *humEventSchema.json*

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e20",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20"
  },
  "name" : "humEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e21",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

El identificador de tipo de suceso *5846cd7c6522050001db0e21* que se devuelve en respuesta al método POST se utiliza para añadir un tipo de suceso a la interfaz física.


## Paso 4. Cree una interfaz física
{: #step7}

Para crear una interfaz física, utilice la siguiente API:

```
POST /draft/physicalinterfaces
```

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

En este caso de ejemplo, necesitamos una interfaz física para el tipo de suceso que hemos creado anteriormente.

En el ejemplo siguiente se muestra cómo utilizar cURL para crear la interfaz física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

El identificador de interfaz física *5846cd7c6522050001db0e22* que se devuelve en la respuesta se utiliza en el URL del método POST al que se llama para añadir un suceso de temperatura que se mide en grados Celsius a la interfaz física.


## Paso 5. Añada el tipo de suceso a la interfaz física
{: #step8}

Para añadir un tipo de suceso a la interfaz física, utilice la siguiente API:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

En este caso de ejemplo, el tipo de suceso siguiente se añade a las interfaces físicas especificadas:
- el suceso de humedad *humevt* se añade a la interfaz física con el identificador *5846cd7c6522050001db0e22* utilizando el *eventId* del tema y el *eventTypeId* de la creación del recurso de esquema de sucesos.

En el ejemplo siguiente se muestra cómo utilizar cURL para añadir el suceso de humedad *humevt* a la interfaz física con el identificador *5846cd7c6522050001db0e22*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## Paso 6. Actualice el tipo de dispositivo para que se conecte a la interfaz física
{: #step9}

Para actualizar un tipo de dispositivo, utilice la siguiente API:

```
POST /draft/device/types/{typeId}/physicalinterface
```

donde *typeId* es el identificador de tipo de dispositivo.


Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

En este caso de ejemplo, el tipo de dispositivo *HumiditySensor* se actualiza para conectarse a la interfaz física *5846cd7c6522050001db0e22*.

En el ejemplo siguiente se muestra cómo utilizar cURL para actualizar el tipo de dispositivo *HumiditySensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

El ejemplo siguiente muestra una respuesta al método POST:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
El identificador de dispositivo *HumiditySensor* es necesario cuando se añade la interfaz física y la interfaz lógica.
  


## Paso 7: Crear un archivo de esquema de interfaz lógica
{: #step4}

En el ejemplo siguiente se muestra cómo crear un archivo de esquema de interfaz lógica que se denomina *hygrometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema that defines the canonical interface for a hygrometer",
    "properties" : {
        "humidity" : {
            "description" : "percentage humidity",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## Paso 8: Crear un recurso de esquema de interfaz lógica
{: #step5}
En el siguiente ejemplo se muestra cómo utilizar cURL para crear una interfaz lógica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
El ejemplo siguiente muestra una respuesta al método POST:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
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

## Paso 9: Crear una interfaz lógica que haga referencia a un esquema de interfaz lógica
{: #step6}

Para crear una interfaz lógica, utilice la API siguiente:

```
POST /draft/logicalinterfaces
```

Opcionalmente, puede especificar un nombre de alias significativo para la interfaz lógica. Se puede hacer referencia al alias en la suscripción de llamada de API o de serie de tema que se utiliza para recuperar el estado de un dispositivo, en lugar de utilizar el identificador de interfaz lógica generado automáticamente.

**Nota:** El nombre de alias debe tener entre 1 y 36 caracteres de longitud y puede incluir caracteres alfanuméricos, de guión, de punto y de subrayado. El nombre de alias no puede ser una serie hexadecimal de 24 caracteres. 

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

En este caso de ejemplo, utilice el identificador de esquema *5846cd7c6522050001db0e23* que se ha devuelto en la respuesta anterior para añadir el esquema de interfaz lógica a la interfaz lógica.

En el siguiente ejemplo se muestra cómo utilizar cURL para crear una interfaz lógica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
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
En este caso de ejemplo, utilice el identificador de interfaz lógica *5846cd7c6522050001db0e24* que se devuelve como respuesta al método POST para añadir la interfaz lógica al tipo de dispositivo. También utilizará este identificador para correlacionar un suceso de dispositivo de entrada a una propiedad definida por la interfaz lógica. Puede utilizar el alias de interfaz lógica *IHygrometer* para recuperar el estado del dispositivo, utilizando las API REST de HTTP, o bien suscribiéndose a una serie de tema.


## Paso 10: Añadir la interfaz lógica al tipo de dispositivo

En el ejemplo siguiente se muestra cómo utilizar cURL para añadir la interfaz lógica 5846cd7c6522050001db0e24 que hace referencia al identificador de esquema de interfaz lógica 5846cd7c6522050001db0e23 al tipo de dispositivo HumiditySensor:
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
El ejemplo siguiente muestra una respuesta al método POST:
```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
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

## Paso 11: Añadir las correlaciones al tipo de dispositivo

    
En el ejemplo siguiente se muestra cómo utilizar cURL para añadir una correlación al tipo de dispositivo *HumiditySensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

El ejemplo siguiente muestra una respuesta al método POST:
```
{
  "logicalInterfaceId": "5846cd7c6522050001db0e24",
  "propertyMappings": {
    "humevt": {
      "humidity": "$event.hum"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## Paso 12: Activar la configuración para el tipo de dispositivo


    
En el ejemplo siguiente se muestra cómo utilizar cURL para validar y activar la configuración para el tipo de dispositivo *HumiditySensor*:
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
El ejemplo siguiente muestra una respuesta al método PATCH:
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
