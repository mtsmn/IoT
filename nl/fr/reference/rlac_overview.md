---

copyright:
  years: 2017
lastupdated: "2017-10-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Présentation du contrôle d'accès au niveau de la ressource (bêta)
{: #RLAC_overview}

Le contrôle d'accès au niveau de la ressource vous permet de contrôler l'accès des utilisateurs et des clés d'API afin de gérer les terminaux. Vous pouvez utiliser des groupes de ressources pour définir les terminaux d'une organisation que chaque utilisateur ou clé d'API peut gérer. Pour plus d'informations sur la façon de configurer le contrôle d'accès au niveau de la ressource, voir [Configuration du contrôle d'accès au niveau de la ressource](rlac.html#configure_RLAC).

**Important :** La fonction de contrôle d'accès au niveau de la ressource {{site.data.keyword.iot_full}} est disponible uniquement dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Notions relatives au contrôle d'accès au niveau de la ressource 
{: #RLAC_concepts}

Les groupes de terminaux font partie des contrôles d'accès au niveau de la ressource pour {{site.data.keyword.iot_short_notm}}. Vous pouvez la fonctionnalité des groupes de terminaux pour gérer le nombre de terminaux accessibles par le personnel en fonction des rôles de chacun. Avant d'implémenter des groupes de terminaux, déterminez le sens et la portée des groupes de terminaux afin d'améliorer l'efficacité de votre solution. 

Vous pouvez implémenter des groupes de terminaux dans le cadre de votre solution IoT pour permettre à un utilisateur ou une application d'accéder à un sous-ensemble de terminaux et non à la totalité des terminaux qui sont associés à une organisation. Pour améliorer l'efficacité des groupes de terminaux, utilisez-les lorsqu'une personne doit avoir accès à de nombreux terminaux.

Prenons l'exemple d'une société qui emploie beaucoup de techniciens de maintenance et de personnel d'opérations et qui souhaite implémenter une solution IoT au Royaume-Uni. La société configure un groupe de terminaux pour chacune des 69 villes, 9 groupes de terminaux pour chacune des régions géographiques et un groupe de terminaux pour l'ensemble du Royaume-Uni. Les terminaux sont affectés à ces groupes et les groupes sont affectés à 15 techniciens de maintenance et personnel d'opérations, dont certains possèdent un droit d'accès sur plusieurs groupes. Les utilisateurs responsables d'un ou deux terminaux peuvent continuer à utiliser les applications mobiles et les architectures de l'entreprise qui sont externes à {{site.data.keyword.iot_short_notm}} et viennent en complément de la solution IoT globale.

Vous pouvez poser vos questions sur les groupes de terminaux dans l'espace [Questions in Internet of Things ![External link icon](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window}.

## Limites du groupe de ressources quant au contrôle d'accès

Des limites de groupe de ressources sont appliquées pour garantir le bon fonctionnement de la fonction de contrôle d'accès au niveau de la ressource. Ces limites restreignent le nombre de groupes de ressources affectés à un sujet, le nombre de terminaux au sein des groupes, et le nombre de groupes auxquels une ressource peut appartenir.

Les limites appliquées sont les suivantes :

 - 10 groupes de ressources maximum peuvent être affectés à une clé d'API, un utilisateur ou une passerelle.
 - Un groupe de ressources peut 300 ressources maximum.
 - Une ressource peut appartenir à 10 groupes maximum.

## Terminologie liée au contrôle d'accès au niveau de la ressource

Les sections ci-dessous vont vous aider à comprendre la terminologie utilisée dans le cadre de la fonctionnalité du contrôle d'accès au niveau de la ressource. 

### Objets
{: #subjects}

Un objet est une entité authentifiée qui demande l'accès à la plateforme, qui doit être autorisé. Les différents type d'objets valides sont les suivants :

- *Utilisateurs* : Utilisateurs finaux des applications. Un membre est un utilisateur dont le compte n'a pas de date d'expiration et un invité est un utilisateur dont le compte a une date d'expiration.
- *Terminaux* : Terminaux physiques qui accèdent à la plateforme {{site.data.keyword.iot_short_notm}} et utilisent le type Terminal.
- *Passerelles* : Terminaux physiques qui accèdent à la plateforme {{site.data.keyword.iot_short_notm}} et utilisent le type Passerelle.
- *Applications* : Applications ou services qui accèdent à la plateforme {{site.data.keyword.iot_short_notm}} à l'aide de clés d'API à des fins d'autorisation et de contrôle d'accès.

### Ressources
{: #resources}

Une ressource est une entité sur laquelle le sujet effectue une action. Le sujet demande l'accès à la plateforme autorisée pour la ressource. Les terminaux représentent les seules ressources prises en charge dans la version bêta du contrôle d'accès au niveau de la ressource.

### Actions
{: #actions}

Action effectuée par le sujet, principalement les actions Créer, Lire, Mettre à jour, Supprimer ou Exécuter. Les opérations sont utilisées pour regrouper les actions sur les ressources.

### Applications
{: #applications}

Une application peut agir pour le compte d'un sujet qui est inconnu de la plateforme, par exemple une application mobile qui contrôle un appareil ménager, et pour le compte d'un terminal réel ou simulé qui est connu ou inconnu de la plateforme. Une application peut également désigner une application côté serveur qui n'agit pas pour le compte d'un autre type de terminal.

L'authentification est gérée par la plateforme {{site.data.keyword.iot_short_notm}} et son accès est fourni en fonction de la clé d'API ou du jeton.

### Opérations
{: #operations}

Une opération contient une ou plusieurs actions qui sont exécutées sur certains types de ressources à l'aide d'interfaces utilisateur et d'interfaces de programmation, telles que les services REST et les interfaces MQTT. Les opérations sont utilisées par différents sujets, notamment les membres, les applications et les terminaux. Les opérations sont identifiées par leur ID d'opération (OID).

### Rôles
{: #roles}

Les rôles sont des ensembles d'opérations utilisés pour accorder l'autorisation d'utiliser les opérations. Un sujet peut être associé à plusieurs rôles, et le rôle est un attribut d'un sujet.

**Format des rôles**

Tableau de 0..n rôles

    { "roles": [ "PD_ADMIN_APP" ] }

**Format des "rôles-groupes"**

Mappage de 0..n <string, list<string>> paires

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### Droits
{: #permissions}

Les droits autorisent un sujet à exécuter des opérations sur une ressource ou un groupe de ressources.

### Groupes de ressources
{: #resource_groups}

Un groupe de ressources est une collection de terminaux en tant que ressources. Vous pouvez ajouter des terminaux à des groupes de ressources et affecter des groupes de ressources à des objets dans des paires "rôles-groupes". Ces relations permettent aux objets d'effectuer des opérations pour un rôle donné sur des groupes de terminaux spécifiques.

### Utilisateurs
{: #users}

Les utilisateurs se connectent au tableau de bord de la plateforme {{site.data.keyword.iot_short_notm}} en utilisant un ID membre d'une organisation. Les utilisateurs sont associés à des rôles qui leur accordent le droit d'appeler certains ensembles d'API HTTP. Pour en savoir plus sur les rôles utilisateur, voir [Niveaux d'accès pour des rôles d'utilisateur](roles_access.html#levels-of-access-for-user-roles).

### Clés d'API
{: #API_keys}

Les clés d'API sont des jetons utilisés pour appeler les API HTTP de la plateforme {{site.data.keyword.iot_short_notm}}. Les clés d'API sont associées à des rôles qui leur accordent le droit d'appeler certains ensembles d'API HTTP. Pour en savoir plus sur le rôle des clés d'API, voir [Niveaux d'accès pour les rôles d'application](app_roles_access.html#levels-of-access-for-application-roles).

## API où le contrôle d'accès au niveau de la ressource est appliqué
{: #RLAC_enforced_APIs}
Lorsque le contrôle au niveau de la ressource est activé, les API liées aux terminaux qui sont incluses dans cette section ne fonctionnent que si le terminal est accessible à l'appelant.

### API de terminaux (individuelles)

**Afficher la liste des terminaux par type**

    GET /api/v0002/device/types/${typeId}

Seul le sous-ensemble de terminaux appartenant aux groupes appropriés est renvoyé. 

**Obtenir un terminal**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Obtenir des détails sur la gestion des terminaux**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Obtenir la liste des terminaux enregistrés auprès d'une passerelle**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

Renvoie une erreur 404 si la passerelle n'est pas accessible par l'appelant. Seul le sous-ensemble de terminaux appartenant aux groupes appropriés est renvoyé. Les terminaux et les passerelles doivent se trouver dans le groupe de l'appelant.

**Mettre à jour un terminal**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Supprimer un terminal**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

### API de terminaux (collectives)

**Afficher la liste des terminaux**

    GET /api/v0002/bulk/devices

Seul le sous-ensemble de terminaux appartenant aux groupes appropriés est renvoyé. 

**Supprimer des terminaux**

    DELETE /api/v0002/bulk/devices/remove

Seul le sous-ensemble de terminaux appartenant aux groupes appropriés est supprimé. Les terminaux qui ne sont pas accessibles continuent de renvoyer success = true.

**Mettre à jour des terminaux**

    PUT /api/v0002/bulk/devices/update

### API de gestion des terminaux

**Initier une demande**

    POST /api/v0002/mgmt/requests

Echoue si le terminal n'est pas accessible par l'appelant.

**Supprimer une demande**

    DELETE /api/v0002/mgmt/requests/${requestId}

Echoue si le terminal n'est pas accessible par l'appelant.

**Voir la demande**

    GET /api/v0002/mgmt/requests/${requestId}

**Afficher la liste des demandes**

    GET /api/v0002/mgmt/requests

**Obtenir une demande**

    GET /api/v0002/mgmt/requests/${requestId}

**Obtenir des détails sur une demande**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**Obtenir des détails sur une demande d'un terminal**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### API d'identification des problèmes

**Journaux de connexion d'un terminal**

    GET /api/v0002/logs/connection

Renvoie un tableau vide si le terminal n'est pas accessible.

**Ajouter un code d'erreur de terminal**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Afficher les codes d'erreur d'un terminal**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Effacer les codes d'erreur d'un terminal**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Ajouter le journal de diagnostic d'un terminal**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Afficher la liste des journaux de diagnostic d'un terminal**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Obtenir le journal de diagnostic d'un terminal**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Effacer le journal de diagnostic d'un terminal**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Supprimer le journal de diagnostic d'un terminal**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

###API Cache des derniers événements

**Obtenir les événements du terminal**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.

**Obtenir les événements du terminal**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

Renvoie une erreur 404 si le terminal n'est pas accessible par l'appelant.
