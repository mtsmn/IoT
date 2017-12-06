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


# Analyse de données à l'aide de Watson Analytics
{: #WA_integration}  

Vous pouvez utiliser {{site.data.keyword.iot_full}} avec Watson Analytics (WA) à des fins de visualisation et de compréhension des données envoyées par les terminaux connectés à la plateforme.
{: shortdesc}

## Présentation et objectifs

Ce guide présente les étapes détaillées du processus de visualisation des données d'événement du terminal {{site.data.keyword.iot_short_notm}} à l'aide de l'outil d'analyse Watson Analytics (WA).

Les données du terminal envoyées à {{site.data.keyword.iot_short_notm}} peuvent être collectées et stockées dans {{site.data.keyword.Bluemix}} à l'aide du service {{site.data.keyword.cloudantfull}} NoSQL DB. Pour collecter les données, vous devez d'abord connecter {{site.data.keyword.iot_short_notm}} au service {{site.data.keyword.cloudant_short_notm}}. Une fois les données collectées, exportez-les dans un fichier CSV. Téléchargez ce fichier dans WA pour voir et analyser les données de terminal. Les données de terminal sont stockées dans des bases de données {{site.data.keyword.cloudant_short_notm}} quotidiennes, hebdomadaires ou mensuelles en fonction de l'intervalle configuré.

![Présentation de l'utilisation de WA à des fins d'analyse de données](images/WA_overview.png)

Dans le cadre de ce guide, vous allez apprendre :

 - Comment configurer le stockage de données de plateforme pour que Cloudant NoSQL DB soit utilisé en tant que service d'historique.
 - Comment utiliser le simulateur Weather Sensors pour générer les données à utiliser par la plateforme.
 - Comment exporter les données, puis les importer dans WA à des fins d'analyse de données.


## Prérequis

Pour exécuter les étapes ci-dessous, vous devez avoir accès à [{{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} avec [Cloudant NoSQL DB ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}, ainsi qu'à [Watson Analytics ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson-analytics){: new_window}.


## Etape 1. Installation du simulateur
{: #WA_sensor_data}


Pour effectuer une analyse sérieuse, vous devez disposer de données pertinentes. Vous pouvez simuler des données de capteur en temps réel pour savoir comment analyser les données de terminal Watson IoT Platform à l'aide de Wastson Analytic. Cette étape fournit des instructions pour les actions suivantes :
 - [Installation du simulateur avec une **instance existante de {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Installation du simulateur avec une **nouvelle instance de {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)
 - [Téléchargement d'un modèle de fichier CSV prédéfini avec données](#WA_sensor_premade), si vous ne souhaitez pas faire appel au simulateur


### Installation du simulateur Weather Sensors avec une instance existante de {{site.data.keyword.iot_short_notm}}
{: #sim_existing_platform}

Pour simuler des données de détection réelles avec des données de votre organisation à l'aide de Weather Sensors, vous devez d'abord installer le simulateur. Ces étapes impliquent que vous disposez déjà d'une instance de {{site.data.keyword.iot_short_notm}} en cours d'exécution.

1. [Générez la clé d'API et le jeton requis pour exécuter le simulateur.![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Déployez l'application Web du simulateur Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} et suivez la procédure détaillée.

   Pour en savoir plus sur Weather Sensors, voir le [guide du simulateur Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Passez à l'[Etape 2. Configuration du connecteur de base de données](#WA_config_db).


### Installation du simulateur avec une nouvelle instance de {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Pour simuler des données de détection réelles avec des données de votre organisation à l'aide de Weather Sensors, vous devez d'abord installer le simulateur. Ces étapes incluent les instructions de création d'une instance {{site.data.keyword.iot_short_notm}} avec le simulateur.

1. [Déployez l'application Web du simulateur Weather Sensors avec une instance de {{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} et suivez la procédure détaillée.

   Pour en savoir plus sur Weather Sensors, voir le [guide du simulateur Weather Sensors ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Attendez que le déploiement soit terminé et accédez au tableau de bord Bluemix.
3. Lancez le service {{site.data.keyword.iot_short_notm}} "wiotp-for-weather-sensors-simulator" qui a été créé par le processus de déploiement.
4. Passez à l'[Etape 2. Configuration du connecteur de base de données](#WA_config_db).


### Utilisation des données de capteur provenant d'un modèle de fichier CSV prédéfini
{: #WA_sensor_premade}

Pour simuler les événements de données de capteur en temps réel sur vos organisations à l'aide d'un fichier CSV prédéfini :

1. [Téléchargez le fichier CSV Cloudant ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator/releases/download/v1.0/cloudant.csv){: new_window}.
2. Passez à l'[Etape 5. Configuration de WA et visualisation des données](#WA_import_data).


## Etape 2. Configuration du connecteur de base de données
{: #WA_config_db}

Pour utiliser {{site.data.keyword.cloudant_short_notm}} avec Watson Analytics, vous devez configurer le stockage de données de la plateforme de manière à utiliser la base de données Cloudant NoSQL en tant que service d'historique.

1. Sur le tableau de bord {{site.data.keyword.cloudant_short_notm}}, cliquez sur **Extensions** dans la barre de navigation.
2. Sous **Stockage des données d'historique**, cliquez sur **Configurer**. La section **Configuration du stockage des données d'historique** affiche la liste de tous les services Cloudant NoSQL DB qui sont disponibles au sein du même espace Bluemix que celui de {{site.data.keyword.cloudant_short_notm}}.
3. Sélectionnez le service Cloudant NoSQL DB que vous souhaitez connecter.
4. Spécifiez les options de configuration Cloudant NoSQL DB suivantes :
  - Intervalle = Jour
  - Fuseau horaire = Temps universel coordonné
  - Nom de la base de données = Valeur par défaut 
5. Cliquez sur **Terminé** et confirmez l'autorisation de connexion au service Cloudant. Assurez-vous que les fenêtres en incrustation sont activées dans votre navigateur pour pouvoir accéder à la fenêtre de confirmation. Une fois que vous avez correctement configuré la base de données Cloudant NoSQL, le statut du stockage de données d'historique devient Configuré et les données du terminal sont stockées dans la base de données {{site.data.keyword.cloudant_short_notm}} NoSQL.
6. Passez à l'[Etape 3. Exécution du simulateur](#run_simulator).


## Etape 3. Exécution du simulateur
{: #run_simulator}

Le simulateur publie des données de sonde météo réelles, provenant de 17 stations de la zone Haifa, dans votre organisation {{site.data.keyword.iot_short_notm}}.

1. Accédez au simulateur.
2. Entrez les informations suivantes :
   - Organisation Watson IoT Platform
   - Clé d'API
   - Jeton d'authentification

3. Cliquez sur **Exécuter le simulateur**. La génération des données nécessite quelques minutes.
4. Une fois le simulateur démarré, accédez à Watson IoT Platform et vérifiez que les terminaux ont été créés et que les événements arrivent dans ces terminaux. 
5. Passez à l'[Etape 4. Exportation d'une base de données Cloudant](#WA_export_csv).


## Etape 4. Exportation d'une base de données Cloudant
{: #WA_export_csv}

Lorsque vous configurez une base de données {{site.data.keyword.cloudant_short_notm}} NoSQL pour stocker des données de terminal, trois bases de données sont automatiquement créées par le connecteur : une base de données pour l'intervalle en cours, une base de données pour l'intervalle à venir et une base de données de configuration. A la fin de l'intervalle, les données de terminal sont stockées dans la base de données pour le nouvel intervalle, et la nouvelle base de données est créée pour l'intervalle suivant.

La fonction d'extension de stockage des données d'historique de {{site.data.keyword.cloudant_short_notm}} crée dans Cloudant un document de conception appelé “iotp”. Ce document comporte une fonction “list” appelée “csv” que vous pouvez utiliser pour exporter des événements de terminal, stockés en tant que documents dans Cloudant, au format CSV. Seuls les événements au format JSON sont envoyés au fichier CSV. Ce document de conception est propagé automatiquement à toutes les nouvelles bases de données dans l'intervalle programmé.

Le fichier CSV contient des informations sur les métadonnées d'événement de terminal et sur sa charge. La liste ci-dessous indique des exemples de métadonnées d'événement :
 -	ID de terminal
 -	Type de terminal
 - 	Type d'événement
 - 	Horodatage au format ISO 8601

La fonction de liste CSV sépare l'horodatage d'origine en deux nouvelles zones Heure et Date. Outre les métadonnées, la fonction de liste CSV inclut les attributs de données de la charge du terminal. Cette charge est présentée dans le document Cloudant sous la clé “data”. Les documents qui sont générés par le simulateur Weather Sensors possèdent une structure similaire à l'exemple suivant :

```
{"deviceType": "WS",
  "deviceId": "Old-Market",
  "eventType": "sensorData",
  "format": "json",
  "timestamp": "2017-08-09T16:28:14.666Z",
  "data": { "NO2": 3.2, … }}
```

Dans le fichier de sortie CSV, tous les attributs de charge sont représentés sous forme de colonnes et reçoivent le préfixe :

```
<deviceType>_<eventType>_  
```

Dans l'exemple ci-dessus, une colonne appelée WS_sensorData_NO2 est ajoutée au fichier CSV.

Pour exporter la base de données Cloudant au format CSV :  

1. Connectez-vous à la base de données Cloudant NoSQL.
2. Sélectionnez la base de données que vous souhaitez exporter.
3. Ouvrez la base de données sélectionnée.
4. Ouvrez un nouvel onglet dans le navigateur et tapez l'adresse URL suivante :
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-date?include_docs=true
```
   L'ID de service Cloudant et le nom de base de données doivent être modifiés selon vos propres valeurs. L'ID de service Cloudant peut être copié à partir de l'URL du tableau de bord de gestion Cloudant.
   **Exemple :**
   ```
   https://ccf73725-b617-4f3e-8a7e-f5fb09569af4-bluemix.cloudant.com/iotp_115ccv_default_2017-08-23/_design/iotp/_list/csv/by-date?include_docs=true
   ```

   Dans cet exemple, les données sont triées par horodatage, car la vue Par date est utilisée pour appeler la fonction de liste. Vous pouvez également filtrer les données à l'aide de la fonction de filtre native Cloudant en changeant la vue qui est utilisée dans l'URL et en application les attributs startkey et endkey.

   **Exemple :**
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-deviceType?include_docs=true&startkey='WS'&endkey='WS'
   ```
   Dans cet exemple, la vue deviceType est utilisée pour générer le fichier CSV et seuls les documents deviceType=WS sont inclus dans le fichier de téléchargement. Pour sélectionner des documents dans une période spécifique, utilisez la vue Par date et l'URL de requête suivante (en remplaçant les horodatages pour la plage désirée) :
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-date?statkey="2017-08-29T12:25:50.995Z"&endkey="2017-08-29T12:25:51.514Z"
   ```
5. Fournissez les données d'identification Cloudant si nécessaire et téléchargez le fichier CSV. Le nom du fichier est généré d'après la vue qui est définie dans l'URL. Par exemple, le nom de fichier peut être by-date.csv ou by-deviceType.csv.
6. Passez à l'[Etape 5. Configuration de WA et visualisation des données](#WA_import_data).


## Etape 5. Configuration de WA et visualisation des données
{: #WA_import_data}

Pour configurer WA et commencer à visualiser les données :

1. Connectez-vous à WA à l'adresse : https://watson.analytics.ibmcloud.com.
2. Sur la page d'accueil de WA, sélectionnez **Données**.
3. Cliquez sur **Fichier local** pour importer votre fichier CSV local. Le nom du fichier CSV dépend de la vue qui est utilisée pour exporter les données (par exemple, by-deviceType ou by-date.)
4. Sélectionnez l'actif de données CSV que vous avez téléchargé précédemment.
5. Dans la zone **Ask a question about your data**, posez votre question en langage naturel.
5. Ouvrez la suggestion de visualisation qui correspond le mieux à votre question. Vous pouvez corriger manuellement la suggestion.
7. Sauvegardez la visualisation.


## Exemples de visualisation de données à l'aide de WA
{: #WA_visualize}

Cette section propose des exemples d'analyse de données à l'aide de WA comme outil d'analyse.

**Remarque :** Ces exemples sont destinés à vous donner une idée de ce que vous pouvez attendre de vos propres visualisations. Les résultats présentés ici peuvent être différents de ceux que vous obtiendrez en effectuant ces visualisations à l'aide de données d'échantillons, par exemple, des données collectées à différentes dates et heures.

### Visualisation de la santé des données

Dans cette section, vous pourrez en savoir plus sur la population des terminaux IoT et vous serez en mesure de répondre à des questions de type :

1. Combien de terminaux ont été signalés ?
2. Quelle est la répartition des terminaux par type de terminal ?
3. Combien un terminal possède-t-il de rapports ?
4. Combien de rapports ont été envoyés par chaque terminal ?

**Combien de terminaux ont été signalés ?**

Dans cet exemple, nous allons comptabiliser le nombre de terminaux ayant été fait l'objet d'un rapport au cours de l'intervalle donné, afin de savoir si les terminaux ont bien été signalés comme prévu. Pour effectuer cette analyse, copiez-collez ou saisissez la question suivante dans WA :

*"Combien existe-t-il d'ID de terminaux ?"*

La réponse indique la présence de 17 terminaux :

![Résultat du nombre de terminaux](images/device_count.png)

**Quelle est la répartition des terminaux par type de terminal ?**

Dans cet exemple, nous allons comparer le nombre de type de terminal ayant été signalés au cours de l'intervalle afin de déterminer si les terminaux issus de tous les types ont bien été détectés, comme prévu. Pour effectuer cette analyse, copiez-collez ou saisissez la question suivante dans WA :

*"Quelle est la répartition des ID de terminaux par Type de terminal ?"*

Le résultat ci-dessous affiche la répartition des terminaux par type de terminal :

![Répartition des terminaux par type de terminal](images/deviceID_deviceType.png)

Pour visualiser ces données sous forme de graphique circulaire, cliquez sur l'icône de **visualisation** en bas à gauche, et sélectionnez la forme **graphique**.

![Graphique circulaire affichant la répartition des terminaux par type de terminal](images/deviceID_deviceType_pie.png)


**Combien un terminal possède-t-il de rapports ?**

Dans cet exemple, nous allons calculer le nombre de rapports ayant été générés par un terminal pour détecter les conditions réseaux et autres anomalies liées aux terminaux. Pour effectuer cette analyse, copiez-collez ou saisissez la question suivante dans WA :

*"Combien y-a-t-il de lignes ? Avec application du filtre deviceId : Ahuza"*

**Remarque :** Il est inutile de taper le nom complet des zones, car WA tente de deviner les noms complets des zones. En revanche, les valeurs de filtre (par exemple, "Ahuza") doivent être entièrement et correctement orthographiées. Si aucune suggestion ne vous convient avec le filtre, cliquez sur le lien **Show Next** ou posez la question *"Combien y-a-t-il de lignes ?"*. Ouvrez ensuite le diagramme, cliquez sur **Multiplier** en dessous du diagramme, puis sélectionnez le paramètre deviceId dans la liste. Décochez tous les ID de terminaux inappropriés.

Le résultat ci-dessous indique qu'il y a 25 lignes ou que 25 rapports ont été générés par le terminal Ahuza :

![Nombre de rapports](images/25_rows.png)


**Combien de rapports ont été générés par chacun des différents terminaux ?**

Dans cet exemple, nous allons comparer le niveau d'activité des terminaux en fonction du nombre de rapports ayant été envoyés par chaque terminal au cours de l'intervalle spécifié. Pour effectuer cette analyse, copiez-collez ou saisissez la question suivante dans WA :

*"Quelle est la répartition des lignes par ID de terminal ?"*

Le résultat ci-dessous indique un diagramme à barres représentant l'activité de différents terminaux :

![Comparaison de l'activité des terminaux](images/compare_activity.png)


### Visualisation des données du capteur de type de terminal

Dans cette section, nous allons en savoir plus sur les données de capteur récapitulatives qui sont rapportées par tous les terminaux d'un type de terminal, et nous allons répondre à des questions de  type :

1. Quelles sont les valeurs Moyenne/Min/Max de toutes les valeurs de capteur rapportées ?
2. Puis-je voir un histogramme de la sortie d'un capteur ?  
3. Quel lien y a-t-il entre les deux capteurs ?


**Quelles sont les valeurs Moyenne/Min/Max de toutes les valeurs de capteur rapportées ?**

Dans cet exemple, nous allons résumer les paramètres numériques rapportés par tous les terminaux d'un certain type sous forme de tableau. A partir de ce tableau, nous pourrons ensuite en savaoir davantage sur la plage des valeurs relevées dans l'environnement afin d'avoir une perspective plus large des données analysées.

Cette visualisation peut être générée manuellement en suivant les étapes ci-dessous :

1.	Dans la section **Create your own visualization**, sélectionnez **Table**.
2.	Cliquez sur le signe plus "create new column" et choisissez **Calculation**.
3.	Attribuez un nom à la nouvelle colonne, sélectionnez la colonne pour ce calcul dans la liste déroulante **Columns**, puis cliquez sur **Done** pour dupliquer la colonne. La nouvelle colonne est ajoutée à la fin du tableau de données.
4.	Cliquez avec le bouton droit de la souris sur le titre de la colonne, sélectionnez un type d'agrégation (min, max ou average), puis fermez la fenêtre des propriétés.
6.	Répétez la procédure pour ajouter d'autres colonnes, puis masquez le tableau de données.
7.	Cliquez sur **Columns** et sélectionnez **Measures** au bas de la liste.
8.	Cliquez sur **Aggregated by** et sélectionnez tous les calculs que vous avez ajoutés.
9.	Cliquez sur **Terminé**.
10.	Sauvegardez la visualisation.

Le résultat ci-dessous illustre la plage de valeurs :

![Valeur de plage du capteur](images/sensor_range_values.png)


**Puis-je afficher un histogramme de la sortie d'un capteur de terminal ?**

Dans cet exemple, nous allons évaluer le comportement d'un capteur sur tous les terminaux d'un certain type, en identifiant la distribution des avaleurs qui sont analysées dans l'environnement. Nous pourrons nous servir de cette visualisation pour en savoir plus sur l'environnement qui est analysé par les capteurs, ainsi que sur les limites liées aux capteurs. Pour effectuer cette analyse, copiez-collez ou saisissez la question suivante dans WA :

*"Quelle est la répartition des lignes par température ?"*

Le résultat ci-dessous indique la comparaison du nombre de lignes :

![Comparaison du nombre de lignes](images/number_rows.png)


**Quel lien y a-t-il entre les deux capteurs ?**

Dans cet exemple, nous allons comprendre les relations qui existent au sein de l'environnement en comparant les mesures de deux capteurs de terminal dans tous les terminaux d'un certain type. Pour effectuer cette analyse, copiez-collez ou saisissez l'une des questions ci-dessous dans WA :

*"Quelle relation existe-t-il entre NO2 et NOX ?"* ou *"Comment les valeurs de NO2 et NOX sont-elles associées ?"*

Le résultat ci-dessous illustre la relation qui existe entre les capteurs :

![Relation des capteurs](images/sensor_relationship.png)

Vous pouvez également visualiser les données du capteur en appliquant des points de couleur différente à chaque ID de terminal. Pour ce faire, sélectionnez l'ID de terminal dans la zone **Color**.

Le résultat ci-dessous affiche un sous-ensemble limité de terminaux :

![Relation des capteurs à l'aide de points de couleur](images/sensor_color_dots.png)


### Visualisation des informations relatives aux capteurs (analyse approfondie)

Dans cette section, nous allons étudier des paramètres spécifiques qui sont signalés par un terminal spécifique, en répondant aux questions suivantes :

1.	Quelles sont les valeurs Moyenne/Min/Max rapportées ?
2.	Puis-je voir un histogramme de la sortie d'un capteur de terminal ?
3.	Comment une valeur de capteur de terminal spécifique évolue-t-elle au cours du temps ?
4.	Comment comparer les valeurs de capteur de deux terminaux au fil du temps ?
5.	Comment comparer les valeurs de capteur d'un même terminal au fil du temps ?
6.	Quel lien y a-t-il entre deux capteurs d'un terminal ?


**Quelles sont les valeurs Moyenne/Min/Max rapportées ?**

Dans cet exemple, nous allons présenter les paramètres numériques rapportés par un terminal spécifique sous forme de tableau afin de découvrir, par exemple, la plage de valeurs analysées dans l'environnement ou les dysfonctionnements du capteur.

Cette visualisation doit être générée manuellement en suivant les étapes ci-dessous :

1)	Dans la section **Create your own visualization**, sélectionnez **Table**.
2)	Cliquez sur le signe plus "create new column" et choisissez **Calculation**.
3)	Attribuez un nom à la nouvelle colonne, sélectionnez la colonne pour ce calcul dans la liste déroulante **Columns**, puis cliquez sur **Done** pour dupliquer la colonne. La nouvelle colonne est ajoutée à la fin du tableau de données.
4)	Cliquez avec le bouton droit de la souris sur le titre de la nouvelle colonne, sélectionnez un type d'agrégation (min, max ou average), puis fermez la fenêtre **Propriétés**.
6)	Répétez la procédure pour ajouter d'autres colonnes, puis masquez le tableau de données.
7)	Cliquez sur **Columns** et sélectionnez **Measures**.
8)	Cliquez sur **Aggregated by** et sélectionnez tous les calcules que vous avez ajoutés.
9)	Cliquez sur **Terminé**.
10)	Dans la zone Multiplier, sélectionnez le paramètre deviceId et choisissez les terminaux à afficher.
11)	Sauvegardez la visualisation.

Le résultat ci-dessous illustre les valeurs spécifiées :

![Analyse approfondie des valeurs de rapport](images/deep_avg_min_max.png)


**Puis-je voir un histogramme de la sortie d'un capteur de terminal ?**

Dans cet exemple, nous allons évaluer le comportement d'un capteur de terminal spécifique, en identifiant la distribution des valeurs analysées dans l'environnement. Nous pourrons nous servir de cette visualisation pour en savoir plus sur l'environnement analysé par le capteur, ainsi que sur les dysfonctionnements possibles du capteur. Pour effectuer cette analyse, copiez-collez ou saisissez l'une des questions ci-dessous dans WA :

*"Quelle est la distribution de TEMP ? Avec application du filtre deviceId : Ahuza"* ou *"Quelle est la répartition du nombre de lignes par TEMP ? Avec application du filtre deviceId : Ahuza"*

Le résultat ci-dessous affiche les données de sortie du capteur de terminal dans un histogramme :

![Données du capteur de terminal sous forme d'histogramme](images/deep_histogram.png)


**Comment une valeur de capteur de terminal spécifique évolue-t-elle au cours du temps ?**

Dans cet exemple, nous allons découvrir comment les relevés d'un capteur spécifique changent, en reflétant les changements dans l'environnement au fil du temps. Ceci peut faciliter la planification et la détection des problèmes. Pour effectuer cette analyse, copiez-collez ou saisissez la question suivante dans WA :

*"Quelle est la tendance de la température au fil du temps ? Avec application du filtre deviceId : Ahuza".*

Le résultat ci-dessous illustre la tendance des données du capteur dans le temps :

![Tendance des données du capteur de terminal dans le temps](images/deep_sensor_trend.png)


**Comment comparer les valeurs de capteur de deux terminaux au fil du temps ?**

Dans cet exemple, nous allons comparer les tendances des relevés du capteur de différents terminaux, en identifiant les relations qui existent entre les terminaux afin de détecter les anomalies, les dysfonctionnements de terminaux, et ainsi de suite. Pour effectuer cette analyse, copiez-collez ou saisissez l'une des questions ci-dessous dans WA :

*"Quelle est la tendance des températures dans le temps par deviceId ?"* ou *"Quelle est la tendance des températures dans le temps par deviceId ?  Avec application du filtre deviceId : Ahuza, Igud"*

Le résultat ci-dessous indique la comparaison des valeurs de capteur au fil du temps :

![Comparaison des données de capteur entre les terminaux au fil du temps](images/deep_sensor_compare.png)

Vous pouvez également afficher ces informations en cliquant sur le nom du paramètre en bas du graphique. Plusieurs lignes sont tracées (une par ID de terminal). Les ID appropriés peuvent être sélectionné dans la liste.

![Comparaison des données de capteur entre les terminaux au fil du temps, représentée par des lignes](images/deep_sensor_compare_lines.png)

Vous pouvez utiliser la case **Multiplier** en dessous du graphique et choisir les ID de terminal à comparer.

![Comparaison simultanée des données de capteur](images/deep_sensor_compare_side.png)


**Comment comparer les valeurs de capteur d'un même terminal au fil du temps ?**

Dans cet exemple, nous allons afficher la tendance de deux capteurs de terminaux afin de mieux appréhender les changements d'environnement dans le temps. Pour effectuer cette analyse, copiez-collez ou saisissez la question suivante dans WA :

*"Quelle est la tendance de NO2 et NOX dans le temps par ID de terminal ? Avec application du filtre deviceId : Ahuza"*

Le résultat ci-dessous illustre la tendance des deux capteurs de terminal au fil du temps :

![Comparaison des deux capteurs de terminal au fil du temps](images/deep_two_devices.png)


**Quel lien y a-t-il entre deux capteurs d'un terminal ?**

Dans cet exemple, nous allons comprendre les relations qui existent dans l'environnement en comparant les mesures de deux capteurs de terminal. Pour effectuer cette analyse, copiez-collez ou saisissez l'une des questions ci-dessous dans WA :

*"Quelle relation existe-t-il entre NO2 et NOX ? Avec application du filtre deviceId : Ahuza"* ou *"Comment les valeurs de NO2 et NOX sont-elles associées ? Avec application du filtre deviceId : Ahuza"*

Le résultat ci-dessous affiche la relation entre les deux capteurs d'un terminal :

![Comparaison de deux capteurs d'un terminal](images/deep_two_sensors.png)


## Etape suivante ?

Pour plus d'informations sur WA, voir les ressources suivantes :
- [Watson Analytics Developer Center ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/watson-analytics/){: new_window}
- [Watson Analytics community ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/communities/analytics/watson-analytics/){: new_window}
- [Watson Analytics forum ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://community.watsonanalytics.com/discussions/spaces/15/view.html){: new_window}
