---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guide 1 : Connexion d'un terminal de tapis roulant  
Créez un tapis roulant de base avec un terminal IoT qui envoie des données de surveillance à {{site.data.keyword.iot_full}} sur {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Présentation et objectif
{: #overview}  
Ce guide est le premier d'une série de guides détaillés consacrés au processus de connexion des terminaux à {{site.data.keyword.iot_short_notm}}, à la surveillance et l'analyse des données de terminaux, et bien plus encore. Chaque guide s'appuie sur le précédent et le code exemple de chaque document sert de point d'entrée au suivant. Vous pouvez aussi vous servir des guides de manière autonome, en adaptant le code exemple à vos besoins.

Le premier guide explique comment configurer un tapis roulant connecté et comment l'utiliser pour envoyer des données IoT à {{site.data.keyword.iot_short_notm}}.
Selon votre niveau de connaissance, vous pouvez suivre l'un et/ou l'autre des chemins ci-dessous pour configurer votre tapis roulant :
- Chemin A  
Ce chemin a été conçu pour que vous puissiez rapidement installer une application de simulation de tapis roulant sur {{site.data.keyword.Bluemix_notm}}. L'application enregistre automatiquement le terminal auprès de {{site.data.keyword.iot_short_notm}} et envoie automatiquement les données correctement formatées à la plateforme. Les instructions relatives à ce chemin sont disponibles dans [Etape 2A - Utilisation du modèle d'application de simulation depuis GitHub](#deploy_app).  
- Chemin B  
Ce chemin nécessite plus de connaissances techniques en termes de matériel et de programmation Python, et requiert l'enregistrement manuel de votre terminal auprès de {{site.data.keyword.iot_short_notm}}. Les instructions relatives à ce chemin sont disponibles dans [Etape 2B - Génération d'un tapis roulant physique à l'aie d'un Raspberry Pi et d'un moteur électrique](#raspberry).

Dans le cadre de ce guide, vous allez apprendre à :
- Créer et déployer une organisation {{site.data.keyword.iot_short_notm}} en utilisant l'interface de ligne de commande Cloud Foundry.
- Générer et déployer un modèle de terminal de tapis roulant.
- Connecter le terminal de tapis roulant simulé à {{site.data.keyword.iot_short_notm}}.

Pour vous familiariser avec {{site.data.keyword.iot_short_notm}} à l'aide d'un terminal IoT différent, voir le [Tutoriel d'initiation](/docs/services/IoT/getting-started.html).
{: tip}

## Prérequis
{: #prereqs}

Vous devez préalablement disposer des comptes et outils suivants :
* [Un compte {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.ng.bluemix.net/registration/){: new_window}
* [Une interface de ligne de commande Cloud Foundry (CLI CF) ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilisez l'interface CLI CF pour déployer des applications et des services sur {{site.data.keyword.Bluemix_notm}}. Pour plus d'informations, voir la documentation [Cloud Foundry CLI![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.cloudfoundry.org/cf-cli/){: new_window}
* Facultatif : [Git ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://git-scm.com/downloads){: new_window}  
Si vous choisissez d''utiliser Git pour télécharger les exemples de code, vous devez disposer d'un [compte GitHub.com ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com){: new_window}. Vous pouvez aussi télécharger le code sous forme de fichier compressé si vous n'avez pas de compte GitHub.com.
* Facultatif : Un téléphone mobile sur lequel exécuter le modèle d'application Web *Tapis roulant* permettant d'envoyer les données de l'accéléromètre.

## Etape 1 - Déploiement de {{site.data.keyword.iot_short_notm}}  
{: #deploy_watson_iot_platform_service}
Les étapes décrites ci-après vont vous permettre de déployer une instance du service {{site.data.keyword.iot_short_notm}} nommée `iotp-for-conveyor` au sein de votre environnement {{site.data.keyword.Bluemix_notm}}. Si vous disposez déjà d'une instance en cours d'exécution, vous pouvez l'utiliser dans le cadre de guide et ignorer la première étape. Dans ce cas, assurez-vous simplement d'utiliser le bon nom de service et le bon espace {{site.data.keyword.Bluemix_notm}} lors de votre progression dans les guides.
{: tip}
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
3. Déployez le service {{site.data.keyword.iot_short_notm}} sur {{site.data.keyword.Bluemix_notm}}.  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
   ```
Pour YOUR_IOT_PLATFORM_NAME, utilisez *iotp-for-conveyor*.  
Exemple : `cf create-service iotf-service iotf-service-free iotp-for-conveyor`
3. Créez votre exemple de terminal de tapis roulant.  
 - Chemin A : [Etape 2A - Utilisez le modèle d'application de simulateur de GitHub](#deploy_app).  
 - Chemin B : [Etape 2B - Créez un tapis roulant physique avec un Raspberry Pi et un moteur électrique](#raspberry).  

## Etape 2A - Déploiement d'un modèle d'application Web de tapis roulant  
{: #deploy_app}

Le modèle d'application vous permet de simuler un tapis roulant industriel connecté {{site.data.keyword.Bluemix_notm}}. Vous pouvez démarrer et arrêter le tapis et ajuster sa vitesse. Chaque modification apportée au tapis est envoyée à {{site.data.keyword.Bluemix_notm}} sous la forme d'un message MQTT qui s'affiche dans l'application. Vous pouvez surveiller le comportement du tapis en utilisant les cartes de tableaux de bord par défaut.
Le modèle d'application repose sur des bibliothèques client Node.js accessibles depuis : [https://github.com/ibm-watson-iot/iot-nodejs ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![Application de tapis roulant](images/app_conveyor_belt.png "Application de tapis roulant")

1. Servez-vous de votre outil Git favori pour cloner le référentiel ci-dessous :  
https://github.com/ibm-watson-iot/guide-conveyor-simulator
Dans l'interpréteur de commande Git, exécutez la commande suivante :
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. Sur la ligne de commande, accédez au répertoire contenant le modèle d'application.
  ```bash
cd guide-conveyor-simulator
  ```
3. Dans le répertoire *guide-conveyor-simulator*, envoyez l'application à {{site.data.keyword.Bluemix_notm}} et attribuez-lui un nouveau nom en remplaçant *YOUR_APP_NAME* dans la commande cf push. Utilisez l'option `--no-start` car vous devrez redémarrer l'application à l'étape suivante une fois qu'elle sera liée à {{site.data.keyword.iot_short_notm}}.
**Remarque :** Le déploiement de votre application peut prendre quelques minutes.
   ```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. Dans le répertoire *guide-conveyor-simulator*, liez votre application à votre instance {{site.data.keyword.iot_short_notm}} en utilisant les noms que vous avez déjà fournis.
  ```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
Pour plus d'informations sur la liaison d'applications, voir [Connexion d'applications](/docs/services/IoT/platform_authorization.html#bluemix-binding).
2. Démarrez votre application pour que la liaison prenne effet.
  ```bash
cf start YOUR_APP_NAME
  ```
A ce stade, votre modèle d'application Web est déployée sur {{site.data.keyword.Bluemix_notm}}.
Une fois le déploiement terminé, un message s'affiche pour indiquer que votre application est en cours d'exécution.   
Exemple :
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
start command:     ./vendor/initial_startup.rb

     state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
Pour voir le statut de déploiement et l'URL de l'application, exécutez la commande suivante :
  ```bash
cf apps
  ```
Corrigez les erreurs du processus de déploiement en utilisant la commande `cf logs YOUR_APP_NAME --recent`.
{: tip}
1. Accédez à l'application depuis un navigateur.  
Ouvrez l'adresse URL suivante : `https://YOUR_APP_NAME.mybluemix.net`    
Exemple : `https://conveyorbelt.mybluemix.net/`.
2. Entrez un ID de terminal et un jeton pour votre terminal.  
Le modèle d'application enregistre automatiquement un terminal de type `iot-conveyor-belt` avec le jeton et l'ID de terminal que vous avez indiqués. Pour en savoir plus sur l'enregistrement de terminaux, voir [Connexion de terminaux](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
4. Passez à l'[Etape 3 - Visualisation des données brutes dans {{site.data.keyword.iot_short_notm}}](#see_live_data).

## Etape 2B - Génération d'un tapis roulant à l'aide de Raspberry Pi
{: #raspberry}

La solution Raspberry Pi est générée à l'aide des bibliothèques client Python qui sont accessibles à l'adresse : [https://github.com/ibm-watson-iot/iot-python ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-python){: new_window}

### Schéma du circuit

![Schéma du circuit](images/raspi_circuit.png)

### Connexions requises

Les connexions suivantes entre la carte Raspberry Pi et la carte L298N Dual H Bridge Motor Driver Board sont requises. Raccordez :

- La broche 2 du Raspberry Pi à +5v sur la carte L298N
- La broche 6 du Raspberry Pi à GND sur la carte L298N
- La broche 13 du Raspberry Pi à IN1 sur la carte L298N
- La broche 15 du Raspberry Pi à IN2 sur la carte L298N
- Le connecteur +9v de la pile au connecteur +12v sur la carte L298N
- Le connecteur -9v de la pile à GND sur la carte L298N
- Le noeud +ve du moteur à OUT1 sur la carte L298N
- Le noeud -ve du moteur à OUT2 sur la carte L298N

### Configuration matérielle

1. [Raspberry Pi(2/3) ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://www.raspberrypi.org/){: new_window} avec Raspbian Jessie (8.x).
2. [L298N Dual H Bridge Motor Driver Board ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. Moteur 9V CC
4. Pile 9V

### Configuration logicielle Raspberry Pi
- Python version 2.7.9 et ultérieure, installée avec Raspbian Jessie (8.x).
- [Bibliothèque Python Watson IoT ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- Facultatif : Git.  
Si Git n'est pas déjà installé sur votre Raspberry Pi, vous pouvez l'installer en exécutant la commande suivante :  
```bash
$ sudo apt-get install git
```

### Etapes détaillées

1. Ouvrez le terminal ou SSH sur votre Raspberry Pi.
2. Utilisez votre outil Git favori pour cloner le référentiel suivant sur votre Raspberry Pi :
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
A partir de l'interpréteur de commande Git, utilisez la commande suivante :
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. Enregistrez le terminal auprès de {{site.data.keyword.iot_short_notm}}.  
Pour en savoir plus sur l'enregistrement de terminaux, voir [Connexion de terminaux](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
	 1. Dans la console {{site.data.keyword.Bluemix_notm}}, cliquez sur **Lancer** sur la page des informations détaillées du service {{site.data.keyword.iot_short_notm}}.
	     La console web {{site.data.keyword.iot_short_notm}} s'ouvre dans un nouvel onglet de navigateur à l'URL suivante :
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     Où ORG_ID est l'ID unique à six caractères de [votre {{site.data.keyword.iot_short_notm}} organisation](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}.
	 2. Dans le tableau de bord Présentation, à partir du panneau de menu, sélectionnez **Terminaux**, puis cliquez sur **Ajouter un terminal**.
	 3. Créez un type de terminal pour le terminal que vous ajoutez.
	     1. Cliquez sur **Créer un type de terminal**.
	     2. Entrez le nom du type de terminal `iotp-for-conveyor` et une description du type de terminal.  
	**Astuce :** vous pouvez entrer n'importe quel nom de type de terminal, mais les autres guides supposent que vous utilisiez des terminaux de type `iotp-for-conveyor`. Si vous choisissez un type de terminal différent, pensez à modifier les paramètres des autres guides en conséquence.
	     3. Facultatif : Entrez des métadonnées et des attributs de type de terminal.
	 4. Cliquez sur **Suivant** pour commencer le processus d'ajout de votre terminal avec le type de terminal sélectionné.
	 5. Entrez un ID de terminal, par exemple `conveyor_belt`.
	 5. Cliquez sur **Suivant** pour finaliser le processus.
	 6. Fournissez un jeton d'authentification ou acceptez un jeton généré automatiquement.
	 7. Vérifiez que les informations récapitulatives sont correctes puis cliquez sur **Ajouter** pour ajouter la connexion.
	 8. Sur la page Informations sur le terminal, copiez et sauvegardez les détails suivants :
	     * ID d'organisation
	     * Type de terminal
	     * ID de terminal
	     * Méthode d'authentification
	     * Jeton d'authentification
	     Vous aurez besoin de l'ID d'organisation, du type de terminal, de l'ID du terminal et du jeton d'authentification pour configurer votre terminal pour qu'il se connecte à {{site.data.keyword.iot_short_notm}}.
4. Accédez à la racine du référentiel cloné *guide-conveyor-rasp-pi*.
5. Rendez le script shell exécutable à l'aide de la commande `sudo chmod +x setup.sh`
6. Exécutez le fichier *setup.sh* et entrez les détails que vous avez copiés de la page d'information sur le terminal à l'invite.
```bash  
./setup.sh
```  

7. Exécutez le programme deviceClient.  
Lorsque vous exécutez le programme, le Raspberry Pi démarre le moteur et celui-ci fonctionne pendant 2 minutes en fonction de vos paramètres.
```bash
python deviceClient.py -t 2
```

Pendant que le moteur s'exécute, le programme publie des événement de type `sensorData` qui ont le modèle de structure de contenu suivant :  
```
{
"d": {
  "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. Passez à [Etape 3 - Visualisation des données brutes dans {{site.data.keyword.iot_short_notm}}](#see_live_data).

## Etape 3 - Visualisation des données brutes dans {{site.data.keyword.iot_short_notm}}
{: #see_live_data}
1. Vérifiez que le terminal est enregistré auprès de {{site.data.keyword.iot_short_notm}}.
 1. Connectez-vous à votre tableau de bord {{site.data.keyword.Bluemix_notm}} à l'adresse : https://bluemix.net
 2. Dans [votre liste de services ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://bluemix.net/dashboard/services){: new_window}, cliquez sur le service *iotp-for-conveyor* de {{site.data.keyword.iot_short_notm}}.
 3. Cliquez sur *Lancer* pour ouvrir le tableau de bord {{site.data.keyword.iot_short_notm}} dans un nouvel onglet du navigateur.  
Vous pouvez ajouter l'URL à vos signets afin d'y accéder plus facilement à l'avenir.   
Exemple : `https://*iot-org-id*.internetofthings.ibmcloud.com`.
 4. Dans le menu, sélectionnez **Terminaux** et vérifiez que votre nouveau terminal s'affiche.
2. Envoyez les données du détecteur à la plateforme.   
Le terminal envoie les données à {{site.data.keyword.iot_short_notm}} lorsque les relevés de capteur changent. Vous pouvez simuler cet envoi de données en arrêtant le tapis roulant, en le démarrant ou en changeant sa vitesse.   
**Chemin :** si vous accédez à l'application depuis un navigateur mobile, essayez de secouer votre smartphone pour déclencher les données de l'accéléromètre du tapis roulant.
{: tip}

## Etapes suivantes
{: @whats_next}  
Passez au guide suivant ou à une autre rubrique qui vous intéresse :
- Chemin A : Modifiez l'application du tapis roulant conformément à vos besoins.  
Pour plus de détails techniques, voir :
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Bibliothèques Node.js clienmt ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **Remarque :** Bluemix est maintenant IBM Cloud. Consultez le [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe") pour plus de détails.
 
- Chemin B : Modification de la configuration Raspberry Pi selon vos besoins.  
Pour plus de détails techniques, voir :
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}


**Remarque :** Bluemix est maintenant IBM Cloud. Consultez le [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe") pour plus de détails.

- [Guide 2 : Surveillance des données de votre terminal](getting-started-iot-monitoring.html)  
Maintenant que vous avez connecté un ou plusieurs terminaux et que vous avez commencé à exploiter les données de terminal, vous pouvez commencer à surveiller une collection de terminaux.
- [Guide 3 : Simulation d'un grand nombre de terminaux](getting-started-iot-large-scale-simulation.html)  
Le modèle d'application de tapis roulant du chemin A vous permet de simuler manuellement un ou quelques terminaux de tapis roulant. Ce guide vous permet de configurer un environnement simulé qui contient un grand nombre de terminaux.
- [En savoir plus sur {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html)
- [En savoir plus sur les API {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/reference/api.html)
