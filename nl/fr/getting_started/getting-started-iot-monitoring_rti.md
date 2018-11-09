---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guide 3 : Surveillance des données de votre terminal
Maintenant que vous avez un ou plusieurs terminaux connectés, il est temps de commencer à surveiller les données de terminal en temps réel.
{:shortdesc}

## Présentation et objectif
{: #overview}  

Dans ce guide, vous allez déployer une application de surveillance sur {{site.data.keyword.Bluemix}} pour afficher des données à partir de vos terminaux.

A l'instar du guide 1, vous pouvez suivre l'un et/ou l'autre des chemins ci-dessous :
- Chemin A : [Etape 1A - Déploiement et connexion de l'application Web de surveillance](#deploy_app)  
En suivant le chemin A, vous déployez une application prête à l'emploi de surveillance des terminaux du tapis roulant qui sont exécutés au sein de votre organisation.
- Chemin B : [Etape 1B - Création d'une interface utilisateur de surveillance à l'aide de la bibliothèque de widgets](#widget-library)
Le chemin B, un peu plus complexe, vous présente la bibliothèque de widgets et vous guide tout au long de la création d'une interface utilisateur de base.

## Prérequis
{: #prereqs}  
Avant de commencer, assurez-vous de remplir les conditions ci-dessous.

### Environnement local
- [Node.js ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://nodejs.org){: new_window} 6.x ou version supérieure.
- Chemin A: [Interface CLI Angular ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/angular/angular-cli){: new_window} 1.x ou version supérieure.  
- [Cloud Foundry CLI (client de ligne de commande pour Cloud Foundry) ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilisez l'interface CLI CF pour déployer des applications et des services sur {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations, voir la documentation [Cloud Foundry CLI![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
- Facultatif : [Git ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://git-scm.com/downloads){: new_window}  
Si vous choisissez d''utiliser Git pour télécharger les exemples de code, vous devez disposer d'un [compte GitHub.com ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com){: new_window}. Vous pouvez aussi télécharger le code sous forme de fichier compressé si vous n'avez pas de compte GitHub.com.

### Autres exigences
Vous devez posséder un terminal connecté de type `iot-conveyor-belt` qui envoie des événements avec le nom d'événement `sensorData` et avec une charge de message qui contient les propriétés suivantes :
```
{
	"d": {
		"id": "belt1",
		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
Pour plus d'informations sur les événements de terminaux et le format des messages, voir [Publication d'événements](/docs/services/IoT/devices/mqtt.html#publishing_events).  
Si vous avez terminé l'étape [Guide 1 : Initiation à {{site.data.keyword.iot_short_notm}} et à un tapis roulant simulé](getting-started-iot-conveyor.html), vous êtes prêt.  
{: tip}

## Etape 1A - Déploiement et connexion de l'application Web de surveillance
{: #deploy_app}

Le modèle d'application Plant Floor Monitoring répertorie tous les terminaux de type iot-conveyor-belt qui sont connectés à votre organisation {{site.data.keyword.iot_short_notm}} avec un sous-ensemble de données d'événement, notamment la valeur RPM, la date de dernière mise à jour et l'ID de terminal.

Le modèle d'application repose sur des bibliothèques client Node.js accessibles depuis : [https://github.com/ibm-watson-iot/iot-nodejs ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![Application de surveillance basée sur Node.js](images/app_monitor.png "Application de surveillance basée sur Node.js")

Dans le cadre de cette étape, vous allez :
- Déployer un modèle d'application Web de surveillance basé sur GitHub à l'aide de Cloud Foundry.
- Configurer le modèle d'application pour vous connecter à {{site.data.keyword.iot_short_notm}} à l'aide d'une clé d'API et d'un jeton d'authentification.
- Utiliser l'application Web pour surveiller vos terminaux de tapis roulant connectés.  

### Etapes détaillées de déploiement et de connexion de l'application de surveillance
Les étapes ci-dessous vont vous guider tout au long de la procédure de création et de déploiement de l'application sur {{site.data.keyword.Bluemix_notm}}. Pour en savoir plus sur l'exécution de l'application en local, consultez le fichier README de GitHub.
1. Clonez le référentiel GitHub du modèle d'application *Plant Floor Monitoring* Node.js.  
Servez-vous de votre outil Git favori pour cloner le référentiel ci-dessous :  
https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
Dans l'interpréteur de commande Git, exécutez la commande suivante :
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
  ```
2. Créez une combinaison "clé d'API-jeton d'authentification" pour votre application.  
Vous aurez besoin de ces éléments lorsque vous configurerez l'application pour qu'elle se connecte à votre organisation. Pour plus d'informations sur l'enregistrement des terminaux, voir [Connexion d'applications](/docs/services/IoT/platform_authorization.html).  
 1. Ouvrez le tableau de bord {{site.data.keyword.iot_short_notm}}.
 2. Sélectionnez **Applications**.
 3. Cliquez sur **Générer une clé d'API**
 4. Copiez les valeurs de la clé d'API et du jeton d'authentification qui sont affichées.
 5. Sélectionnez **Application de visualisation** comme rôle d'API.  
**Astuce :** Si vous ajoutez des fonctionnalités supplémentaires à l'application, vous devrez lui conférer un rôle plus élevé.
 6. Ajoutez un commentaire pour pouvoir identifier facilement cette combinaison de clé d'API et jeton d'authentification.
 7. Cliquez sur **Générer**.
3. Configurez votre application de sorte qu'elle se connecte à {{site.data.keyword.Bluemix_notm}}.
Placez-vous à la racine du référentiel *guide-conveyor-ui-angular* et créez un fichier `basicConfig.json` avec le contenu suivant :
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
Remplacez les valeurs de paramètre par les valeurs correspondantes de votre organisation {{site.data.keyword.Bluemix_notm}} : ID organisation, clé d'API et jeton d'authentification que vous venez de créer.  
Exemple :
```
 {   
  "org": "3v5whr",    
  "apiKey": "a-3v5whr-jhkmsghlqv",  
  "apiToken": "-q0MkPN2cNYB6+?ISk"  
}
```
4. Connectez-vous à votre compte {{site.data.keyword.Bluemix_notm}} à l'aide de l'interface de ligne de commande Cloud Foundry.  
Pour plus d'informations, voir la documentation [Cloud Foundry CLI![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
A partir de la ligne de commande, entrez la commande suivante :  
  ```
cf login
  ```
Si le système vous y invite, sélectionnez l'organisation et l'espace où vous souhaitez déployer le modèle d'application de surveillance.
5. Si nécessaire, définissez le noeud final de votre API en exécutant la commande d'API CF.   
Remplacez la valeur `API-ENDPOINT` par le noeud final d'API de votre région.
  ```
cf api API-ENDPOINT
  ```
Exemple : `cf api https://api.ng.bluemix.net`
<table>
<tr>
<th>Région</th>
<th>Noeud final d'API</th>
</tr>
<tr>
<td>Sud des Etats-Unis</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>Royaume-Uni</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
<!--<tr>
<td>Germany</td>
<td>https://api.eu-de.bluemix.net</td>
</tr>-->
</table>
6. Modifiez le répertoire pour indiquer celui où se trouve le modèle d'application.  
  ```
cd guide-conveyor-ui-angular
  ```
7. Exécutez `npm install -g @angular/cli` pour installer l'interface de ligne de commande Angular.
8. Exécutez `npm install`.
9. Exécutez `npm run push` pour générer le projet et l'envoyer à votre organisation.  
Votre modèle d'application Web est déployé sur {{site.data.keyword.Bluemix_notm}}.  
Une fois le déploiement terminé, un message s'affiche pour indiquer que votre application est en cours d'exécution.   
Exemple :  
  ```
requested state: started
instances: 1/1
usage: 64M x 1 instances
urls: iotmonitoringcontrol-undertided-eng.mybluemix.net
last uploaded: Tue May 16 19:01:13 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     state     since                    cpu    memory     disk        details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
10. Ouvrez l'application Web.  
Dans le tableau de bord des applications {{site.data.keyword.Bluemix_notm}}, cliquez sur **Visite de l'URL de l'application** pour ouvrir l'application Web.  
Accédez à l'URL de l'application à des fins de gestion en cliquant sur **Routes**.   
L'adresse URL par défaut est similaire à la suivante :  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## Etape 1B - Création et surveillance de l'interface utilisateur de surveillance à l'aide de la bibliothèque de widgets
{: #widget-library}

Le modèle d'application basé sur la bibliothèque de widgets inclut une jauge de vitesse du moteur, une jauge des données de l'accéléromètre et un diagramme de vitesse du moteur afin d'afficher les données d'un terminal unique de type iot-conveyor-belt connecté à votre organisation {{site.data.keyword.iot_short_notm}}. Vous pouvez utiliser l'exemple de code pour générer une application frontale complète pour vos terminaux connectés {{site.data.keyword.iot_short_notm}}.

![Application de surveillance basée sur la bibliothèque de widgets](images/app_monitor_b.png "Application de surveillance basée sur la bibliothèque de widgets")

Dans le cadre de cette étape, vous allez :
- Déployer un modèle d'application Web basé sur GitHub à l'aide de Cloud Foundry.
- Configurer le modèle d'application pour vous connecter à {{site.data.keyword.iot_short_notm}} à l'aide d'une clé d'API et d'un jeton d'authentification.
- Configurer trois widgets d'interface utilisateur pour afficher les données de terminaux sous forme de jauges et de graphiques.
- Utiliser l'application Web pour surveiller le terminal de tapis roulant connecté.  

### Etapes détaillées de création d'une interface utilisateur de surveillance à l'aide de la bibliothèque de widgets
Les étapes ci-dessous vont vous guider tout au long de la procédure de création et de déploiement de l'application sur {{site.data.keyword.Bluemix_notm}}. Pour en savoir plus sur l'exécution de l'application en local, consultez le fichier README de GitHub.
1. Clonez le référentiel GitHub du modèle d'application *Widget Library Monitoring*.  
Servez-vous de votre outil Git favori pour cloner le référentiel ci-dessous :  
https://github.com/ibm-watson-iot/guide-conveyor-ui-html
Dans l'interpréteur de commande Git, exécutez la commande suivante :
```
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-html
```
2. Installez les dépendances d'application.  
Placez-vous à la racine du référentiel *guide-conveyor-ui-html* et exécutez la commande suivante :
```
npm install
```
3. Construisez l'interface utilisateur.  
Pour construire l'interface utilisateur d'application, vous devez ajouter les widgets sous forme de code JavaScript dans le fichier index.html de l'application pour chaque composant d'interface utilisateur.  
Chaque widget utilise les paramètres JavaScript suivants :  
`WIoTPWidget.CreateWIDGET_TYPE("ELEMENT_ID","EVENT_NAME", "DEVICE_TYPE", "DEVICE_ID", "PROPERTY" , {WIDGET_DEFAULT_OVERRIDE}, [WIDGET_SPECIFIC_SETTINGS])`

Le tableau ci-dessous propose une description des paramètres :

| Paramètre | Description |    
| ----- | ---- |   
| WIDGET_TYPE | Type de widget à créer. Exemple : `Jauge` ou `Diagramme` |
| ELEMENT_ID | ID d'élément du widget, tel qu'il apparaîtra dans l'application. Exemple : `RPM` |
| EVENT_NAME | Nom d'événement de terminal qui inclut la propriété à afficher. Exemple : `sensorData` |
| DEVICE_TYPE | Type de terminal. Exemple : `iot-conveyor-belt` |
| DEVICE_ID | ID du terminal qui fournit les données à afficher. Exemple : `belt1` |
| PROPERTY | Propriété de la charge de message de terminal à afficher. Exemple : `rpm` |
| WIDGET_DEFAULT_OVERRIDE | Paramètres de configuration du widget devant remplacer les paramètres par défaut.|
| WIDGET_SPECIFIC_SETTINGS | Un ou plusieurs paramètres de widget supplémentaires (voir les exemples). |

Pour plus de détails sur chaque type de widget, consultez les exemples ci-après, ainsi que la documentation du [référentiel IoT Widgets GitHub ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-widgets){: new_page}.
 1. Ajoutez une jauge RPM.  
Cette jauge affiche les tours par minute (RPM) du tapis roulant avec une valeur minimale de 0 et une valeur maximale de 5 tours par minute (RPM).
    1. Ouvrez le modèle suivant pour l'éditer : `public/index.html`  
    2. Localisez la marque de réservation de la jauge RPM : `<!--- place holder for rpm gauge  -->`
    3. Ajoutez l'élément div suivant avec un ID unique, comme indiqué ci-dessous :
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. Localisez la marque de réservation JavaScript : `/// Add your scripts here`
    4. Ajoutez le code JavaScript RPM.  
Exemple :  
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iot-conveyor-belt", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. Ajoutez une jauge d'accélération.  
Cette jauge affiche le relevé de l'accéléromètre dans une plage de valeurs comprise entre -1 et 1.
    1. Localisez la marque de réservation de la jauge d'accéléromètre : `<!--- place holder for accelerometer gauge  -->`
    2. Ajoutez l'élément div suivant avec un ID unique, comme indiqué ci-après :
 ```html
 <div id="aygauge" ></div>
 ```  
    3. Localisez la marque de réservation javascript : `/// Add your scripts here`
    4. Ajoutez le code JavaScript de l'accéléromètre.  
Exemple :   
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iot-conveyor-belt", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
          },
          show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. Ajoutez un graphique de vitesse du moteur.  
Ce graphique illustre la vitesse du moteur sous forme de graphique linéaire.
    1. Localisez la marque de réservation de la jauge de vitesse du moteur : `<!--- place holder for motor speed gauge  -->`
    2. Ajoutez l'élément div suivant avec un ID unique, comme indiqué ci-après :
 ```html
 <div id="speedchart" ></div>
 ```  
    3. Localisez la marque de réservation JavaScript : `/// Add your scripts here`
    4. Ajoutez le code JavaScript du graphique de vitesse.  
Exemple :  
 ```javascript
 WIoTPWidget.CreateChart("speedchart ","sensorData", "iot-conveyor-belt", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. Déployez l'application sur {{site.data.keyword.Bluemix_notm}}  
 1. Mettez à jour le fichier manifest.yml avec le nom de votre service {{site.data.keyword.iot_short_notm}}.  
Par exemple, si vous avez déployé un service {{site.data.keyword.iot_short_notm}} dans le cadre du [Guide 1 : Connexion d'un terminal de tapis roulant](getting-started-iot-monitoring.html), le nom de votre plateforme YOUR_PLATFORM_NAME est `iotp-for-conveyor`.
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. Connectez-vous à votre compte {{site.data.keyword.Bluemix_notm}} à l'aide de l'interface de ligne de commande Cloud Foundry.  
 Pour plus d'informations, voir la documentation [Cloud Foundry CLI![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
 A partir de la ligne de commande, entre la commande suivante :  
   ```
 cf login
   ```
 Si le système vous y invite, sélectionnez l'organisation et l'espace où vous souhaitez déployer le modèle d'application de surveillance.
 5. Si nécessaire, définissez le noeud final de votre API en exécutant la commande d'API CF.   
 Remplacez la valeur `API-ENDPOINT` par le noeud final d'API de votre région.
   ```
 cf api API-ENDPOINT
   ```
 Exemple : `cf api https://api.ng.bluemix.net`
 <table>
 <tr>
 <th>Région</th>
 <th>Noeud final d'API</th>
 </tr>
 <tr>
 <td>Sud des Etats-Unis</td>
 <td>https://api.ng.bluemix.net</td>
 </tr>
 <tr>
 <td>Royaume-Uni</td>
 <td>https://api.eu-gb.bluemix.net</td>
 </tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
 </table>
 6. Modifiez le répertoire pour indiquer celui où se trouve le modèle d'application.  
   ```
 cd guide-conveyor-ui-html
   ```
 2. Exécutez la commande push CF pour envoyer l'application à {{site.data.keyword.Bluemix_notm}} :  
Attribuez un nom unique à votre application.
```
cf push YOUR_APP_NAME
```
5. Ouvrez l'application Web basée sur la bibliothèque de widgets.  
Dans le tableau de bord des applications {{site.data.keyword.Bluemix_notm}}, cliquez sur **Visite de l'URL de l'application** pour ouvrir l'application Web.  
Accédez à l'URL de l'application à des fins de gestion en cliquant sur **Routes**.   
L'adresse URL par défaut est similaire à la suivante :  
`https://YOUR_APP_NAME.mybluemix.net`

## Etape 2 - Affichage de vos terminaux connectés
{: #view_devices}

Maintenant que la console Web est en cours d'exécution, vous pouvez voir vos terminaux de tapis roulant connectés.
1. Dans la section **Terminaux** de la console Web, vérifiez que vos tapis roulants sont répertoriés et que la valeur RPM correcte et l'état En cours d'exécution sont affichés.
2. Modifiez la valeur RPM de votre terminal de tapis roulant et vérifiez que la valeur est mise à jour comme prévu sur l'application de surveillance.


## Etape suivante ?
{: @whats_next}  
Passez au guide suivant ou à une autre rubrique qui vous intéresse :
- Chemin A : Modification de l'application de surveillance selon vos besoins.  
Pour plus de détails techniques, voir :
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md){: new_window}
 - [Bibliothèques client Node.js ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- Chemin B : Modification de l'application bibliothèque de widgets selon vos besoins.  
Pour plus de détails techniques, voir :
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md){: new_window}
- [Guide 4 : Simulation d'un grand nombre de terminaux](getting-started-iot-large-scale-simulation.html)  
Etendez la simulation de base en ajoutant à votre environnement un très grand nombre de simulateurs à exécution automatique. Cette extension vous permettra de tester les analyses et la surveillance de base des guides précédents dans un environnement multiterminal plus réaliste.
- [En savoir plus sur {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [En savoir plus sur les {{site.data.keyword.iot_short_notm}} API](/docs/services/IoT/reference/api.html){:new_window}
