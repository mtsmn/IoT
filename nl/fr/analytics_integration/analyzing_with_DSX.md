---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-01"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Analyse de données à l'aide de Data Science Experience
{: #DSX_integration}

Vous pouvez utiliser {{site.data.keyword.iot_full}} avec Data Science Experience (DSX) pour visualiser et découvrir les données qui sont envoyées à partir de terminaux connectés à la plateforme.
{: shortdesc}

## Présentation et objectifs

Le présent guide vous accompagne à travers chacune des étapes du processus de visualisation des données d'événement de terminal {{site.data.keyword.iot_short_notm}} à l'aide de l'outil d'analyse DSX.

DSX fournit un environnement interactif et collaboratif basé sur le Cloud dans lequel vous pouvez utiliser plusieurs outils afin d'activer les fonctions d'analyse. Utilisez Jupyter Notebook, qui est disponible dans IBM DSX, pour charger les données d'historique {{site.data.keyword.iot_short_notm}}, puis utilisez ces données pour créer des visualisations facilitant l'analyse des données et la détection d'anomalies. Vous pouvez établir des valeurs de seuil à partir des données d'historique et les utiliser pour créer des règles Cloud dans {{site.data.keyword.iot_short_notm}}. Les règles Cloud avertissent les utilisateurs lorsqu'un terminal IoT associé à une règle publie un relevé qui se trouve en dehors des seuils configurés.

Les données du terminal envoyées à {{site.data.keyword.iot_short_notm}} peuvent être collectées et stockées dans {{site.data.keyword.Bluemix}} à l'aide du service {{site.data.keyword.cloudantfull}} NoSQL DB. Pour collecter les données, vous devez d'abord connecter {{site.data.keyword.iot_short_notm}} au service {{site.data.keyword.cloudant_short_notm}}. Les données de terminal sont stockées dans des bases de données {{site.data.keyword.cloudant_short_notm}} quotidiennes, hebdomadaires ou mensuelles en fonction de l'intervalle choisi.


![Présentation de l'utilisation de DSX à des fins d'analyse de données](images/DSX_overview.png)

Dans le cadre de ce guide, vous apprendrez à :

 - Configurer le stockage de données de plateforme de sorte que Cloudant NoSQL DB soit utilisé en tant que service d'historique.
 - Utiliser le simulateur Weather Sensors pour générer les données à utiliser par la plateforme.
 - Exporter les données, puis à les importer dans DSX à des fins d'analyse de données.
 - Visualiser les données IoT, détecter des anomalies et configurer des alertes.


## Prérequis

Pour exécuter les étapes ci-dessous, vous devez avoir accès à [{{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} avec [Cloudant NoSQL DB ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}, au service [Apache Spark ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} et à un [compte DSX ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window}.


## Etape 1. Installation du simulateur
{:#DSX_sensor_data}

Pour effectuer une analyse sérieuse, vous devez disposer de données pertinentes. Vous pouvez simuler des données de détecteur réelles pour savoir comment analyser les données de terminal Watson IoT Platform à l'aide de DSX. Cette étape fournit des instructions pour les actions suivantes :
 - [Installation du simulateur avec une **instance existante de {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Installation du simulateur avec une **nouvelle instance de {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)


### Installation du simulateur Weather Sensors avec une instance existante de {{site.data.keyword.iot_short_notm}}
{: #sim_existing_platform}

Pour simuler des données de détection réelles avec des données de votre organisation à l'aide de Weather Sensors, vous devez d'abord installer le simulateur. Ces étapes impliquent que vous disposez déjà d'une instance de {{site.data.keyword.iot_short_notm}} en cours d'exécution.

1. [Générez la clé d'API et le jeton requis pour exécuter le simulateur.![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Déployez l'application Web du simulateur Weather Sensors ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} et suivez la procédure détaillée.

   Pour en savoir plus sur Weather Sensors, voir le [guide du simulateur Weather Sensors ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Passez à l'[Etape 2. Configuration du connecteur de base de données](#DSX_config_db).


### Installation du simulateur avec une nouvelle instance de {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Pour simuler des données de détection réelles avec des données de votre organisation à l'aide de Weather Sensors, vous devez d'abord installer le simulateur. Ces étapes incluent les instructions de création d'une instance {{site.data.keyword.iot_short_notm}} avec le simulateur.

1. [Déployez l'application Web du simulateur Weather Sensors avec une instance de {{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} et suivez la procédure détaillée.

   Pour en savoir plus sur Weather Sensors, voir le [guide du simulateur Weather Sensors ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Attendez que le déploiement soit terminé puis accédez au tableau de bord IBM Cloud.
3. Lancez le service {{site.data.keyword.iot_short_notm}} "wiotp-for-weather-sensors-simulator" qui a été créé par le processus de déploiement.
4. Passez à l'[Etape 2. Configuration du connecteur de base de données](#DSX_config_db).


## Etape 2. Configuration du connecteur de base de données
{: #DSX_config_db}

Vous pouvez stocker les données de terminal dans des bases de données Cloudant quotidiennes, hebdomadaires ou mensuelles en fonction de l'intervalle que vous avez sélectionné. Les données collectées à partir des terminaux qui partagent le même intervalle (jour, semaine ou mois) sont stockées dans la même base de données.

Quelle que soit la configuration d'intervalle, trois bases de données sont automatiquement créées par le connecteur : une base de données pour l'intervalle en cours, une base de données pour l'intervalle à venir et une base de données de configuration. A la fin de l'intervalle, les données de terminal sont stockées dans la base de données pour le nouvel intervalle, et la nouvelle base de données est créée pour l'intervalle suivant.

Pour utiliser {{site.data.keyword.cloudant_short_notm}} avec DSX, vous devez configurer le stockage de données de plateforme de sorte que Cloudant NoSQL DB soit utilisé comme service d'historique.

1. Sur le tableau de bord {{site.data.keyword.cloudant_short_notm}}, cliquez sur **Extensions** dans la barre de navigation.
2. Sous **Stockage des données d'historique**, cliquez sur **Configurer**. La section **Configuration du stockage des données d'historique** affiche la liste de tous les services Cloudant NoSQL DB qui sont disponibles au sein du même espace IBM Cloud que celui de {{site.data.keyword.cloudant_short_notm}}.
3. Sélectionnez le service Cloudant NoSQL DB que vous souhaitez connecter.
4. Spécifiez les options de configuration Cloudant NoSQL DB suivantes :
  - Intervalle = Jour
  - Fuseau horaire = Temps universel coordonné
  - Nom de la base de données = Valeur par défaut
5. Cliquez sur **Terminé** et confirmez l'autorisation de connexion au service Cloudant. Assurez-vous que les fenêtres en incrustation sont activées dans votre navigateur pour pouvoir accéder à la fenêtre de confirmation. Une fois que vous avez correctement configuré Cloudant NoSQL DB, le statut du stockage de données d'historique devient Configuré et les données de terminal sont stockées dans {{site.data.keyword.cloudant_short_notm}}.
6. Passez à l'[Etape 3. Exécution du simulateur](#run_simulator).


## Etape 3. Exécution du simulateur
{: #run_simulator}

Le simulateur publie des données de sonde météo réelles, provenant de 17 stations de la zone Haifa, dans votre organisation {{site.data.keyword.iot_short_notm}}.

1. Accédez au simulateur.
2. Si vous avez déployé le simulateur avec une instance {{site.data.keyword.iot_short_notm}} liée, passez à l'étape 3. Si vous avez déployé une version autonome du simulateur, entrez les informations suivantes :
   - Organisation Watson IoT Platform
   - Clé d'API
   - Jeton d'authentification

3. Cliquez sur **Exécuter le simulateur**. La génération des données nécessite quelques minutes.
4. Une fois le simulateur démarré, accédez à Watson IoT Platform et vérifiez que les terminaux ont été créés et que les événements arrivent dans ces terminaux.
5. Passez à l'[Etape 4. Configuration de DSX et visualisation des données](#DSX_visualize_data).


## Etape 4. Configuration de DSX et visualisation des données
{: #DSX_visualize_data}

Pour configurer DSX et commencer à visualiser les données :

1. [Définissez un notebook Jupyter préconfiguré](#setup_jupyter_notebook) pour avoir une vue d'ensemble de vos données et détecter les éventuelles anomalies.
2. [Exécutez l'analyse.](#run_analysis)
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. Définissez un notebook Jupyter préconfiguré
{: #setup_jupyter_notebook}

Jupyter Notebook est une application Web qui permet de créer et de partager des documents contenant du code exécutable, des formules mathématiques, des graphiques/visualisations (matplotlib) et du texte explicatif.

Pour définir un notebook Jupyter préconfiguré afin d'avoir une vue d'ensemble de vos données et de détecter les éventuelles anomalies :
1. Utilisez un navigateur pris en charge pour vous connecter à [DSX ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://datascience.ibm.com/){: new_window} avec votre ID IBM Cloud.
2. Cliquez sur **+ Nouveau projet** pour créer un nouveau projet et sélectionnez la vignette **Jupyter Notebooks**. Les projets créent un espace dans lequel vous pouvez collecter et partager des notebooks, vous connecter à des sources de données, créer des pipelines et ajouter des jeux de données.
3. Cliquez sur la liste déroulante **+ Ajouter au projet** et sélectionnez **Connexion**. Dans la liste des services, sélectionnez **Cloudant** puis entrez dans les zones indiquées l'adresse URL Cloudant, le nom d'utilisateur et le mot de passe que vous trouverez dans l'onglet Données d'identification du service de la page du service {{site.data.keyword.cloudant_short_notm}}. Spécifiez un nom pour la connexion et cliquez sur **Créer**.
4. Vérifiez que la connexion est disponible dans la section Data assets du tableau de bord.
5. Cliquez sur **+ New notebook** pour créer un nouveau notebook Jupyter.
6. Dans la liste déroulante Select runtime sous Services, sélectionnez **spark-iw**. Si l'option ne figure pas dans la liste, cela signifie que le service Apache Spark n'a pas été fourni correctement. Vérifiez qu'il figure dans le tableau de bord IBM Cloud, et si ce n'est pas le cas, accédez à la page de service pour l'obtenir.
7. Définissez le langage sur **Python 2** et la version de Spark sur **2.1**.
8. Sélectionnez **Charger à partir d'une URL** pour charger un notebook existant, attribuez-lui un nom descriptif et entrez l'adresse URL suivante pour ouvrir l'exemple de notebook :
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. Cliquez sur **Create Notebook**. Vérifiez que le notebook est créé avec des métadonnées et du code.
10. Sélectionnez la cellule commençant par '!pip install --upgrade pixiedust, ,' puis cliquez sur **Run** pour exécuter le code.
11. Lorsque l'installation est terminée, redémarrez le noyau Spark en cliquant sur l'icône **Restart Kernel** ou en sélectionnant **Restart** dans le menu du noyau.
12. Attendez environ 10 secondes que le noyau redémarre puis cliquez sur la cellule de code vide avec le commentaire pour la sélectionner.
13. Importez vos données d'identification Cloudant dans cette cellule en procédant comme indiqué ci-après :

    1. Cliquez sur ![Find and add data](images/find_add_data_icon.png).
    2. Sélectionnez l'onglet **Connections**.
    3. Cliquez sur **Insert to code**.
Un dictionnaire appelé credentials_1" est créé avec vos données d'identification Cloudant. Si le nom spécifié n'est pas "credentials_1", renommez-le en "credentials_1" pour que le code du notebook s'exécute.
14. Dans la cellule qui comporte le nom de la base de données (dbname), entrez le nom de la base de données Cloudant correspondant à la source de données, par exemple, `iotp_yourWIoTPorgId_default_Year-month-day`.
15. Enregistrez le notebook. Ce dernier est prêt à être exécuté.


### 2. Exécutez l'analyse
{: #run_analysis}

1.	Sélectionnez la cellule qui contient vos données d'identification Cloudant.
2.	Cliquez sur **Play** pour exécuter le code de la cellule.
3.	Vérifiez les résultats d'exécution et analysez le code Python qui est utilisé dans chaque cellule.
4.	Répétez les étapes 2 et 3 pour chaque cellule. Lorsqu'une **entrée utilisateur est obligatoire**, fournissez de nouvelles valeurs d'entrée à la variable définie dans la cellule de code voisine avant de l'exécuter.

**Remarque :** Certaines cellules exécutant des tâches Spark en arrière-plan, l'exécution du code peut prendre plus de temps. Lorsque l'exécution du code à l'intérieur d'une cellule s'achève, l'astérisque `*` prend la forme d'un nombre, par exemple `[*]` devient `[1]`. Une fois la procédure terminée, vous pouvez créer des règles Cloud dans {{site.data.keyword.iot_short_notm}} pour générer automatiquement des alertes en cas d'anomalies.


<!-- ### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails. -->


## Etape suivante ?

Pour plus d'informations sur DSX, voir les ressources suivantes :

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [DSX Community Notebooks and Tutorials ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} following the links to learn more about Jupyter notebooks.
 - [Analytics recipes in the Watson IoT Platform cookbook ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
