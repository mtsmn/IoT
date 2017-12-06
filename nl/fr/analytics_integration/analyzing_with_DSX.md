---

copyright:
  years: 2017
lastupdated: "2017-09-18"
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

Les données du terminal envoyées à {{site.data.keyword.iot_short_notm}} peuvent être collectées et stockées dans {{site.data.keyword.Bluemix}} à l'aide du service {{site.data.keyword.cloudantfull}} NoSQL DB. Pour collecter les données, vous devez d'abord connecter {{site.data.keyword.iot_short_notm}} au service {{site.data.keyword.cloudant_short_notm}}. Les données de terminal sont stockées dans des bases de données {{site.data.keyword.cloudant_short_notm}} quotidiennes, hebdomadaires ou mensuelles en fonction de l'intervalle configuré.


![Présentation de l'utilisation de DSX à des fins d'analyse de données](images/DSX_overview.png)

Dans le cadre de ce guide, vous allez apprendre :

 - Comment configurer le stockage de données de plateforme pour que Cloudant NoSQL DB soit utilisé en tant que service d'historique.
 - Comment utiliser le simulateur Weather Sensors pour générer les données à utiliser par la plateforme.
 - Comment exporter les données, puis les importer dans DSX à des fins d'analyse de données.
 - Comment visualiser les données IoT, détecter des anomalies et configurer des alertes.


## Prérequis

