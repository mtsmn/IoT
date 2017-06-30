---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-15"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Configuración de certificados
{: #set_up_certificates}

Los certificados se utilizan para la autenticación de dispositivos o para sustituir el certificado de servidor de {{site.data.keyword.iot_full}} predeterminado para la mensajería MQTT. Se deniega el acceso y no puede comunicar con el servidor cualquier dispositivo que no tenga certificados firmados válidos.

Para configurar certificados y el acceso al servidor de los dispositivos, el operador del sistema registra los certificados de la entidad emisora de certificados (CA) asociada y, opcionalmente, registra los certificados del servidor de mensajes en la plataforma {{site.data.keyword.iot_short_notm}}.

Para obtener información sobre la utilización de API para gestionar certificados de servidor de mensajería y certificados de CA, consulte las [API de autorización y autenticación de IBM Watson IoT Platform ![Icono de enlace externo](../../../../icons/launch-glyph.svg)"Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}.

## Certificados de CA
Los certificados de CA permiten a la organización reconocer los certificados de clientes en dispositivos como fiables de modo que los dispositivos puedan conectarse al servidor.

Si añade un certificado de CA o sustituye el certificado del servidor de mensajería, todos los dispositivos se deben conectar utilizando un cliente MQTT que admita la indicación de nombre de servidor (SNI). 

Puede establecer el nivel de seguridad de la conexión configurando la política de seguridad de conexión. Para obtener más información sobre cómo hacer esto, consulte [Configuración de políticas de seguridad](set_up_policies.html).

## Certificados de cliente

Los certificados de cliente (dispositivo o pasarela) permanecen en los dispositivos y no se cargan en la plataforma. El certificado firmado por la CA que se utiliza para firmar todos los certificados de dispositivo y pasarela es el único certificado que se carga en la plataforma. Si utiliza certificados de servidor de mensajería autofirmados, debe cargar los certificados raíz e intermedios que se utilizan para firmar el certificado cliente (cert.pem). 

El certificado de dispositivo individual o de pasarela que firma con el certificado de CA debe tener el ID de dispositivo especificado como valor de Nombre común (CN) o de SubjectAltName en el certificado. 

Para los dispositivos, el formato del campo **CN** es `CN=d:devtype:devid` y formato del campo **SubjectAltName** es `SubjectAltName=email:d:*devtype:devid*` donde `email:d` es constante y `*devtype*` es el tipo de dispositivo y `*devid*` es el ID de dispositivo en la plataforma. 

Para pasarelas, el formato del campo **CN** es `CN=g:typeId:deviceId`y el formato del campo **SubjectAltName** es SubjectAltName=email:g:*devtype:devid*

Nota: No incluya el `orgId` en los campos **CN** o **SubjectAltName** para certificados de dispositivo o de pasarela. `orgId` se debería proporcionar como parte de la información SNI que la implementación de cliente proporciona al conectarse al servidor de mensajería. 

Para obtener más información sobre los certificados de cliente, consulte la [receta para conectar Raspberry Pi a IBM Watson IoT Platform utilizando certificados del lado del cliente ![Icono de enlace externo](../../../../icons/launch-glyph.svg) "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}

## Gestión de certificados de mensajería

Los certificados de servidor de mensajería aceptan el dominio predeterminado, internetofthings.ibmcloud.com. El certificado de CN o SubjectAltName deben seguir el siguiente formato: 

- `orgId.messaging.internetofthings.ibmcloud.com` (dominio IoTP)

En el siguiente ejemplo se muestra un CN válido para el certificado de servidor: 

`mtxpd0.messaging.internetofthings.ibmcloud.com`

Para obtener más información sobre los certificados de servidor de mensajería, consulte la [receta para conectar Connect Raspberry Pi a IBM Watson IoT Platform utilizando certificados de servidor autofirmados ![Icono de enlace externo](../../../../icons/launch-glyph.svg) "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}

### Dominios personalizados (Beta)
{: #custom_domains}

**Importante**: La característica de dominios personalizados para certificados de servidor de mensajería únicamente está disponible como parte de un programa Beta limitado. Si desea habilitar los dominios personalizados, active **Características experimentales** en la página **Valores**. 

Como parte de la característica Beta, el certificado de servidor de mensajería acepta dominios personalizados. El certificado de CN o SubjectAltName deben seguir el siguiente formato: 

- `orgId.messaging<dominio_personalizado>`

El campo **CN** acepta caracteres comodín, tal como se muestra en el ejemplo siguiente: 

- `CN=*.messaging.fab-iot.com`

**Importante**: Para dominios personalizados, se necesita un servicio DNS externo para resolver el dominio personalizado para el servidor de mensajería {{site.data.keyword.iot_full}}. Este servicio de DNS no lo proporciona la plataforma. 

## Registro de certificados de la entidad emisora de certificados (CA) para la autenticación de dispositivos
{: #reg_ca_cert}

1. Inicie una sesión en {{site.data.keyword.iot_short_notm}} y vaya a **Valores generales**.
2. En la sección **Seguridad**, bajo **Certificados CA**, pulse **Añadir certificado**.
3. Navegue hasta el archivo de certificado que desea cargar o arrástrelo en la ventana **Añadir certificado**.El archivo sólo puede contener un certificado y las fechas del certificado deben ser válidas. Solo se aceptan certificados en formato .pem o .der. Puede obtener una vista previa de la información del certificado del archivo seleccionado.
4. Especifique una descripción para el archivo de certificado.
5. Confirme que se ha seleccionado el archivo correcto y pulse **Guardar**. El certificado seleccionado aparece en la tabla y está activo de forma predeterminada.

## Registro de certificados del servidor de mensajería
{: #reg_msg_cert}

Con la plataforma se proporciona un certificado de servidor predeterminado. Puede utilizar el certificado predeterminado o cargar uno de la organización. Si no tiene todavía un certificado que utilizar, puede crear una solicitud de un nuevo certificado. Cuando reciba el nuevo certificado, debe firmarlo y volverlo a cargar en la plataforma.

**Nota:** es posible que las páginas del panel de control de la plataforma establezcan conexiones internas con el servidor de mensajería para recuperar información del dispositivo. Cuando se configuran certificados de servidor autofirmados para una organización, los usuarios del panel de control deben añadir el certificado de servidor como certificado de confianza en sus navegadores para evitar problemas de conexión ya que los navegadores, de forma predeterminada, no reconocerán el servidor de mensajería como un servidor de confianza.

## Carga de un certificado de servidor de mensajería de la organización
{: #upload_cert}
1. En la sección **Seguridad** de la **Configuración general**, bajo **Certificados del servidor de mensajería** pulse **Añadir certificado**.
2. Navegue hasta el archivo de certificado que desea cargar o arrástrelo en la ventana **Añadir certificado**.
3. Navegue hasta el archivo de claves privadas que desea cargar o arrástrelo en la ventana **Añadir certificado**.
4. Escriba la frase de contraseña de la clave privada si esta se ha cifrado con una frase de contraseña.
5. Confirme que se ha seleccionado el archivo correcto y pulse **Guardar**.

## Activación de un certificado de servidor de mensajería

Para activar el certificado predeterminado u otro certificado que ya se haya cargado, seleccione el certificado que desea utilizar en la lista desplegable **Certificado predeterminado del servidor de mensajería** bajo **Certificados del servidor de mensajería**. El certificado seleccionado aparece en la tabla como certificado activo.

## Solicitud de un nuevo certificado de servidor de mensajería
{: #request_cert}

Si desea utilizar un nuevo certificado de servidor de mensajería, puede generar una solicitud de firma de certificado (CSR) para solicitar un nuevo certificado.

1. En la sección **Seguridad** de la **Configuración general**, bajo **Certificados del servidor de mensajería** pulse **Generar CSR**.
2. Especifique los detalles para realizar una solicitud firmada de certificado (CSR) para el servidor y pulse **Generar**. El CSR se visualiza en la tabla.
3. Descargue la solicitud y envíela a una autoridad de certificación para que la firme.
4. Después de obtener un certificado, vuelva a la entrada CSR en la tabla y cargue el nuevo certificado. Después de cargar el certificado, la CSR de la tabla se sustituye por el certificado cargado.
