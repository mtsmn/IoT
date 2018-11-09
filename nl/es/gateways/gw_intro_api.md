---

copyright:
 years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de mensajería HTTP para dispositivos de pasarela
{: #api}

## Acceso a la documentación de la API de mensajería HTTP para dispositivos de pasarela
{: #rest_messaging_api}

Para acceder a la documentación de la API de mensajería HTTP de {{site.data.keyword.iot_short_notm}} y obtener más información sobre cómo enviar sucesos desde dispositivos de pasarela, consulte [API de mensajería HTTP de {{site.data.keyword.iot_short_notm}}![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Conexiones de clientes
{: #client_connections}

Para obtener información sobre la seguridad del cliente y cómo conectar clientes a dispositivos de pasarela en {{site.data.keyword.iot_short_notm}}, consulte [Conexión de aplicaciones, dispositivos y pasarelas a {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).


## Publicación de sucesos
{: #event_publication}

Además de utilizar el protocolo de mensajería MQTT, también puede configurar los dispositivos de pasarela para publicar sucesos en el {{site.data.keyword.iot_short_notm}} a través de HTTP utilizando los mandatos de la API de mensajería HTTP.

Para enviar una solicitud ``POST`` desde un dispositivo conectado a {{site.data.keyword.iot_short_notm}}, utilice uno de los URL siguientes:

### Solicitud POST no segura para publicar sucesos

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Solicitud POST segura para publicar sucesos

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Notas importantes:**
- Solo puede enviar sucesos de dispositivo de pasarela mediante la mensajería HTTP. Utilice el protocolo de mensajería MQTT para enviar solicitudes para otras características de gestión de dispositivos de pasarela y de control.
- El puerto 443, el puerto SSL predeterminado, también se puede especificar para llamadas de API HTTP seguras.
- Si a una pasarela no se le ha asignado el rol *Pasarela estándar*, puede publicar sucesos en nombre de cualquiera de los dispositivos de la organización. Si el dispositivo que está conectado a la pasarela no está registrado, la pasarela automáticamente registra ese dispositivo.
- Asigne el rol *Pasarela estándar* si quiere comprobar los niveles de autorización del dispositivo.

Para obtener más información sobre el rol de las pasarelas y los grupos de recursos, consulte [Control de acceso de pasarela](../gateways/gateway-access-control.html).

### Autenticación

Todas las solicitudes deben incluir una cabecera de autorización. La autenticación básica es el único método que se admite. Cuando un dispositivo realiza una solicitud HTTP a través de la API REST HTTP de {{site.data.keyword.iot_short_notm}}, serán necesarias las siguientes credenciales:

|Credencial|Entrada necesaria|
|:---|:---|
|Nombre de usuario| `g/{orgId}/{gwType}/{gwDevId}` o `g-{orgId}-{gwType}-{gwDevId}`
|Contraseña| La señal de autenticación que se ha generado automáticamente o que se ha especificado manualmente al registrar el dispositivo de pasarela.

Donde:

<dl>
<dt>orgId</dt>  
<dd>El nombre de organización, que debe coincidir con el nombre que el especificado en la cabecera de host.</dd>

<p></p>
<dt>gwType</dt>  
<dd>El tipo de pasarela. </dd>
<dd>Si utiliza un guión "-" como delimitador en el nombre de usuario, este valor no debe incluir un guión. </dd>
<p></p>
<dt>gwDevId</dt>  
<dd>El identificador de dispositivo de la pasarela. </dd>
</dl>


### Cabeceras de solicitud Content-Type

Debe proporcionarse una cabecera de solicitud `Content-Type` con la solicitud si el contenido no es JSON. En la tabla siguiente se muestra cómo se correlacionan los tipos soportados con los formatos internos de {{site.data.keyword.iot_short_notm}}:

|Cabecera Content-Type|Formato en {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

### Calidad de servicio

De forma parecida a la calidad de servicio MQTT "at most once" del servicio de entrega de nivel 0, la mensajería REST HTTP proporciona entrega de mensajes no persistentes pero valida que la solicitud sea correcta y que se pueda entregar al servidor antes de que envíe la respuesta HTTP. Una respuesta que contiene un código de estado HTTP de 200 confirma que el mensaje se ha entregado al servidor. Al utilizar la calidad MQTT "at most once" del nivel de servicio o el equivalente HTTP para entregar mensajes de sucesos, el dispositivo o la aplicación debe implementar la lógica de reintento para garantizar la entrega.

Para obtener más información sobre el protocolo MQTT y la calidad de los niveles de servicio para {{site.data.keyword.iot_short_notm}}, consulte [Mensajería MQTT](../reference/mqtt/index.html).

Para obtener más información sobre la gestión de dispositivos de pasarela mediante API, consulte [API REST HTTP para dispositivos de pasarela](../gateways/gw_api.html).

## Recepción de mandatos
{: #receive_commands}

Además de utilizar el protocolo de mensajería MQTT, es posible configurar los dispositivos de pasarela para recibir mandatos desde {{site.data.keyword.iot_short_notm}} sobre HTTP utilizando mandatos de API de mensajería HTTP. Un dispositivo de pasarela puede recibir mandatos dirigidos a otros dispositivos dentro de su grupo de recursos asociado. Para obtener más información sobre los grupos de recursos de pasarela, consulte [Control de acceso de pasarela](../gateways/gateway-access-control.html).

Utilice uno de los URL siguientes para enviar una solicitud ``POST`` desde una pasarela conectada a {{site.data.keyword.iot_short_notm}}:

### Solicitud POST no segura para recibir mandatos

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Solicitud POST segura para recibir mandatos

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Nota:** el puerto 443, el puerto SSL predeterminado, también se puede especificar para llamadas de API HTTP seguras.

Opcionalmente se puede incluir el mandato *waitTimeSecs* en el cuerpo de la solicitud HTTP para especificar un entero que represente el número máximo de segundos por lo que se esperará un mandato:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Notas importantes:**
- El valor de *waitTimeSecs* debe ser un entero entre 0 y 3600 segundos. El valor predeterminado es 0.
- Para recibir mandatos de cualquier tipo de dispositivo, utilice el carácter "cualquiera" (+) para el componente `typeId`. Si se utiliza el carácter de comodín, el tipo de dispositivo estará en el campo de cabecera de respuesta *X-deviceType*.
- Para recibir mandatos de cualquier dispositivo, utilice el carácter "cualquiera" (+) para el componente `deviceId`. Si se utiliza el carácter de comodín, el identificador de dispositivo estará en el campo de cabecera de respuesta *X-deviceId*.
- Para recibir cualquier mandato, utilice el carácter de comodín "cualquiera" (+) para el componente `command`. Si se utiliza el carácter de comodín, el identificador de mandato estará en el campo de cabecera de respuesta *X-commandId*.
- Si el código de estado de respuesta HTTP es 200, los datos del mandato se incluyen en el cuerpo de la respuesta. Revise el campo de cabecera de respuesta *Content-Type* para encontrar el tipo de contenido.
- Si el código de estado de respuesta HTTP es 204, quiere decir que no hay disponibles datos de mandato.
