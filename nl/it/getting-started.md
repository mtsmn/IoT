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

# Esercitazione introduttiva
{: #getting-started-with-iotp}

In questa esercitazione di introduzione a {{site.data.keyword.iot_full}}, connettiamo un dispositivo IoT a {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

<div id="prerequisites"></div>

## Prima di cominciare
{: #prereqs}

Ti servirà un [account IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/registration/){: new_window},
una istanza del servizio {{site.data.keyword.iot_short_notm}} e un dispositivo che soddisfa i seguenti requisiti:

*	Il tuo dispositivo deve essere in grado di comunicare utilizzando i protocolli HTTP o MQTT.

* I messaggi del dispositivo devono essere conformi ai requisiti del payload del messaggio di {{site.data.keyword.iot_short_notm}}.

Per ulteriori informazioni, consulta [Sviluppo dei dispositivi su Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Passo 1: registra il tuo dispositivo

Puoi aggiungere i dispositivi uno per volta dal [dashboard {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://internetofthings.ibmcloud.com){: new_window} o puoi utilizzare l'API [Watson IoT Platform ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} per aggiungere più dispositivi.

Per aggiungere un dispositivo dal dashboard {{site.data.keyword.iot_short_notm}}:

1. Nella console {{site.data.keyword.Bluemix_notm}}, fai clic su **Avvia** nella pagina dei dettagli del servizio {{site.data.keyword.iot_short_notm}}.

    Si apre la console web {{site.data.keyword.iot_short_notm}} in una nuova scheda del browser al seguente URL:

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Dove *org_id* è l'ID della [tua {{site.data.keyword.iot_short_notm}} organizzazione](iotplatform_overview.html#organizations){: new_window}.

2. Nel dashboard della panoramica, dal pannello del menu, seleziona **Devices** e fai clic su **Add Device**.

3. Crea un tipo di dispositivo per il dispositivo che stai aggiungendo.

    Ogni dispositivo collegato a {{site.data.keyword.iot_short_notm}} deve essere associato a un tipo dispositivo. I tipi di dispositivo sono gruppi di dispositivi che condividono caratteristiche comuni.

    1. Fai clic su **Create device type**.
    2. Immetti un nome dispositivo, come `my_device_type`, e una descrizione per il tipo di dispositivo.
    
        > **Nota:** il nome del tipo di dispositivo non deve avere una lunghezza superiore ai 36 caratteri e può contenere solo caratteri alfanumerici (a-z, A-Z, 0-9) e i seguenti caratteri: `_`, `.` e `-`.

    3. Facoltativo: immetti i metadati e gli attributi per il tipo dispositivo.

        Puoi aggiungere e modificare gli attributi e i metadati successivamente.
        {: tip}

4. Fai clic su **Next** per avviare il processo di aggiunta del tuo dispositivo con il tipo dispositivo.

5. Immetti un ID dispositivo, ad esempio `my_first_device`.

    L'ID dispositivo viene utilizzato per identificare il dispositivo nel dashboard {{site.data.keyword.iot_short_notm}} ed è anche un parametro obbligatorio per la connessione del tuo dispositivo a {{site.data.keyword.iot_short_notm}}.

    > **Nota:** il nome del tipo di dispositivo non deve avere una lunghezza superiore ai 36 caratteri e può contenere solo caratteri alfanumerici (a-z, A-Z, 0-9) e i seguenti caratteri: `_`, `.` e `-`.

    Per i dispositivi connessi alla rete, l'ID dispositivo può essere l'indirizzo MAC del dispositivo senza i caratteri due punti di separazione.

5. Fai clic su **Next** per completare il processo.

6. Fornisci un token di autenticazione oppure accetta un token generato automaticamente. Se scegli di creare un tuo token, assicurati che abbia un nome di lunghezza compresa tra gli 8 e i 36 caratteri e che contenga solo caratteri alfanumerici e i seguenti caratteri: `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)`, and `-`.

    Il token non deve contenere sequenze di caratteri ripetuti, parole del dizionario, nomi utente o altre sequenze predefinite.

7. Verifica che le informazioni di riepilogo siano corrette e quindi fai clic su **Add** per aggiungere la connessione.

8. Nella pagina delle informazioni del dispositivo, copia e salva i seguenti dettagli:

    * ID organizzazione
    * Tipo di dispositivo
    * ID dispositivo
    * Metodo di autenticazione
    * Token di autenticazione

    Ti serviranno l'ID organizzazione, il tipo di dispositivo, l'ID del dispositivo e il token di configurazione per configurare il tuo dispositivo per la connessione a {{site.data.keyword.iot_short_notm}}.

## Passo 2: connetti il tuo dispositivo a {{site.data.keyword.iot_short_notm}}

1. Configura il tuo dispositivo per la messaggistica MQTT e per l'autenticazione utilizzando l'ID organizzazione, il tipo di dispositivo, l'ID dispositivo e il token di autenticazione.

2. Invio dei messaggi del dispositivo alla tua organizzazione {{site.data.keyword.iot_short_notm}} utilizzando il protocollo MQTT.

    Le seguenti informazioni sono necessarie quando connetti il tuo dispositivo:

    * URL: *org_id*.messaging.internetofthings.ibmcloud.com

      Dove *org_id* è l'ID della tua organizzazione {{site.data.keyword.iot_short_notm}}.

    * Porta:
      * 1883
      * 8883 (crittografata)
      * 443 (socket web)
    * ID dispositivo: d:_id_organizzzione:tipo_dispositivo:id_dispositivo_
    * Nome utente: use-token-auth
    * Password: _Token di autenticazione_
    * Formato argomento evento: iot-2/evt/_id_evento/fmt/stringa_formato_

      Dove _id_evento_ specifica il nome evento visualizzato in {{site.data.keyword.iot_short_notm}} e _stringa_formato_ è il formato dell'evento, come ad esempio JSON.

    * Formato messaggio: JSON

  Per ulteriori informazioni, consulta [Connettività MQTT per i dispositivi](/docs/services/IoT/devices/mqtt.html).

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

## Fasi successive

Crea e collega le tue applicazioni per utilizzare i dati del dispositivo cronologici e in tempo reale.

  * Consulta le [librerie client](/docs/services/IoT/iot_platform_client_lib.html) per gli strumenti per creare il codice per integrare e connettere i tuoi dispositivi e le tue applicazioni.

  * Esplora le [recipe di Watson IoT Platform ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} per ulteriori esercitazioni sull'integrazione dell'analisi avanzata nei tuoi dispositivi e nelle tue applicazioni connessi.