Pour exécuter les étapes ci-dessous, vous devez avoir accès à [{{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} avec [Cloudant NoSQL DB ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}, ainsi qu'à un [compte DSX ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://datascience.ibm.com/docs/content/getting-started/get-started.html){: new_window}.


## Etape 1. Installation du simulateur
{:#DSX_sensor_data}

Pour effectuer une analyse sérieuse, vous devez disposer de données pertinentes. Vous pouvez simuler des données de détecteur réelles pour savoir comment analyser les données de terminal Watson IoT Platform à l'aide de DSX. Cette étape fournit des instructions pour les actions suivantes :
 - [Installation du simulateur avec une **instance existante de {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Installation du simulateur avec une **nouvelle instance de {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)


### Installation du simulateur Weather Sensors avec une instance existante de {{site.data.keyword.iot_short_notm}}
{: #sim_existing_platform}

Pour simuler des données de détection réelles avec des données de votre organisation à l'aide de Weather Sensors, vous devez d'abord installer le simulateur. Ces étapes impliquent que vous disposez déjà d'une instance de {{site.data.keyword.iot_short_notm}} en cours d'exécution.

1. [Générez la clé d'API et le jeton requis pour exécuter le simulateur. ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Déployez l'application Web du simulateur Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} et suivez la procédure détaillée.

   Pour en savoir plus sur Weather Sensors, voir le [guide du simulateur Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Passez à l'[Etape 2. Configuration du connecteur de base de données](#DSX_config_db).


### Installation du simulateur avec une nouvelle instance de {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Pour simuler des données de détection réelles avec des données de votre organisation à l'aide de Weather Sensors, vous devez d'abord installer le simulateur. Ces étapes incluent les instructions de création d'une instance {{site.data.keyword.iot_short_notm}} avec le simulateur.

1. [Déployez l'application Web du simulateur Weather Sensors avec une instance de {{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} et suivez la procédure détaillée.

   Pour en savoir plus sur Weather Sensors, voir le [guide du simulateur Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Attendez que le déploiement soit terminé et accédez au tableau de bord Bluemix.
3. Lancez le service {{site.data.keyword.iot_short_notm}} "wiotp-for-weather-sensors-simulator" qui a été créé par le processus de déploiement.
4. Passez à l'[Etape 2. Configuration du connecteur de base de données](#DSX_config_db).


## Etape 2. Configuration du connecteur de base de données
{: #DSX_config_db}

Vous pouvez stocker les données de terminal dans des bases de données Cloudant quotidiennes, hebdomadaires ou mensuelles en fonction de l'intervalle que que vous avez sélectionné. Les données collectées à partir des terminaux qui partagent le même intervalle (jour, semaine ou mois) sont stockées dans la même base de données. 

Quelle que soit la configuration d'intervalle, trois bases de données sont automatiquement créées par le connecteur : une base de données pour l'intervalle en cours, une base de données pour l'intervalle à venir et une base de données de configuration. A la fin de l'intervalle, les données de terminal sont stockées dans la base de données pour le nouvel intervalle, et la nouvelle base de données est créée pour l'intervalle suivant.

Pour utiliser {{site.data.keyword.cloudant_short_notm}} avec DSX, vous devez configurer le stockage de données de plateforme de sorte que Cloudant NoSQL DB soit utilisé comme service d'historique.

1. Sur le tableau de bord {{site.data.keyword.cloudant_short_notm}}, cliquez sur **Extensions** dans la barre de navigation.
2. Sous **Stockage des données d'historique**, cliquez sur **Configurer**. La section **Configuration du stockage des données d'historique** affiche la liste de tous les services Cloudant NoSQL DB qui sont disponibles au sein du même espace Bluemix que celui de {{site.data.keyword.cloudant_short_notm}}.
3. Sélectionnez le service Cloudant NoSQL DB que vous souhaitez connecter.
4. Spécifiez les options de configuration Cloudant NoSQL DB suivantes :
  - Intervalle = Jour
  - Fuseau horaire = Temps universel coordonné
  - Nom de la base de données = Valeur par défaut 
5. Cliquez sur **Terminé** et confirmez l'autorisation de connexion au service Cloudant. Assurez-vous que les fenêtres en incrustation sont activées dans votre navigateur pour pouvoir accéder à la fenêtre de confirmation. Une fois que vous avez correctement configuré Cloudant NoSQL DB, le statut du stockage de données d'historique devient Configuré et les données de terminal sont stockées dans {{site.data.keyword.cloudant_short_notm}} NoSQL DB.
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
3. [Configurez les alertes en cas d'anomalie du capteur](#config_alerts).


### 1. Définissez un notebook Jupyter préconfiguré
{: #setup_jupyter_notebook}

Jupyter Notebook est une application Web qui permet de créer et de partager des documents contenant du code exécutable, des formules mathématiques, des graphiques/visualisations (matplotlib) et du texte explicatif.

Pour définir un notebook Jupyter préconfiguré afin d'avoir une vue d'ensemble de vos données et de détecter les éventuelles anomalies :
1. Utilisez un navigateur pris en charge pour vous connecter à [DSX ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://datascience.ibm.com/){: new_window} avec votre ID Bluemix.
2. Cliquez sur "+" et choisissez **Create project** pour créer un nouveau projet. Les projets créent un espace dans lequel vous pouvez collecter et partager des notebooks, vous connecter à des sources de données, créer des pipelines et ajouter des jeux de données.
3. Spécifiez le nom du projet et cliquez sur **Create**. Lors de la configuration du compte DSX, le service Spark et l'instance Object Storage sont automatiquement créés. Vous pouvez également les créer par le biais de l'interface Bluemix et les associer à votre projet DSX ultérieurement.
4. Importez vos données d'identification Cloudant en sélectionnant votre projet dans le menu **DSX** et en cliquant sur ![Icône de recherche et d'ajout de données](images/find_add_data_icon.png).
5. Cliquez sur l'onglet **Connexions**.
6. Cliquez sur **Créer une connexion** pour importer vos données d'identification Cloudant et les rendre disponibles dans n'importe quel notebook de votre projet.
7. Renseignez les informations ci-dessous de l'écran **New Connections** :
   - Nom de connexion
   - Définissez **Service Category** sur `Data Service`.
   - Sélectionnez votre service Cloudant dans le menu déroulant **Instance de service cible**.
   - Choisissez la base de données Cloudant correspondant à la date du jour.
8. Cliquez sur **Créer**.
9. Cliquez sur l'option permettant d'**ajouter des notebooks** pour créer un nouveau notebook Jupyter.
10. Sélectionnez **From URL** pour charger un notebook existant, attribuez-lui un nom descriptif et entrez l'adresse URL suivante pour ouvrir l'exemple de notebook :
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```
11. Cliquez sur **Create Notebook**. Vérifiez que le notebook est créé avec des métadonnées et du code.
12. Sélectionnez la cellule commençant par !pip install --upgrade pixiedust, ,' et cliquez sur **Play** pour exécuter le code.
13. Une fois l'installation terminée, redémarrez le noyau Spark en cliquant sur l'icône **Restart Kernel**.
14. Attendez environ 10 secondes que le noyau redémarre et cliquez sur la cellule avec le commentaire pour la sélectionner.
15. Importez vos données d'identification Cloudant dans cette cellule en procédant comme indiqué ci-après :

    1. Cliquez sur ![Icône de recherche et d'ajout de données](images/find_add_data_icon.png).
    2. Sélectionnez l'onglet **Connections**.
    3. Cliquez sur **Insert to code**.
Un dictionnaire appelé credentials_1" est créé avec vos données d'identification Cloudant. Si le nom spécifié n'est pas "credentials_1", renommez-le en "credentials_1" pour que le code du notebook s'exécute.
16. Dans la cellule comportant le nom de la base de données (dbname), entrez le nom de la base de données Cloudant correspondant à la source de données, par exemple, `iotp_yourWIoTPorgId_default_Year-month-day`.
17. Enregistrez le notebook. Ce dernier est prêt à être exécuté.


### 2. Exécutez l'analyse
{: #run_analysis}

1.	Sélectionnez la cellule comportant vos données d'identification Cloudant.
2.	Cliquez sur **Play** pour exécuter le code de la cellule.
3.	Vérifiez les résultats d'exécution et analysez le code Python qui est utilisé dans chaque cellule.
4.	Répétez les étapes 2 et 3 pour chaque cellule. Pour les cellules pour lesquelles l'**entrée utilisateur est obligatoire**, vous devez fournir de nouvelles valeurs d'entrée à la variable qui est définie dans la cellule de code suivante avant son exécution.

**Remarque :** Certaines cellules exécutent des tâches Spark en arrière-plan ce qui peut prendre plus de temps. Lorsque l'exécution du code à l'intérieur d'une cellule s'achève, l'astérisque `*` prend la forme d'un nombre, par exemple `[*]` devient `[1]`. Une fois la procédure terminée, vous pouvez créer des règles Cloud dans {{site.data.keyword.iot_short_notm}} pour générer automatiquement des alertes en cas d'anomalies.


### 3. Configurez des alertes en cas d'anomalie du capteur
{: #config_alerts}


Vous pouvez créer des règles Cloud dans {{site.data.keyword.iot_short_notm}}. Celles-ci peuvent générer des alertes si des anomalies sont détectées lorsque des événements publiés dépassent les valeurs de seuil que vous avez définies dans le notebook.

Dans l'exemple ci-dessous, nous utilisons le dioxyde d'azote (NO2) et les seuils supérieur et inférieur pour un terminal spécifique. Nous allons créer une action d'envoi de courrier électronique de sorte qu'un e-mail soit envoyé à une adresse électronique spécifique chaque fois que la valeur NO2 dépasse les seuils définis.

Pour créer des règles Cloud :

1. Créez un schéma :
    1. Dans l'onglet **Terminaux** de votre tableau de bord {{site.data.keyword.iot_short_notm}}, sélectionnez l'onglet **Gestion des schémas**.
    2. Cliquez sur **Ajouter un schéma**.
    3. Sélectionnez le type de terminal WS pour lequel le schéma est créé, puis cliquez sur **Suivant**.
    4. Cliquez sur **Ajouter une propriété** pour ajouter le point de données depuis les terminaux connectés.
    5. Dans l'onglet **Manuel**, définissez la zone du type de données sur `Variable flottante` et la zone de propriété sur `NO2`.
    6. Cliquez sur **OK**.
    7. Cliquez sur **Terminer**.

2. Créez une action :
    1. Sélectionnez l'onglet **Règles** dans le tableau de bord {{site.data.keyword.iot_short_notm}}.
    2. Sélectionnez l'onglet **Actions**.
    3. Cliquez sur **Créer une action**.
    4. Dans la fenêtre **Créer une action**, entrez un nom et sélectionnez le type d'action "Envoyer un e-mail".
    5. Cliquez sur **Suivant**.
    6. Dans la fenêtre **Editer une action**, activez l'option **Inclure des données**.
    7. Cliquez sur **Terminer**.
    8. Dans l'onglet **Règles**, sélectionnez **Parcourir**.
    9. Cliquez sur **Créer une règle Cloud**.
    10. Dans la fenêtre **Ajouter une nouvelle règle Cloud**, attribuez un nom à la règle et choisissez le nom de votre schéma dans la zone **S'applique à**. Dans notre exemple, le nom de schéma est "WS".
    11. Cliquez sur **Suivant**.

3. Définissez la condition - Les valeurs de seuil que vous utilisez dans ces étapes se trouvent dans le dernier bloc de code exécuté dans le notebook, en regard du graphique NO2 :
    1. Cliquez sur l'option permettant de créer une nouvelle condition et définissez-la : 
    - Dans la zone **Propriété**, entrez `Dioxyde d'azote`.
    - Dans la zone **Opérateur**, sélectionnez l'icône supérieure ou égale `>`.
    - Dans la zone **Valeur**, entrez la valeur de seuil supérieure.
    - Cliquez sur **OK**.
    2. Sélectionnez OR et ajoutez la seconde condition :
    - Dans la zone **Propriété**, entrez `Dioxyde d'azote`.
    - Dans la zone **Opérateur**, sélectionnez l'icône inférieure ou égale `<`.
    - Dans la zone **Valeur**, entrez la valeur de seuil inférieure.
    - Cliquez sur **OK**. Les conditions qui déclenchent la règle sont maintenant configurées.
4. Définissez l'action "Envoyer un e-mail" et cliquez sur **OK** pour activer la règle. Une alerte e-mail est générée lorsque la valeur du relevé de dioxyde d'azote publié par un terminal dépasse les valeurs de seuil. Vous pouvez exécuter le simulateur pour voir les e-mails d'alerte.


## Etape suivante ?

Pour plus d'informations sur DSX, voir les ressources suivantes :

 - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window}
 - [DSX Community Notebooks and Tutorials ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} following the links to learn more about Jupyter notebooks.
 - [Analytics recipes in the Watson IoT Platform cookbook ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
