---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# API de HTTP para dispositivos
{: #api}

Existen dos tipos de API HTTP que se pueden utilizar con {{site.data.keyword.iot_full}}. Puede utilizar las API REST HTTP para
crear, actualizar, listar y suprimir dispositivos. Utilice las API de mensajería HTTP para enviar información de sucesos de dispositivos a la nube y para aceptar
mandatos de las aplicaciones de la nube. 

## Conexiones de clientes
{: #client_connections}

Para obtener información sobre la seguridad del cliente y cómo conectar clientes a dispositivos en {{site.data.keyword.iot_short_notm}}, consulte [Conexión de aplicaciones, dispositivos y pasarelas a {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

**Nota:** El puerto HTTP 1883 está inhabilitado de forma predeterminada. Para obtener información sobre cómo cambiar el valor predeterminado, consulte [Configuración de políticas de seguridad](../reference/security/set_up_policies.html#set_up_policies.md).

## Autenticación

Todas las solicitudes de autenticación deben incluir una cabecera de autorización. La autenticación básica es el único método que se admite. Cuando un dispositivo realiza una solicitud HTTP a través de la API REST HTTP de {{site.data.keyword.iot_short_notm}}, serán necesarias las siguientes credenciales:

|Credencial|Entrada necesaria|
|:---|:---|
|Nombre de usuario|`use-token-auth`
|Contraseña| La señal de autenticación que se ha generado automáticamente o que se ha especificado manualmente al registrar el dispositivo.


## Cabeceras de solicitud Content-Type

Debe proporcionarse una cabecera de solicitud `Content-Type` con la solicitud. En la tabla siguiente se muestra cómo se correlacionan los tipos soportados con los formatos internos de {{site.data.keyword.iot_short_notm}}.

|Cabecera Content-Type|Formato en {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

## Calidad de servicio

De forma parecida a la calidad de servicio MQTT "at most once" del servicio de entrega de nivel 0, la mensajería REST HTTP proporciona entrega de mensajes no persistentes pero valida que la solicitud sea correcta y que se pueda entregar al servidor antes de que envíe la respuesta HTTP. Una respuesta que contiene un código de estado HTTP de 200 confirma que el mensaje se ha entregado al servidor. Al utilizar la calidad MQTT "at most once" del nivel de servicio o el equivalente HTTP para entregar mensajes de sucesos, el dispositivo o la aplicación debe implementar la lógica de reintento para garantizar la entrega.

Para obtener más información sobre el protocolo MQTT y la calidad de los niveles de servicio para {{site.data.keyword.iot_short_notm}}, consulte [Mensajería MQTT](../reference/mqtt/index.html).

Puede utilizar las API de mensajería HTTP para publicar sucesos desde dispositivos a la nube, y para recibir mandatos desde aplicaciones de la nube.

## Publicación de sucesos
{: #event_publication}

Además de utilizar el protocolo de mensajes MQTT, también puede configurar los dispositivos para publicar sucesos en el {{site.data.keyword.iot_short_notm}} a través de HTTP mediante los mandatos de la API de mensajería HTTP.

Utilice uno de los URL siguientes para enviar una solicitud ``POST`` desde un dispositivo conectado a {{site.data.keyword.iot_short_notm}}:

### Solicitud POST no segura para publicar sucesos

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Solicitud POST segura para publicar sucesos

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Nota:** el puerto 443, el puerto SSL predeterminado, también se puede especificar para llamadas de API HTTP seguras.

## Recepción de mandatos
{: #receive_commands}

Además de utilizar el protocolo de mensajería MQTT, es posible configurar los dispositivos para recibir mandatos desde {{site.data.keyword.iot_short_notm}} sobre HTTP utilizando mandatos de API de mensajería HTTP. Un dispositivo puede recibir mandatos dirigidos a sí mismo.

Utilice uno de los URL siguientes para enviar una solicitud ``POST`` desde un dispositivo conectado a {{site.data.keyword.iot_short_notm}}:

### Solicitud POST no segura para recibir mandatos

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Solicitud POST segura para recibir mandatos

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Nota:** el puerto 443, el puerto SSL predeterminado, también se puede especificar para llamadas de API HTTP seguras.

Opcionalmente se puede incluir el mandato *waitTimeSecs* en el cuerpo de la solicitud HTTP para especificar un entero que represente el número máximo de segundos por lo que se esperará un mandato:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Notas importantes:**
- El valor de *waitTimeSecs* debe ser un entero entre 0 y 3600 segundos. El valor predeterminado es 0.
- Para recibir cualquier mandato, utilice el carácter de comodín "cualquiera" (+) para el componente {command}. Si se utiliza el carácter de comodín, el identificador de mandato estará en el campo de cabecera de respuesta *X-commandId*.
- Si el código de estado de respuesta HTTP es 200, los datos del mandato se incluyen en el cuerpo de la respuesta. Revise el campo de cabecera de respuesta *Content-Type* para encontrar el tipo de contenido.
- Si el código de estado de respuesta HTTP es 204, quiere decir que no hay disponibles datos de mandato.

Para acceder a las API REST HTTP de {{site.data.keyword.iot_short_notm}}, consulte [API REST HTTP {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

Para acceder a las API de mensajería HTTP de {{site.data.keyword.iot_short_notm}}, consulte [API de mensajería HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

Para obtener información sobre el desarrollo de dispositivos, consulte [Desarrollo de dispositivos en {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).

## Ejemplo de una solicitud POST segura

En el ejemplo siguiente se envía un mensaje de un dispositivo a {{site.data.keyword.iot_short_notm}} utilizando el protocolo HTTP. Una aplicación recibe el mensaje suscribiéndose al tema en el que ha publicado el mensaje. La tabla siguiente proporciona información sobre el dispositivo. 

|Parámetro|Valor|
|:---|:---|
|ID de organización |myOrgID
|Tipo de dispositivo| TestDevices
|ID de dispositivo | TestPublishEvent
|Método de autenticación|use-token-auth
|Señal de autenticación|passw0rd


El ejemplo siguiente utiliza curl para enviar un suceso que se denomina *TestMessage* desde el dispositivo *TestPublishEvent* a {{site.data.keyword.iot_short_notm}} a través del protocolo HTTP: 

```
curl -v -X POST -H "Content-Type: application/json" -u "use-token-auth:passw0rd" -d @message.txt  https://myOrgID.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/TestDevices/devices/TestPublishEvent/events/TestMessage
```

El mensaje se encuentra en un archivo de texto que se denomina *message.txt*. El contenido del archivo *message.txt* se muestra en el ejemplo siguiente: 
```
{ "d": { "myName": "Publish Test","cputemp": 46,"sine": -10,"cpuload": 1.45 }  } 
```
El mensaje se publica en el tema *iot-2/evt/TestMessage/fmt/json*. Se crea una aplicación que se denomina *testSubscriber* que se suscribe a un filtro de tema *iot-2/type/+/id/+/evt/+/fmt/+*. El filtro de tema coincide con el tema en el que se publica el mensaje.

Puede utilizar la muestra de python *simpleApp.py* que se proporciona en la [biblioteca de python ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://github.com/ibm-watson-iot/iot-python/tree/master/samples/simpleApp) para ayudar a configurar la aplicación para suscribirse al filtro de tema *iot-2/type/+/id/+/evt/+/fmt/+*. 

El script de python siguiente se suscribe a un filtro de tema que coincide con la serie de tema en la que se publica el mensaje:

```
python simpleApp.py -o "myOrgID" -i "testSubscriber" -k "a-myOrgID-jg95dkpscn" -t "L(r4gfPa3bLP2kz4Nb"
```
donde:

- **-o** es el nombre de la organización.
- **-i** es el nombre de la aplicación.
- **-k** es la clave de API que se genera cuando se crea la aplicación.  
- **-t** es la señal de autenticación que se genera cuando se crea la aplicación.   


En el ejemplo siguiente se muestra una respuesta satisfactoria a la recepción del mensaje publicado del dispositivo *TestPublishEvent*. 
```
=============================================================================
Timestamp                        Device                        Event
2017-12-06 15:09:40,438   ibmiotf.application.Client  INFO    Connected successfully: a:myOrgID:testSubscriber
=============================================================================
2017-12-06T15:09:59.691290+00:00 TestDevices:TestPublishEvent  TestMessage: {"d": {"myName": "Publish Test", "cputemp": 46, "sine": -10, "cpuload": 1.45}}
2017-12-06T15:10:01.056000+00:00 TestDevices:TestPublishEvent  Connect 199.999.99.99
2017-12-06T15:10:01.446000+00:00 TestDevices:TestPublishEvent  Disconnect 199.999.99.99 (None)

```

## Memoria caché del último suceso
{: #last-event-cache}

Puede utilizar la última memoria caché de sucesos para almacenar información sobre el último suceso que se ha enviado a {{site.data.keyword.iot_short_notm}} por parte de un dispositivo conectado. Al utilizar una [API ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}, puede recuperar el último valor registrado de un id de suceso específico para un dispositivo o el último valor registrado para cada id de suceso notificado por un dispositivo específico. 

### Configuración de la última memoria caché de sucesos

La última característica de memoria caché de sucesos está inhabilitada de forma predeterminada, pero puede habilitar la funcionalidad de las formas siguientes: 

-	Establezca el parámetro *enabled* en **true** utilizando una API.
-	En la página *Valores* del panel de control de {{site.data.keyword.iot_short_notm}}, establezca **Habilitar LEC** en **On**.

El tiempo que los datos de suceso se almacenan en la memoria caché se especifica en el valor de tiempo de vida (TTL). Los últimos datos de suceso para cualquier suceso específico se almacenan durante siete días de forma predeterminada.  Puede cambiar la duración utilizando una API para actualizar el parámetro **ttlDays**, o seleccionando un valor para el campo **TTL de datos de suceso** en la página *Valores* del panel de control de {{site.data.keyword.iot_short_notm}}.

Para los planes Lite, puede almacenar información durante un mínimo de un día y un máximo de siete días. En el caso de otros planes, puede almacenar información durante un mínimo de un día y un máximo de 45 días.

Utilice la información siguiente para ayudarle a configurar los últimos valores de memoria caché de sucesos utilizando las API.

Para recuperar la configuración actual de la última memoria caché de sucesos para una organización, utilice la API siguiente:

```
    GET /api/v0002/config/lec
```   

   
En el ejemplo siguiente se muestra una respuesta satisfactoria al método GET:
```
{
    "enabled": false,
    "ttlDays": 7
}
```
Cualquier persona o aplicación de una organización tiene autorización para acceder a este punto final. 

Si la última memoria caché de sucesos está inhabilitada para una organización por el administrador de {{site.data.keyword.iot_short_notm}}, se devuelve el siguiente código de estado HTTP 403 FORBIDDEN en respuesta al método GET:

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: Last event cache is disabled for this organization. For more information, contact your support team."
}
```

Si la última memoria caché de sucesos no está habilitada, se devuelve un código de estado HTTP 404 en respuesta al método GET. Para habilitar la última característica de memoria caché de sucesos para una organización, utilice la API siguiente:

```
    PUT /api/v0002/config/lec
```

con la carga útil de ejemplo siguiente: 
```
{
    "enabled": true,
    "ttlDays": 5
}
```
donde: 

- **enabled** es necesario y especifica si la última memoria caché de sucesos está habilitada para una organización. El valor debe ser un valor booleano. 

- **ttlDays** es opcional y especifica el número de días que se retiene un suceso en la última memoria cache de sucesos para una organización. El valor debe ser un entero entre 1 y 45, inclusive.
   
 
Únicamente el administrador, los usuarios operador o las aplicaciones de operador están autorizados para acceder a este punto final. Si los parámetros o JSON no son válidos, se devuelve un código de estado HTTP 401 BAD REQUEST en respuesta al método PUT. 

**Nota:** Puede tardar hasta 30 segundos en que se complete un cambio de configuración.

### Recuperando información de la última memoria caché de sucesos

Al utilizar una API, puede recuperar el último suceso que ha enviado un dispositivo. Puede recuperar el estado del dispositivo independientemente de la ubicación física del dispositivo o del estado de uso. Puede recuperar el último valor grabado de un ID de suceso para un dispositivo específico, o el último valor grabado para cada ID de suceso del que ha informado un dispositivo específico. Los últimos datos de sucesos de un dispositivo se pueden recuperar para cualquier suceso específico que se haya hace 45 días como máximo.

Para solicitar el valor más reciente para un ID de suceso específico, utilice la siguiente solicitud de API, que devuelve el último valor grabado para el ID de suceso “power”.

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

La respuesta se devuelve en el siguiente formato JSON:

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**Nota:** Mientras la respuesta de la API está en formato JSON, las cargas útiles de sucesos se pueden grabar en cualquier formato. Las cargas útiles devueltas por la API de memoria caché de último suceso están codificadas en base64.

Para solicitar el valor más reciente para cada ID de suceso del que ha informado un dispositivo, utilice la siguiente solicitud API:

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

La respuesta incluye todos los ID de suceso enviados por el dispositivo. En el ejemplo siguiente, se devuelven valores para los sucesos “power” y “temperature”.

```
[
    {
        "deviceId": "<device-id>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

Para acceder a las API de última memoria caché de sucesos de {{site.data.keyword.iot_short_notm}}, consulte [API de gestión de información HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}.
