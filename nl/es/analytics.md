---

copyright:
  years: 2016, 2018
lastupdated: "2017-03-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Análisis de datos del Internet de las cosas en tiempo real
{: #analytics}  

**Importante:** estamos lanzando una versión Beta con una nueva forma de definir reglas en los datos del dispositivo IoT como parte de un programa más ambicioso de cambios para mejorar la forma en que {{site.data.keyword.iot_full}} distribuye reglas y acciones.

Para ver más información, consulte la publicación del blog sobre [Un enfoque alternativo a la definición de reglas en datos de IoT ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}.

Para empezar a definir sus propias reglas, consulte la documentación sobre [Creación de reglas incorporadas (Beta)](information_management/im_rules.html).

## Acerca de los análisis de datos IoT en tiempo real

Utilice el análisis del {{site.data.keyword.iot_short_notm}} de Watson para obtener la información analítica en tiempo real que necesita a partir de los datos en bruto que producen los dispositivos.  
{: shortdesc}

Al utilizar [paneles y tarjetas](data_visualization.html), puede ver gráficos que representan valores de conjuntos de datos de uno o varios dispositivos para una visión general rápida y una comprensión de los datos del dispositivo.

Con reglas analíticas, especifica las condiciones que desencadenan acciones. Por ejemplo, puede crear una regla para asegurarse de que se envía una alerta al panel de control del dispositivo de un usuario, y que se enviará un correo electrónico al administrador, cuando el dispositivo se ha eliminado, o cuando la temperatura del dispositivo se dispara.

Utilice [reglas de nube](cloud_analytics.html) para desencadenar reglas para dispositivos que se conectan directamente a {{site.data.keyword.iot_short_notm}} en la nube, y [reglas de extremo](edge_analytics.html) para desencadenar reglas para dispositivos conectados a pasarelas habilitadas para analíticas de extremo.
