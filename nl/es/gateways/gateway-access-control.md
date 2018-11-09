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

# Control de acceso de pasarela
{: #gateway-access-control}

Los dispositivos de pasarela son una clase especializada de dispositivo y pueden actuar en nombre de otro dispositivo. Los grupos de recursos de pasarela definen los dispositivos de una organización en cuyo nombre puede actuar cada pasarela. Los roles de pasarela determinan la capacidad de la pasarela para registrar dispositivos en la plataforma Watson IoT Platform.
{: #shortdesc}

Los grupos de recursos se utilizan para permitir a las pasarelas actuar en nombre de un subconjunto de los dispositivos de una organización. Las pasarelas solo pueden publicar o suscribirse a mensajes para ellas mismas y en nombre de dispositivos que se encuentran en su grupo de recursos. 

Cuando se crea una nueva pasarela, también se crea un grupo de recursos predeterminado y se asigna a la pasarela. El grupo de recursos predeterminado no se puede eliminar de la pasarela. Las pasarelas existentes sin ningún grupo de recursos pueden continuar actuando en nombre de todos los dispositivos de la organización.

Para obtener información sobre la publicación de sucesos desde dispositivos de pasarela utilizando las API, consulte [API HTTP para dispositivos de pasarela](gw_api.html).

## Asignación de un rol a una pasarela
{: #gw_roles}

Para asignar una pasarela a un grupo de recursos, debe asignar en primer lugar la pasarela a un rol. Las pasarelas creadas recientemente tienen asignado el rol *Pasarela privilegiada* de forma predeterminada y se asignan a un grupo de recursos predeterminado. Las pasarelas con el rol privilegiado pueden registrar automáticamente dispositivos, que se añaden al grupo de recursos predeterminado.

Las pasarelas con el rol *Pasarela estándar* no pueden registrar automáticamente dispositivos. 

Una vez que se ha asignado a una pasarela un grupo de recursos, la pasarela solo puede actuar el nombre de los dispositivos de dicho grupo de recursos y en sí misma, incluso si su rol se cambia.

Para obtener más información sobre los roles de pasarela, consulte [Roles de pasarela](../roles_index.html#gateway_roles).

Para asignar un rol a una pasarela, utilice la siguiente API, donde *${clientID}* es el ID de cliente codificado como URL en formato *d:${orgId}:${typeId}:${deviceId}* para dispositivos o *g:${orgId}:${typeId}:${deviceId}* para pasarelas:

```
PUT /authorization/devices/${clientID}/roles

Request Body:
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ]
}

Request Response: 200
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ],
    "rolesToGroups": {
        "PD_STANDARD_GW_DEVICE": ["gw_def_res_grp:abcdef:gatewayTypeId:gatewayDeviceId"]
    }
}
```

Para ver información sobre el esquema de la solicitud, consulte la documentación de la API de [{{site.data.keyword.iot_full}} Limited Gateway ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_authorization_devices_deviceId_roles){: new_window}.

## Adición de dispositivos a un grupo de recursos y eliminación de dispositivos del mismo
{: #devices_groups}

Para que una pasarela con el rol *Pasarela estándar* pueda actuar en nombre de un dispositivo, el dispositivo debe ser miembro del grupo de recursos asignado a la pasarela. Para añadir muchos dispositivos a un grupo de recursos simultáneamente, utilice la siguiente API:

```
 PUT /bulk/devices/{groupId}/add
```

El grupo al que van a añadir dispositivos debe estar especificado en la vía de acceso de la solicitud, y los dispositivos que se van a añadir deben estar especificados en el cuerpo de la solicitud. Para obtener más información sobre el esquema de solicitud y las respuestas, consulte la documentación de la API de [{{site.data.keyword.iot_short_notm}} Limited Gateway ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_add){: new_window}.

Para eliminar varios dispositivos de un grupo de recursos, utilice la siguiente API:

```
PUT /bulk/devices/{groupId}/remove
```

Los dispositivos especificados en el cuerpo de la solicitud se eliminarán del grupo especificado en la vía de acceso de la solicitud. Para obtener más información sobre el esquema de solicitud y la respuesta, consulte la documentación de la API de [{{site.data.keyword.iot_short_notm}} Limited Gateway ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_remove){: new_window}.

## Búsqueda de un grupo de recursos
{: #finding_groups}

Los grupos de recursos pueden tener etiquetas de búsqueda asociadas. Las etiquetas de búsqueda se puede utilizar para recuperar los detalles de un grupo de recursos utilizando la siguiente API:

```
GET /groups
```

Esta API devuelve los grupos de recursos asociados con la etiqueta de búsqueda utilizada. Si no se especifica ninguna etiqueta de búsqueda, se devuelven todos los grupos de recursos. <!-- For more information about the request schema, response, and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

El identificador de un grupo de recurso asignado a una pasarela se puede encontrar utilizando la siguiente API, donde *${clientID}* es el ID de cliente codificado como URL en formato *d:${orgId}:${typeId}:${deviceId}* para dispositivos o *g:${orgId}:${typeId}:${deviceId}* para pasarelas:

```
GET /authorization/devices/${clientId}
```

Esta API devuelve el identificador exclusivo del grupo o grupos de recursos asignado a este dispositivo. Encontrará más información sobre esta API en la documentación de la API de [{{site.data.keyword.iot_short_notm}} Limited Gateway ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_authorization_devices_deviceId){: new_window}.


## Consulta de un grupo de recursos
{: #querying_groups}

Los grupos de recursos se pueden consultar con varios parámetros que devuelven todas las propiedades de todos los dispositivos del grupo, los identificadores exclusivos de todos los dispositivos del grupo o las propiedades del grupo de recursos.

Para que se devuelvan todas las propiedades de todos los dispositivos del grupo de recursos especificado, utilice la siguiente API:

```
GET /bulk/devices/{groupId}
```

Esta API devuelve la lista completa de propiedades de todos los miembros del grupo de recursos especificado. Para obtener más información sobre el esquema de solicitud, las respuestas y cómo pasar por las páginas de resultados, consulte la documentación de la API de [{{site.data.keyword.iot_short_notm}} Limited Gateway ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_bulk_devices_groupId){: new_window}.

Para que se devuelvan solo los identificadores exclusivos de los miembros del grupo de recursos, utilice la siguiente API:

```
GET /bulk/devices/{groupId}/ids
```

Esta API devuelve los identificadores exclusivos de todos los dispositivos que son miembros del grupo de recursos. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para que se devuelvan las propiedades del grupo de recursos, utilice la siguiente API:

```
GET /groups/{groupId}
```

Esta API devuelve las propiedades del grupo de recursos (nombre, descripción, etiquetas de búsqueda e identificador exclusivo) especificado en la vía de acceso, pero no devuelve la lista de miembros del grupo de recursos.
<!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Creación y supresión de un grupo de recursos.
{: #crt_del_groups}

Los grupos de recursos se pueden crear y suprimir de forma independiente de las pasarelas de conexión. Para crear un grupo de recursos, utilice la siguiente API:

```
POST /groups
```

Esta API crea un grupo de recursos y devuelve los detalles del grupo. <!-- For details on the request schema and the responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para suprimir un grupo de recursos, utilice la siguiente API:

```
DELETE /groups/{groupId}
```

Esta API suprime el grupo de recursos especificado. Los dispositivos que han sido miembros del grupo se eliminan del grupo, pero, por lo demás, los propios dispositivos no se ven afectados.<!-- For more information, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Actualización de las propiedades de un grupo
{: #update_groups}

  - /groups/{groupId}
    - PUT: actualiza las propiedades del grupo especificado.

## Recuperación y actualización de propiedades del dispositivo.
{: #fdevice_group_props}

Hay varias formas de recuperar propiedades de un dispositivo mediante API; cada API devuelve información diferente. Para recuperar las propiedades de todos los dispositivos existentes en su organización de {{site.data.keyword.iot_short_notm}}, utilice la siguiente API:

```
GET /authorization/devices

```

Esta API devuelve las propiedades de todos los dispositivos existentes de la organización, incluidas sus propiedades relevantes de control de acceso (rol, estado, fecha de caducidad).<!-- For more information on responses and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para recuperar las propiedades de dispositivo de un único dispositivo de la organización, utilice la siguiente API, donde *${clientID}* es el ID de cliente codificado como URL en formato *d:${orgId}:${typeId}:${deviceId}* para dispositivos o *g:${orgId}:${typeId}:${deviceId}* para pasarelas:

```
GET /authorization/devices/${clientId}
```

Esta API devuelve todas las propiedades del dispositivo especificado. <!-- For more information, see the [{{site.data.keyword.iot_short_notm}} device model documentation](LINK TO DEVICE MODEL) and [API documentation](LINK TO CORRECT API). -->

Para recuperar solo la información de control de acceso de un único dispositivo, utilice la siguiente API, donde *${clientID}* es el ID de cliente codificado como URL en formato *d:${orgId}:${typeId}:${deviceId}* para dispositivos o *g:${orgId}:${typeId}:${deviceId}* para pasarelas:

```
GET /authorization/devices/${clientId}/roles
```

Esta API recupera la información relevante de control de acceso del dispositivo especificado sin devolver otras propiedades del dispositivo.
<!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Las propiedades de un dispositivo se pueden actualizar de dos maneras. Las propiedades se pueden actualizar sin cambiar las propiedades de control de acceso o las propiedades de control de acceso se pueden actualizar directamente.

Para actualizar las propiedades de dispositivo sin afectar a las propiedades de control de acceso, utilice la siguiente API, donde *${clientID}* es el ID de cliente codificado como URL en formato *d:${orgId}:${typeId}:${deviceId}* para dispositivos o *g:${orgId}:${typeId}:${deviceId}* para pasarelas:

```
PUT /authorization/devices/${clientId}
```

Esta API solo actualiza las propiedades del dispositivo que no están asociadas al control de acceso.
<!-- For more information on request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para actualizar solo las propiedades de control de acceso del dispositivo especificado, utilice la siguiente API, donde *${clientID}* es el ID de cliente codificado como URL en formato *d:${orgId}:${typeId}:${deviceId}* para dispositivos o *g:${orgId}:${typeId}:${deviceId}* para pasarelas:

```
PUT /authorization/devices/${clientId}/withroles
```

Esta API solo actualizará las propiedades de control de acceso del dispositivo especificado. <!-- For more information on the request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->
