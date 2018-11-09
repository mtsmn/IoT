---

copyright:
years: 2018
lastupdated: "2018-06-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Gestión de grupos (Beta)
{: #groups_overview}

Los usuarios con el rol de administrador pueden utilizar grupos de {{site.data.keyword.iot_full}} para otorgar a los miembros y a las claves de API acceso a dispositivos específicos. Después de crear un grupo de añadir dispositivos al mismo, puede añadir miembros y claves de API al grupo y asignarles roles dentro del grupo. La combinación de roles y grupos determina los dispositivos a los que pueden acceder los usuarios y las claves de API y las acciones que pueden realizar sobre los dispositivos.
{: shortdesc}

Puede gestionar grupos mediante la interfaz de usuario del panel de control de {{site.data.keyword.iot_short_notm}} o mediante las API de control de acceso de {{site.data.keyword.iot_short_notm}}.

Para obtener detalles sobre cómo gestionar los roles, consulte [Gestión de roles de usuario](managing_user_roles.html#managing-user-roles).

**Importante:** la característica de grupos en la IU de {{site.data.keyword.iot_short_notm}} solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Para obtener más información sobre grupos y control de acceso y para ver instrucciones sobre cómo utilizar las API de control de acceso de {{site.data.keyword.iot_short_notm}} para gestionar grupos, consulte la sección [Visión general del control de acceso a nivel de recurso](reference/rlac_overview.html#RLAC_overview).

## Adición de grupos

**Nota:** para empezar a añadir grupos, active la característica **Grupos** en la página **Valores**. 

1. En el panel de control de {{site.data.keyword.iot_short_notm}}, seleccione **Gestión de acceso** en la barra de navegación izquierda.
2. Seleccione el separador **Grupos** y examine la lista de grupos.
3. Pulse **Añadir grupo**.
4. En la ventana **Añadir grupo**, escriba el nombre del grupo y, si lo desea, una descripción.
5. Pulse **Siguiente**.
6. Pulse **Añadir dispositivos al grupo**.
7. Seleccione los dispositivos que desea añadir al grupo y pulse **Terminado**.
8. Pulse **Finalizar** para crear el grupo.
Si lo desea, puede editar el grupo para añadir o eliminar dispositivo y también puede añadir más grupos.

