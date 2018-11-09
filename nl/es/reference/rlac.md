---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configuración del control de acceso a nivel de recursos
{: #configure_RLAC}

El control de acceso a nivel de recurso le permite controlar el acceso de usuario y de clave de API para gestionar dispositivos. Puede utilizar grupos de recursos para definir qué dispositivos en una organización puede gestionar cada usuario o clave de API. Los usuarios y claves de API pueden asignarse a un par "rol para grupos", que define que solo pueden realizar operaciones cubiertas por el rol especificado en los dispositivos de los grupos especificados. Para obtener más información sobre el control de acceso a nivel de recursos, consulte [Visión general del control de acceso a nivel de recurso](rlac_overview.html) y [Documentación de la API de control de acceso de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Configuración del control de acceso a nivel de recurso: Flujo de proceso
{: #RLAC_process}

El siguiente flujo de proceso se utiliza normalmente para habilitar y utilizar el control de acceso a nivel de recurso:
1. [Crear una organización](../iotplatform_overview.html#organizations).
2. [Crear usuarios](../add_users.html#adding-new-users) y [claves de API](../platform_authorization.html#api-key).
3. [Crear grupos de recursos](rlac.html#create_delete_group).
4. [Asignar correlaciones rol para grupos para los usuarios y claves de API](rlac.html#assign_roletogroup).
5. [Añadir dispositivos a los grupos de recursos](rlac.html#add_device).
6. [Habilitar el control de acceso a nivel de recurso](rlac.html#RLAC_enable).

El control de acceso a nivel de recurso viene impuesto en varias API relacionadas con dispositivo. Para obtener una lista de las API afectadas, consulte [API donde el control de acceso a nivel de recurso viene impuesto](rlac_overview.html#RLAC_enforced_APIs).

## Creación y supresión de grupos de recursos
{: #create_delete_group}

Los grupos de recursos se pueden crear y suprimir de forma independiente de las pasarelas de conexión.

Para crear un grupo de recursos y devolver los detalles del grupo, utilice la siguiente API:

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

Cuando suprime un grupo de recurso, los dispositivos que estaban en el grupo se eliminan del grupo, pero no se ven afectados de ningún otro modo. Para suprimir un grupo de recursos, utilice la siguiente API:

    DELETE /api/v0002/groups/{groupUid}


## Asignación de pares "rol para grupos" a usuarios o claves de API
{: #assign_roletogroup}

Asignar un par "rol para grupos" a un usuario o a una clave de API es necesario para restringir el usuario o la clave de API a un grupo de dispositivos. Los usuarios y claves de API sin ningún grupo de recurso pueden gestionar todos los dispositivos de su organización sin restricción. Una clave de API solo puede tener un rol si se utiliza un par "rol para grupos". El rol especificado en "rolesToGroups" debe coincidir con el rol especificado en "roles".

Para asignar un par "rol para grupos" a un usuario, utilice la siguiente API:

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



Para asignar un par "rol para grupos" a una clave de API, utilice la siguiente API:

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## Adición de dispositivos a grupos de recursos y eliminación de dispositivos del mismo
{: #add_device}

Antes de que un usuario o una clave de API con un par "rol para grupos" pueda gestionar un dispositivo, el dispositivo debe ser miembro del grupo de recursos que está asignado al usuario o clave de API. Cuando añade dispositivos a un grupo de recursos, el grupo al que está añadiendo dispositivos debe estar especificado en la vía de acceso de la solicitud y los dispositivos añadidos deben estar especificados en el cuerpo de la solicitud. Para añadir varios dispositivos a un grupo de recursos simultáneamente, utilice la siguiente API:

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Cuando elimina dispositivos de un grupo de recursos, los dispositivos especificados en el cuerpo de la solicitud se eliminan del grupo especificado en la vía de acceso de la solicitud. Para eliminar varios dispositivos de un grupo de recursos, utilice la siguiente API:

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Para obtener más información acerca del esquema y respuesta de solicitud, consulte [Documentación de la API de control de acceso de {{site.data.keyword.iot_short_notm}}![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Habilitar el control de acceso a nivel de recurso
{: #RLAC_enable}

Habilitar el control de acceso a nivel de recurso para una organización permitirá restringir a los usuarios y las claves API a sus grupos de recursos asignados. Para utilizar el control de acceso a nivel de recurso, debe habilitar un distintivo de configuración a nivel de organización utilizando la siguiente API:

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Búsqueda de grupos de recursos
{: #find_group}

Los grupos de recursos pueden tener etiquetas de búsqueda asociadas. Las etiquetas de búsqueda se puede utilizar para recuperar los detalles de un grupo de recursos utilizando la siguiente API:

    GET /api/v0002/groups

Esta API devuelve los grupos de recursos asociados con la etiqueta de búsqueda utilizada. Si no se especifica ninguna etiqueta de búsqueda, se devuelven todos los grupos de recursos. 

Para encontrar el ID exclusivo de los grupos de recursos asignados a un usuario, utilice la siguiente API:

    GET /api/v0002/authorization/users/{userUid}

Para encontrar el ID exclusivo de los grupos de recursos asignados a una clave de API, utilice la siguiente API:

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## Consulta de grupos de recursos
{: #query_group}

Los grupos de recursos se pueden consultar utilizando varios parámetros que devuelven todas las propiedades de todos los dispositivos del grupo, los identificadores exclusivos de todos los dispositivos del grupo o las propiedades del grupo de recursos.

Para que se devuelvan todas las propiedades de todos los dispositivos del grupo de recursos especificado, utilice la siguiente API:

    GET /api/v0002/bulk/devices/{groupUid}

Para que se devuelvan solo los identificadores exclusivos de los miembros del grupo de recursos, utilice la siguiente API:

    GET /api/v0002/bulk/devices/{groupUid}/ids

Para devolver las propiedades del grupo de recursos, incluyendo el nombre, la descripción, las etiquetas de búsqueda y el identificador exclusivo especificados en la vía de acceso, utilice la siguiente API:

    GET /api/v0002/groups/{groupUid}

Esta API no devuelve la lista de miembros del grupo de recursos.

## Actualización de las propiedades de un grupo
{: #update_group}

Para actualizar las propiedades de un grupo, utilice la siguiente API:

    PUT /api/v0002/groups/{groupId}
