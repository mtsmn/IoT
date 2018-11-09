---

copyright:
 years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API de messagerie HTTP pour les terminaux
{: #api}


## Accès à la documentation de l'API de messagerie HTTP pour les terminaux
{: #rest_messaging_api}

Pour accéder à la documentation de l'API de messagerie HTTP {{site.data.keyword.iot_short_notm}},
consultez [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Connexions client
{: #client_connections}

Pour plus d'informations sur la sécurité du client et pour savoir comment connecter des clients dans {{site.data.keyword.iot_short_notm}}, voir [Connexion d'applications, de terminaux et de passerelles à {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

## Publication d'événements 
{: #event_publication}

Outre l'utilisation du protocole de messagerie MQTT, vous pouvez également configurer vos terminaux pour qu'ils publient des événements sur {{site.data.keyword.iot_short_notm}} via HTTP en exécutant des commandes d'API REST HTTP.

Utilisez l'une des URL suivantes pour soumettre une demande ``POST`` à partir d'un terminal qui est connecté à {{site.data.keyword.iot_short_notm}} :

### Demande POST non sécurisée pour la publication d'événements

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Demande POST sécurisée pour la publication d'événements

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Remarque :** Le port 443, port SSL par défaut, peut également être spécifié pour les appels API HTTP sécurisés.

### Authentification

Toutes les demandes doivent inclure un en-tête d'autorisation. L'authentification de base est la seule méthode prise en charge. Lorsqu'un terminal effectue une demande HTTP via l'API REST HTTP de {{site.data.keyword.iot_short_notm}}, les données d'identification suivantes sont requises :

|Données d'identification|Entrée requise|
|:---|:---|
|Nom d'utilisateur|`use-token-auth`
|Mot de passe| Jeton d'authentification qui a été généré automatiquement ou que vous avez spécifié manuellement lors de l'enregistrement du terminal.


### En-têtes de demande Content-Type

Un en-tête de requête `Content-Type` doit être fourni avec la demande si le contenu n'est pas JSON. Le tableau suivant présente le mappage entre les types pris en charge et les formats internes de {{site.data.keyword.iot_short_notm}}.

|En-tête Content-Type|Format dans {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"


## Réception de commandes
{: #receive_commands}

Outre l'utilisation du protocole de messagerie MQTT, vous pouvez également
configurer vos terminaux pour qu'ils reçoivent des commandes
de {{site.data.keyword.iot_short_notm}} sur HTTP en utilisant des commandes de l'API de
messagerie HTTP. Un terminal peut recevoir des commandes qui lui sont destinées.

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


Pour des informations sur le développement de terminaux, consultez [Développement de terminaux sur {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).
