---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guías de iniciación para {{site.data.keyword.iot_short_notm}}
{: #getting-started}

Las guías de iniciación están diseñadas para guiarle a través de las bases del desarrollo de un sistema prototipo de IoT completo listo para producción con {{site.data.keyword.iot_short_notm}} y están escritas por desarrolladores que son nuevos trabajando con {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

Cada guía proporciona un proceso paso a paso que le lleva rápidamente a una solución desplegada y en ejecución que muestra una o varias características de {{site.data.keyword.iot_short_notm}}.

## Utilización de las guías  
{: #using-guides}
En cada guía obtendrá, por lo menos, una muestra de GitHub codificada según las prácticas recomendadas de {{site.data.keyword.iot_short_notm}}. Estas muestras se pueden escalar, asegurar, integrar con sistemas y ajustar generalmente en sus procesos de desarrollo y compilación devOps.

La más técnica puede expandir la solución rápida adaptando el código de muestra a su propio entorno o complementando la solución rápida con ejemplos de hardware que simulan de forma más precisa lo que se puede encontrar en su entorno de producción. Las guías proporcionan enlaces adicionales a documentación relevantes para tareas, documentos de API, bibliotecas de cliente (SDK), vídeos didácticos y otro material de formación.

Puede seguir las guías en orden, donde la primera guía proporciona una base sobre la que construyen las guías siguientes. Si desea ir saltando y seguir su propia vía de acceso a través de las series, cada guía viene también con una lista de requisitos. Por ejemplo, si ya tiene configurada una organización de {{site.data.keyword.iot_short_notm}}, puede saltar directamente a las guías dos y tres y empezar a explorar reglas, acciones y apps de supervisión de muestra. Los requisitos le dicen el formato de los datos de dispositivo que requieren estas guías y puede configurar su propio entorno en consecuencia.
{: tip}

## Lista de guías
{: #list-of-guides}  

Están disponibles las siguientes guías:

| Guía | Descripción |    
| ----- | ---- |   
| [1. Conectar un dispositivo de cinta transportadora](getting-started-iot-conveyor.html) | Empiece instalando una organización de {{site.data.keyword.iot_short_notm}} y conectando una aplicación de simulación de cinta transportadora de muestra obtenida en GitHub, o si prefiere algo más físico, su propio simulador de cinta transportadora basado en Raspberry-Pi. </br> A continuación configure algunas tarjetas de panel de control de {{site.data.keyword.iot_short_notm}} básicas para visualizar y supervisar sus datos de dispositivo. |   
| [2. Utilización de reglas y acciones básicas en tiempo real](getting-started-iot-rules.html) | Con uno o varios dispositivos conectados a su organización de {{site.data.keyword.iot_short_notm}} y enviando datos, puede configurar reglas empresariales y desencadenar acciones para, por ejemplo, notificar a un operador de planta si la velocidad de la cinta transportadora desciende por debajo de cierto umbral.  
| [3. Supervisión de los datos de dispositivo](getting-started-iot-monitoring.html) | Expanda los paneles de control de supervisión de dispositivos incorporados de {{site.data.keyword.iot_short_notm}} conectando una app de supervisión basada en Node.js o en una biblioteca de widgets para ver los datos de cinta transportadora en tiempo real.  
| [4. Simulación de un gran número de dispositivos](getting-started-iot-large-scale-simulation.html) | Expanda la simulación de dispositivos simple añadiendo una mayor cantidad de simuladores de dispositivo a su entorno para probar las analíticas y la supervisión de las guías anteriores en un entorno más realista y multi-dispositivo. </br>**Importante:** La aplicación requiere 512 MB de memoria, que es más de lo asignado por defecto y que también excede la cantidad disponible en las cuentas de prueba gratuita, que debe primero actualizarse a una cuenta de Subscripción o de Pago según uso. |   
