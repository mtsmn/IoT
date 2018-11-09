---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configuración de {{site.data.keyword.iot_short_notm}} Edge (vista previa)
{: #edge_configure}

**Importante:** La característica {{site.data.keyword.iot_full}} Edge solo está disponible como parte de un programa de vista previa limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Antes de empezar a recibir datos de dispositivos que están conectados al nodo Edge, debe conectar la pasarela a {{site.data.keyword.iot_short_notm}}. La conexión de una pasarela a {{site.data.keyword.iot_short_notm}} implica crear un tipo de dispositivo de pasarela y registrar la pasarela con {{site.data.keyword.iot_short_notm}}. A continuación, puede utilizar la información de registro para conectar el dispositivo de pasarela a {{site.data.keyword.iot_short_notm}}.

## Flujo de configuración de alto nivel

1. [Cree una organización](#edge_org) dentro del entorno de prueba de vista previa de {{site.data.keyword.iot_short_notm}} Edge.
2. [Habilite la vista previa de {{site.data.keyword.iot_short_notm}}](#edge_enable).
3. Configure la organización para que utilice nodos de pasarela Edge con {{site.data.keyword.iot_short_notm}} [creando un tipo de pasarela con Edge habilitado](#edge_gw_type), [añadiendo dispositivos de pasarela](#edge_gw) y seleccionando los servicios Edge que se van a ejecutar en los nodos Edge. Puede [crear servicios personalizados](WIoTP_edge_dev.html#create_service) si es necesario.
4. [Configure los servicios Edge](#config_service).
5. [Instale Edge en los dispositivos](#edge_install_device).
6. [Gestione y supervise los servicios Edge](#monitor_service).

## Crear una organización
{: #edge_org}

Cree una organización dentro del entorno de prueba de vista previa de {{site.data.keyword.iot_short_notm}} Edge completando los pasos siguientes:
1. Regístrese para una cuenta de {{site.data.keyword.Bluemix_notm}} y cree una instancia del servicio de {{site.data.keyword.iot_short_notm}} en la organización de {{site.data.keyword.Bluemix_notm}}. Puede crear una instancia de {{site.data.keyword.iot_short_notm}} directamente desde la página [{{site.data.keyword.iot_short_notm}} del Catálogo de servicios de IBM Cloud ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}.  
2. Complete la información para suministrar la organización de IBM Cloud. **Importante:** Debe desplegar la organización de IBM Cloud en la ubicación **EE.UU. sur** para utilizar la vista previa de {{site.data.keyword.iot_short_notm}} Edge.
3. Pulse **Crear**.
4. En la instancia de {{site.data.keyword.iot_short_notm}} que se abre, pulse **Iniciar**.
Se crea una nueva organización de {{site.data.keyword.iot_short_notm}}. El ID de organización se muestra al principio del URL y en el panel de control bajo el nombre de usuario. Ahora puede habilitar {{site.data.keyword.iot_short_notm}} Edge para la organización.

## Habilitar la vista previa de {{site.data.keyword.iot_short_notm}} Edge
{: #edge_enable}

La vista previa de {{site.data.keyword.iot_short_notm}} Edge está desactivada de forma predeterminada. Para habilitar {{site.data.keyword.iot_short_notm}} Edge:

1. En el panel de control de {{site.data.keyword.iot_short_notm}} de la organización, seleccione **Valores**.
2. En la sección **Características experimentales**, active las características experimentales, incluida la vista previa de {{site.data.keyword.iot_short_notm}} Edge.
3. Renueve el navegador para ver la nueva característica **Servicios Edge** y el acceso directo en el menú de navegación del panel de control. **Nota:** Si el icono **Servicios Edge** no aparece en el menú de navegación del panel de control, asegúrese de que ha seleccionado **EE.UU. sur** para su ubicación.

El distintivo se añade al almacenamiento del navegador local.

## Crear un tipo de pasarela con las prestaciones de Edge habilitadas
{: #edge_gw_type}

Cada pasarela conectada a {{site.data.keyword.iot_short_notm}} debe estar asociada a un tipo de pasarela. Los tipos de pasarela son grupos de dispositivos que comparten características comunes.

1. En el menú de navegación principal del panel de control de {{site.data.keyword.iot_short_notm}}, seleccione **Dispositivos**.
2. En el separador **Tipos de dispositivo**, pulse **Añadir tipo de dispositivo**.
3. En el separador **Identidad** de la página **Añadir tipo**, seleccione el tipo **Pasarela** y especifique un nombre de tipo de pasarela como `my_gateway_type` y una descripción para el tipo de pasarela.
**Importante:** El nombre de tipo de pasarela no debe tener más de 36 caracteres y solo puede contener:
<ul>
 <li>Caracteres alfanuméricos (a-z, A-Z, 0-9)</li>
 <li>Guiones (-)</li>
 <li>Signos de subrayado (&lowbar;)</li>
 <li>Puntos (.)</li>
 </ul>
3. Opcional: Especifique los atributos y metadatos de tipo de pasarela.
4. Encienda el botón **Prestaciones de Edge**.
5. Seleccione el tipo de arquitectura para el tipo de pasarela. El tipo de arquitectura debe coincidir con la arquitectura del dispositivo. Por ejemplo, un dispositivo Odroid se ejecuta en la arquitectura ARM64.
6. Pulse **Siguiente**.
7. Opcional: En el separador **Información de dispositivo**, especifique detalles adicionales y metadatos relacionados con el tipo de dispositivo de pasarela.
8. Seleccione los servicios Edge que se van a agregar a los dispositivos Edge. El servicio Core IoT se añade a todos los dispositivos, pero puede añadir otros servicios del catálogo completando los pasos siguientes:
 1. Pulse **Añadir servicios**.
 2. En la ventana **Añadir servicios Edge**, seleccione la versión adecuada de los servicios que desea añadir y, a continuación, pulse **Terminado**.
9. En la ventana **Añadir tipo de pasarela**, pulse **Terminado**.
Puede proceder a [añadir dispositivos de pasarela a este tipo de pasarela](#edge_gw).

## Añadir dispositivos de pasarela
{: #edge_gw}

1. En el menú de navegación principal del panel de control de {{site.data.keyword.iot_short_notm}}, seleccione **Dispositivos**.
2. Pulse **Añadir dispositivo**.
3. En el separador **Identidad**, seleccione el tipo de dispositivo de pasarela al que está añadiendo dispositivos y especifique un ID exclusivo para el dispositivo de pasarela que está añadiendo.
El ID de dispositivo se utiliza para identificar el dispositivo de pasarela en el panel de instrumentos de {{site.data.keyword.iot_short_notm}} y también es un parámetro obligatorio para conectar el dispositivo de pasarela a {{site.data.keyword.iot_short_notm}}.
**Importante:** el ID de dispositivo no debe tener más de 36 caracteres y sólo puede contener:
 <ul>
 <li>Caracteres alfanuméricos (a-z, A-Z, 0-9)</li>
 <li>Guiones (-)</li>
 <li>Signos de subrayado (&lowbar;)</li>
 <li>Puntos (.)</li>
 </ul>
 **Consejo:** para los dispositivos conectados a la red, el ID de dispositivo podría ser, por ejemplo, la dirección MAC del dispositivo sin dos puntos de separación.
4. Pulse **Siguiente**.
5. Opcional: En el separador **Información de dispositivo**, especifique detalles adicionales y metadatos relacionados con el dispositivo de pasarela.
6. Pulse **Siguiente**.
7. En el separador **Seguridad** de la página **Añadir dispositivo**, genere automáticamente una señal de autenticación o proporcione su propia señal de autenticación para este dispositivo. Si decide crear su propia señal, asegúrese de que se encuentre entre los 8 y 36 caracteres, que contenga una mezcla de letras en mayúscula y minúscula, números, y guión, subrayado o punto. La señal no debe contener secuencias de caracteres repetidos, palabras de diccionario, nombres de usuario ni otras secuencias predefinidas.
9. Pulse **Siguiente**.
10. Pulse **Listo**.
11. En la página de información de dispositivos, copie y guarde la información de dispositivos siguiente:
 - ID de organización, como por ejemplo `tubo8x`
 - Tipo de dispositivo, como por ejemplo `my_gateway_type`
 - ID de dispositivo. **Consejo:** Para dispositivos conectados a la red, esto podría ser, por ejemplo, la dirección MAC sin dos puntos de separación.
 - Método de autenticación, como por ejemplo `señal`
 - Señal de autenticación, como por ejemplo `PtBVriRqIg4uh)_-Kl`
  Utilice esta información para conectar la pasarela a {{site.data.keyword.iot_short_notm}} y empezar a recibir datos de dispositivos conectados a la pasarela.

## Configuración de servicios Edge
{: #config_service}

Después de crear un nuevo tipo de dispositivo con las prestaciones de Edge habilitadas, puede seleccionar distintos servicios que se instalarán en el nodo Edge.

1. En el menú de navegación principal del panel de control de {{site.data.keyword.iot_short_notm}}, seleccione **Dispositivos**.
2. Pulse **Tipos de dispositivos**.
3. Seleccione el tipo de dispositivo Edge que desea configurar.
4. En el separador **Servicios Edge**, seleccione el icono de lápiz para editar el registro.
6. Pulse **Añadir servicios Edge** para ver una lista de los servicios que se han subido para esta organización.
7. En la ventana **Añadir servicios Edge**, seleccione la versión adecuada de los servicios que desea añadir y, a continuación, pulse **Terminado**.
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## Gestión y supervisión servicios Edge
{: #monitor_service}

Una vez instalados los nodos Edge, {{site.data.keyword.iot_short_notm}} le permite supervisar el estado de los servicios que se ejecutan en los nodos Edge.

1.	En el menú de navegación principal del panel de control de {{site.data.keyword.iot_short_notm}}, seleccione **Dispositivos**.
2.	Seleccione el dispositivo correspondiente a su nodo Edge
3.	Pulse **Servicios Edge** para ver el estado de los servicios instalados en el nodo Edge.

## Búsqueda de información adicional
{: #more_info}

Para obtener información sobre la conexión de la pasarela a {{site.data.keyword.iot_short_notm}}, consulte [Conectividad de MQTT para pasarelas](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt).

Para obtener información más detallada sobre el {{site.data.keyword.iot_short_notm}}, consulte [Cómo empezar con Watson IoT Platform](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp).

Para obtener más información sobre {{site.data.keyword.iot_short_notm}} Edge, consulte los temas siguientes:
- [Visión general de {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge.html#edge_overview)
- [Instalación de {{site.data.keyword.iot_short_notm}} Edge en dispositivos edge](WIoTP_edge_install.html#edge_install_device)
- [Desarrollo para {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
