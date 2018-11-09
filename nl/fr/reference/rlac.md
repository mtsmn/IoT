---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configuration du contrôle d'accès au niveau de la ressource
{: #configure_RLAC}

Le contrôle d'accès au niveau de la ressource vous permet de contrôler l'accès des utilisateurs et des clés d'API afin de gérer les terminaux. Vous utilisez les groupes de ressources pour définir les terminaux d'une organisation que chaque utilisateur ou clé d'API peut gérer. Les utilisateurs et les clés d'API peuvent être associés à une paire "rôle-groupes", qui indique qu'ils peuvent uniquement effectuer des opérations couvertes par le rôle spécifié sur les terminaux qui se trouvent dans les groupes spécifiés. Pour en savoir plus sur le contrôle d'accès au niveau de la ressource, voir [Aperçu du contrôle d'accès au niveau de la ressource](rlac_overview.html) et la documentation sur les [API de contrôle d'accès de {{site.data.keyword.iot_short_notm}}![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Configuration du contrôle d'accès au niveau de la ressource - flux de processus
{: #RLAC_process}

Le flux de processus ci-dessous est généralement utilisé pour activer et utiliser le contrôle d'accès au niveau de la ressource :
1. [Créer une organisation](../iotplatform_overview.html#organizations).
2. [Créer des utilisateurs](../add_users.html#adding-new-users) et des [clés d'API](../platform_authorization.html#api-key).
3. [Créer des groupes de ressources](rlac.html#create_delete_group).
4. [Affecter des mappages "rôle-groupes" pour les utilisateurs et les clés d'API](rlac.html#assign_roletogroup).
5. [Ajouter des terminaux aux groupes de ressources](rlac.html#add_device).
6. [Activer le contrôle d'accès au niveau de la ressource](rlac.html#RLAC_enable).

Le contrôle d'accès au niveau de la ressource est appliqué dans diverses API liées aux terminaux. Pour obtenir la liste des API concernées, voir [API dans lesquelles le contrôle d'accès au niveau de la ressource est appliqué](rlac_overview.html#RLAC_enforced_APIs).

## Création et suppression de groupes de ressources
{: #create_delete_group}

Des groupes de ressources sont créés et supprimés indépendamment des passerelles de connexion.

Pour créer un groupe de ressources et renvoyer les détails du groupe, utilisez l'API suivante :

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

Lorsque vous supprimez un groupe de ressources, les terminaux qui étaient membres du groupe sont retirés de celui-ci, mais les terminaux proprement dits ne sont pas affectés. Pour supprimer un groupe de ressources, utilisez l'API suivante :

    DELETE /api/v0002/groups/{groupUid}


## Affectation de paires "rôle-groupes" à des utilisateurs ou des clés d'API
{: #assign_roletogroup}

L'affectation d'une paire de type "rôle-groupes" à un utilisateur ou une clé d'API est requise pour limiter l'accès de l'utilisateur ou de la clé d'API à un groupe de terminaux. Les utilisateurs et les clés d'API qui ne possèdent pas de groupes de ressources peuvent gérer tous les terminaux de leur organisation sans aucune restriction. Une clé d'API ne peut avoir qu'un seul rôle si une paire "rôles-groupes" est utilisée. Le rôle spécifié dans "rolesToGroups" doit correspondre à celui spécifié dans "roles".

Pour affecter une paire "rôle-groupes" à un utilisateur, utilisez l'API suivante :

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



Pour affecter une paire "rôle-groupes" à une clé d'API, utilisez l'API suivante :

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## Ajout et retrait de terminaux dans des groupes de ressources
{: #add_device}

Pour qu'un utilisateur ou une clé d'API avec une paire "rôle-groupes" puisse gérer un terminal, celui-ci doit être membre du groupe de ressources affecté à l'utilisateur ou à la clé d'API. Lorsque vous ajoutez des terminaux à un groupe de ressources, le groupe auquel vous ajoutez des terminaux doit être spécifié dans le chemin de la demande, et les terminaux que vous ajoutez doivent être spécifiés dans le corps de la demande. Pour ajouter plusieurs terminaux à un groupe de ressources simultanément, utilisez l'API suivante :

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Lorsque vous supprimez des terminaux d'un groupe de ressources, les terminaux spécifiés dans le corps de la demande sont retirés du groupe spécifié dans le chemin de la demande. Pour retirer plusieurs terminaux d'un groupe de ressources, utilisez l'API suivante :

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Pour plus d'informations sur le schéma de demande et la réponse, consultez la documentation de l'[API de contrôle d'accès {{site.data.keyword.iot_short_notm}}![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Activation du contrôle d'accès au niveau de la ressource
{: #RLAC_enable}

L'activation du contrôle d'accès au niveau de la ressource permet de limiter l'accès des utilisateurs et clés d'API aux groupes de ressources auxquels ils appartiennent. Pour utiliser le contrôle d'accès au niveau de la ressource, vous devez activer l'indicateur de configuration au niveau de l'organisation en utilisant l'API suivante :

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Recherche de groupes de ressources
{: #find_group}

Des étiquettes de recherche peuvent être associées à des groupes de ressources. Les étiquettes de recherche peuvent être utilisées pour extraire les détails d'un groupe de ressources à l'aide de l'API suivante :

    GET /api/v0002/groups

Cette API renvoie les groupes de ressources associés à l'étiquette de recherche utilisée. Si aucune étiquette de recherche n'est spécifiée, tous les groupes de ressources sont renvoyés. 

Pour rechercher l'ID unique des groupes de ressources associés à un utilisateur, utilisez l'API suivante :

    GET /api/v0002/authorization/users/{userUid}

Pour recherche l'ID unique des groupes de ressources associés à une clé d'API, utilisez l'API suivante :

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## Analyse des groupes de ressources
{: #query_group}

Les groupes de ressources peuvent être analysés avec divers paramètres afin de renvoyer les propriétés complètes de tous les terminaux du groupe, les identificateurs uniques de tous les terminaux du groupe ou les propriétés du groupe de ressources.

Pour renvoyer les propriétés complètes de tous les terminaux du groupe de ressources spécifié, utilisez l'API suivante :

    GET /api/v0002/bulk/devices/{groupUid}

Pour renvoyer uniquement les identificateurs uniques des membres du groupe de ressources, utilisez l'API suivante :

    GET /api/v0002/bulk/devices/{groupUid}/ids

Pour renvoyer les propriétés du groupe de ressources, y compris le nom, la description, les étiquettes de recherche et l'identificateur unique spécifiés dans le chemin, utilisez l'API suivante :

    GET /api/v0002/groups/{groupUid}

Cette API renvoie la liste des membres du groupe de ressources.

## Mise à jour des propriétés de groupe
{: #update_group}

Pour mettre à jour les propriétés d'un groupe, utilisez l'API suivante :

    PUT /api/v0002/groups/{groupId}
