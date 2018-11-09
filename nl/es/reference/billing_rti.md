---
copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Facturación

El plan de servicios de pago de {{site.data.keyword.iot_full}} (los que nos son 'Lite'), están basados en el concepto de intercambio de megabytes y se analizan en periodos de un mes.  Este documento detalla cómo mide {{site.data.keyword.iot_short_notm}} los datos para crear información de uso que determine el coste de utilización del servicio.  La información de uso puede utilizarse para calcular aproximadamente el coste de utilización de {{site.data.keyword.iot_short_notm}} en función del diseño y el número de dispositivos, aplicaciones y pasarelas.

Para obtener información sobre el corte de cada megabyte de datos intercambiados o analizados, consulte el servicio de {{site.data.keyword.iot_short_notm}} en el catálogo de {{site.data.keyword.Bluemix_notm}} para la región necesaria.

Puede utilizar la [Calculadora de tarifas ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://iot-cost-calculator.ng.bluemix.net/) para ayudarle a calcular el coste de un servicio de {{site.data.keyword.iot_short_notm}}.

Los siguientes tres elementos se miden de forma separada y se facturan según su uso: 
- Datos intercambiados
- Datos analizados
- Datos analizados Edge

## Datos intercambiados
El cálculo de *datos intercambiados* incluye los datos de cuenta intercambiados por aplicaciones, dispositivos y pasarelas utilizando la mensajería MQTT o HTTP, así como los datos intercambiados por las aplicaciones utilizando la API HTTP.

### Datos intercambiados por MQTT
Al utilizar MQTT, el contenido stream de TCP cuenta hacia los *datos intercambiados*.  La carga útil de cualquier mensaje MQTT se incluye en la acumulación de datos intercambiados utilizando el protocolo MQTT.  La acumulación del tamaño de datos de carga útil tiene lugar cuando se publica el mensaje, así como cuando el mensaje se entrega al suscriptor.

El protocolo MQTT está diseñado para ser tan ligero como sea posible.  Aunque {{site.data.keyword.iot_short_notm}} incluye los tamaños de los paquetes MQTT al acumular *datos intercambiados*, puesto que es un protocolo ligero, esta sobrecarga es baja.  La siguiente tabla proporciona una indicación del tamaño mínimo de cada paquete MQTT:

|Paquete MQTT                    |Tamaño mínimo en bytes  |Variables y tamaño mínimo asociado en bytes|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |ID de cliente (12 if d:xxxxxx:t:i), tema (0) mensaje (0) si se establece, nombre de usuario (14 para dispositivos: 'use-token-auth') y contraseña (18 si se genera automáticamente)|
|CONNACK                        |4                   |Ninguna|
|PUBLISH                        |21                  |Nombre de tema (17 si es iot-2/evt/i/fmt/f) y carga útil (0 es el mínimo).  Se aplica tanto a paquetes PUBLISH de entrada como de salida.|
|PUBACK                         |4                   |Ninguna|
|PUBREC                         |4                   |Ninguna|
|PUBREL                         |4                   |Ninguna|
|PUBCOMP                        |4                   |Ninguna|
|SUBSCRIBE                      |26                  |Puede incluir varios filtros de tema de cada (19 si son iot-2/evt/i/fmt/f) y QoS (1)|
|SUBACK                         |5                   |(1) para cada filtro de temas en SUBSCRIBE|
|UNSUBSCRIBE                    |23                  |Puede incluir varios filtros de tema (19 si son iot-2/evt/i/fmt/f)|
|UNSUBACK                       |4                   |Ninguna|
|PINGREQ                        |2                   |Ninguna|
|PINGRESP                       |2                   |Ninguna|
|DISCONNECT                     |2                   |Ninguna|

Al utilizar TLS, también se tiene en cuenta el reconocimiento seguro. El reconocimiento son aproximadamente 8 kilobytes. Es, por lo tanto, más económico mantener las conexiones MQTT abiertas tanto como sea posible. Las conexiones abiertas solo incurren en la sobrecarga de PINGREQ y PINGRESP (4 bytes) por intervalo de estado activo, que es específico de cliente y depende del periodo de estado activo especificado.  Reabrir una conexión TLS utilizando una sesión TLS existente disminuye el número de bytes intercambiados porque los certificados no se envían en cualquier dirección.  Muchos clientes TLS soportan esta característica de forma predeterminada, pero la sesión tiene un tiempo de vida limitado.  La utilización de certificados de cliente aumenta el tamaño del reconocimiento TLS, dependiendo del tamaño del certificado de cliente. 

No se tienen en cuenta los registros TLS ni las sobrecargas TCP e IP.

**Nota**: Al utilizar el panel de control de {{site.data.keyword.iot_short_notm}} para ver un dispositivo, se realiza una suscripción MQTT para que el panel de control pueda mostrar sucesos activos.  Esta suscripción MQTT también se tiene en cuenta para el recuento de la cantidad total de datos intercambiados.

### Datos intercambiados de mensajería HTTP
Al utilizar la mensajería HTTP, la carga útil del mensaje se incluye en la acumulación de *datos intercambiados*, al publicar sucesos o recibir mandatos.

Como con MQTT, la sobrecarga de HTTP se tiene en cuenta para el recuento.  La sobrecarga de HTTP es significativamente mayor que la de MQTT. La sobrecarga de HTTP por solicitud es de aproximadamente 300 bytes. El tamaño de la carga útil de mensaje también se debe añadir.

Al utilizar TLS, se tiene en cuenta el reconocimiento seguro.  El reconocimiento son aproximadamente 8 kilobytes.  Es, por lo tanto, más económico mantener las conexiones HTTPS abiertas tanto como sea posible.  Las conexiones abiertas no incurren en sobrecargas adicionales en términos de *datos intercambiados*.  Reabrir una conexión TLS utilizando una sesión TLS existente disminuye el número de bytes intercambiados porque los certificados no se envían en cualquier dirección.  Muchos clientes TLS soportan esta característica de forma predeterminada, pero la sesión tiene un tiempo de vida limitado.  La utilización de certificados de cliente aumenta el tamaño del reconocimiento TLS, dependiendo del tamaño del certificado de cliente.

No se tienen en cuenta los registros TLS ni las sobrecargas TCP e IP.

### Datos intercambiados de API HTTP
Cuando una aplicación invoca cualquier API, los cuerpos de solicitud y de respuesta se acumulan en los *datos intercambiados*.  Los tamaños de cada uno pueden variar de forma significativa dependiendo de la API.

A diferencia de la mensajería de MQTT y HTTP, ni las sobrecargas de HTTP ni las de TLS se tienen en cuenta en los datos intercambiados de API HTTP.

**Nota**: Al utilizar el panel de control de {{site.data.keyword.iot_short_notm}}, el panel de control utiliza la API HTTP para listar información incluyendo dispositivos, tipos de dispositivos y registros de conexión de dispositivo.  Estas llamadas de API HTTP se cuentan en los *datos intercambiados*.

## Datos analizados
El cálculo de *datos analizados* mide los datos de suceso que procesa el motor de reglas en la plataforma.  Se considera que los datos están procesados por el motor de reglas cuando se evalúan los sucesos de dispositivo por una o varias reglas, en función del dispositivo específico y el tipo de suceso. 

## Datos analizados Edge
El cálculo de *datos analizados Edge* mide los datos de suceso que procesa el Edge Analytics Agent de {{site.data.keyword.iot_short_notm}} en un dispositivo de pasarela.  Se considera que los datos están procesados por el Edge Analytics Agent cuando se evalúan los sucesos de dispositivo por una o varias reglas Edge, en función del dispositivo específico y el tipo de suceso. 
