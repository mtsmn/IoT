---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Visión general de {{site.data.keyword.iot_short_notm}} Edge (presentación)
{: #edge_overview}

**Importante:** La característica {{site.data.keyword.iot_full}} Edge solo está disponible como parte de un programa de vista previa limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} Edge le permite ampliar las prestaciones de {{site.data.keyword.iot_short_notm}} a dispositivos Edge. Los dispositivos Edge se encuentran en el borde de una red dentro de una organización. Los ejemplos incluyen sensores y controladores industriales en una ubicación física, como por ejemplo una fábrica. Con {{site.data.keyword.iot_short_notm}} Edge, los datos se pueden procesar en el interior de los dispositivos antes de enviarse a la nube.

Para utilizar {{site.data.keyword.iot_short_notm}} Edge, puede crear nodos Edge y configurar esos nodos con los servicios que se van a ejecutar dentro de ellos. A continuación, WIoTP Edge despliega dichos servicios en los nodos. Con los servicios Edge que se ejecutan en ellos, los nodos Edge pueden enviar mensajes desde dispositivos con la pasarela de Edge. La pasarela procesa los mensajes, transforma los datos y, a continuación, puede enviar los datos a la nube, en función de cómo se configuran las interfaces.

La vista previa de {{site.data.keyword.iot_short_notm}} Edge proporciona el servicio predeterminado Core IoT, junto con la posibilidad de crear servicios personalizados como, por ejemplo, un servicio para predecir anomalías en un robot de planta de fabricación y enviar notificaciones de anomalía. Un ejemplo de un dispositivo de extremo es un sensor que supervisa el uso de la CPU y envía una alerta cuando el uso excede un determinado porcentaje.

## Componentes de {{site.data.keyword.iot_short_notm}} Edge

**Nodo Edge**
Un nodo Edge consta de una pasarela y un dispositivo que reside en el borde de la red.

**Dispositivo Edge**
Un dispositivo como, por ejemplo, un sensor, reside en el borde de una red y ejecuta servicios que procesan datos antes de enviarlos a la nube. Un ejemplo de un dispositivo de extremo es un sensor que supervisa el uso de la CPU y envía una alerta cuando el uso excede un determinado porcentaje.

**Pasarela Edge**
Las pasarelas son dispositivos especializados que tienen las prestaciones combinadas de una aplicación y de un dispositivo, lo que les permite servir como puntos de acceso para otros dispositivos. Cuando Edge está habilitado, las pasarelas Edge permiten a los dispositivos que no pueden conectarse directamente a Internet para acceder al servicio de {{site.data.keyword.iot_short_notm}} conectarse en primer lugar al dispositivo de pasarela en el Edge.

**Tipo de pasarela Edge**
Un tipo de pasarela que agrupa dispositivos de pasarela que comparten atributos, como por ejemplo el número o el número de modelo o la ubicación. Cuando las prestaciones de Edge están habilitadas y se añade un dispositivo de pasarela de extremo a {{site.data.keyword.iot_short_notm}}, los atributos del tipo de pasarela de extremo se utilizan como plantilla para el nuevo dispositivo de pasarela de extremo.

**Servicios Edge**
Los servicios son prestaciones que se añaden a una pasarela de {{site.data.keyword.iot_short_notm}} Edge y que realizan procesos como la manipulación de datos. Las aplicaciones que utilizan los servicios se crean.

**Catálogo de servicios Edge**
El servicio de Core IoT predeterminado se incluye en el catálogo y se añade a todos los nodos Edge. Puede añadir servicios personalizados en función de las necesidades de su empresa. El catálogo de servicios Edge contiene todos los servicios personalizados disponibles.

**Repositorio de servidor de archivos**
El Repositorio de servidor de archivos contiene contenedores e imágenes docker. Todas las prestaciones de proceso de Edge, o servicios, funcionan dentro de contenedores docker. Debe seleccionar las imágenes docker que se van a ejecutar dentro de los dispositivos.

## Búsqueda de información adicional
{: #more_info}

Para obtener más información sobre la configuración, la instalación y el desarrollo para {{site.data.keyword.iot_short_notm}} Edge, consulte los temas siguientes:
 - [Configuración de {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
 - [Instalación de {{site.data.keyword.iot_short_notm}} Edge en dispositivos edge](WIoTP_edge_install.html#edge_install_device)
 - [Desarrollo para {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
