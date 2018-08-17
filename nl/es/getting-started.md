---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Guía de aprendizaje de iniciación
{: #getting-started-with-iotp}

En esta guía de aprendizaje de iniciación de {{site.data.keyword.iot_full}}, se conectará un dispositivo IoT a {{site.data.keyword.iot_short_notm}}.
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

<!--## Step 3: Create boards and cards to keep track of device data

By using boards and cards, you can view graphics that represent data set values from one or more devices for a quick overview and understanding of the device data.

1. In your {{site.data.keyword.iot_short_notm}} dashboard, click **Create New Board**.

2. Enter a name and description for the board.

3. Click **Next** and then **Create**.

4. Click the board you just created, and then click **Add New Card**. Select Devices as the card type. The following table describes the visualization options you can choose from.

| Type | Data that is displayed |
| Generic visualization | The value of one or more data sets. Choose the large widget size to see up to three data point values in a small table. |
| Line chart | One or more data sets in a real-time scrolling chart. Use the Settings menu to set the data range and retention, the look and feel of the graphs, and more. |
| Bar chart | Data set values in labelled bars. Use the Settings menu to toggle horizontal or vertical bar direction. |
| Donut chart | Two or more data sets in a circular representation. |
| Value | The raw value of one or more data sets. |
| Gauge | The value of a data set shown as a gauge. Use the Settings menu to optionally set gauge thresholds for lower, middle, and upper data ranges. |
| Device properties | Specific properties for one or more devices. |
| All device properties | All properties for one or more devices. |
| Device list | A list to monitor multiple devices. A list can be used as a data source for other cards. |
| Device info | Basic information for a single device. |
| Device map | The location of devices in a device list. |

{: caption="Table 1. Visualization options" caption-side="top"}

## Step 4: Create device type schemas

At this point, you need to create a device type schema and map device properties to then create rules that are triggered based on the datapoints from your mapped device properties.

1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Devices > Manage Schemas** and click **Add Schema**.

2. Select a device type to associate with the message schema. Only one schema can be defined for a device type.

3. Click **Add property** and add one or more properties.

    You can select properties from a connected device, create virtual properties that modify or combine existing properties, or add properties manually.

    The available properties are defined in the payload of the messages that are sent by a device.
    {: tip}

    * To add a property manually, select the **Manual tab** and define the following property details:
        * Name
        * Data type
        * Property
        * Data unit (optional)
        * Decimal places (optional)

    * To create a virtual property, select the **Virtual Property** tab and define the following property details:
        * Name
        * Data type
        * Property
        * Calculation
        * Data unit (optional)
        * Decimal places

    * To select properties from a connected device, select the **From Connected** tab, and then select one or more properties to add to the schema. The selected properties are added and the description is set to the name of the property.

4. Click **Finish** to create the properties.

## Step 5: Create rules and actions

Rules are condition-based decision points that match real-time device data with predefined threshold values or other property data to trigger an alert if a condition is met. In addition to the alert that's displayed in the {{site.data.keyword.iot_short_notm}} dashboard, you can add one or more actions to run business logic when a rule is triggered.

1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Rules** and click **Create Cloud Rule**.

2. Enter a name and description for the rule, select a device type that the rule applies to, and click **Next**.

3. To set up the rule logic, add one or more IF conditions to use as triggers for the rule.

    You can add conditions in parallel rows to apply them as OR conditions, or you can add conditions in sequential columns to apply them as AND conditions. Take a look at the following examples:

      * A simple rule might trigger an alert if a parameter value is larger than a specified value: Condition = `temp_cpu>80`
      * A more complex rule might trigger when a combination of thresholds are met: Condition = `temp_cpu>60 AND cpu_load>90`

    To trigger a condition that compares two properties, or to trigger two or more property conditions that are combined sequentially by using AND, include the triggering data points in the same device message. If the data is received in more than one message, the condition or sequential conditions don't trigger.
    {: tip}

4. Configure conditional trigger requirements for your rule.

    You can use conditional trigger requirements to control the number of alerts that are triggered for a rule over a time period. The conditional triggering acts on any condition in the rule. For example, if a rule has five parallel conditions set by using OR, each condition that's true counts towards the conditional trigger count.

      1. In the rule editor, click the default **Trigger each time conditions are met** link to open the set frequency requirement dialog.
      2. Select and configure the conditional trigger you want to use in the rule.

        * Trigger every time conditions are met
        * Trigger if conditions are met N times in M _Unit of time_

5. Create or select one or more actions that occur if the rule conditions are met.

    For example, an action can be to send an email or post a webhook.

6. Optional: Select an alert priority for the rule.

    The priority is used to classify the alerts that are displayed in the **Boards > Rule-Based Analytics** board. The default priority is Low.

7. Click **Save** to save without activating,  or click **Activate** to save and activate your rule.

    When you activate the rule, an alert is added to the **Rule-Based Analytics** board when the conditions are met, and any rule action is run. -->

## Pasos siguientes

Cree y conecte sus propias apps de modo que consuman datos históricos y en tiempo real del dispositivo.

  * Extraiga herramientas de las [bibliotecas de cliente](/docs/services/IoT/iot_platform_client_lib.html) para crear código para integrar y conectar sus dispositivos y apps.

  * Explore las [recetas de Watson IoT Platform ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} para conocer más guías de aprendizaje sobre cómo incluir un análisis más avanzado en sus dispositivos y apps conectados.
