---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-18"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Visión general del control de acceso a nivel de recurso
{: #RLAC_overview}

El control de acceso a nivel de recurso le permite controlar el acceso de usuario y de clave de API para gestionar dispositivos. Puede utilizar grupos de recursos para especificar los dispositivos en una organización que puede gestionar cada usuario o clave de API. Para obtener información acerca de cómo configurar el control de acceso a nivel de recurso, consulte [Configuración del control de acceso a nivel de recurso](rlac.html#configure_RLAC).

## Conceptos del control de acceso a nivel de recurso 
{: #RLAC_concepts}

Los grupos de dispositivos son parte de los controles de acceso a nivel de recurso para {{site.data.keyword.iot_short_notm}}. Puede utilizar la característica de grupos de dispositivos para gestionar el número de dispositivos al que puede acceder el personal según su rol. Antes de implementar los grupos de dispositivos, determine y registre la intención y el ámbito de los grupos de dispositivos para aumentar la eficiencia de su solución. 

Puede implementar grupos de dispositivos como parte de su solución de IoT para permitir que un usuario o aplicación acceda a un subconjunto de dispositivos en lugar de a todos los dispositivos asociados con la organización. Para aumentar la eficiencia de los grupos de dispositivos, utilícelos cuando una persona deba tener acceso a muchos dispositivos.

Por ejemplo, una empresa que emplea a un número de ingenieros de campo y personal de operaciones desea implementar una solución de IoT en Reino Unido. La empresa configura un grupo de dispositivos para cada una de las 69 ciudades, 9 grupos de dispositivos para cada región geográfica y un grupo de dispositivos para todo el Reino Unido. Los dispositivos se asignan a estos grupos y los grupos se asignan a los 15 ingenieros de campo y personal de operaciones, algunos de los cuales tienen acceso a más de un grupo. Los usuarios que son responsables de solo uno o dos dispositivos pueden seguir utilizando las aplicaciones móvil y las arquitecturas de empresa externas a {{site.data.keyword.iot_short_notm}} pero que son complementarias a la solución de IoT.

Las respuestas a las preguntas sobre grupos de dispositivos que pueden surgir se pueden encontrar en [Questions in Internet of Things space ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window}.

## Límites de grupos de recursos para el control de acceso

Los límites de grupos de recursos se imponen para asegurarse de que la característica de control de acceso a nivel de recurso funciona de forma efectiva. Los límites restringen el número de grupos de recursos asignados a un sujeto, el número de dispositivos dentro de los grupos y el número de grupos a los que puede pertenecer un recurso.

Los siguientes parámetros vienen impuestos:

 - Se pueden asignar un máximo de 10 grupos de recursos a una clave de API, usuario o pasarela.
 - Un grupo de recursos puede tener un máximo de 300 recursos.
 - Un recurso puede pertenecer a un máximo de 10 grupos.

## Terminología del control de acceso a nivel de recurso

Puede utilizar las siguientes secciones para ayudarle a entender la terminología utilizada en la característica de control de acceso a nivel de recurso. 

### Sujetos
{: #subjects}

Un sujeto es una entidad autenticada que solicita acceso a la plataforma, que debe estar autorizada. Son válidos los siguientes tipos de sujeto:

- *Usuarios*: Usuarios finales que son las personas que utilizan las aplicaciones. Un miembro es un usuario cuya cuenta no tiene fecha de caducidad y un invitado es un usuario cuya cuenta tiene fecha de caducidad.
- *Dispositivos*: Dispositivos físicos que acceden a la plataforma de {{site.data.keyword.iot_short_notm}} y utilizan el tipo Dispositivo.
- *Pasarelas*: Dispositivos físicos que acceden a la plataforma de {{site.data.keyword.iot_short_notm}} y utilizan el tipo Pasarelas.
- *Aplicaciones*: Aplicaciones o servicios que acceden a la plataforma de {{site.data.keyword.iot_short_notm}} utilizando claves de API para autorización y control de acceso.

### Recursos
{: #resources}

Un recurso es una entidad donde el sujeto realiza la acción. El sujeto solicita acceso a la plataforma autorizado para el recurso. 

### Acciones
{: #actions}

La acción es lo que el sujeto realiza, que comúnmente es crear, leer, actualizar, suprimir o ejecutar. Las operaciones se utilizan para agrupar acciones en recursos.

### Aplicaciones
{: #applications}

Una aplicación puede actuar en nombre de un sujeto desconocido para la plataforma, como una aplicación móvil que controla un aparato doméstico, y una aplicación puede actuar en nombre de un dispositivo real o simulado que puede ser conocido o desconocido para la plataforma. Una aplicación también puede ser una aplicación del lado del servidor verdadera que no esté actuando en nombre de otro tipo de dispositivo.

La autenticación y el acceso a la plataforma de {{site.data.keyword.iot_short_notm}} se proporcionan en función de la clave de API o señal.

### Operaciones
{: #operations}

Una operación contiene una o varias acciones que se ejecutan en cierto tipos de recursos mediante interfaces de usuario e interfaces programáticas, como los servicios REST y las interfaces MQTT. Las utilizan diferentes sujetos incluyendo miembros, aplicaciones y dispositivos. Las operaciones se identifican por su ID de operación (OID).

### Roles
{: #roles}

Los roles son conjuntos de operaciones que se utilizan para proporcionar permisos para utilizar las operaciones. Un sujeto puede tener varios roles y el rol es un atributo de un sujeto.

**Formato de roles**

Matriz de 0..n roles

    { "roles": [ "PD_ADMIN_APP" ] }

**Formato de roles para grupos**

Correlación de 0..n pares de <serie, lista<serie>>

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### Permisos
{: #permissions}

Los permisos permiten a un sujeto ejecutar operaciones en un recurso o grupo de recursos.

### Grupos de recursos
{: #resource_groups}

Un grupo de recursos es una colección de dispositivos y recursos. Los dispositivos se pueden añadir a grupos de recursos y los grupos de recursos se pueden asignar a sujetos en pares de roles para grupos. Esta relación permite al sujeto realizar operaciones para un rol dado en un grupo de dispositivos específico.

### Usuarios
{: #users}

Los usuarios inician sesión en el panel de control de la plataforma de {{site.data.keyword.iot_short_notm}} utilizando un identificador que es un miembro de una organización. Los usuarios son roles asignados que les garantizan la autorización para llamar a ciertos conjuntos de API de HTTP. Para obtener más información acerca de los roles de usuario, consulte [Niveles de acceso para roles de usuario](roles_access.html#levels-of-access-for-user-roles).

### Claves de API
{: #API_keys}

Las claves de API son señales que se utilizan para llamar a la API de HTTP de la plataforma de {{site.data.keyword.iot_short_notm}}. Las claves de API son roles asignados que les garantizan la autorización para llamar a ciertos conjuntos de API de HTTP. Para obtener más información acerca de los roles de clave de API, consulte [Niveles de acceso para roles de aplicación](app_roles_access.html#levels-of-access-for-application-roles).

## API donde el control de acceso a nivel de recurso viene impuesto
{: #RLAC_enforced_APIs}
Cuando el control de acceso a nivel de recurso está habilitado, las API relacionadas con el dispositivo incluidas en esta sección solo funcionan si el dispositivo especificado está accesible para el interlocutor.

### API de dispositivo (individual)

**Enumerar dispositivos de tipo**

    GET /api/v0002/device/types/${typeId}

Solo devuelve el subconjunto de dispositivos que pertenece a los grupos adecuados.

**Obtener dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Obtener detalles de gestión de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Obtener dispositivos registrados por una pasarela**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

Devuelve un error 404 si la pasarela no está accesible para el interlocutor. Solo devuelve el subconjunto de dispositivos que pertenece a los grupos adecuados. Los dispositivos y las pasarelas deben estar en el grupo del interlocutor.

**Actualizar dispositivo**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Suprimir dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

### API de dispositivo (masivo)

**Enumerar dispositivos**

    GET /api/v0002/bulk/devices

Solo devuelve el subconjunto de dispositivos que pertenece a los grupos adecuados.

**Suprimir dispositivos**

    DELETE /api/v0002/bulk/devices/remove

Solo suprime el subconjunto de dispositivos que pertenece a los grupos adecuados. Los dispositivos que no están accesibles se siguen devolviendo con success = true.

**Actualizar dispositivos:**

    PUT /api/v0002/bulk/devices/update

### API de gestión de dispositivo

**Iniciar solicitud**

    POST /api/v0002/mgmt/requests

Falla si algún dispositivo no está accesible para el interlocutor.

**Suprimir solicitud**

    DELETE /api/v0002/mgmt/requests/${requestId}

Falla si algún dispositivo no está accesible para el interlocutor.

**Ver solicitud**

    GET /api/v0002/mgmt/requests/${requestId}

**Enumerar solicitudes**

    GET /api/v0002/mgmt/requests

**Obtener una solicitud**

    GET /api/v0002/mgmt/requests/${requestId}

**Obtener detalles de la solicitud de dispositivo**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**Obtener detalles de la solicitud de dispositivo para un dispositivo**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### API de determinación de problemas

**Registros de conexión de un dispositivo**

    GET /api/v0002/logs/connection

Devuelve una matriz vacía si el dispositivo no está accesible.

**Añadir código de error de dispositivo**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Enumerar códigos de error de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Borrar códigos de error de dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Añadir registro de diagnóstico de dispositivo**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Enumerar registro de diagnóstico de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Obtener un registro de diagnóstico de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Borrar registro de diagnóstico de dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Suprimir un registro de diagnóstico de dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

###API de última memoria caché de sucesos

**Obtener sucesos para el dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Obtener sucesos para el dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

Devuelve un error 404 si el dispositivo no está accesible para el interlocutor.

**Obtener interfaz lógica para el dispositivo**

    GET /api/v0002/device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}

Devuelve un error 404 si el dispositivo no está en el grupo del interlocutor.
