---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Supervisión de la conectividad de cliente (Beta)
{: #connect_status}

{{site.data.keyword.iot_full}} le permite consultar y supervisar el estado de conexión de los clientes. Puede saber en tiempo real si un cliente está conectado, y puede resolver problemas de conexión.
{:shortdesc}

Por ejemplo, puede ver cuándo un cliente ha estado activo por última vez y por qué se ha desconectado, o puede ver todos los clientes que no se conectaron en el último día para que pueda abordar cualquier problema.

**Importante:** La característica de conectividad de cliente de supervisión está disponible solo como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Consulta del estado de conexión de cliente
{: #query_status}

La API de estado de conexión de cliente le permite recuperar y consultar el estado de conexión de cualquier cliente que se haya conectado a {{site.data.keyword.iot_short_notm}}.

Cuando consulta el estado de conexión, puede ver la información siguiente:

 - el estado de conectividad (ya sea conectado o desconectado)
 - la hora de la última conexión, en formato ISO 8601
 - la hora de la última desconexión, en formato ISO 8601
 - el último suceso de actividad (por ejemplo, publicación de MQTT o suscripción de MQTT)
 - la hora de la última actividad, en formato ISO 8601
 - la entidad que ha causado la desconexión, si el cliente está desconectado (cliente, servidor o red)
 - el código de razón de MQTT para la desconexión, si el cliente está desconectado

**Ejemplos**

Para recuperar el estado de conexión de cliente para un único cliente:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

Para recuperar el estado de conexión de todos los clientes:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

Para recuperar todos los clientes conectados:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

Para recuperar todos los clientes que han estado activos en los dos últimos días:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**Nota:** Puede haber un breve retraso entre la hora en que se conecta un cliente y la hora a la que el estado de conexión de la API refleja el estado correcto.

Para obtener más información sobre la API, incluidos todos los filtros de consulta disponibles, consulte [el documento Client Connection State API Swagger ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}.

## Supervisión del estado de conexión de cliente
{: #monitor_status}

 Todos los clientes tienen un tema de supervisión que le permite suscribirse para recibir actualizaciones de estado de conectividad en directo para el cliente. Para obtener más información, consulte [Suscripción a mensajes de estado de dispositivos](../../applications/mqtt.html#subscribe_device_commands).

## Resolución de problemas de conectividad de cliente

La API de Registros de conexión le permite recuperar una lista de sucesos de registros de conexión para un dispositivo para diagnosticar problemas de conectividad. Las entradas registran las conexiones correctas, los intentos de conexión no satisfactorios, las desconexiones intencionales y las desconexiones iniciadas por el servidor.

Para obtener más información, consulte el [documento Connection Logs API Swagger ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}.
