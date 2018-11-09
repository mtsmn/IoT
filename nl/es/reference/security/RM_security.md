---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-03"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Gestión de riesgos y de seguridad
{: #RM_security}

Puede mejorar la seguridad para habilitar la creación, imposición y notificación de la seguridad de la conexión de dispositivos. Con esta seguridad mejorada, se utilizan certificados y autenticación TLS (seguridad de capa de transporte), además de los ID de usuario y las señales que utiliza {{site.data.keyword.iot_short_notm}} para determinar cómo y cuándo se conectan los dispositivos a la plataforma.

## Listas de revocación de certificados y certificados de cliente
{: #CRLs}

Las listas de revocación de certificados (CRL) están soportadas para las configuraciones que utilizan certificados de cliente para autenticar los dispositivos. La ubicación que se especifica como punto de distribución de CRL debe dar soporte a acceso HTTP o HTTPS, y el sitio debe ser accesible. La conexión de cliente se rechaza si no se puede alcanzar el punto de distribución de la CRL, o si la CRL no es válida o no se encuentra en el punto de distribución. 
 

Los certificados de cliente deben estar firmados por certificados de la entidad emisora de certificados (CA) que tengan "Firma de certificado" especificada en la sección "Uso de clave X509v3" en "Extensiones X509v3". Para utilizar las CRL para revocar los certificados de cliente, el certificado de CA que firma un certificado de cliente también debe incluir "Registro de CRL" en la sección "Uso de clave X509v3" en "Extensiones X509v3". Esta configuración permite que el certificado de CA emita y firme actualizaciones de CRL cuando sea necesario. Únicamente se utiliza la CRL que se especifica en el punto de distribución de certificados de cliente para determinar si se ha revocado un certificado.

Asegúrese de probar los certificados y las CRL, en particular si genera sus propios certificados y CRL en lugar de utilizar los certificados de una entidad emisora de certificados reconocida.


## Configuración de certificados
{: #certificates}

Si los certificados están habilitados, durante la comunicación entre los dispositivos y el servidor, a cualquier dispositivo que no tenga certificados válidos configurados en los valores de seguridad, se le deniega el acceso, aunque utilice ID de usuario y contraseñas válidos.

Para configurar certificados y el acceso al servidor de los dispositivos, el operador del sistema registra los certificados de la entidad emisora de certificados (CA) asociada y, opcionalmente, registra los certificados del servidor de mensajes en Watson IoT Platform.
Para configurar certificados los certificados del cliente y el acceso al servidor de los dispositivos, el operador del sistema importa los certificados de la entidad emisora de certificados (CA) asociada y los certificados del servidor de mensajería en {{site.data.keyword.iot_short_notm}}. Luego el analista de seguridad configura las políticas de seguridad de conexión predeterminadas entre los dispositivos y la plataforma. El analista puede añadir distintas políticas para los distintos tipos de dispositivos.

Para obtener información sobre cómo configurar certificados, consulte [Configuración de certificados](set_up_certificates.html).

## Valores de seguridad
{: #connect_policy}

Los valores de seguridad, incluidos los de utilización de valores de políticas de conexión, certificados de CA y certificados de servidor de mensajería, regulan la forma en que los dispositivos se conectan a la plataforma. Puede configurar políticas de conexión predeterminadas para todos los tipos de dispositivo y crear valores personalizados para tipos de dispositivo específicos. La política se puede definir de modo que permita conexiones sin cifrar, que imponga solo conexiones de seguridad de la capa de transporte (sólo TLS) y que habilite los dispositivos para que se autentiquen con certificados del lado del cliente.

Para obtener información sobre cómo configurar las políticas de seguridad de conexión, consulte [Configuración de políticas de seguridad](set_up_policies.html).

Las políticas de lista negra y lista blanca proporcionan la posibilidad de controlar las ubicaciones desde las que los dispositivos pueden conectar con la cuenta de la organización. Una lista negra identifica todas las direcciones IP, CIDR o países a los que se denegará el acceso al servidor. Una lista blanca proporciona un acceso explícito a direcciones IP específicas.

Para obtener información sobre cómo configurar políticas de lista negra y lista blanca, consulte [Configuración de listas negras y listas blancas](set_up_policies.html#config_black_white).

## Planes de organización y políticas de seguridad
Las políticas de seguridad reforzadas permiten a las organizaciones determinar cómo quieren conectar los dispositivos y la autenticación en la plataforma, utilizando políticas de conexión y políticas de listas negras y listas blancas. Las opciones de las políticas de seguridad disponibles para una organización dependen del tipo de plan de la organización:

**Plan estándar:**
- Los operadores del sistema pueden configurar las políticas de conexión con las siguientes opciones:
    - TLS opcional
    - TLS con autenticación de señal
    - TLS con autenticación de señal y de certificado de cliente

**Plan de seguridad avanzada (ASP) o plan Lite:**
- Los operadores del sistema pueden configurar las políticas de conexión con las siguientes opciones:
    - TLS opcional
    - TLS con autenticación de señal
    - TLS con autenticación de certificado de cliente
    - TLS con autenticación de señal y de certificado de cliente
    - TLS con señal o certificado de cliente
- Los operadores del sistema pueden configura las listas negras o las listas blancas

## Creación de informes y panel de control de Gestión de riesgos y de seguridad
{: #dashboard}

El operador del sistema y el analista de seguridad pueden utilizar el panel de control de Gestión de riesgos y de seguridad para ver el estado general de la seguridad. Las tarjetas en el panel de control ofrecen una visión general completa y el estado de conexión de los dispositivos.

Hay disponibles las siguientes tarjetas de panel de control para analizar el riesgo y la conformidad:
 - **Conformidad de seguridad de conexión**- Muestra el nivel de conformidad de los dispositivos conectados al servidor.
 - **Conformidad de lista blanca/lista negra** - Muestra el nivel de conformidad de los dispositivos en función de la política de lista blanca o lista negra activa.
 - **Conformidad de política general** (Beta) - Muestra el nivel general de conformidad en función de todas las políticas activas.
 - **Incumplimientos de política** (Beta) - Muestra los incumplimientos generales de cada política.

**Importante**: Las tarjetas **Cumplimiento de política general** e **Incumplimientos de política** únicamente están disponibles como parte de un programa Beta limitado. Si desea habilitar estas tarjetas de panel de control, active **Características experimentales** en la página **Valores**.

### Creación de informes de política profundizables (Beta)
{: #drill_down}

**Importante**: La característica de creación de informes profundizables para las políticas de Gestión de riesgos únicamente está disponible como parte de un programa Beta limitado. Si desea habilitar la creación de informes profundizables, active **Características experimentales** en la página **Valores**.

Como parte de la característica Beta, el operador del sistema puede profundizar en cada informe para ver detalles específicos de los dispositivos que cumplen o incumplen las políticas.

Es posible acceder a los informes profundizables desde tarjetas de panel de control, desde la página **Políticas** donde se muestra una lista de todas las políticas de seguridad y desde la página de detalles de cada política. Profundice hasta en tres niveles de detalles en los informes:
1. En el primer nivel se encuentra una lista de todas las políticas contenidas en el tipo de política seleccionada, por ejemplo, la política de conexión predeterminada y cualquier política de conexión personalizada.
2. El segundo nivel muestra los dispositivos que incumplen la política y la razón por la que la incumplen.
3. El tercer nivel muestra los detalles específicos de un dispositivo individual que está incumpliendo la política.
