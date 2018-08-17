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

# Tutoriel Initiation
{: #getting-started-with-iotp}

Dans ce tutoriel d'initiation à {{site.data.keyword.iot_full}}, nous connectons un terminal IoT à {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

<div id="prerequisites"></div>

## Avant de commencer
{: #prereqs}

Vous aurez besoin d'un [compte IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/registration/){: new_window},
d'une instance du service {{site.data.keyword.iot_short_notm}} et d'un terminal remplissant les conditions suivantes :

*	Votre terminal doit pouvoir communiquer à l'aide des protocoles HTTP ou MQTT.

* Les messages du terminal doivent obéir aux règles concernant la charge utile des messages {{site.data.keyword.iot_short_notm}}.

Pour plus d'informations, consultez [Développement de terminaux sur Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Etape 1 : enregistrez votre terminal

Vous pouvez ajouter les terminaux un à un à partir du tableau de bord [{{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://internetofthings.ibmcloud.com){: new_window} ou utiliser l'[API Watson IoT Platform ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} pour ajouter plusieurs terminaux à la fois.

Pour ajouter un terminal depuis le tableau de bord {{site.data.keyword.iot_short_notm}} :

1. Dans la console {{site.data.keyword.Bluemix_notm}}, cliquez sur **Lancer** dans la page
de détails du service {{site.data.keyword.iot_short_notm}}.

    La console web {{site.data.keyword.iot_short_notm}} s'ouvre dans un nouvel onglet de navigateur à l'URL suivante :

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Où *org_id* est l'ID de [votre organisation {{site.data.keyword.iot_short_notm}} ](iotplatform_overview.html#organizations){: new_window}.

2. Dans le tableau de bord Présentation, à partir du panneau de menu, sélectionnez **Terminaux**, puis cliquez sur **Ajouter un terminal**.

3. Créez un type de terminal pour le terminal que vous ajoutez.

    Chaque terminal connecté à {{site.data.keyword.iot_short_notm}} doit être associé à un type de terminal. Les types de terminaux sont des groupes de terminaux ayant des caractéristiques communes.

    1. Cliquez sur **Créer un type de terminal**.
    2. Entrez un nom de type de terminal, par exemple, `my_device_type`, et une description pour
le type de terminal.
    
        > **Remarque :** Le nom du type de terminal ne doit pas dépasser 36 caractères et ne doit contenir que des caractères alphanumériques (a-z, A-Z, 0-9) et les caractères suivants : `_`, `.`, `-`.

    3. Facultatif : Entrez des métadonnées et des attributs de type de terminal.

        Vous pourrez ajouter et éditer des attributs et des métadonnées ultérieurement.
        {: tip}

4. Cliquez sur **Suivant** pour commencer le processus d'ajout de votre terminal avec le type de terminal sélectionné.

5. Entrez un ID de terminal. Par exemple, `my_first_device`.

    L'ID de terminal permet d'identifier le terminal dans le tableau de bord {{site.data.keyword.iot_short_notm}}. Il constitue
aussi un paramètre indispensable à la connexion de votre terminal à {{site.data.keyword.iot_short_notm}}.

    > **Remarque :** Le nom du type de terminal ne doit pas dépasser 36 caractères et ne doit contenir que des caractères alphanumériques (a-z, A-Z, 0-9) et les caractères suivants : `_`, `.`, `-`.

    Pour les terminaux connectés à un réseau, l'ID de chaque terminal pourrait être son adresse MAC sans les signes deux-points de séparation.

5. Cliquez sur **Suivant** pour finaliser le processus.

6. Fournissez un jeton d'authentification ou acceptez celui qui est généré automatiquement. Si vous choisissez de créer votre propre jeton, assurez-vous qu'il comporte entre 8 et 36 caractères composés uniquement de caractères alphanumériques et des caractères suivants : `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)` et `-`.

    Le jeton ne doit pas contenir des séquences de caractères répétés, des mots de dictionnaire, des noms d'utilisateur ni d'autres séquences prédéfinies.

7. Vérifiez que les informations récapitulatives sont correctes, puis cliquez sur **Ajouter** pour ajouter la connexion.

8. Sur la page Informations sur le terminal, copiez et sauvegardez les détails suivants :

    * ID d'organisation
    * Type de terminal
    * ID de terminal
    * Méthode d'authentification
    * Jeton d'authentification

    Vous aurez besoin d'un ID d'organisation, d'un type de terminal, d'un ID de terminal et d'un jeton d'authentification pour configurer votre terminal afin qu'il se connecte à {{site.data.keyword.iot_short_notm}}.

## Etape 2 : connectez votre terminal à {{site.data.keyword.iot_short_notm}}

1. Configurer votre terminal pour la messagerie MQTT et de sorte qu'il soit authentifié avec
l'ID d'organisation, le type de terminal, l'ID de terminal et le jeton d'authentification.

2. Envoyer des messages du terminal à votre organisation {{site.data.keyword.iot_short_notm}} en utilisant le protocole MQTT.

    Les informations suivantes sont requises lors de la connexion de votre terminal :

    * URL : *org_id*.messaging.internetofthings.ibmcloud.com

      Où *org_id* est l'ID de votre organisation {{site.data.keyword.iot_short_notm}}.

    * Port :
      * 1883
      * 8883 (chiffré)
      * 443 (websockets)
    * ID de terminal : d:_org_id:device_type:device_id_
    * Nom d'utilisateur : use-token-auth
    * Mot de passe : _jeton d'authentification_
    * Format de sujet d'événement : iot-2/evt/_event_id/fmt/format_string_

      Où _event_id_ spécifie le nom d'événement affiché dans {{site.data.keyword.iot_short_notm}} et _format_string_ est le format de l'événement, par exemple, JSON.

    * Format de message : JSON

  Pour plus d'informations, consultez [Connectivité MQTT pour les terminaux](/docs/services/IoT/devices/mqtt.html).

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

## Etapes suivantes

Créez et connectez vos propres applications pour consommer des données temps réel et historiques.

  * Consultez les [bibliothèques client](/docs/services/IoT/iot_platform_client_lib.html) en quête d'outils de construction du
code pour l'intégration et la connexion de vos terminaux et applis.

  * Explorez les [recettes Watson IoT Platform ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} pour d'autres
tutoriels expliquant comment incorporer des analyses avancées dans vos terminaux connectés et applications.
