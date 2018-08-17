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

# Lernprogramm zur Einführung
{: #getting-started-with-iotp}

In diesem {{site.data.keyword.iot_full}}-Lernprogramm zur Einführung wird eine Verbindung von einem IoT-Gerät zu {{site.data.keyword.iot_short_notm}} hergestellt.
{:shortdesc}

<div id="prerequisites"></div>

## Vorbereitende Schritte
{: #prereqs}

Sie benötigen ein [IBM Cloud-Konto ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/registration/){: new_window},
eine Instanz des {{site.data.keyword.iot_short_notm}}-Service und ein Gerät, das die nachfolgend aufgeführten Anforderungen erfüllt:

*	Ihr Gerät muss in der Lage sein, unter Verwendung der HTTP- oder MQTT-Protokolle zu kommunizieren.

* Die Gerätenachrichten müssen den Anforderungen an die {{site.data.keyword.iot_short_notm}}-Nachrichtennutzdaten entsprechen.

Weitere Informationen finden Sie in [Entwickeln von Geräten in Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Schritt 1: Gerät registrieren

Sie können Geräte einzeln über das [{{site.data.keyword.iot_short_notm}}-Dashboard ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://internetofthings.ibmcloud.com){: new_window} hinzufügen oder die [Watson IoT Platform-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} verwenden, um mehrere Geräte hinzuzufügen.

Gehen Sie wie folgt vor, um ein Gerät über das {{site.data.keyword.iot_short_notm}}-Dashboard hinzuzufügen:

1. Klicken Sie in der {{site.data.keyword.Bluemix_notm}}-Konsole auf der Detailseite für den {{site.data.keyword.iot_short_notm}}-Service auf **Starten**.

    Die {{site.data.keyword.iot_short_notm}}-Webkonsole wird in einer neuen Browserregisterkarte unter der folgenden URL geöffnet:

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Dabei ist *Organisations-ID* die ID [Ihrer {{site.data.keyword.iot_short_notm}}-Organisation](iotplatform_overview.html#organizations){: new_window}.

2. Wählen Sie im Dashboard 'Überblick' im Menüteilfenster die Option **Geräte** aus und klicken Sie anschließend auf **Gerät hinzufügen**.

3. Erstellen Sie einen Gerätetyp für das Gerät, das Sie hinzufügen.

    Jedem Gerät, das mit {{site.data.keyword.iot_short_notm}} verbunden ist, muss ein Gerätetyp zugeordnet sein. Gerätetypen sind Gruppen von Geräten, denen allgemeine Merkmale gemeinsam sind.

    1. Klicken Sie auf **Gerätetyp erstellen**.
    2. Geben Sie einen Gerätenamen ein, z. B. `my_device_type`, sowie eine Beschreibung für den Gerätetyp.
    
        > **Hinweis:** Der Name des Gerätetyps darf maximal 36 Zeichen umfassen und nur alphanumerische Zeichen (a-z, A-Z, 0-9) sowie folgende Zeichen enthalten: `_`, `.` und `-`.

    3. Optional: Geben Sie Attribute und Metadaten zu dem Gerätetyp ein.

        Sie können Attribute und Metadaten später hinzufügen und bearbeiten.
        {: tip}

4. Klicken Sie auf **Weiter**, um mit dem Hinzufügen Ihres Geräts mit dem ausgewählten Gerätetyp zu beginnen.

5. Geben Sie eine Geräte-ID ein, z. B. `my_first_device`.

    Die Geräte-ID wird zum Ermitteln des Geräts im {{site.data.keyword.iot_short_notm}}-Dashboard verwendet und ist auch ein erforderlicher Parameter für das Herstellen einer Verbindung zwischen Ihrem Gerät und {{site.data.keyword.iot_short_notm}}.

    > **Hinweis:** Der Name des Gerätetyps darf maximal 36 Zeichen umfassen und nur alphanumerische Zeichen (a-z, A-Z, 0-9) sowie folgende Zeichen enthalten: `_`, `.` und `-`.

    Bei netzverbundenen Geräten kann die Geräte-ID beispielsweise die MAC-Adresse des Geräts ohne trennende Doppelpunkte sein.

5. Klicken Sie auf **Weiter**, um den Prozess abzuschließen.

6. Geben Sie ein Authentifizierungstoken an oder akzeptieren Sie ein automatisch generiertes Token. Wenn Sie ein eigenes Token erstellen möchten, achten Sie darauf, dass es zwischen 8 und 36 Zeichen lang ist und nur aus alphanumerischen Zeichen und den folgenden Zeichen besteht: `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)` und `-`.

    Das Token darf keine Folgen aus wiederholten Zeichen, Wörterbuchwörter, Benutzernamen oder andere vordefinierte Folgen enthalten.

7. Überprüfen Sie die Übersichtsinformationen auf ihre Richtigkeit und klicken Sie anschließend auf **Hinzufügen**, um die Verbindung hinzuzufügen.

8. Kopieren Sie auf der Seite mit den Geräteinformationen die folgenden Einzelangaben und speichern Sie sie:

    * Organisations-ID
    * Gerätetyp
    * Geräte-ID
    * Authentifizierungsmethode
    * Authentifizierungstoken

    Sie benötigen die Organisations-ID, den Gerätetyp, die Geräte-ID und das Authentifizierungstoken, um Ihr Gerät für die Verbindung mit {{site.data.keyword.iot_short_notm}} zu konfigurieren.

## Schritt 2: Gerät mit {{site.data.keyword.iot_short_notm}} verbinden

1. Richten Sie Ihr Gerät für MQTT-Messaging ein und verwenden Sie zur Authentifizierung die Organisations-ID, den Gerätetyp, die Geräte-ID und das Authentifizierungstoken.

2. Senden Sie mithilfe des MQTT-Protokolls Gerätenachrichten an Ihre {{site.data.keyword.iot_short_notm}}-Organisation.

    Folgende Informationen sind zum Verbinden Ihrer Geräte erforderlich:

    * URL: *Organisations-ID*.messaging.internetofthings.ibmcloud.com

      Dabei ist *Organisations-ID* die ID Ihrer {{site.data.keyword.iot_short_notm}}-Organisation.

    * Port:
      * 1883
      * 8883 (verschlüsselt)
      * 443 (Websockets)
    * Geräte-ID: d:_org_id:device_type:device_id_
    * Benutzername: use-token-auth
    * Kennwort: _Authentication token_
    * Format des Ereignisthemas: iot-2/evt/_event_id/fmt/format_string_

      Hierbei gibt _event_id_ den Ereignisnamen an, der in {{site.data.keyword.iot_short_notm}} angezeigt wird, und _format_string_ das Format des Ereignisses, z. B. JSON.

    * Nachrichtenformat: JSON

  Weitere Informationen finden Sie in [MQTT-Konnektivität für Geräte](/docs/services/IoT/devices/mqtt.html).

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

## Nächste Schritte

Erstellen Sie eigene Apps und verbinden Sie sie, um Echtzeit- und Langzeitdaten für Geräte zu verarbeiten.

  * Rufen Sie die [Clientbibliotheken](/docs/services/IoT/iot_platform_client_lib.html) auf, um auf Tools zuzugreifen, die dem Erstellen von Code für die Integration und Verbindung Ihrer Geräte und Apps dienen.

  * Durchsuchen Sie die [Anleitungen für Watson IoT Platform![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} nach weiteren Lernprogrammen zur Integration erweiterter Analysefunktionen in Ihre verbundenen Geräte und Apps.
