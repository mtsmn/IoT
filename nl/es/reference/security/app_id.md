---

copyright:
years: 2018
lastupdated: "2018-08-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Autenticación de ID de app para Watson IoT Platform (Beta)
{: #app_id}

El ID de app se utiliza para autenticar a los usuarios que necesitan acceso a las aplicaciones alojadas en IBM Cloud. Estos usuarios acceden al servicio mediante aplicaciones móviles o web proporcionadas por el servicio.
{: shortdesc}

**Importante:** La característica Autenticación y autorización de ID de app para {{site.data.keyword.iot_short_notm}} solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} también da soporte a la autenticación de usuarios a través de Cloud IAM. Cloud IAM se crea en IBM Cloud y se utiliza para autenticar y autorizar a los usuarios administrativos y de desarrollador que necesitan configurar y gestionar sus servicios de IBM. Para obtener más información sobre Cloud IAM, consulte [Cloud IAM Authentication and Authorization for Watson IoT Platform](cloud_iam.html#cloud_iam).

Los usuarios de ID de app normalmente no realizan actividades administrativas o de desarrollo en un servicio de nube, y no pueden iniciar sesión en el panel de control web de {{site.data.keyword.iot_short_notm}}. Únicamente los usuarios de Cloud IAM pueden iniciar sesión en el panel de control.

Las API de {{site.data.keyword.iot_short_notm}} dan soporte a la autenticación de usuarios del servicio de ID de app de IBM Cloud.  Puede configurar su organización de {{site.data.keyword.iot_short_notm}} para que utilice una instancia el servicio de ID de app y, a continuación, añada los usuarios de ID de app a la organización. La aplicación autentica a estos usuarios con el ID de app que se puede utilizar para invocar las API de {{site.data.keyword.iot_short_notm}}. Para obtener más información sobre el servicio del ID de app de IBM Cloud, consulte [Guía de aprendizaje de iniciación del ID de app de IBM Cloud ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}.

El soporte de ID de app actual es para las API REST de no mensajería, incluidas las API para configurar y gestionar dispositivos, usuarios y otras prestaciones de {{site.data.keyword.iot_short_notm}}. El ID de app no se puede utilizar para publicar ni suscribirse a los mensajes.

Normalmente, se utiliza el ID de app para autenticar las API REST que se invocan en nombre de un usuario no administrativo. Un desarrollador puede crear una aplicación que utiliza una señal de ID de app para invocar una API REST en nombre de un usuario registrado, como por ejemplo un técnico de mantenimiento, que no tiene acceso administrativo. La ventaja de utilizar el ID de app para autenticar usuarios no administrativos en lugar de utilizar una sola señal de autenticación es que el seguimiento de auditoría en {{site.data.keyword.iot_short_notm}} muestra el usuario que ha invocado las API REST en lugar de una señal de autenticación compartida. El ID de app también es útil para autenticar a los que no son administradores que necesitan utilizar la interfaz de línea de mandatos (CLI) de {{site.data.keyword.iot_short_notm}}.

**Importante:** La invocación de las API REST de {{site.data.keyword.iot_short_notm}} se debe realizar utilizando una aplicación de usuario y no directamente desde un navegador o apps móviles. La aplicación de usuario puede proporcionar un nivel adicional de control de acceso para determinar las funciones de dispositivo a las que se permite acceder a un usuario determinado, así como la validación de los mensajes MQTT que se envían a dispositivos. En la imagen siguiente se muestra este flujo de invocación:

![Invocación de API REST mediante una aplicación de usuario](images/app_id_user_app.PNG)

Los usuarios de ID de app deben estar registrados en el {{site.data.keyword.iot_short_notm}} utilizando el mismo proceso que utilizan los usuarios de Cloud IAM, y ambos aparecen en la interfaz de panel de control.

Dado que los usuarios de ID de app no suelen ser administradores, es importante tener en cuenta a qué recursos y mandatos deberían tener acceso y, a continuación, establecer los valores de control de acceso a nivel de recurso de {{site.data.keyword.iot_short_notm}} de forma adecuada. En la mayoría de los casos, a los usuarios de ID de app se les asigna un rol personalizado que es más restrictivo que los roles predeterminados definidos de una organización de {{site.data.keyword.iot_short_notm}}. Para obtener más información sobre los roles, consulte [Roles de usuario, aplicación y pasarela](../../roles_index.html#user-application-and-gateway-roles).

De forma predeterminada, los usuarios de ID de app pueden acceder a todos los dispositivos. Si un usuario de ID de app solo debe poder gestionar una lista restringida de dispositivos, asigne a los grupos de recursos los usuarios del ID de app.

**Nota:** {{site.data.keyword.iot_short_notm}} limita el número total de usuarios registrados a 1.000.

## Configuración de un servicio de ID de app
{: #set_up_app_id}

Antes de configurar un servicio de ID de app, debe añadir una instancia del servicio de ID de app de IBM Cloud y configurar proveedores de identidad. Si todavía no tiene una instancia de ID de app, puede suministrar una desde el [Catálogo de servicios de IBM Cloud ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog){: new_window}.

Una vez que haya configurado el servicio de ID de app, debe obtener las credenciales de servicio para configurar el ID de app como proveedor para la organización de {{site.data.keyword.iot_short_notm}}. Puede obtener o crear credenciales de servicio en la IU de servicio del ID de app.

1. En el panel de control de IBM Cloud, seleccione el servicio de ID de app y pulse **Credenciales de servicio**.
2. Pulse **Ver credenciales** para ubicar `tenantId`, `clientId` y `secret` en las credenciales. Estos valores son necesarios para configurar el ID de app en pasos posteriores. El emisor será el nombre de host de `oauthServerUrl`, por ejemplo, `appid-oauth.ng.bluemix.net`.

En la imagen siguiente se muestra un ejemplo de cómo recuperar las credenciales:

![Recuperación de credenciales de ID de app](images/app_id_credentials.PNG "Ejemplo de recuperación de credenciales de ID de app")


## Configuración del ID de app en {{site.data.keyword.iot_short_notm}}
{: #config_app_id}

Debe configurar el ID de app antes de poder utilizar la señal de ID de app para la autorización. Para crear la configuración del ID de app, utilice la API de `POST /api/v0002/authentication/providers`, donde `tenant_Id` e `issuer` se obtienen desde el paso 2 en [Configuración de un servicio de ID de app](#set_up_app_id):

Cuerpo de la solicitud:

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

Respuesta de solicitud: 200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## Adición de usuarios a {{site.data.keyword.iot_short_notm}}
{: #users_app_id}

Debe añadir usuarios y otorgarles autorizaciones basándose en sus roles. Para crear usuarios, utilice la API de `POST /api/v0002/authorization/users`, con los detalles siguientes:

 - `issuer` se obtiene del paso 2 en [Configuración de un servicio de ID de app](#set_up_app_id).
 - `uniqueSecurityName` se llena con los detalles de `appIdConfigName`, `amr` y el correo electrónico del usuario, que se configuran en los proveedores de identidad del servicio de ID de app.
 - `appIdConfigName` es el nombre que se utiliza en [Configuración de ID de app](##config_app_id), por ejemplo “TestAppIdConfigName”.
 - `amr` es la referencia del método de autenticación, que es el proveedor de identidades que se utiliza en el servicio de ID de app. El ID de app da soporte a los siguientes Proveedores de identidad:

	 - Cloud Directory
	 - SAML 2.0 Federation
	 - Facebook
	 - Google

	 Basándose en el proveedor de identidades que se utiliza en el servicio de ID de app, `amr` sería `cloud_directory`, `saml`, `facebook` o `google`.

En el ejemplo siguiente, se está utilizando Cloud Directory:

Cuerpo de la solicitud:

```
{
    "uniqueSecurityName": "TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
    "PD_ADMIN_USER"
  ]
}
```

Respuesta de solicitud: 200

```
{
    "uniqueSecurityName": “TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
        "PD_ADMIN_USER"
    ]
}
```

## Generación de una señal de ID de app
{: #token_app_id}

Una vez que haya configurado el ID de app y que haya añadido usuarios, su aplicación puede empezar a generar señales de ID de app para los usuarios, y utilizarlos para invocar API de plataforma. Para obtener más información sobre cómo empezar con el ID de app, consulte el vídeo siguiente: [ID de app de IBM Bluemix ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}.

Un usuario puede utilizar el punto final de ID de app para iniciar la sesión y recuperar su señal de ID de app publicando los ejemplos siguientes.

### Ejemplo de HTTP de generación de una señal de ID de app

Para generar la señal de ID de app, utilice la API siguiente autenticándose en IBM Cloud con las credenciales de servicio:

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

Donde `tenantId`, `issuer`, `clientId` y `secret` se obtienen del paso 2 en la [Configuración de un servicio de ID de app](#set_up_app_id).

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

Cuerpo de solicitud para grant_type=password:

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

Cuerpo de solicitud para grant_type= authorization_code:

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

Respuesta de solicitud:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Ejemplo de Curl de generación de una señal de ID de app

El fragmento de código siguiente muestra un ejemplo de CURL de generación de una señal de ID de app:

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

Donde `username` y `password` se definen para el usuario en el ID de app.

Respuesta de solicitud:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Ejemplo de Swagger de generación de una señal de ID de app

Para utilizar Swagger para generar una señal de ID de app, utilice el [ejemplo de Swagger ![Icono de enlace externo](../../../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}.

Complete todos los detalles de los parámetros de Swagger y pulse **Pruébelo** al final del método `/oauth/v3/{tenantId}/token`.

La respuesta de punto final de señal contiene las señales de acceso e ID, la señal de renovación opcional y la información de caducidad.

## Utilización de la señal de ID de app
{: #use_app_id}

Una vez que se ha creado la señal de ID de app, la aplicación puede empezar a utilizarla para llamar a las API de la plataforma. La señal de ID de app es la propiedad `id_token` de la respuesta donde se ha generado en [Generación y señal de ID de app](#token_app_id). Cuando caduca la señal de ID de app, la aplicación debe generar una señal nueva para seguir llamando a las API de plataforma.

En los ejemplos siguientes se muestra cómo utilizar la señal de ID de app al llamar a las API.

###	Ejemplo de HTTP de la utilización de una señal de ID de app

`GET https://org.domain/api/v0002/bulk/devices`

Parámetros de entrada  |	Valores
----------------- | -----------
Cabeceras	|	Content-Type: application/json<br>Authorization: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### Ejemplo de Curl de utilización de una señal de ID de app

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
