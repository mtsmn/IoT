---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Iniciación a {{site.data.keyword.iot_short_notm}}
{: #gettingstartedtemplate}

{{site.data.keyword.iot_full}} para {{site.data.keyword.Bluemix_notm}} le ofrece un kit de utilidades versátiles que incluye dispositivos de pasarela, gestión de dispositivos y acceso de aplicaciones potente. Al utilizar {{site.data.keyword.iot_short_notm}}, puede recopilar datos de dispositivo conectados y realizar las analíticas en datos en tiempo real desde la organización.
{:shortdesc}

## Antes de empezar
{: #byb}

Antes de conectar dispositivos y de utilizar datos, regístrese para una cuenta de {{site.data.keyword.Bluemix_notm}} y cree una instancia del servicio de {{site.data.keyword.iot_short_notm}} en la organización de {{site.data.keyword.Bluemix_notm}}. Puede crear una instancia de {{site.data.keyword.iot_short_notm}} directamente desde la página [{{site.data.keyword.iot_short_notm}} del Catálogo de servicios de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}.  

Para obtener información detallada sobre cómo registrarse para una cuenta en {{site.data.keyword.Bluemix_notm}}, configurar regiones y otros valores de gestión de cuentas, consulte [Gestión de la cuenta de IBM Cloud](https://console.ng.bluemix.net/docs/admin/account.html#signup).

Puede establecer y configurar la instancia de {{site.data.keyword.iot_short_notm}} desde el panel de control. Para abrir el panel de control, vaya a la instancia de servicio de {{site.data.keyword.iot_short_notm}} en {{site.data.keyword.Bluemix_notm}} y, a continuación, pulse **Iniciar**.

## Acerca de esta tarea

Los siguientes pasos describen cómo puede empezar rápidamente con su servicio de {{site.data.keyword.iot_short_notm}}.

También hay disponible un conjunto más detallado de guías de iniciación y aplicaciones de muestra que le guían a través de las bases del desarrollo de un sistema prototipo de IoT completo listo para producción con {{site.data.keyword.iot_short_notm}}. Si es un desarrollador que no ha trabajado antes con {{site.data.keyword.iot_short_notm}}, utilice los procesos paso a paso de la sección [Guías de iniciación](https://console.bluemix.net/docs/services/IoT/getting_started/getting-started-iot-overview.html#getting-started).

## Paso 1: Conectar los dispositivos
{: #up_and_running}

Para ejecutar el servicio, explore las opciones siguientes en función de su situación:

|  |   El servicio se despliega | El servicio no se despliega
 | -------------| ------------- | -------------
  |**Tengo ningún dispositivo que conectar** | [Conecte el dispositivo a {{site.data.keyword.iot_short_notm}}](iotplatform_task.html#iotplatform_task).| Explore la conexión de dispositivos en el apartado sobre [Practicar con {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play){:new_window}.
  |**No tengo un dispositivo para conectar** | [Cree y conecte un simulador de dispositivos Node-RED](nodereddevice_sample.html){:new_window}. O [Conecte su teléfono inteligente ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play/device/smartphone){:new_window}. | Iníciese a [Watson IoT Platform Starter](https://console.bluemix.net/docs/starters/IoT-starter/iot500.html).
  
Para obtener más información sobre cómo conectarse a tipos de dispositivos específicos a {{site.data.keyword.iot_short_notm}}, consulte [Recetas de developerWorks ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){:new_window}.  

Para la documentación de desarrollador de conexiones de dispositivos, consulte:
- [Conectividad de MQTT para dispositivos](devices/mqtt.html).
- [Conectividad de MQTT para pasarelas](gateways/mqtt.html).

<!--
## Step 2: Analyze your device data
{: #analyzing_data}
Start exploring the real-time data that the devices are sending to {{site.data.keyword.iot_short_notm}}.
{{site.data.keyword.iot_short_notm}} includes the following analytics tools:  
- [Boards and cards](data_visualization.html) to visualize your real-time device data.
- [Rules and actions](analytics.html) that are triggered by real-time device data.
For a quick getting started example, see the [Using Rules and Actions with IBM Watson IoT Platform Cloud Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/using-rules-and-actions-with-ibm-watson-iot-platform-cloud-analytics/){:new_window} developerWorks recipe.
-->

## Paso 2: Crear las aplicaciones que van a consumir los datos de dispositivo
{: #develop_applications}

Cree y conecte sus propias aplicaciones de modo que consuman datos del dispositivo.

Para obtener más información consulte los siguientes temas:   
- Explore la [documentación del desarrollador de aplicaciones](applications/api.html) y la [documentación de la API de {{site.data.keyword.iot_short_notm}}](reference/api.html).
- Explore las [bibliotecas de cliente de {{site.data.keyword.iot_short_notm}}](iot_platform_client_lib.html) que proporcionan herramientas y archivos para crear y desarrollar código para integrar y conectar los dispositivos y aplicaciones.
- [Conecte un servicio de {{site.data.keyword.cloudantfull}}](cloudant_connector.html) a su {{site.data.keyword.iot_short_notm}} para almacenar datos de dispositivos históricos.
- Cree sus propias reglas mediante la nueva característica de [reglas incorporadas (beta)](information_management/im_rules.html).
