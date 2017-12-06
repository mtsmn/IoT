---

copyright:
 years: 2015, 2017
lastupdated: "2017-10-04"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de mensajería HTTP para dispositivos
{: #api}


## Acceso a la documentación de la API de mensajería HTTP para dispositivos
{: #rest_messaging_api}

Para acceder a la documentación de la API de mensajería HTTP de {{site.data.keyword.iot_short_notm}}, consulte [API de mensajería HTTP de {{site.data.keyword.iot_short_notm}}![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Conexiones de clientes
{: #client_connections}

Para obtener información sobre la seguridad del cliente y cómo conectar clientes a dispositivos en {{site.data.keyword.iot_short_notm}}, consulte [Conexión de aplicaciones, dispositivos y pasarelas a {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

## Publicación de sucesos
{: #event_publication}

Además de utilizar el protocolo de mensajería MQTT, también puede configurar los dispositivos para publicar sucesos en el {{site.data.keyword.iot_short_notm}} a través de HTTP utilizando mandatos de la API REST HTTP.

Utilice uno de los URL siguientes para enviar una solicitud ``POST`` desde un dispositivo conectado a {{site.data.keyword.iot_short_notm}}:

### Solicitud POST no segura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Solicitud POST segura

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Nota:** el puerto 443, el puerto SSL predeterminado, también se puede especificar para llamadas de API HTTP seguras.

### Autenticación

Todas las solicitudes deben incluir una cabecera de autorización. La autenticación básica es el único método que se admite. Cuando un dispositivo realiza una solicitud HTTP a través de la API REST HTTP de {{site.data.keyword.iot_short_notm}}, serán necesarias las siguientes credenciales:

|Credencial|Entrada necesaria|
|:---|:---|
|Nombre de usuario|`use-token-auth`
|Contraseña| La señal de autenticación que se ha generado automáticamente o que se ha especificado manualmente al registrar el dispositivo.


### Cabeceras de solicitud Content-Type

Debe proporcionarse una cabecera de solicitud `Content-Type` con la solicitud si el contenido no es JSON. En la tabla siguiente se muestra cómo se correlacionan los tipos soportados con los formatos internos de {{site.data.keyword.iot_short_notm}}.

|Cabecera Content-Type|Formato en {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"


## Recepción de mandatos
{: #receive_commands}

Además de utilizar el protocolo de mensajería MQTT, es posible configurar los dispositivos para recibir mandatos desde {{site.data.keyword.iot_short_notm}} sobre HTTP utilizando mandatos de API de mensajería HTTP. Un dispositivo puede recibir mandatos dirigidos a sí mismo.

Utilice uno de los URL siguientes para enviar una solicitud ``POST`` desde un dispositivo conectado a {{site.data.keyword.iot_short_notm}}:

### Solicitud POST no segura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Solicitud POST segura

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Nota:** el puerto 443, el puerto SSL predeterminado, también se puede especificar para llamadas de API HTTP seguras.

Opcionalmente se puede incluir el mandato *waitTimeSecs* en el cuerpo de la solicitud HTTP para especificar un entero que represente el número máximo de segundos por lo que se esperará un mandato:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Notas importantes:**
- El valor de *waitTimeSecs* debe ser un entero entre 0 y 3600 segundos. El valor predeterminado es 0.
- Para recibir cualquier mandato, utilice el carácter de comodín "cualquiera" (+) para el componente {command}. Si se utiliza el carácter de comodín, el identificador de mandato estará en el campo de cabecera de respuesta *X-commandId*.
- Si el código de estado de respuesta HTTP es 200, los datos del mandato se incluyen en el cuerpo de la respuesta. Revise el campo de cabecera de respuesta *Content-Type* para encontrar el tipo de contenido.
- Si el código de estado de respuesta HTTP es 204, quiere decir que no hay disponibles datos de mandato.


Para obtener información sobre el desarrollo de dispositivos, consulte [Desarrollo de dispositivos en {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).
