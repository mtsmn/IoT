---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Surveillance de la connectivité des clients (bêta)
{: #connect_status}

{{site.data.keyword.iot_full}} vous permet d'interroger et de surveiller l'état de connexion des clients. Vous pouvez savoir en temps réel si un client est connecté et résoudre tous les problèmes de connexion.
{:shortdesc}

Par exemple, vous pouvez voir quand un client a été actif pour la dernière fois et pourquoi il s'est déconnecté ou vous pouvez afficher tous les clients qui ne se sont pas connecté depuis la veille afin de pouvoir résoudre un problème éventuel.

**Important :** la fonction de surveillance de la connectivité des clients n'est disponible que dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Interrogation de l'état de connexion des clients
{: #query_status}

L'API Etat de connexion des client vous permet d'extraire et d'interroger l'état de connexion de n'importe quel client s'étant connecté à {{site.data.keyword.iot_short_notm}}.

Lorsque vous interrogez l'état de connexion, vous pouvez afficher les informations suivantes :

 - l'état de connectivité (connecté ou déconnecté)
 - l'heure de la dernière connexion au format ISO 8601 
 - l'heure de la dernière déconnexion au format ISO 8601
 - le dernier événement d'activité (par exemple publication MQTT ou abonnement MQTT)
 - l'heure de la dernière activité au format ISO 8601
 - l'entité qui a causé la déconnexion, si le client est déconnecté (client, serveur ou réseau)
 - le code raison MQTT de la déconnexion, si le client est déconnecté

**Exemples**

Pour extraire l'état de connexion d'un client spécifique :

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

Pour extraire d'état de connexion de tous les clients :

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

Pour extraire tous les clients connectés :

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

Pour extraire tous les clients qui ont été actifs au cours des deux derniers jours :

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**Remarque :** il peut exister un bref délai entre l'heure à laquelle un client se connecte et l'heure à laquelle l'état de connexion de l'API reflète l'état correct.

Pour en savoir plus sur l'API, y compris tous les filtres de requête disponibles, consultez [le document Swagger Client Connection State API![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}.

## Surveillance de l'état de connexion des clients
{: #monitor_status}

 Tous les clients ont une rubrique de surveillance qui vous permet de vous abonner pour recevoir des mises à jour en temps réel de l'état de connexion du client. Pour en savoir plus, voir [Abonnement aux messages d'état d'un terminal](../../applications/mqtt.html#subscribe_device_commands).

## Résolution des problèmes de connectivité des clients

L'API Journaux de connexion vous permet d'extraire une liste des événements du journal des connexions d'un terminal afin de diagnostiquer les problèmes de connectivité. Les entrées enregistrent les connexions réussies, les tentatives de connexion ayant échoué, les déconnexions intentionnelles et les déconnexions initiées par le serveur. 

Pour plus d'informations, consultez le [document Swagger Connection Logs API ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}.
