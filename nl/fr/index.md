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

# Initiation à {{site.data.keyword.iot_short_notm}}
{: #gettingstartedtemplate}

{{site.data.keyword.iot_full}} for {{site.data.keyword.Bluemix_notm}} fournit un kit d'outils polyvalent comportant des terminaux passerelle, la gestion des terminaux et un accès puissant aux applications. {{site.data.keyword.iot_short_notm}} vous permet de collecter des données relatives à des terminaux connectés et d'effectuer des analyses sur des données temps réel à partir de votre organisation.
{:shortdesc}

## Avant de commencer
{: #byb}

Avant de connecter des terminaux et d'utiliser des données, inscrivez-vous à un compte {{site.data.keyword.Bluemix_notm}} et créez une instance du service {{site.data.keyword.iot_short_notm}} dans votre organisation {{site.data.keyword.Bluemix_notm}}. Vous pouvez créer une instance {{site.data.keyword.iot_short_notm}} directement depuis la page [{{site.data.keyword.iot_short_notm}} dans le catalogue de services IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}.  

Pour des informations détaillées sur l'inscription à un compte {{site.data.keyword.Bluemix_notm}}, la configuration de régions et d'autres paramètres de gestion de compte, voir [Gestion de votre compte IBM Cloud](https://console.ng.bluemix.net/docs/admin/account.html#signup).

Vous pouvez définir et configurer votre instance {{site.data.keyword.iot_short_notm}} à partir du tableau de bord. Pour ouvrir le tableau de bord, accédez à votre instance de service {{site.data.keyword.iot_short_notm}} dans {{site.data.keyword.Bluemix_notm}}, puis cliquez sur **Lancer**.

## A propos de cette tâche

Les étapes ci-dessous expliquent comment vous familiariser rapidement avec votre service {{site.data.keyword.iot_short_notm}}.

Un ensemble plus détaillé de guides d'initiation et d'exemples d'applications permettant de découvrir les bases du développement d'un prototype Iot de bout-en-bout prêt à être utilisé en production avec {{site.data.keyword.iot_short_notm}} est également disponible. Si vous êtes novice en développement sous {{site.data.keyword.iot_short_notm}}, utilisez les processus détaillés dans la section [Guides d'initiation](https://console.bluemix.net/docs/services/IoT/getting_started/getting-started-iot-overview.html#getting-started).

## Etape 1 : Connecter vos terminaux
{: #up_and_running}

Pour être rapidement opérationnel avec le service, examinez les options suivantes selon votre situation :

|  |   Le service est déployé | Le service n'est pas déployé
 | -------------| ------------- | -------------
  |**J'ai un terminal à connecter** | [Connectez votre terminal à {{site.data.keyword.iot_short_notm}}](iotplatform_task.html#iotplatform_task).| Explorez la connexion de terminal dans la [démonstration Play with {{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play){:new_window}.
  |**Je n'ai aucun terminal à connecter** | [Créez et connectez un simulateur de terminal Node-RED](nodereddevice_sample.html){:new_window}. Ou [Connectez votre Smartphone ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play/device/smartphone){:new_window}. | Commencez à utiliser [Watson IoT Platform Starter](https://console.bluemix.net/docs/starters/IoT-starter/iot500.html).
  
Pour plus d'informations sur la connexion de types de terminaux spécifiques à {{site.data.keyword.iot_short_notm}}, voir les [recettes developerWorks ![Icône de lien externe](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){:new_window}.  

Pour la documentation du développeur de connexion de terminal, voir :
- [Connectivité MQTT pour les terminaux](devices/mqtt.html)
- [Connectivité MQTT pour les passerelles](gateways/mqtt.html)

<!--
## Step 2: Analyze your device data
{: #analyzing_data}
Start exploring the real-time data that the devices are sending to {{site.data.keyword.iot_short_notm}}.
{{site.data.keyword.iot_short_notm}} includes the following analytics tools:  
- [Boards and cards](data_visualization.html) to visualize your real-time device data.
- [Rules and actions](analytics.html) that are triggered by real-time device data.
For a quick getting started example, see the [Using Rules and Actions with IBM Watson IoT Platform Cloud Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/using-rules-and-actions-with-ibm-watson-iot-platform-cloud-analytics/){:new_window} developerWorks recipe.
-->

## Etape 2 : Créer des applications destinées à consommer vos données de terminal
{: #develop_applications}

Créez et connectez vos propres applications pour consommer des données de terminal.

Pour plus d'informations, voir les rubriques suivantes :   
- Explorez la [documentation de développeur d'applications](applications/api.html) et la [{{site.data.keyword.iot_short_notm}}documentation d'API](reference/api.html).
- Explorez les [bibliothèques client {{site.data.keyword.iot_short_notm}} ](iot_platform_client_lib.html) qui fournissent des outils et des fichiers pour générer et développer du code afin d'intégrer et de connecter vos terminaux et vos applications.
- [Connectez un service {{site.data.keyword.cloudantfull}} ](cloudant_connector.html) à votre {{site.data.keyword.iot_short_notm}} pour stocker les données de terminal historiques.
- Créez vos propres règles à l'aide de la fonction nouvelle de [règles imbriquées (bêta)](information_management/im_rules.html).
