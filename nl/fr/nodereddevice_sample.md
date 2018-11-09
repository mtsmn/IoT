---

copyright:
  years: 2016, 2018
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Création et connexion d'un simulateur de terminal Node-RED
Utilisez Node-RED pour créer un simulateur de terminal et envoyer des données de terminal simulé à votre organisation {{site.data.keyword.iot_full}}.  
{:shortdesc}

## Présentation
Node-RED est un outil qui permet de relier des terminaux matériels, des API et des services en ligne de façon inédite et intéressante. Pour plus d'informations, voir le site Web [Node-RED ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://nodered.org/){: new_window}.  

Vous pouvez exécuter votre instance Node-RED dans votre propre environnement ou l'utiliser en tant qu'application {{site.data.keyword.Bluemix_notm}}. La procédure suivante inclut les instructions relatives à {{site.data.keyword.Bluemix_notm}}.

Créez et connectez le simulateur de terminal Node-RED pour envoyer des messages de terminal MQTT à {{site.data.keyword.iot_short_notm}}. Dans l'exemple ci-dessous, le simulateur de terminal simule l'envoi de données pour un conteneur de fret à un courtier MQTT, par exemple {{site.data.keyword.iot_short_notm}}.

## Procédure

1. Créez le simulateur de terminal Node-RED en procédant comme indiqué ci-après :   
    1. Connectez-vous à {{site.data.keyword.Bluemix_notm}} (https://console.ng.bluemix.net).
    2. Sélectionnez l'onglet **Catalogue**.
    3. Localisez la section Boilerplates du catalogue de service et cliquez sur **Internet of Things Platform Starter**. **Astuce :** Cliquez [ici ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window} pour accéder directement à la page Internet of Things Platform Starter.
    4. Sur la page Internet of Things Platform Starter, sélectionnez l'espace où vous souhaitez déployer Node-RED, vérifiez les sélections pour créer une application, puis cliquez sur **Créer** pour ajouter Node-RED à votre organisation Bluemix. Par exemple :
    <ul>
     <li> Espace : dev
     <li> Nom d'application : myDevice
     <li> Nom d'hôte : myDevice  
    </ul>  
Laissez les autres options avec leurs valeurs par défaut. Une fois l'application déployée, la page Start coding with Node-RED s'affiche.
**Remarque :** Le processus de transfert peut prendre quelques minutes.  

2. Cliquez sur le lien Routes pour ouvrir Node-RED ou cliquez sur Visite de l'URL de l'application, par exemple `http://myDevice.mybluemix.net`  
3. Procédez comme suit pour **sécuriser votre éditeur Node-RED** :
    1. Cliquez sur **Suivant**.
    2. Ajoutez un nom d'utilisateur et un mot de passe.  
    **Remarque :** Vous devez respecter les règles de création de mot de passe pour que le bouton **Suivant** devienne actif.  
    3. Facultatif. Cochez la case **autorisant tout le monde à afficher l'éditeur, mais pas à apporter de modifications** ou celle **autorisant tout le monde à accéder à l'éditeur et à apporter des modifications (non recommandée)**
    4. Cliquez sur **Terminer** pour terminer la configuration.
4. Cliquez sur **Go to your Node-RED flow editor**
5. Entrez le nom d'utilisateur et le mot de passe que vous avez créés précédemment.  
Le flux du simulateur de terminal est disponible dans l'éditeur de flux. Le flux simule un **Thermostat** qui envoie sa position, la température et les données d'humidité relevées à {{site.data.keyword.iot_short_notm}}.  
6. Testez la connexion du terminal en vérifiant qu'un message de connexion s'affiche pour le noeud **Send to IBM IoT Platform**. Cette vérification s'applique également au noeud **IBM IoT App In** pour le flux secondaire **Temperature Monitor**.  
7. Envoyez et recevez des messages de terminal MQTT en procédant comme indiqué ci-après :  
    1. Cliquez sur le noeud d'injection **Send Data** pour déclencher l'envoi des données à {{site.data.keyword.iot_short_notm}}.
       **Remarque :** Vous pouvez activer le noeud de débogage **Debug output payload** pour voir quelles sont les données envoyées et vérifier le noeud de fonction **Device payload** pour voir le code qui génère le contenu. 
    2. Après avoir cliqué sur **Send Data**, les messages MQTT sont envoyés à {{site.data.keyword.iot_short_notm}} et sont reçus par le noeud **IBM IoT App In**. Le flux secondaire **Temperature Monitor** détermine si la température est comprise dans la plage et affiche un message sur l'onglet de débogage. 
       **Remarque :** Cliquez sur le noeud de commutation **temp thresh** pour vérifier et modifier les valeurs de seuil.
    3. Vérifiez l'onglet de débogage. Un message s'affiche, par exemple **Temperature (17) within safe limits**.
    
Vous avez connecté votre exemple de terminal IoT à {{site.data.keyword.iot_short_notm}} et vous pouvez voir les données le concernant.
