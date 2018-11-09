---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guide 3 : Simulation d'un grand nombre de terminaux
Dans le premier guide, vous avez configuré un simulateur de terminal basique afin de simuler manuellement un ou un petit nombre de tapis roulants. A présent, vous allez étendre cette simulation en ajoutant un grand nombre de simulateurs à exécution automatique à votre environnement à des fins de test des analyses et de surveillance d'un environnement plus réaliste comportant plusieurs terminaux.
{:shortdesc}

**Important :** l'application nécessite 512 Mo de mémoire, ce qui est plus que la valeur allouée par défaut et que la quantité disponible pour les comptes d'essai gratuits, y compris le compte d'essai et le compte standard {{site.data.keyword.Bluemix}}. Les titulaires d'un compte d'abonnement ou de paiement à la carte peuvent accroître la mémoire allouée de 512 Mo. Les titulaires de compte gratuit doivent effectuer une mise à niveau vers un compte d'abonnement ou de paiement à la carte. Pour plus d'informations sur les types de compte {{site.data.keyword.Bluemix_notm}}, voir [Types de compte](/docs/pricing/index.html#pricing).

**Remarque :** Bluemix est maintenant IBM Cloud. Consultez le [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe") pour plus de détails.

## Présentation et objectif
{: #overview}

Dans le présent guide, vous allez configurer une application {{site.data.keyword.Bluemix_notm}} afin de simuler plusieurs terminaux de tapis roulants à l'aide d'un système de back end [Node-RED ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://nodered.org){: new_window}.

L'application comporte trois flux destinés à :   
1. Créer plusieurs terminaux.   
2. Simuler des publications d'événements simultanées.
3. Supprimer des terminaux.   

![Création et suppression de terminaux](images/device_creation.png "Création et suppression de terminaux")

![Simulation d'événement de terminal](images/device_event_simulation.png "Simulation d'événement de terminal")

Dans le cadre de ce guide, vous allez :
- Utiliser Cloud Foundry pour déployer une application simulateur de terminal webhook et Node-RED.
- Utiliser des appels API pour créer et enregistrer des terminaux, publier des événements de terminaux et supprimer des terminaux.

**Important :** Le fait de simuler un très grand nombre de terminaux qui envoient simultanément des données de terminaux à {{site.data.keyword.iot_full}} peut consommer une grande quantité de données. 

## Prérequis
{: #prereqs}  
Vous devez préalablement disposer des comptes et outils suivants :

* [Un compte {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/registration/) avec    
 - Plus de 512 Mo de mémoire RAM   
 - Plus de 1 Go de quota de disque  
 - Deux services disponibles
* [Une interface de ligne de commande Cloud Foundry (CLI CF) ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilisez l'interface de ligne de commande CF pour déployer et gérer vos applications {{site.data.keyword.Bluemix_notm}}.
* Facultatif : [Git ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://git-scm.com/downloads){: new_window}  
Si vous choisissez d''utiliser Git pour télécharger les exemples de code, vous devez disposer d'un [compte GitHub.com ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com){: new_window}. Vous pouvez aussi télécharger le code sous forme de fichier compressé si vous n'avez pas de compte GitHub.com.

Pour les appels REST présentés dans les étapes ci-dessous, vous pouvez soit utiliser cURL ou le plug-in de module complémentaire du client REST disponible dans Mozilla.  
{: tip}

## Etape 1 - Déploiement de l'application vers {{site.data.keyword.Bluemix_notm}}
{: #step1}

Suivez la procédure ci-dessous pour créer et déployer votre application manuellement.   

1. Clonez le référentiel GitHub du modèle d'application *Lesson4*.  
Servez-vous de votre outil Git favori pour cloner le référentiel ci-dessous :  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
Dans l'interpréteur de commande Git, exécutez la commande suivante :
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
```
3. Configurez l'application associée à votre environnement en éditant le fichier manifest.yml.  
Eléments à éditer :
 - Si vous souhaitez utiliser un service {{site.data.keyword.iot_short_notm}} existant, mettez à jour toutes les instances de `lesson4-simulate-iotf-service` afin de refléter le nom du service. Par exemple, si vous utilisez le service {{site.data.keyword.iot_short_notm}} du guide 1, utilisez `iotp-for-conveyor` comme nom de service.    
 - Définissez le nom de terminal et l'hôte.   
Dans la section des applications, remplacez les entrées `name` et `host` par une valeur unique, telle que `YOUR_NAME-lesson4-simulate`.   
**Astuce :** L'adresse URL ROUTES que vous utilisez pour accéder à l'application est créée à partir de l'entrée `host`, par exemple : `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plan: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
1. A partir de la ligne de commande, définissez le noeud final de votre API en exécutant la commande d'API CF.   
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
2. Connectez-vous à votre compte {{site.data.keyword.Bluemix_notm}}.
  ```
cf login
  ```
Si le système vous y invite, sélectionnez l'organisation et l'espace où vous souhaitez déployer {{site.data.keyword.iot_short_notm}} et le modèle d'application.
6. Créez les services requis dans {{site.data.keyword.Bluemix_notm}}.   
 1. Utilisez la commande ci-dessous pour créer le service cloudantNoSQLDB Lite :
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. Utilisez la commande ci-dessous pour créer le service {{site.data.keyword.iot_short_notm}}.
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**Important :** Si vous utilisez un service {{site.data.keyword.iot_short_notm}} existant et que vous avez mis à jour le fichier manifest.yml en conséquence, assurez-vous d'utiliser le nom de service existant. Par exemple, utilisez `iotp-for-conveyor` du guide 1 à la place de `lesson4-simulate-iotf-service` dans l'appel CF.
7. Exécutez la commande `cf push` pour générer le projet et l'envoyer à votre organisation.  
Votre modèle d'application est déployé sur {{site.data.keyword.Bluemix_notm}}.  
Une fois le déploiement terminé, un message s'affiche pour indiquer que votre application est en cours d'exécution.   
Exemple :  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. Récupérez les données d'identification du service pour votre application.
 1. Connectez-vous à {{site.data.keyword.Bluemix_notm}} à l'adresse :  
 [https://bluemix.net ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://bluemix.net){: new_window}.
 2. Sélectionnez le compte et l'espace où vous avez déployé l'application.
 3. Dans le menu, sélectionnez **Applications** et choisissez **Tableau de bord**.
 4. Dans les applications Cloud Foundry, cliquez sur le nom de l'application que vous venez de déployer.
 4. Sélectionnez **Connexions**.
 5. Localisez le service iotf-service que vous utilisez avec votre application et cliquez sur **Afficher les données d'identification**.  
 6. Sur la page des données d'identification du service, recherchez `apiKey` et `apiToken`.   
La clé d'API et le jeton d'authentification sont utilisés comme données d'identification lorsque vous utilisez l'API d'application pour créer et exécuter vos terminaux simulés.

## Etape 2 - Sécurisation de votre flux Node-RED
{: #step2}

Bien que cette étape soit optionnelle, il est conseillé de sécuriser votre flux Node-RED.

1. Sécurisez votre éditeur afin que seuls les utilisateurs autorisés puissent y accéder.
2. Pour sécuriser le flux Node-RED, accédez au tableau de bord {{site.data.keyword.Bluemix_notm}} et sélectionnez l'application que vous avez précédemment déployée.
3. Cliquez sur **Exécution**, puis sur **Variables d'environnement**.
4. Ajoutez les variables d'environnement suivantes définies par l'utilisateur, ainsi que leurs valeurs respectives :
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. Sauvegardez le flux.

Lorsque votre flux Node-RED est sécurisé, si vous prévoyez d'utiliser des commandes API REST pour créer, supprimer ou simuler plusieurs terminaux, vous devez fournir le nom d'utilisateur et le mot de passe Node-RED lors de l'authentification de base.

## Etape 3 - Création et connexion de terminaux
{: #step3}

Vous pouvez utiliser l'interface de flux Node-RED ou l'API REST d'application pour effectuer les tâches ci-dessous.

### Création et connexion de terminaux à l'aide de Node-RED  
Pour enregistrer plusieurs terminaux :  
1. Connectez-vous à {{site.data.keyword.Bluemix_notm}} à l'adresse :  
[https://bluemix.net ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://bluemix.net){: new_window}.
2. Sélectionnez le compte et l'espace où vous avez déployé l'application.
3. Dans le menu, sélectionnez **Applications** et choisissez **Tableau de bord**.
4. Dans les applications Cloud Foundry, cliquez sur l'URL **ROUTE** de l'application que vous venez de déployer.  
La variable ROUTE_URL est générée à partir de l'entrée `host` que vous avez utilisée dans le fichier manifest.yml : `HOST.mybluemix.net`  
Exemple : `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
L'interface Node-RED s'ouvre.
5. Cliquez sur **Go to your Node-RED flow editor**.
6. Dans l'éditeur de flux, sélectionnez l'onglet **Device Type and Instance**.
7. Pour enregistrer 5 terminaux, cliquez sur le noeud d'injection nommé **Register 5 motorController devices**.

### Création et connexion de terminaux à l'aide de l'API REST  
Pour enregistrer plusieurs terminaux :  

1. Faites une demande d'envoi HTTP POST à l'adresse URL suivante : `ROUTE_URL/rest/devices`  
Exemple : `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - Définissez 'Content-Type' et 'Accept' sur 'application/json'  
 - Utilisez la charge JSON suivante :  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
  Où :  
    - numberDevices désigne le nombre de terminaux à créer et à enregistrer.
    - Facultatif : typeId est le type de terminal sous lequel seront enregistrés les terminaux. Si la valeur typeId n'est pas renseignée, la valeur par défaut "iotp-for-conveyor" est utilisée. **Astuce :** Vous pouvez entrer n'importe quel nom de terminal, mais les exemples des autres guides supposent que vous utilisiez des terminaux de type `iotp-for-conveyor`. Si vous choisissez un type de terminal différent, vous devrez modifier les paramètres du guide en conséquence.
    - Facultatif : authToken est le jeton d'autorisation avec lequel sont enregistrés les terminaux.
    - Facultatif : Si chunkSize n'est pas renseigné, il est défini sur 500 par défaut. La valeur 'chunksize' doit être inférieure à un facteur de numberDevices.
    - deviceName correspond au modèle de deviceID pour les terminaux créés.

## Etape 4 - Simulation des événements de terminaux
{: #step4}

Etant donné que les terminaux simulés sont enregistrés avec {{site.data.keyword.iot_short_notm}}, vous pouvez maintenant exécuter le simulateur afin de démarrer l'envoi d'événements de terminal.

### Simulation d'événements de terminaux à l'aide de Node-RED  
Pour envoyer des événements de terminaux :  
1. Dans l'éditeur de flux Node-RED, sélectionnez l'onglet **Simulate multiple devices**.
7. Pour simuler 5 terminaux, cliquez sur le noeud d'injection nommé **Simulate 5 devices**.
 


### Simulation d'événements de terminaux à l'aide de l'API REST  
Pour envoyer des événements de terminaux :

1. Faites une demande d'envoi HTTP POST à l'adresse URL suivante : `ROUTE_URL/rest/runtest`  
Exemple : `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - Définissez 'Content-Type' et 'Accept' sur 'application/json'  
 - Utilisez la charge JSON suivante :   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
Où :
    - numberDevices désigne le nombre de terminaux pour lesquels vous souhaitez simuler des données.
    - numberEvents correspond au nombre d'événements envoyés par chaque terminal simulé.
    - timeInterval représente l'intervalle, en millisecondes, entre chaque événement.
    - deviceType est le type de terminal pour lequel vous avez créé des terminaux simulés.
    - deviceName correspond au modèle de deviceID pour les terminaux créés.
 

## Etape 5 - Suppression de terminaux
{: #deleting}

### Node-RED  
Pour supprimer des terminaux :  
1. Dans l'éditeur de flux Node-RED, sélectionnez l'onglet **Device Type and Instance**.
2. Pour supprimer 5 terminaux, cliquez sur le noeud d'injection nommé **Delete 5 devices**.



### API REST  

1. Effectuez une demande d'envoi HTTP POST à l'adresse URL suivante : `ROUTE_URL/rest/deleteDevices`  
Exemple : `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - Définissez 'Content-Type' et 'Accept' sur 'application/json'  
 - Utilisez la charge JSON suivante :      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName": "belt"  
}</code></pre>
  


## Etape suivante ?
{: @whats_next}  
Passez à une autre rubrique qui vous intéresse :
- [Guide 2 : Surveillance des données de votre terminal](getting-started-iot-monitoring.html)  
Maintenant que vous avez connecté un ou plusieurs terminaux et que vous avez commencé à exploiter les données de terminal, vous pouvez commencer à surveiller une collection de terminaux.
- [En savoir plus sur {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [En savoir plus sur les {{site.data.keyword.iot_short_notm}} API](/docs/services/IoT/reference/api.html){:new_window}
