---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# API HTTP pour les terminaux
{: #api}

Deux types d'API HTTP peuvent être utilisés avec {{site.data.keyword.iot_full}}. Vous pouvez utiliser les API REST HTTP pour créer, mettre à jour, répertorier et supprimer les terminaux. Utilisez les API de messagerie HTTP pour envoyer des informations d'événement des terminaux au cloud et pour accepter des commandes des applications dans le cloud. 

## Connexions client
{: #client_connections}

Pour plus d'informations sur la sécurité du client et pour savoir comment connecter des clients dans {{site.data.keyword.iot_short_notm}}, voir [Connexion d'applications, de terminaux et de passerelles à {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

**Remarque :** le port HTTP 1883 est désactivé par défaut. Pour obtenir des informations sur la modification de la configuration par défaut, voir [Configuration des politiques de sécurité](../reference/security/set_up_policies.html#set_up_policies.md).

## Authentification

Toutes les demandes d'authentification doivent inclure un en-tête d'autorisation. L'authentification de base est la seule méthode prise en charge. Lorsqu'un terminal effectue une demande HTTP via l'API REST HTTP de {{site.data.keyword.iot_short_notm}}, les données d'identification suivantes sont requises :

|Données d'identification|Entrée requise|
|:---|:---|
|Nom d'utilisateur|`use-token-auth`
|Mot de passe| Jeton d'authentification qui a été généré automatiquement ou que vous avez spécifié manuellement lors de l'enregistrement du terminal.


## En-têtes de demande Content-Type

Un en-tête de demande `Content-Type` doit être fourni avec la demande. Le tableau suivant présente le mappage entre les types pris en charge et les formats internes de {{site.data.keyword.iot_short_notm}}.

|En-tête Content-Type|Format dans {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

## Qualité de service

Semblable au niveau 0 ("Une fois tout au plus") de la qualité de service MQTT pour la distribution, la messagerie REST HTTP fournit une distribution de message non permanente, mais vérifie que la demande est correcte et qu'elle peut être distribuée au serveur avant que celui-ci n'envoie la réponse HTTP. Une réponse contenant le code de statut HTTP 200 indique que le message a bien été distribué au serveur. Lorsque vous utilisez le niveau de qualité de service MQTT "Une fois tout au plus" ou l'équivalent HTTP pour distribuer des messages d'événement, le terminal ou l'application doit implémenter une logique de relance afin de garantir la distribution.

Pour plus d'informations sur le protocole MQTT et les niveaux de qualité de service pour {{site.data.keyword.iot_short_notm}}, voir [Messagerie MQTT](../reference/mqtt/index.html).

Vous pouvez utiliser les API de messagerie HTTP pour publier des événements des terminaux dans le cloud et pour recevoir des commandes des applications dans le cloud.

## Publication d'événements
{: #event_publication}

Outre l'utilisation du protocole de messagerie MQTT, vous pouvez également configurer vos terminaux pour qu'ils publient des événements sur {{site.data.keyword.iot_short_notm}} via HTTP en exécutant des commandes d'API de messagerie HTTP.

Utilisez l'une des URL suivantes pour soumettre une demande ``POST`` à partir d'un terminal qui est connecté à {{site.data.keyword.iot_short_notm}} :

### Demande POST non sécurisée pour la publication d'événements

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Demande POST sécurisée pour la publication d'événements

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Remarque :** Le port 443, port SSL par défaut, peut également être spécifié pour les appels API HTTP sécurisés.

## Réception de commandes
{: #receive_commands}

Outre l'utilisation du protocole de messagerie MQTT, vous pouvez également configurer vos terminaux pour qu'ils reçoivent des commandes de {{site.data.keyword.iot_short_notm}} sur HTTP en utilisant des commandes de l'API de messagerie HTTP. Un terminal peut recevoir des commandes qui lui sont destinées.

Utilisez l'une des URL suivantes pour soumettre une demande ``POST`` à partir d'un terminal qui est connecté à {{site.data.keyword.iot_short_notm}} :

### Demande POST non sécurisée pour la réception des commandes

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Demande POST sécurisée pour la réception des commandes

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Remarque :** Le port 443, port SSL par défaut, peut également être spécifié pour les appels API HTTP sécurisés.

En option, vous pouvez inclure le paramètre *waitTimeSecs* dans le corps de la demande HTTP afin de
spécifier un entier représentant le nombre maximum de secondes d'attente d'une commande :
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Remarques importantes :**
- La valeur de *waitTimeSecs* doit être un entier compris entre 0 et 3600 (secondes). La valeur par défaut est 0.
- Pour recevoir toute commande, utilisez le caractère générique
"any" (+) en guise de composant {command}. Si le caractère générique est utilisé, l'identificateur de la commande est contenu dans le champ d'en-tête de réponse *X-commandId*.
- Si le code d'état de la réponse HTTP est 200, les données de la commande sont contenues dans le corps de la réponse. Examinez le champ d'en-tête de réponse *Content-Type* pour identifier le type de contenu.
- Si le code d'état de la réponse HTTP est 204, aucune donnée de commande n'est disponible. 

Pour accéder aux API REST HTTP de {{site.data.keyword.iot_short_notm}}, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

Pour accéder aux API de messagerie HTTP de {{site.data.keyword.iot_short_notm}}, voir [API de messagerie HTTP de {{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

Pour obtenir des informations sur le développement de terminaux, consultez [Développement de terminaux sur {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).

## Exemple de demande POST sécurisée

L'exemple suivant envoie un message d'un terminal à {{site.data.keyword.iot_short_notm}} via le protocole HTTP. Une application reçoit le message en s'abonnant à la rubrique sur laquelle le message est publié. Le tableau suivant fournit des informations sur le terminal. 

|Paramètre|Valeur|
|:---|:---|
|ID d'organisation |myOrgID
|Type de terminal| TestDevices
|ID de terminal | TestPublishEvent
|Méthode d'authentification|use-token-auth
|Jeton d'authentification|passw0rd


L'exemple suivant utilise curl pour envoyer un événement appelé *TestMessage* du terminal *TestPublishEvent* à {{site.data.keyword.iot_short_notm}} via le protocole HTTP : 

```
curl -v -X POST -H "Content-Type: application/json" -u "use-token-auth:passw0rd" -d @message.txt  https://myOrgID.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/TestDevices/devices/TestPublishEvent/events/TestMessage
```

Le message est contenu dans un fichier texte appelé *message.txt*. Le contenu du fichier *message.txt* est montré dans l'exemple suivant : 
```
{ "d": { "myName": "Publish Test","cputemp": 46,"sine": -10,"cpuload": 1.45 }  } 
```
Le message est publié à la rubrique *iot-2/evt/TestMessage/fmt/json*. Une application appelée *testSubscriber* souscrivant au filtre de rubrique *iot-2/type/+/id/+/evt/+/fmt/+* est créée. Le filtre de rubrique correspond à la rubrique sur laquelle le message est publié. 

Vous pouvez utiliser l'exemple de python *simpleApp.py* fourni dans la [bibliothèque pythons ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://github.com/ibm-watson-iot/iot-python/tree/master/samples/simpleApp) pour vous aider à configurer votre application pour souscrire au filtre de rubrique *iot-2/type/+/id/+/evt/+/fmt/+*. 

Le script python suivant souscrit à un filtre de rubrique qui correspond à la chaîne de rubrique sur laquelle le message est publié :

```
python simpleApp.py -o "myOrgID" -i "testSubscriber" -k "a-myOrgID-jg95dkpscn" -t "L(r4gfPa3bLP2kz4Nb"
```
où :

- **-o** est le nom de l'organisation.
- **-i** est le nom de l'application.
- **-k** est la clé d'API qui est générée lorsque l'application est créée.  
- **-t** est le jeton d'authentification qui est généré lorsque l'application est créée.   


L'exemple suivant montre une réponse réussie à la réception du message publié par le terminal *TestPublishEvent*. 
```
=============================================================================
Timestamp                        Device                        Event
2017-12-06 15:09:40,438   ibmiotf.application.Client  INFO    Connected successfully: a:myOrgID:testSubscriber
=============================================================================
2017-12-06T15:09:59.691290+00:00 TestDevices:TestPublishEvent  TestMessage: {"d": {"myName": "Publish Test", "cputemp": 46, "sine": -10, "cpuload": 1.45}}
2017-12-06T15:10:01.056000+00:00 TestDevices:TestPublishEvent  Connect 199.999.99.99
2017-12-06T15:10:01.446000+00:00 TestDevices:TestPublishEvent  Disconnect 199.999.99.99 (None)

```

## Cache de dernier événement
{: #last-event-cache}

Vous pouvez utiliser le cache de dernier événement pour stocker des informations sur le dernier événement qui a été envoyé à {{site.data.keyword.iot_short_notm}} par un terminal connecté. En utilisant une [API ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}, vous pouvez extraire la dernière valeur enregistrée d'un ID d'événement spécifique pour un terminal ou la dernière valeur enregistrée de chaque ID d'événement signalé par un terminal spécifique. 

### Configuration du cache de dernier événement

La fonction de cache de dernier événement est désactivée par défaut, mais vous pouvez activer la fonctionnalité de l'une des manières suivantes : 

-	Définissez le paramètre *enabled* sur **true** à l'aide d'une API.
-	Sur la page *Paramètres* du tableau de bord de {{site.data.keyword.iot_short_notm}}, définissez **Enable LEC** sur **On**.

La durée pendant laquelle les données d'événement sont stockées dans le cache est spécifiée par la valeur de durée de vie (TTL). Les données de dernier événement d'un événement spécifique sont stockées pendant 7 jours par défaut. Vous pouvez changer cette durée à l'aide d'une API pour mettre à jour le paramètre **ttlDays** ou en sélectionnant une valeur pour la zone **Event Data TTL** sur la page *Paramètres* du tableau de bord de {{site.data.keyword.iot_short_notm}}.

Dans les plans Lite, vous pouvez stocker des informations pendant un minimum de un jour et un maximum de sept jours. Dans les autres plans, vous pouvez stocker des informations pour un minimum de un jour et un maximum de 45 jours.

Utilisez les informations suivantes pour vous aider à configurer les paramètres du cache de dernier événement à l'aide des API.

Pour extraire la configuration en cours du cache de dernier événement pour une organisation, utilisez l'API suivante :

```
    GET /api/v0002/config/lec
```   

   
L'exemple suivant montre une réponse réussie à la méthode GET :
```
{
    "enabled": false,
    "ttlDays": 7
}
```
Toute personne ou application dans l'organisation est autorisée à accéder à ce point de terminaison. 

Si le cache de dernier événement est désactivé par l'administrateur de {{site.data.keyword.iot_short_notm}} pour une organisation, le code de statut HTTP 403 FORBIDDEN suivant est renvoyé en réponse à la méthode GET :

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: Le cache des derniers événements est désactivé pour cette organisation. Pour plus d'informations, contactez votre équipe de support."
}
```

Si le cache de dernier événement n'est pas activé, un code de statut HTTP 404 est renvoyé en réponse à la méthode GET. Pour activer la fonction de cache de dernier événement pour une organisation, utilisez l'API suivante :

```
    PUT /api/v0002/config/lec
```

avec l'exemple de contenu suivant : 
```
{
    "enabled": true,
    "ttlDays": 5
}
```
où : 

- **enabled** est obligatoire et spécifie si le cache de dernier événement est activé pour une organisation. La valeur doit être une valeur booléenne. 

- **ttlDays** est facultatif et spécifie le nombre de jours pendant lesquels un événement est conservé dans le cache de dernier événement pour une organisation. La valeur doit être un nombre entier compris entre 1 et 45 inclus.
   
 
Seuls l'administrateur, les opérateurs et les applications d'opérateurs sont autorisées à accéder à ce point de terminaison. Si les paramètres ou le JSON ne sont pas valide, un code de statut HTTP 401 DEMANDE INCORRECTE est renvoyé à la méthode PUT. 

**Remarque :** Un changement de configuration peut prendre jusqu'à 30 secondes.

### Extraction des informations du cache de dernier événement

A l'aide d'une API, vous pouvez extraire le dernier événement qui a été envoyé par un terminal. Vous pouvez extraire le statut du terminal quel que soit son emplacement physique ou son statut d'utilisation. Vous pouvez extraire la dernière valeur enregistrée d'un ID d'événement pour un terminal spécifique ou la dernière valeur enregistrée pour chaque ID d'événement signalé par un terminal spécifique. Les données de dernier événement d'un terminal peuvent être extraites pour n'importe quel événement spécifique qui s'est produit au cours des 45 derniers jours.

Pour obtenir la valeur la plus récente d'un ID d'événement spécifique, utilisez la demande d'API ci-après, qui renvoie la dernière valeur enregistrée pour l'ID d'événement "power".

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

La réponse est renvoyée au format JSON suivant :

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**Remarque :** Si la réponse d'API est au format JSON, les contenus d'événement quant à eux peuvent être écrits dans n'importe quel format. Les contenus qui sont renvoyés par l'API Cache de dernier événement sont codés en base64.

Pour obtenir la valeur la plus récente de chaque ID d'événement signalé par un terminal, utilisez la demande d'API suivante :

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

La réponse inclut tous les ID d'événement qui ont été envoyés par le terminal. Dans l'exemple suivant, des valeurs sont renvoyées pour les événements "power" et "temperature".

```
[
    {
        "deviceId": "<device-id>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

Pour accéder aux API de cache de dernier événement de {{site.data.keyword.iot_short_notm}}, reportez-vous à [API de gestion des informations HTTP de {{site.data.keyword.iot_short_notm}}![Icône de lien externe](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}.
