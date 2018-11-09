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


# Guía paso a paso 1: Un ejemplo detallado sobre cómo trabajar con dispositivos a través de una interfaz común
{: #scenario}

Utilice la información siguiente para crear un caso de ejemplo en el que dos dispositivos de temperatura publican sucesos en {{site.data.keyword.iot_full}}. Un dispositivo mide la temperatura en grados Celsius. El otro dispositivo mide la temperatura en grados Fahrenheit. Estas lecturas se correlacionan con una única lectura de temperatura que está en grados centígrados. Cuando estos dispositivos publican una nueva lectura de temperatura, el valor de la propiedad asociada al estado del dispositivo se modifica.
{: shortdesc}

## Antes de empezar

Cuando crea un [recurso](ga_im_definitions.html#definitions_resources), ese recurso se crea como versión borrador. La versión borrador es una copia de trabajo de su recurso que puede consultar, actualizar y suprimir directamente utilizando las API. Al utilizar las API REST, el prefijo **draft/** se utiliza para identificar los recursos que están en un estado borrador. 

Para obtener más información sobre versiones de borrador y activas de los recursos, consulte [Visión general de la gestión de datos](ga_im_definitions.html).

## Requisitos previos

Debe tener una [instancia de organización](../iotplatform_overview.html#organizations) de {{site.data.keyword.iot_short_notm}} y una clave API y señal de autenticación para que dicha organización autentique las solicitudes. 

Para obtener información sobre la generación de una clave de API, consulte la documentación de [API](../reference/api.html). Para obtener más información sobre la generación de una señal de API, consulte la documentación de [Guía de aprendizaje de iniciación](../getting-started.html).


## Acerca de esta tarea

En este caso de ejemplo, se crean dos dispositivos.

Un dispositivo se denomina *tSensor*. Este dispositivo publica los sucesos de temperatura que se miden en grados centígrados. El suceso de temperatura se publica en el tema `iot-2/evt/tevt/fmt/json` y tiene la siguiente carga útil de ejemplo:
```
{
  "t" : 34.5
}
```

**Nota:** el identificador de suceso es *tevt*. Este identificador es necesario cuando se añade un suceso de temperatura de este tipo a la interfaz física y cuando se definen las correlaciones para correlacionar una propiedad asociada a un suceso de entrada de este tipo a una propiedad de la interfaz lógica. En este caso de ejemplo, la propiedad definida en la interfaz lógica se denomina **temperature**.

El otro dispositivo se denomina *tempSensor*. Este dispositivo publica los sucesos de temperatura que se miden en grados Fahrenheit. El suceso de temperatura se publica en el tema `iot-2/evt/tempevt/fmt/json` y tiene la siguiente carga útil de ejemplo:
```
{
  "temp" : 72.55
}
```

**Nota:** el identificador de suceso es *tempevt*. Este identificador es necesario cuando se añade un suceso de temperatura de este tipo a la interfaz física y cuando se definen las correlaciones para correlacionar una propiedad asociada a un suceso de entrada de este tipo a una propiedad de la interfaz lógica. En este caso de ejemplo, la propiedad definida en la interfaz lógica se denomina **temperature**.

También se configura una interfaz lógica. Esta interfaz lógica representa el estado de los dispositivos de este tipo en la estructura siguiente:
```
{
  "temperature" : <valor temperatura actual en grados centígrados>
  }
```
Esta configuración significa que puede configurar la aplicación para que procese el valor asociado con **temperature** en lugar de configurar la aplicación para que procese el valor asociado con **t** y procese el valor asociado con **temp** después de convertir dicho valor a grados centígrados.

![Correlación entre dispositivos de sensor de temperatura y aplicaciones en {{site.data.keyword.iot_short_notm}}.](../information_management/images/Information)  

Utilice el siguiente caso de ejemplo para configurar su propio entorno de interfaces.

**Nota importante:** Debe guardar los ID que se generan en las respuestas de curl, ya que los ID son necesarios para completar otras tareas.
Una tabla que enumera los nombres, valores e identificadores de propiedad de recursos que se utilizan en esta guía se documenta en [Información adicional para las Guías paso a paso 1 y 2 - nombres e identificadores de recursos](../information_management/im_id_reference.html).

## Si es necesario, añada un tipo de dispositivo y un dispositivo
{: #step14}

En este caso de ejemplo, se presupone que hay dos tipos de dispositivo y dos instancias de dispositivo. La instancia de dispositivo *tSensor* está asociada con el tipo de dispositivo *TSensor*. La instancia de dispositivo *tempSensor* está asociada con el tipo de dispositivo *TempSensor*. 

Puede crear tipos de dispositivos y dispositivos utilizando el [panel de control de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://internetofthings.ibmcloud.com){: new_window}, o utilizando API REST. Para obtener más información sobre cómo utilizar el panel de control de {{site.data.keyword.iot_short_notm}} para añadir tipos de dispositivos y dispositivos, consulte la documentación [Cómo empezar con la Gestión de datos utilizando la interfaz web](im_ui_flow.html).

En el ejemplo siguiente se muestra cómo crear un tipo de dispositivo denominado *TSensor* utilizando la API REST:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TSensor", "description" : "The Celsius sensor device type", "metadata": {"tempThresholdMax": 44,
    "tempThresholdMin": 10}}' \
 ```
 
 En el ejemplo siguiente se muestra cómo crear un tipo de dispositivo denominado *TempSensor* utilizando la API REST:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TempSensor", "description" : "The Fahrenheit sensor device type"}' \
 ```

**Nota:** Puede añadir metadatos al crear un tipo de dispositivo y un dispositivo. En este caso de ejemplo, se añaden los metadatos siguientes al tipo de dispositivo *TSensor*:
```
{
    "tempThresholdMax": 44,
    "tempThresholdMin": 10 
}
```
Estos metadatos se utilizan cuando se crean [reglas](../information_management/im_rules.html) que se desencadenan cuando un suceso de temperatura que hace que la propiedad *temperatura* del estado del dispositivo supere los 44 grados Celsius se recibe mediante {{site.data.keyword.iot_short_notm}} desde el dispositivo *tSensor*. 


A continuación, debe registrar una instancia de dispositivo que esté asociada a un tipo de dispositivo. El ejemplo siguiente muestra cómo registrar una instancia de dispositivo llamada *tSensor* que está asociada con el tipo de dispositivo *TSensor* utilizando la API REST:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tSensor", "authToken": "password"}' \
```

El ejemplo siguiente muestra cómo registrar una instancia de dispositivo llamada *tempSensor* que está asociada con el tipo de dispositivo *TempSensor* utilizando la API REST:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tempSensor", "authToken": "password"}' \
```

Para obtener información sobre el uso de las API REST para añadir tipos de dispositivos y dispositivos, consulte la documentación de la [API REST de HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

**Nota:** Cuando un dispositivo realiza una solicitud HTTP a través de la API REST de HTTP de Watson IoT Platform, se necesita un nombre de usuario y una contraseña. La contraseña es el valor de la señal de autenticación que se genera automáticamente o que se especifica manualmente cuando se registra un dispositivo. Si está utilizando un cliente MQTT, debe conservar una nota de la señal de autenticación del dispositivo, ya que necesita que la señal recupere el estado de dispositivo o de cosa mediante la suscripción a una serie de tema.

## Paso 1. Cree un archivo de esquema de suceso
{: #step1}

Para este caso de ejemplo, cree dos archivos de esquema de suceso para definir la estructura de cada uno de los sucesos de temperatura de entrada.

En el siguiente ejemplo se muestra cómo crear un archivo de esquema denominado *tEventSchema.json*. Este archivo define la estructura de un suceso de entrada a partir de un dispositivo de temperatura que mide la temperatura en grados Celsius:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tEventSchema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
      "description" : "temperatura en grados centígrados",
      "type" : "number",
      "minimum" : -273.15,
      "default" : 0.0
    }
  },
  "required" : ["t"]
}
  ```
**Sugerencia:** Utilice el parámetro **necesario** para marcar una o más propiedades según sea necesario. Las propiedades obligatorias se deben incluir en un mensaje de dispositivo para que {{site.data.keyword.iot_short_notm}} actualice el estado del dispositivo con los datos del dispositivo. Un mensaje que no incluya la propiedad obligatoria no se procesa.   

El archivo de esquema llamado *tEventSchema* se utiliza al crear un suceso de recurso de esquema para el tipo de suceso.

En el siguiente ejemplo se muestra cómo crear un archivo de esquema denominado *tempEventSchema.json*. Este archivo define la estructura de un suceso de entrada a partir de un dispositivo de temperatura que mide la temperatura en grados Fahrenheit:

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
El archivo de esquema llamado *tempEventSchema* se utiliza al crear un suceso de recurso de esquema para el tipo de suceso.   

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

En el siguiente ejemplo se muestra cómo utilizar cURL para crear el recurso de esquema de suceso *tEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

**Sugerencia:** El valor de autorización de ejemplo `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` consta de la información siguiente:
`{API Key}:{authorization token}`, que está codificada como Base64.

El ejemplo siguiente muestra una respuesta al método POST:

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
El identificador de esquema *5846cd7c6522050001db0e0d* que se devuelve como respuesta al método POST es necesario cuando se añade un esquema de suceso al tipo de suceso.

En el siguiente ejemplo se muestra cómo utilizar cURL para crear el recurso de esquema de suceso *tempEventSchema.json*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

El ejemplo siguiente muestra una respuesta al método POST:

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
El identificador de esquema *5846cee36522050001db0e0e* que se devuelve como respuesta al método POST es necesario cuando se añade un esquema de suceso al tipo de suceso.

## Paso 3. Cree un tipo de suceso que haga referencia al esquema de suceso
{: #step3}

Cada tipo de suceso hace referencia al esquema de suceso relevante que se ha creado en el ejemplo anterior utilizando el identificador de esquema devuelto como respuesta al método POST utilizado para crear el recurso de esquema de suceso.

Para crear un tipo de suceso, utilice la siguiente API:

```
POST /draft/event/types
```
El tipo de suceso de borrador debe hacer referencia a la definición de esquema que define la estructura del suceso MQTT de entrada.


Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


El siguiente ejemplo muestra cómo utilizar cURL para crear un tipo de suceso para un suceso de temperatura que se mide en grados centígrados:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

El identificador de esquema *5846cd7c6522050001db0e0d* se utiliza para añadir el esquema de suceso al tipo de suceso. Este identificador se ha devuelto como respuesta al método POST utilizado para crear el recurso de esquema de suceso *tEventSchema.json*

El ejemplo siguiente muestra una respuesta al método POST:

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

El identificador de tipo de suceso *5846d0fd6522050001db0e0f* que se devuelve como respuesta al método POST se utiliza para añadir un tipo de suceso a la interfaz física.

El siguiente ejemplo muestra cómo utilizar cURL para crear un tipo de suceso para un suceso de temperatura que se mide en grados Fahrenheit:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
El identificador de esquema *5846cee36522050001db0e0e* se utiliza para añadir el esquema de suceso al tipo de suceso. Este identificador se ha devuelto como respuesta al método POST utilizado para crear el recurso de esquema de suceso *tempEventSchema.json*

El ejemplo siguiente muestra una respuesta al método POST:

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
El identificador de tipo de suceso *5846d2846522050001db0e10* que se devuelve como respuesta al método POST se utiliza para añadir un tipo de suceso a la interfaz física.

## Paso 4. Cree una interfaz física
{: #step7}

Para crear una interfaz física, utilice la siguiente API:

```
POST /draft/physicalinterfaces
```

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

En este caso de ejemplo, necesitamos dos interfaces físicas, una para cada tipo de suceso.

En el siguiente ejemplo se muestra cómo utilizar cURL para crear la primera interfaz física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TSensor Physical Interface"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

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

El identificador de la interfaz física *5847d1df6522050001db0e1a* que se devuelve como respuesta se utiliza en el URL del método POST al que se llama para añadir un suceso de temperatura que se mide en grados centígrados a la interfaz física.

En el siguiente ejemplo se muestra cómo utilizar cURL para crear la segunda interfaz física:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TempSensor Physical Interface"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

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

El identificador de la interfaz física *5847d1df6522050001db0e1b* que se devuelve como respuesta se utiliza en el URL del método POST al que se llama para añadir un suceso de temperatura que se mide en grados Fahrenheit a la interfaz física.   

## Paso 5. Añada el tipo de suceso a la interfaz física
{: #step8}

Para añadir un tipo de suceso a la interfaz física, utilice la siguiente API:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

En este caso de ejemplo, se añaden los siguientes tipos de suceso a las interfaces físicas especificadas:
- El suceso de temperatura en grados centígrados *tevt* se añade a la interfaz física con el identificador *5847d1df6522050001db0e1a* utilizando el *eventId* procedente del tema y el valor de *eventTypeId* procedente de la creación del recurso de esquema de suceso.
- El suceso de temperatura en grados Fahrenheit *tempevt* se añade a la interfaz física con el identificador *5847d1df6522050001db0e1b* utilizando el *eventId* procedente del tema y el valor de *eventTypeId* procedente de la creación del recurso de esquema de suceso.


En el siguiente ejemplo se muestra cómo utilizar cURL para añadir el suceso de temperatura *tevt* a la interfaz física con el identificador *5847d1df6522050001db0e1a*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

En el siguiente ejemplo se muestra cómo utilizar cURL para añadir el suceso de temperatura *tempevt* a la interfaz física con el identificador *5847d1df6522050001db0e1b*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
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

En este caso de ejemplo, el tipo de dispositivo *TSensor* se actualiza para conectarse a la interfaz física *5847d1df6522050001db0e1b* y el tipo de dispositivo *TempSensor* se actualiza para conectarse a la interfaz física *5847d1df6522050001db0e1a*.


En el ejemplo siguiente se muestra cómo utilizar cURL para actualizar el tipo de dispositivo *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

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
El identificador de dispositivo *TSensor* es necesario cuando se añade la interfaz física y la interfaz lógica.

En el ejemplo siguiente se muestra cómo utilizar cURL para actualizar el tipo de dispositivo *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

El ejemplo siguiente muestra una respuesta al método POST:
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
El identificador de dispositivo *TempSensor* es necesario cuando se añade la interfaz física y la interfaz lógica.


## Paso 7: Crear un archivo de esquema de interfaz lógica
{: #step4}

En el ejemplo siguiente se muestra cómo crear un archivo de esquema de interfaz lógica que se denomina *thermometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "thermometerSchema",
    "description" : "Schema that defines the canonical interface for a thermometer",
    "properties" : {
        "temperature" : {
            "description" : "temperatura en grados centígrados",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## Paso 8: Crear un recurso de esquema de interfaz lógica
{: #step5}

Para crear un recurso de esquema de interfaz lógica, utilice la API siguiente:

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
  --form name=thermometerSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/thermometer.json"'
```

El ejemplo siguiente muestra una respuesta al método POST:

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
Utilice el identificador de esquema *5846ec826522050001db0e11* que se devuelve como respuesta al método POST para añadir el esquema de interfaz lógica a la interfaz lógica.

## Paso 9: Crear una interfaz lógica que haga referencia a un esquema de interfaz lógica
{: #step6}

Para crear una interfaz lógica, utilice la API siguiente:

```
POST /draft/logicalinterfaces
```

Opcionalmente, puede especificar un nombre de alias significativo para la interfaz lógica. Se puede hacer referencia al alias en la suscripción de llamada de API o de serie de tema que se utiliza para recuperar el estado de un dispositivo, en lugar de utilizar el identificador de interfaz lógica generado automáticamente.

**Nota:** El nombre de alias debe tener entre 1 y 36 caracteres de longitud y puede incluir caracteres alfanuméricos, de guión, de punto y de subrayado. El nombre de alias no puede ser una serie hexadecimal de 24 caracteres. 

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

En este caso de ejemplo, utilice el identificador de esquema *5846ec826522050001db0e11* que se ha devuelto en la respuesta anterior para añadir el esquema de interfaz lógica a la interfaz lógica.

En el siguiente ejemplo se muestra cómo utilizar cURL para crear una interfaz lógica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Thermometer Interface", "alias" : "IThermometer", "schemaId" : "5846ec826522050001db0e11"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

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
En este caso de ejemplo, utilice el identificador de interfaz lógica *5846ed076522050001db0e12* que se devuelve como respuesta al método POST para añadir la interfaz lógica al tipo de dispositivo. También utilizará este identificador para correlacionar un suceso de dispositivo de entrada a una propiedad definida por la interfaz lógica. Puede utilizar el alias de interfaz lógica *IThermometer* para [recuperar el estado del dispositivo](##step13), bien utilizando las API REST de HTTP, o bien mediante la suscripción a una serie de tema.

## Paso 10: Añadir la interfaz lógica a un tipo de dispositivo
{: #step10}

Para añadir una interfaz lógica a un tipo de dispositivo, utilice la API siguiente:

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

donde *typeId* es el nombre de tipo de dispositivo. 

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).  
**Nota:** En este caso de ejemplo, la misma interfaz lógica *5846ed076522050001db0e12* se asocia con *TSensor* y *TempSensor*.

En el ejemplo siguiente se muestra cómo utilizar cURL para añadir la interfaz lógica *5846ed076522050001db0e12* asociada con el identificador de esquema lógico *5846ec826522050001db0e11* al tipo de dispositivo *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

En el siguiente ejemplo se muestra cómo utilizar cURL para añadir la interfaz lógica *5846ed076522050001db0e12* que hace referencia al identificador de esquema de interfaz lógica *5846ec826522050001db0e11* al tipo de dispositivo *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

## Paso 11: Definir las correlaciones para correlacionar las propiedades del suceso de entrada con las propiedades de la interfaz lógica
{: #step11}  
**Importante:** Si está utilizando una aplicación de cliente MQTT y desea suscribirse para recibir notificaciones acerca de las actualizaciones de estado de dispositivo, debe configurar los valores de notificación cuando defina las correlaciones. Configurando su estrategia de notificaciones de este modo, puede reutilizar una interfaz lógica en varios tipos de dispositivo, con cada tipo de dispositivo configurado para recibir notificaciones de cambio de estado de la manera que elija. La estrategia de notificaciones de puede definir independientemente de si un cliente MQTT se suscribe a la serie de tema especificada.  
  
Se pueden configurar los siguientes valores de notificación:  

Parámetro	|	Descripción
------	|	-----
never	|	No se envían notificaciones (valor predeterminado)
on-every-event	|	Se envían notificaciones en cada suceso, independientemente de si cambia o no el estado del dispositivo
on-state-change	|	Se envían notificaciones solo cuando cambia el estado del dispositivo en un suceso  

**Nota:** Si selecciona un valor de notificación de *never*, su aplicación nunca recibe notificaciones acerca de un cambio en el estado del dispositivo. Por lo tanto, es posible que desee elegir una estrategia de notificación de *on-every-event* u *on-state-change*.  

Para correlacionar sucesos, utilice la siguiente API:

```
POST /draft/device/types/{typeId}/mappings
```

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

En este caso de ejemplo, definimos las correlaciones para el tipo de dispositivo *TSensor* para correlacionar la propiedad **t** en el suceso de entrada *tevt* con la propiedad **temperature** en la interfaz lógica. También definimos las correlaciones para el tipo de dispositivo *TempSensor* para correlacionar la propiedad **temp** en el suceso de entrada *tempevt* con la propiedad **temperature** en la interfaz lógica.  Los valores de notificación están definidos en "on-state-change". 

En el ejemplo siguiente se muestra cómo utilizar cURL para añadir una correlación al tipo de dispositivo *TSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Importante:** para cargas útiles estructuradas de sucesos [JSON ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://json-schema.org/){:new_window}, asegúrese que la entrada $event.*property* coincide correctamente con la estructura JSON de sus propiedades. Por ejemplo, si la carga útil es `{"d":{"t":<temp>}}` utilice `$event.d.t`

Especifique el identificador de interfaz lógica *5846ed076522050001db0e12* que se devuelve en la respuesta al método POST que se utiliza para crear la interfaz lógica y el tipo de dispositivo *TSensor*.

El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt" : {
      "temperature" : "$event.t"
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
En el ejemplo siguiente se muestra cómo utilizar cURL para añadir una correlación al tipo de dispositivo *TempSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Especifique el identificador de interfaz lógica *5846ed076522050001db0e12* que se devuelve en la respuesta al método POST que se utiliza para crear la interfaz lógica y el tipo de dispositivo *TempSensor*.
Se aplica una conversión para cambiar el valor de una medida en grados Fahrenheit a una medida en grados centígrados.


El ejemplo siguiente muestra una respuesta al método POST:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
     "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
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

## Paso 12: Validar y activar la configuración
{: #step15}

Valide y active la configuración relacionada con la actualización de estado de dispositivo para cada tipo de dispositivo. Esta configuración incluye los esquemas, los tipos de sucesos, las interfaces físicas, las interfaces lógicas y las correlaciones.

Para validar y activar la configuración de tipo de dispositivo, utilice la API siguiente:

```
PATCH /draft/device/types/{typeId}
```

donde *typeId* es el identificador de tipo de dispositivo.

Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

En este caso de ejemplo, es necesario validar y activar la configuración para dos tipos de dispositivos.

En el ejemplo siguiente se muestra cómo utilizar cURL para validar y activar la configuración para el tipo de dispositivo *TSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
El ejemplo siguiente muestra una respuesta al método PATCH:

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

En el ejemplo siguiente se muestra cómo utilizar cURL para validar y activar la configuración para el tipo de dispositivo *TempSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

El ejemplo siguiente muestra una respuesta al método PATCH:

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

## Paso 13: Publicar un suceso de dispositivo de entrada
{: #step12}

Publique un suceso de temperatura de *tSensor* en el tema `iot-2/evt/tevt/fmt/json` con la carga útil de ejemplo siguiente:
```
{
  "t" : 34.5
}
```
Publique un suceso de temperatura de *tempSensor* en el tema `iot-2/evt/tempevt/fmt/json` con la carga útil de ejemplo siguiente:
```
{
  "temp" : 72.55
}
```

Para obtener información sobre cómo publicar un suceso de entrada procedente de un dispositivo, consulte [Conectividad de MQTT para las aplicaciones](../applications/mqtt.html#publishing_device_events).


## Paso 14: Recuperar el estado del dispositivo
{: #step13}

Puede recuperar el estado del dispositivo utilizando las API REST de HTTP, o bien mediante la suscripción a una serie de tema. Si ha especificado un nombre de alias al crear la interfaz lógica, puede utilizar el nombre de alias en lugar del parámetro *logicalInterfaceId* para recuperar el estado del dispositivo. 

- Si tiene una aplicación cliente MQTT, puede suscribirse a la siguiente serie de tema:  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- Como alternativa, puede recuperar el último estado de dispositivo utilizando la siguiente API REST HTTP:  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

Los parámetros siguientes son obligatorios:  

Parámetro	|	Descripción
------	|	-----
typeId	|	El identificador de tipo de dispositivo.
deviceId	|	El identificador de dispositivo.
logicalInterfaceId o alias|	El identificador creado para la interfaz lógica o el nombre de alias especificado por el usuario. 


Para obtener información más detallada, consulte la documentación de [{{site.data.keyword.iot_short_notm}}API REST HTTP](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

En el ejemplo siguiente se muestra cómo utilizar cURL para recuperar el estado actual de *tSensor* haciendo referencia al identificador de la interfaz lógica que se ha creado:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/tSensor/devices/tSensor/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

Se utiliza el identificador de interfaz lógica *5846ed076522050001db0e12* en el método GET. Este identificador se ha devuelto como respuesta al método POST utilizado para crear la interfaz lógica.
El ejemplo siguiente muestra una respuesta al método GET:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
  }
}
```

En el ejemplo siguiente se muestra cómo utilizar cURL para recuperar el estado actual de *tempSensor* haciendo referencia al alias de la interfaz lógica que se ha creado:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices/tempSensor/state/IThermometer \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

Se utiliza el alias de interfaz lógica *IThermometer* en el método GET. Este identificador se ha devuelto como respuesta al método POST utilizado para crear la interfaz lógica.
El ejemplo siguiente muestra una respuesta al método GET:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
  }
}
```

Observe que la lectura de temperatura que se devuelve está en grados centígrados, no en grados Fahrenheit.

La aplicación puede procesar estos datos normalizados sin que haya que realizar ninguna configuración para comprender o convertir las distintas escalas de temperatura.

## Pasos siguientes

Es posible que desee compilar en este caso de ejemplo para utilizar la característica de duplicado de activo de gestión de datos configurando una instancia de cosa y un tipo de cosa. Para obtener información sobre cómo configurar cosas, consulte [Guía paso a paso 2: Un ejemplo detallado sobre cómo trabajar con las cosas a través de una interfaz común (Beta)](../information_management/im_index_scenario_thing.html).

También puede crear reglas que se pueden utilizar para desencadenar una acción cuando un suceso recibido por {{site.data.keyword.iot_short_notm}} causa un cambio en el estado del dispositivo o de la cosa. Para obtener información sobre la creación de reglas, consulte [Creación de reglas incorporadas (Beta)](../information_management/im_rules.html).

