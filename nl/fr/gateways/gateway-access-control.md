---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Contrôle d'accès aux passerelles
{: #gateway-access-control}

Les terminaux de passerelle représentent une classe spécialisée de terminal et sont habilités à agir pour le compte d'autres terminaux. Les groupes de ressources de passerelle définissent les terminaux d'une organisation pour le compte desquels les passerelles sont autorisées à agir. Les rôles de passerelle déterminent la capacité de la passerelle à enregistrer les terminaux dans Watson IoT Platform.
{: #shortdesc}

Les groupes de ressources sont utilisés pour permettre aux passerelles d'agir pour le compte d'un sous-ensemble des terminaux d'une organisation. Les passerelles peuvent uniquement publier ou souscrire des messages pour eux-mêmes et pour le compte de terminaux présents dans leur groupe de ressources. 

Lorsqu'une nouvelle passerelle est créée, un groupe de ressources par défaut est également créé et affecté à la passerelle. Le groupe de ressources par défaut ne peut pas être supprimé de la passerelle. Les passerelles existantes qui ne possèdent pas de groupes de ressources peuvent continuer d'agir pour le compte de tous les terminaux de l'organisation.

Pour obtenir des informations sur la publication d'événements à partir de terminaux de passerelle à l'aide d'API, voir [API HTTP pour les terminaux de passerelle](gw_api.html).

## Affectation d'un rôle à une passerelle
{: #gw_roles}

Afin d'affecter une passerelle à un groupe de ressources, vous devez d'abord affecter la passerelle à un rôle. Les passerelles nouvellement créées sont affectées au rôle *Passerelle privilégiée* par défaut et sont assignées à un groupe de ressources par défaut. Les passerelles disposant d'un rôle privilégié peuvent enregistrer automatiquement les terminaux qui sont ajoutés au groupe de ressources par défaut.

Les passerelles possédant le rôle *Passerelle standard* ne peuvent pas enregistrer automatiquement les terminaux. 

Une fois qu'une passerelle a été affectée à un groupe de ressources, elle peut uniquement agir pour le compte des terminaux dans ce groupe de ressources et pour elle-même, même si sont rôle est modifié.

Pour plus d'informations sur les rôles de passerelle, voir [Rôles de passerelle](../roles_index.html#gateway_roles).

Pour attribuer un rôle à une passerelle, utilisez l'API suivante où *${clientID}* est l'ID client avec codage d'URL au format *d:${orgId}:${typeId}:${deviceId}* pour les terminaux ou *g:${orgId}:${typeId}:${deviceId}* pour les passerelles :

```
PUT /authorization/devices/${clientID}/roles

Request Body:
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ]
}

Request Response: 200
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ],
    "rolesToGroups": {
        "PD_STANDARD_GW_DEVICE": ["gw_def_res_grp:abcdef:gatewayTypeId:gatewayDeviceId"]
    }
}
```

Pour plus d'informations sur le schéma de demande, consultez la [documentation de l'API {{site.data.keyword.iot_full}} Limited Gateway ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_authorization_devices_deviceId_roles){: new_window}.

## Ajout et retrait de terminaux dans un groupe de ressources
{: #devices_groups}

Pour qu'une passerelle dotée du rôle *Passerelle standard* puisse agir pour le compte d'un terminal, celui-ci doit être membre du groupe de ressources affecté à la passerelle. Pour ajouter un grand nombre de terminaux à la fois à un groupe de ressources, utilisez l'API suivante :

```
 PUT /bulk/devices/{groupId}/add
```

Le groupe auquel ajouter des terminaux doit être spécifié dans le chemin de la demande, et les terminaux à ajouter doivent être spécifiés dans le corps de la demande. Pour plus d'informations sur le schéma de demande et les réponses, consultez la [documentation de l'API {{site.data.keyword.iot_short_notm}} Limited Gateway ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_add){: new_window}.

Pour retirer plusieurs terminaux d'un groupe de ressources, utilisez l'API suivante :

```
PUT /bulk/devices/{groupId}/remove
```

Les terminaux spécifiés dans le corps de la demande seront retirés du groupe spécifié dans le chemin de la demande. Pour plus d'informations sur le schéma de demande et la réponse, consultez la [documentation de l'API {{site.data.keyword.iot_short_notm}} Limited Gateway ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_remove){: new_window}.

## Recherche d'un groupe de ressources
{: #finding_groups}

Des étiquettes de recherche peuvent être associées à des groupes de ressources. Les étiquettes de recherche peuvent être utilisées pour extraire les détails d'un groupe de ressources à l'aide de l'API suivante :

```
GET /groups
```

Cette API renvoie les groupes de ressources associés à l'étiquette de recherche utilisée. Si aucune étiquette de recherche n'est spécifiée, tous les groupes de ressources sont renvoyés. <!-- For more information about the request schema, response, and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

L'ID d'un groupe de ressources attribué à une passerelle peut être obtenu en utilisant l'API suivante où *${clientID}* est l'ID client avec codage d'URL au format *d:${orgId}:${typeId}:${deviceId}* pour les terminaux ou *g:${orgId}:${typeId}:${deviceId}* pour les passerelles :

```
GET /authorization/devices/${clientId}
```

Cette API renvoie l'identificateur unique du ou des groupes de ressources affecté(s) à ce terminal. Vous trouverez davantage d'informations sur cette API dans la [documentation de l'API {{site.data.keyword.iot_short_notm}} Limited Gateway ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_authorization_devices_deviceId){: new_window}.


## Analyse d'un groupe de ressources
{: #querying_groups}

Les groupes de ressources peuvent être analysés dans divers paramètres afin de renvoyer les propriétés complètes de tous les terminaux du groupe, les identificateurs uniques de tous les terminaux du groupe ou les propriétés du groupe de ressources.

Pour renvoyer les propriétés complètes de tous les terminaux du groupe de ressources spécifié, utilisez l'API suivante :

```
GET /bulk/devices/{groupId}
```

Cette API renvoie la liste des propriétés complètes de tous les membres du groupe de ressources spécifié. Pour plus d'informations sur le schéma de demande et les réponses, et la façon de parcourir les  résultats, consultez la [documentation de l'API {{site.data.keyword.iot_short_notm}} Limited Gateway ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_bulk_devices_groupId){: new_window}.

Pour renvoyer uniquement les identificateurs uniques des membres du groupe de ressources, utilisez l'API suivante :

```
GET /bulk/devices/{groupId}/ids
```

Cette API renvoie les identificateurs uniques de tous les terminaux qui sont membres du groupe de ressources. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Pour renvoyer les propriétés du groupe de ressources, utilisez l'API suivante :

```
GET /groups/{groupId}
```

Cette API renvoie les propriétés du groupe de ressources (nom, description, étiquettes de recherche et identificateur unique) spécifié dans le chemin, mais ne renvoie pas la liste de membres du groupe de ressources. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Création et suppression d'un groupe de ressources
{: #crt_del_groups}

Des groupes de ressources sont créés et supprimés indépendamment des passerelles de connexion. Pour créer un groupe de ressources, utilisez l'API suivante :

```
POST /groups
```

Cette API crée un groupe de ressources et renvoie les détails du groupe. <!-- For details on the request schema and the responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Pour supprimer un groupe de ressources, utilisez l'API suivante :

```
DELETE /groups/{groupId}
```

Cette API supprime le groupe de ressources spécifié. Les terminaux qui étaient membres du groupe sont retirés de celui-ci, mais les terminaux proprement dits ne sont pas affectés. <!-- For more information, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Mise à jour des propriétés de groupe
{: #update_groups}

  - /groups/{groupId}
    - PUT : met à jour les propriétés du groupe spécifié.

## Extraction et mise à jour de propriétés de terminal
{: #fdevice_group_props}

Il existe plusieurs façons d'extraire des propriétés de terminal à l'aide d'une API et chaque API renvoie des informations différentes. Pour extraire les propriétés de terminal de tous les terminaux existants dans votre organisation {{site.data.keyword.iot_short_notm}}, utilisez l'API suivante :

```
GET /authorization/devices

```

Cette API renvoie les propriétés de tous les terminaux existantes dans votre organisation, y compris les propriétés relatives au contrôle d'accès (rôle, état, date d'expiration).<!-- For more information on responses and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Pour extraire les propriétés d'un seul terminal de l'organisation, utilisez l'API suivante où *${clientID}* est l'ID client avec codage URL au format *d:${orgId}:${typeId}:${deviceId}* pour les terminaux ou *g:${orgId}:${typeId}:${deviceId}* pour les passerelles :

```
GET /authorization/devices/${clientId}
```

Cette API renvoie toutes les propriétés de terminal du terminal spécifié. <!-- For more information, see the [{{site.data.keyword.iot_short_notm}} device model documentation](LINK TO DEVICE MODEL) and [API documentation](LINK TO CORRECT API). -->

Pour extraire uniquement les informations de contrôle d'accès d'un terminal spécifique, utilisez l'API suivante où *${clientID}* est l'ID client avec codage URL au format *d:${orgId}:${typeId}:${deviceId}* pour les terminaux ou *g:${orgId}:${typeId}:${deviceId}* pour les passerelles :

```
GET /authorization/devices/${clientId}/roles
```

Cette API extrait les informations de contrôle d'accès du terminal spécifié sans renvoyer les autres propriétés de terminal. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Il existe deux façons de mettre à jour des propriétés de terminal. Les propriétés peuvent être mises à jour sans modifier les propriétés de contrôle d'accès ou les propriétés de contrôle d'accès peuvent être mises à jour directement.

Pour mettre à jour les propriétés du terminal sans affecter celles du contrôle d'accès, utilisez l'API suivante où *${clientID}* est l'ID client avec codage URL au format *d:${orgId}:${typeId}:${deviceId}* pour les terminaux ou *g:${orgId}:${typeId}:${deviceId}* pour les passerelles :

```
PUT /authorization/devices/${clientId}
```

Cette API ne met à jour que les propriétés du terminal qui ne sont pas associées au contrôle d'accès. <!-- For more information on request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Pour mettre à jour uniquement les propriétés de contrôle d'accès du terminal spécifié, utilisez l'API suivante où *${clientID}* est l'ID client avec codage URL au format *d:${orgId}:${typeId}:${deviceId}* pour les terminaux ou *g:${orgId}:${typeId}:${deviceId}* pour les passerelles :

```
PUT /authorization/devices/${clientId}/withroles
```

Cette API ne met à jour que les propriétés de contrôle d'accès du terminal spécifié. <!-- For more information on the request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->
