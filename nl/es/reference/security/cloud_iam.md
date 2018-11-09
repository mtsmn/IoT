---

copyright:
  years: 2018
lastupdated: "2018-07-16"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Autenticación y autorización de Cloud IAM para {{site.data.keyword.iot_short_notm}} (Beta)
{: #cloud_iam}

Las API de {{site.data.keyword.iot_full}} soportan la autenticación y la autorización de los usuarios mediante Gestión de accesos e identidades (IAM).

**Importante:** La característica Cloud IAM Authentication and Authorization for {{site.data.keyword.iot_short_notm}} solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Cloud IAM se crea en IBM Cloud y se utiliza para autenticar y autorizar a los usuarios administrativos y de desarrollador que necesitan configurar y gestionar sus servicios de IBM. Los usuarios que necesitan acceso a la IU de Watson IoT Platform se autentican con IBM Cloud IAM. El origen de identidad para Cloud IAM pueden ser usuarios de ID de IBM registrados o puede ser el servicio de directorio de un cliente que da soporte a SAML.  

{{site.data.keyword.iot_short_notm}} también da soporte a la autenticación de usuarios mediante el ID de app. El ID de app se utiliza para autenticar a los usuarios que necesitan acceso a las aplicaciones alojadas en IBM Cloud. Los usuarios de ID de app normalmente no realizan actividades administrativas o de desarrollo en un servicio de nube. Para obtener más información, consulte [Autenticación de ID de app para la plataforma IoT de Watson](app_id.html#app_id).

IAM permite a los usuarios obtener una señal de OAuth de IAM y utilizarla para autenticar las llamadas de API. Por ejemplo, un usuario con la CLI de {{site.data.keyword.containershort_notm}} instalada puede utilizar el mandato siguiente:

`bx iam oauth-tokens`

El mandato devuelve la señal IAM del usuario que ha iniciado la sesión en el formato siguiente:

`IAM Token: Bearer <some token>`

Un usuario también puede utilizar el punto final de señal IAM para iniciar la sesión y recuperar su señal IAM, tal como se muestra en el ejemplo siguiente:
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

Este ejemplo de API utiliza una clave de API IAM para solicitar la señal IAM, y devuelve el `Bearer <token>`.

A continuación, cuando un usuario llama a una API de {{site.data.keyword.iot_short_notm}}, puede proporcionar la cabecera de autorización `Authorization: Bearer <token>` en sus llamadas de API.

## Generación de una señal de IAM
{: #iam_generate}

Puede elegir cómo automatizar la creación de la señal IAM, en función de cómo se autentique con IBM Cloud y el tipo de ID de IBM Cloud que utilice.

Si utiliza un ID no federado, puede elegir una de las opciones siguientes para crear una señal IAM:
 - **Nombre de usuario y contraseña de IBM Cloud:** Puede crear una señal y automatizar completamente la creación de la señal de acceso de IAM.
 - **Generar una clave de API de IBM Cloud:** Utilice las claves de API de IBM Cloud que dependen de la cuenta de IBM Cloud para la que se generan. No puede combinar la clave de API de IBM Cloud con un ID de cuenta distinto en la misma señal de IAM. Para acceder a los clústeres creados con una cuenta distinta de la que se basa en la clave de API de IBM Cloud, debe iniciar la sesión en la cuenta para generar una nueva clave de API.

Si utiliza un ID federado, puede elegir una de las opciones siguientes para crear una señal IAM:
 - **Generar una clave de API de IBM Cloud:** Las claves de API de IBM Cloud dependen de la cuenta de IBM Cloud para la que se generan. No puede combinar la clave de API de IBM Cloud con un ID de cuenta distinto en la misma señal de IAM. Para acceder a los clústeres creados con una cuenta distinta de la que se basa en la clave de API de IBM Cloud, debe iniciar la sesión en la cuenta para generar una nueva clave de API.
 - **Utilizar un código de acceso puntual:** Si se autentica con IBM Cloud utilizando un código de acceso puntual, no puede automatizar completamente la creación de la señal IAM porque la recuperación de un código de acceso puntual requiere una interacción manual con el navegador web. Para automatizar completamente la creación de la señal IAM, debe crear una clave de API de IBM Cloud en su lugar.

Cuando se crea la señal de acceso, la información del cuerpo que se incluye en la solicitud varía en función del método de autenticación de IBM Cloud que utiliza. Sustituya los valores, tal como se indica a continuación:
- `<my_username>` = Su nombre de usuario de IBM Cloud.
- `<my_password>` = Su contraseña de IBM Cloud.
- `<my_api_key>` = Su clave de API de IBM Cloud.
- `<my_passcode>` = Su código de acceso único de IBM Cloud. Ejecute `bx login --sso` y siga las instrucciones de la salida de la CLI para recuperar el código de acceso puntual mediante su navegador web.

Ejemplo:
`POST https://iam.<region>.bluemix.net/oidc/token`

Para especificar una región de IBM Cloud, revise las abreviaturas de la región como se utilizan en los puntos finales de la API. Consulte [Puntos finales de API de la región de IBM Cloud [Icono de enlace externo](../../icons/launch-glyph.svg)(https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions)] para obtener más información.

Parámetro de entrada	 | Valores
---------------- | -----------
Cabecera	| Content-Type:application/x-www-form-urlencoded<br>Authorization: Basic Yng6Yng=<br>Nota: Se le proporciona Yng6Yng=, la autorización codificada con URL para el nombre de usuario bx y la contraseña bx.
Cuerpo para el nombre de usuario y la contraseña de IBM Cloud	|	grant_type: password<br>response_type: cloud_iam, uaa<br>nombre de usuario: `<my_username>`<br>contraseña: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Nota: Añada la clave uaa_client_secret sin ningún valor especificado.
Cuerpo para las claves de API de IBM Cloud	|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Nota: Añada la clave uaa_client_secret sin ningún valor especificado.
Cuerpo para el código de acceso de un solo uso de IBM Cloud	|	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Nota: Añada la clave uaa_client_secret sin ningún valor especificado.

Ejemplo de salida de API:

```
{
"access_token": "<iam_token>",
"refresh_token": "<iam_refresh_token>",
"uaa_token": "<uaa_token>",
"uaa_refresh_token": "<uaa_refresh_token>",
"token_type": "Bearer",
"expires_in": 3600,
"expiration": 1493747503
}
```
Encontrará la señal de IAM en el campo **access_token** de la salida de API.

En el ejemplo siguiente se muestra cómo utilizar la señal IAM al llamar a las API.

```
GET https://org.domain/api/v0002/bulk/devices
```

Parámetros de entrada  |	Valores
----------------- | -----------
Cabeceras	|	Content-Type: application/json<br>Authorization: bearer `<iam_token>`
