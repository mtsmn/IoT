---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configuración de las reglas de direccionamiento de {{site.data.keyword.iot_short_notm}} Edge (vista previa)
{: #edge_rules}

Las reglas de direccionamiento de {{site.data.keyword.iot_full}} Edge definen cómo se reenvían mensajes dentro del nodo Edge.

**Importante:** La característica {{site.data.keyword.iot_full}} Edge solo está disponible como parte de un programa de vista previa limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Puede definir varias reglas de direccionamiento para un nodo Edge. Las reglas se evalúan para cada mensaje que publica un dispositivo local o una aplicación directamente en el nodo Edge. Los mensajes que se publican en el nodo Edge desde la nube siempre se publican localmente y no se ven afectados por el direccionamiento.

## Formato de reglas de direccionamiento
{: #edge_rules_format}

Una regla de direccionamiento se define como un objeto JSON en el formato siguiente:

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

Se especifica una regla de direccionamiento como matriz de los objetos JSON en el archivo `routing.json` que se almacena en la carpeta `/etc/wiotp-edge` en el nodo Edge.

Las propiedades de cada regla de direccionamiento se definen de la forma siguiente:

Propiedad      | Tipo     | Descripción       
------------- | -----------| -----------
rule_id | entero, obligatorio | Un identificador para la regla. Las reglas de direccionamiento se evalúan en orden ascendente de acuerdo con el rule_id. No debería haber más de una regla con el mismo rule_id.
matching_filter | serie, obligatorio | Se utiliza para evaluar si la regla se debe aplicar a un mensaje publicado. Debe estar en el formato de una suscripción MQTT. Se da soporte a los comodines de un solo nivel (+) y de varios niveles (#) de MQTT. Este filtro se evalúa con el tema en el que se publica el mensaje. Esta propiedad se define en el formato de tema de aplicación de {{site.data.keyword.iot_short_notm}} e incluye los campos `/type/deviceType/id/deviceId`. Por ejemplo: `"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
Destino | serie, obligatorio | Define dónde se debe reenviar el mensaje. Establezca uno de los valores siguientes: `"CLOUD"`, `"IM"` (para reenviar el mensaje al componente de Information Management), o `"LOCAL"` (para reenviar el mensaje localmente en el nodo Edge). Por ejemplo, `"destination" : "CLOUD"`
continue_matching | booleano, opcional, valor predeterminado = true | Especifica si se deben evaluar los mensajes adicionales utilizando esta regla si la regla coincide una vez. Esta propiedad proporciona un mejor control sobre el direccionamiento cuando existen reglas de direccionamiento solapadas.
forward_skip_levels | entero, opcional, valor predeterminado = 0) | Especifica el número de niveles del tema de mensaje original que se debe eliminar cuando se vuelve a publicar el mensaje. El valor predeterminado es 0, lo que significa que se mantiene el tema original. Por ejemplo, si `forward_skip_levels` se establece en `1` y se publica un mensaje en el tema `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json`, el mensaje se vuelve a publicar en el tema `iot-2/type/T1/id/D1/evt/E1/fmt/json`.
store | booleano, opcional, valor predeterminado = true) | Define si la característica de almacén y reenvío se debe aplicar a los mensajes que se direccionan mediante esta regla. Esta propiedad solo se aplica cuando la propiedad destination se establece en `"CLOUD"`. Si esta propiedad se establece en false, los mensajes que coincidan con la regla no se almacenarán cuando el nodo Edge esté desconectado de la nube.

## Ejemplo de un archivo routing.json
{: #edge_rules_example}

En el ejemplo siguiente, se configuran tres reglas:
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

La primera regla direcciona todos los sucesos de tipo `AlarmEvent` directamente a la nube.

La segunda regla direcciona todos los sucesos que se publican en un tema que empieza por `sendToCloud/iot-2` directamente a la nube. El mensaje se reenvía en un tema que empieza por `iot-2` y no incluye el prefijo `sendToCloud/`.

La tercera regla es la regla predeterminada, que direcciona el resto de los mensajes al intermediario MQTT local. A continuación, estos mensajes los puede utilizar cualquier consumidor local con una suscripción coincidente. La instalación del nodo Edge incluye la regla predeterminada para direccionar todo localmente, en `/etc/wiotp-edge/routing.json`.

## Edición de reglas de direccionamiento
{: #edge_rules_edit}

Puede editar reglas directamente en el archivo `/etc/wiotp-edge/routing.json`. Después de haber editado el archivo, guarde los cambios y, a continuación, ejecute `docker ps` para determinar el ID del contenedor de conector edge. Reinicie el contenedor docker utilizando ese ID.

## Resolución de problemas de reglas de direccionamiento
{: #edge_rules_ts}

Si el contenedor de conector edge sigue reiniciándose después de un cambio en `routing.json`, significa que el archivo de reglas de direccionamiento contiene errores de sintaxis. Compruebe el archivo de registro `/var/wiotp-edge/trace/edgeConnector_trace.log` para ver los mensajes de error.
