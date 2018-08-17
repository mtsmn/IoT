---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Guía de aprendizaje de iniciación
{: #getting-started-with-iotp}

En esta guía de aprendizaje de iniciación de {{site.data.keyword.iot_full}}, se conectará un dispositivo IoT a {{site.data.keyword.iot_short_notm}} y se configurarán las analíticas para explorar los datos en tiempo real.
{:shortdesc}

<div id="prerequisites"></div>

## Antes de empezar
{: #prereqs}

Necesitará una [cuenta de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/registration/){: new_window}, una instancia del servicio {{site.data.keyword.iot_short_notm}} y un dispositivo que cumpla las siguientes condiciones:

*	El dispositivo debe poder comunicarse mediante protocolos HTTP o MQTT.

* Los mensajes del dispositivo deben ajustarse a los requisitos de carga útil de mensajes de {{site.data.keyword.iot_short_notm}}.

Para obtener más información, consulte [Desarrollo de dispositivos en Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Paso 1: Registrar su dispositivo

Puede añadir dispositivos uno a uno desde el panel de control [{{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://internetofthings.ibmcloud.com){: new_window} o puede utilizar la [API de Watson IoT Platform ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} para añadir varios dispositivos.

Para añadir un dispositivo desde el panel de instrumentos de {{site.data.keyword.iot_short_notm}}:

1. En la consola de {{site.data.keyword.Bluemix_notm}}, pulse **Iniciar** en la página de detalles de servicio de {{site.data.keyword.iot_short_notm}}.

    Se abre la consola web de {{site.data.keyword.iot_short_notm}} en un nuevo separador del navegador con el siguiente URL:

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Donde *org_id* es el ID de [la organización de {{site.data.keyword.iot_short_notm}}](iotplatform_overview.html#organizations){: new_window}.

2. En el panel de instrumentos Visión general, desde el panel del menú, seleccione **Dispositivos** y, a continuación, pulse **Añadir dispositivo**.

3. Cree un tipo de dispositivo para el dispositivo que está añadiendo.

    Cada dispositivo conectado al {{site.data.keyword.iot_short_notm}} debe estar asociado con un tipo de dispositivo. Los tipos de dispositivos son grupos de dispositivos que comparten características comunes.

    1. Pulse **Crear tipo de dispositivo**.
    2. Especifique un nombre de dispositivo como, por ejemplo, `my_device_type` y una descripción para dicho tipo de dispositivo.
    
        > **Nota:** El nombre del tipo de dispositivo no debe tener más de 36 caracteres y solo puede contener caracteres alfanuméricos (a-z, A-Z, 0-9) y cualquiera de los siguientes caracteres: `_`, `.` y `-`.

    3. Opcional: Especifique los atributos y metadatos de tipo de dispositivo.

        Puede añadir y editar atributos y metadatos más tarde.
        {: tip}

4. Pulse **Siguiente** para empezar el proceso de adición de su dispositivo con el tipo de dispositivo seleccionado.

5. Especifique un ID de dispositivo como, por ejemplo, `my_first_device`.

    Este ID de dispositivo se utiliza para identificar el dispositivo en el panel de instrumentos de {{site.data.keyword.iot_short_notm}} y también un parámetro obligatorio para conectar el dispositivo a {{site.data.keyword.iot_short_notm}}.

    > **Nota:** El nombre del tipo de dispositivo no debe tener más de 36 caracteres y solo puede contener caracteres alfanuméricos (a-z, A-Z, 0-9) y cualquiera de los siguientes caracteres: `_`, `.` y `-`.

    Para los dispositivos conectados a la red, el ID de dispositivo podría ser la dirección MAC del dispositivo sin los dos puntos de separación.

5. Pulse **Siguiente** para completar el proceso.

6. Proporcione una señal de autenticación o acepte la señal que se genera de forma automática. Si elige crear su propia señal, asegúrese de que tiene entre 8 y 36 caracteres de longitud y solo consta de caracteres alfanuméricos y: `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)` y `-`.

    La señal no debe contener secuencias de caracteres repetidos, palabras de diccionario, nombres de usuario ni otras secuencias predefinidas.

7. Verifique que la información de resumen sea correcta y, a continuación, pulse **Añadir** para añadir la conexión.

8. En la página de información de dispositivos, copie y guarde los siguientes detalles:

    * ID de organización
    * Tipo de dispositivo
    * ID de dispositivo
    * Método de autenticación
    * Señal de autenticación

    Necesitará el ID de organización, el Tipo de dispositivo, el ID de dispositivo y la Señal de autenticación para configurar su dispositivo para conectarse a {{site.data.keyword.iot_short_notm}}.

## Paso 2: Conectar el dispositivo a {{site.data.keyword.iot_short_notm}}

1. Configure el dispositivo para la mensajería MQTT y la autenticación con la ayuda del ID de organización, el Tipo de dispositivo, el ID de dispositivo y la Señal de autenticación.

2. Envíe mensajes de dispositivo a la organización de {{site.data.keyword.iot_short_notm}} utilizando el protocolo de MQTT.

    Se necesita la siguiente información al conectar el dispositivo:

    * URL: *org_id*.messaging.internetofthings.ibmcloud.com

      Donde *org_id* es el ID de la organización de {{site.data.keyword.iot_short_notm}}.

    * Puerto:
      * 1883
      * 8883 (cifrado)
      * 443 (websockets)
    * ID de dispositivo: d:_org_id:device_type:device_id_
    * Nombre de usuario: use-token-auth
    * Contraseña: _Señal de autenticación_
    * Formato de tema de suceso: iot-2/evt/_event_id/fmt/format_string_

      Donde _event_id_ especifica el nombre de suceso que se muestra en {{site.data.keyword.iot_short_notm}} y _format_string_ es el formato del suceso, como por ejemplo JSON.

    * Formato del mensaje: JSON

  Para obtener más información, consulte [Conectividad de MQTT para dispositivos](/docs/services/IoT/devices/mqtt.html).

## Paso 3: Crear tarjetas y paneles para realizar el seguimiento de los datos del dispositivo

Con la utilización de paneles y tarjetas, visualizará gráficos que representan valores de conjuntos de datos de uno o más dispositivos para una visión general de los datos de los mismos.

1. En su panel de control de {{site.data.keyword.iot_short_notm}}, pulse **Crear nuevo panel**.

2. Especifique el nombre y la descripción para el panel.

3. Pulse **Siguiente** y, a continuación, **Crear**.

4. Pulse en el panel que acaba de crear y, a continuación, pulse **Añadir nueva tarjeta**. Seleccione Dispositivos como tipo de tarjeta. En la siguiente tabla se describen las opciones de visualización que puede elegir.

| Tipo | Datos que se visualizan |
|---------------|---------------|
| Visualización genérica | El valor de uno o más conjuntos de datos. Elija el tamaño del widget grande para ver hasta tres valores de puntos de datos en una tabla pequeña. |
| Gráfico de líneas | Uno o varios conjuntos de datos en un diagrama de desplazamiento en tiempo real. Utilice el menú Valores para establecer el rango y la retención de datos, el aspecto de los gráficos, etc. |
| Gráfico de barras | Valores de conjuntos de datos en barras etiquetadas. Utilice el menú Valores para conmutar la dirección de barras horizontales o verticales. |
| Diagrama de anillo | Dos o varios conjuntos de datos en una representación circular. |
| Valor | El valor original de uno o varios conjuntos de datos. |
| Indicador | El valor de un conjunto de datos que se muestra en forma de medidor. Utilice el menú Valores para establecer opcionalmente umbrales de medidor para los rangos de datos lower, middle y upper. |
| Propiedades de dispositivo | Propiedades específicas de uno o más dispositivos. |
| Todas las propiedades de dispositivo | Todas las propiedades específicas de uno o más dispositivos. |
| Lista de dispositivos | Una lista para supervisar varios dispositivos. Se puede utilizar una lista como un origen de datos para otras tarjetas. |
| Información de dispositivo | Información básica para un dispositivo individual. |
| Correlación de dispositivo | Ubicación de los dispositivos en la lista de dispositivos. |

{: caption="Tabla 1. Opciones de visualización" caption-side="top"}

## Paso 4: Crear esquemas de tipo de dispositivo

Al llegar a este punto, precisa crear un esquema de tipo de dispositivo y correlacionar con las mismas propiedades de dispositivo para a continuación crear reglas que se activan en función de los puntos de datos de las propiedades de dispositivo correlacionadas.

1. En el panel de control {{site.data.keyword.iot_short_notm}}, vaya a **Dispositivos > Gestionar esquemas** y pulse **Añadir esquema**.

2. Seleccione un tipo de dispositivo para asociarlo con el esquema de mensajes. Sólo se puede definir un esquema para un tipo de dispositivo.

3. Pulse **Añadir propiedad** y añada una o más propiedades.

    Puede seleccionar propiedades desde un dispositivo conectado, crear propiedades virtuales que modifiquen o combinen propiedades existentes, o añadir propiedades manualmente.

    Las propiedades disponibles se definen en la carga útil de los mensajes enviados por un dispositivo.
    {: tip}

    * Para añadir una propiedad manualmente, seleccione el **separador Manual** y defina los siguientes detalles de propiedades:
        * Nombre
        * Tipos de datos
        * Propiedad
        * Unidad de datos (opcional)
        * Posiciones decimales (opcional)

    * Para crear una propiedad virtual, seleccione el separador **Propiedad virtual** y defina los siguientes detalles de propiedades:
        * Nombre
        * Tipos de datos
        * Propiedad
        * Cálculo
        * Unidad de datos (opcional)
        * Posiciones decimales

    * Para seleccionar propiedades desde un dispositivo conectado, seleccione el separador **Desde conectados** y, a continuación, seleccione una o más propiedades para añadirlas al esquema. Se añadirán las propiedades seleccionadas y la descripción se establecerá en el nombre de la propiedad.

4. Pulse **Finalizar** para crear las propiedades.

## Paso 5: Crear reglas y acciones

Las reglas son puntos de decisión basadas en condiciones que coinciden con los datos de dispositivos en tiempo real con valores de umbral predefinidos u otros datos de propiedades para desencadenar una alerta si se cumple una condición. Además de la alerta que se muestra en el panel de control de {{site.data.keyword.iot_short_notm}}, puede añadir una o varias acciones para ejecutar la lógica empresarial cuando se desencadena una regla.

1. En el panel de control {{site.data.keyword.iot_short_notm}}, vaya a **Reglas** y pulse **Crear regla en la nube**.

2. Especifique un nombre y una descripción para la regla, seleccione un tipo de dispositivo al que se aplicará la regla y pulse **Siguiente**.

3. Para configurar la lógica de la regla, añada una o varias condiciones IF para utilizarlas como desencadenantes para la regla.

    Puede añadir condiciones en filas paralelas para aplicarlas como condiciones OR, o puede añadir condiciones en columnas secuenciales para aplicarlas como condiciones AND. Examine los ejemplos siguientes:

      * Una regla sencilla puede desencadenar una alerta si un valor de parámetro es mayor que un valor especificado:
Condición = `temp_cpu>80`
      * Una regla más compleja puede desencadenarse cuando se cumple una combinación de umbrales:
Condición = `temp_cpu>60 AND cpu_load>90`

    Para desencadenar una condición que compara dos propiedades, o para desencadenar dos o más condiciones de propiedades que se combinan secuencialmente utilizando AND, los puntos de datos desencadenantes se deben incluir en el mismo mensaje de dispositivo. Si los datos se reciben en más de un mensaje, la condición o las condiciones secuenciales no se desencadenarán.
    {: tip}

4. Configure requisitos desencadenantes condicionales para la regla.

    Utilice los requisitos de desencadenante condicional para controlar el número de alertas que se desencadenan para una regla durante un periodo de tiempo. El desencadenante condicional actúa en cualquier condición de la regla. Por ejemplo, si una regla tiene cinco conjuntos de condiciones paralelas utilizando OR, cada condición que sea true se considera para el recuento del desencadenante condicional.

      1. En el editor de reglas, pulse el enlace predeterminado **Desencadenar cada vez que se cumplan las condiciones** para abrir el diálogo de requisitos de frecuencia establecido.
      2. Seleccione y configure el desencadenante condicional que desea utilizar en la regla.

        * Desencadenar cada vez que se cumplan las condiciones
        * Desencadenar si las condiciones se cumplen N veces en la _Unidad de tiempo_ M

5. Cree o seleccione una o varias acciones que se producen si se cumplen las condiciones de la regla.

    Por ejemplo, una acción puede ser enviar un correo electrónico o publicar un webhook.

6. Opcional: Seleccione una prioridad de alerta para la regla.

    La prioridad se utiliza para clasificar las alertas que se muestran en el panel **Paneles > Herramientas de análisis basadas en reglas**. La prioridad predeterminada es Low.

7. Pulse **Guardar** para guardar sin activar o pulse **Activar** para guardar y activar su regla.

    Cuando activa la regla, se añade la alerta al panel **Herramientas de análisis basadas en reglas** cuando se satisfacen las reglas, y las acciones de regla están en ejecución.

## Pasos siguientes

Amplíe las características de análisis de datos de creando y conectando sus propias apps para consumir datos de dispositivos históricos y en tiempo real.

  * Extraiga herramientas de las [bibliotecas de cliente](/docs/services/IoT/iot_platform_client_lib.html) para crear código para integrar y conectar sus dispositivos y apps.

  * Explore las [recetas de Watson IoT Platform ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} para conocer más guías de aprendizaje sobre cómo incluir un análisis más avanzado en sus dispositivos y apps conectados.
