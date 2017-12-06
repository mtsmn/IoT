---

copyright:

years: 2017
lastupdated: "2017-11-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Cuotas
Esta sección proporciona información acerca de los límites definidos para los servicios de {{site.data.keyword.iot_full}} por tipo de plan. Los límites están ahí como parte de nuestra política de uso legítimo, para ayudar a asegurar que no hay repercusión negativa en el rendimiento debido al uso inapropiado para todos los usuarios del sistema multiusuario. Los límites también ayudan a prevenir que la carga de trabajo de usuario real sea identificada involuntariamente como ataque de denegación de servicio.

## Introducción
{: #metrics_about}

Las cuotas especifican los límites superiores definidos para un recurso. Cuando el uso supera un límite especificado, las solicitudes de usuario podrían denegarse o retrasarse. En circunstancias excepcionales, es posible que la superación de límites, que afecta a la estabilidad del sistema, resulte en la suspensión de la organización de {{site.data.keyword.iot_short_notm}} para impedir que otros usuarios se vean afectados.

Las siguientes tablas enumeran los límites de {{site.data.keyword.iot_short_notm}}. La mayoría de los límites especificados son por organización de {{site.data.keyword.iot_short_notm}}, a menos que se liste explícitamente como por dispositivo. Algunos de los límites tienen un máximo impuesto, mientras que otros se pueden aumentar bajo petición. Si necesita límites más altos de los especificados, abra una [incidencia ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://support.ng.bluemix.net/gethelp/){: new_window}.

## Mensajería
{: #messaging_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Plan Dedicado
------------- | -------------|------------- |
Número máximo de tipos de dispositivos|50 |1000 |1000
Número máximo de dispositivos conectados simultáneamente| 500| 500K. Si desea conectar más de 50.000 dispositivos, póngase en contacto para hablar de ello. | 500K
Número máximo de dispositivos registrados|500 | 1 millón | 100K por incremento
Aplicaciones 'A' de [ máximas: Compartidas](../applications/mqtt.html#scalable_apps) |1 | 10 suscripciones compartidas, cada una con 50 instancias|10 suscripciones compartidas, cada una con 50 instancias
Aplicaciones 'a' de [ máximas: No compartidas](../applications/mqtt.html#client_connections) |500 claves de API| 2000 claves de API| 2000 claves de API
Datos máximos por mes| 200 MB | ilimitado |ilimitado
Conexiones de dispositivo|10/seg | 300/seg |300/seg 
Envíos de dispositivo a nube (MQTT): Dispositivo| 5 msg/seg o 20 KB/seg | 10 msg/seg o 40 KB/seg |10 msg/seg o 40 KB/seg 
Envíos de dispositivo a nube (MQTT): Pasarela| 50 msg/seg o 200 KB/seg | 100 msg/seg o 400 KB/seg| 100 msg/seg o 400 KB/seg
Envíos de dispositivo a nube (MQTT): Aplicación| 10 msg/seg o 40 KB/seg | 20 msg/seg u 80 KB/seg| 20 msg/seg u 80 KB/seg
Envíos de dispositivo a nube (MQTT): Por organización| 5000 msg/seg | 6000 msg/seg | 6000 msg/seg 
Envíos de dispositivo a nube (HTTP): Dispositivo| 30 msg/min o 2 KB/seg | 1 msg/seg o 4 KB/seg | 1 msg/seg o 4 KB/seg 
Envíos de dispositivo a nube (HTTP): Pasarela| 5 msg/seg o 20 KB/seg | 10 msg/seg o 40 KB/seg | 10 msg/seg o 40 KB/seg 
Envíos de dispositivo a nube (HTTP): Aplicación| 1 msg/seg o 4 KB/seg | 2 msg/seg u 8 KB/seg | 2 msg/seg u 8 KB/seg 
Envíos de dispositivo a nube (HTTP): Por organización| 500 msg/seg | 600 msg/seg | 600 msg/seg 
Envíos de nube a dispositivo (MQTT): Dispositivo|1 msg/seg |2 msg/seg u 8 KB/seg |2 msg/seg u 8 KB/seg 
Envíos de nube a dispositivo (MQTT): Pasarela| 10 msg/seg |20 msg/seg u 80 KB/seg|20 msg/seg u 80KB/seg
Envíos de nube a dispositivo (MQTT): Aplicación| 10 msg/seg |20 msg/seg u 80 KB/seg|20 msg/seg u 80 KB/seg
Envíos de nube a dispositivo (MQTT): Por organización|1000 msg/seg | 12K msg/seg|12K msg/seg
Envíos de nube a dispositivo (HTTP): Tasa de ráfaga por dispositivo| 30 msg/min| 30 msg/min  | 30 msg/min
Envíos de nube a dispositivo (HTTP): Por organización|  1000 msg/seg |  1200 msg/seg  |  1200 msg/seg  
Número máximo de mensajes de entrada no reconocidos: Por dispositivo| 10 |10 |10
Número máximo de mensajes de entrada no reconocidos: Por pasarela| 1000 |1000 |1000
Número máximo de mensajes de entrada no reconocidos: Por conexión de aplicación|2000 | 2000|2000
Número máximo de mensajes salientes no reconocidos: Por dispositivo|10  |10 |10
Persistencia dispositivo a nube| 24 horas|24 horas|24 horas
Persistencia nube a dispositivo|48 horas (predeterminado). Máximo 7 días y profundidad de cola de 50| 48 horas (predeterminado). Máximo 7 días y profundidad de cola de 50|48 horas (predeterminado). Máximo 7 días y profundidad de cola de 50
Intervalo de reintento máximo para entregar mensajes de QoS 1| Reintentar reconexión|Reintentar reconexión|Reintentar reconexión
Límite de inactividad de conexión (intervalo de estado activo) | Especificado por el cliente en conexión|Especificado por el cliente en conexión|Especificado por el cliente en conexión
Duración de conexión WebSocket |Especificado por el cliente en conexión| Especificado por el cliente en conexión|Especificado por el cliente en conexión
Tamaño máximo de mensaje| 128 KB |128 KB |128 KB
Suscripciones máximas por dispositivo|50 |50 |50
Suscripciones máximas por aplicación| 500 |500 |500
Profundidad de cola: Mensajes almacenados en búfer máximos por suscripción y dispositivo| 50 |50 |50
Mensajes almacenados en búfer máximos para aplicaciones 'A'|50K duraderos, 50K no duraderos|50K duraderos, 50K no duraderos|50K duraderos, 50K no duraderos


## API
{: #api_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Llamadas de la API |10/seg |10/seg |10/seg 

## Motor de reglas
{: #rule_engine_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Reglas máximas por organización|100|1000|1000
Acciones por regla|10|10|10

## Gestión de dispositivos
{: #device_mgmt_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Tamaño máximo de registros de diagnóstico por dispositivo|500K|500K|500K
Versiones históricas máximas de registros de diagnóstico mantenidos|2  |2 |2
Tiempo máximo de registros de diagnóstico mantenidos|7 días| 7 días|7 días
Número máximo de acciones iniciadas por mensaje en curso|200 |200 |200
Número máximo de dispositivos por acción|5000 |5000 | 5000

## Memoria caché del último suceso
{: #last_event_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Número máximo de ID de suceso por dispositivo|5|5|5
Número máximo de formatos|2|3|3
Caducidad de la última memoria caché de sucesos|12 meses|12 meses| 12 meses

## Extensiones
{: #extensions_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Número máximo de servicios con los que puede enlazar|12|12|12

## Gestión de la información
{: #information_management_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Máxima profundidad de anidamiento para carga útil JSON|5|5|5
Tamaño máximo de serie JSON|512|512|512

## Seguridad
{: #security_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Número máximo de roles personalizados|20|20|20
Número máximo de miembros usuarios|1000|1000|1000
Número máximo de dispositivos en un grupo de recursos|300|300|300
Número máximo de grupos de recursos asignados a una pasarela|10|10|10
Número máximo de grupos de recursos a los que puede pertenecer un dispositivo|10|10|10

## Interfaz de usuario
{: #UI_metrics}

Medida | Plan Lite | Planes de seguridad estándar y avanzados | Dedicado
------------- | -------------|------------- |
Número máximo de paneles de control|50|50|50
Número máximo de tarjetas en un panel|30|30|30
