---

copyright:

years: 2017, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Quotas
La présente section fournit des informations sur les limites qui sont définies pour les services {{site.data.keyword.iot_full}} par type de plan. Ces limites existent dans le cadre de notre politique d'utilisation équitable afin de garantir que les performances ne sont pas affectées par une utilisation abusive de l'ensemble des utilisateurs du système à service partagé. En outre, elles permettent d'éviter que la charge de travail d'utilisateurs réels ne soit involontairement identifiée comme une attaque de type refus de service.

## Introduction
{: #metrics_about}

Les quotas spécifient les limites supérieures qui sont définies pour une ressource. Lorsque l'utilisation dépasse une limite donnée, les demandes utilisateur risquent d'être retardées ou refusées. Dans certains cas exceptionnels, le dépassement de limites liées à la stabilité du système peut entraîner la suspension de l'organisation {{site.data.keyword.iot_short_notm}} afin d'éviter que d'autres utilisateurs ne soient affectés.

Les tableaux ci-dessous dressent la liste des limites relatives à {{site.data.keyword.iot_short_notm}}. La plupart de ces limites s'entendent par organisation {{site.data.keyword.iot_short_notm}} sauf si elles sont répertoriées explicitement par terminal. Certaines limites ont un plafond imposé tandis que d'autres peuvent être ajustées sur demande. Si vous avez besoin de repousser les limites définies, ouvrez un [ticket ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://support.ng.bluemix.net/gethelp/){: new_window}.

## Messagerie
{: #messaging_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security     | Plan dédié
------------- | -------------|------------- |
Nombre maximal de types de terminal |50 |1 000 |1 000
Nombre maximal de terminaux connectés simultanément | 500| 500K. Si vous souhaitez connecter plus de 50 000 terminaux, merci de nous contacter pour discuter de vos plans. | 500K
Nombre maximal de terminaux enregistrés |500 | 1 million | 100K par incrément
Nombre maximal d'applications ['A' - partagées](../applications/mqtt.html#scalable_apps) |1 | 10 abonnements partagés, avec 50 instances chacun|10 abonnements partagés, avec 50 instances chacun
Nombre maximal d'applications ['a' - non partagées](../applications/mqtt.html#client_connections) |500 clés d'API | 2000 clés d'API | 2000 clés d'API
Données maximum par mois | 200 Mo | Illimité |Illimité
Connexions de terminaux|10/s | 300/s |300/s
Envois "Terminal vers Cloud" (MQTT) - Terminal | 5 messages/s ou 20 Ko/s | 10 messages/s ou 40 Ko/s |10 messages/s ou 40 Ko/s
Envois "Terminal vers Cloud" (MQTT) - Passerelle  | 50 messages/s ou 200 Ko/s | 100 messages/s ou 400 Ko/s| 100 messages/s ou 400 Ko/s
Envois "Terminal vers Cloud" (MQTT) - Application | 10 messages/s ou 40 Ko/s | 20 messages/s ou 80 Ko/s| 20 messages/s ou 80 Ko/s
Envois "Terminal vers Cloud" (MQTT) - par organisation | 5 000 messages/s | 6 000 messages/s | 6 000 messages/s
Envois "Terminal vers Cloud" (HTTP) - Terminal | 30 messages/min ou 2 Ko/s | 1 message/s ou 4 Ko/s | 1 message/s ou 4 Ko/s
Envois "Terminal vers Cloud" (HTTP) - Passerelle | 5 messages/s ou 20 Ko/s | 10 messages/s ou 40 Ko/s | 10 messages/s ou 40 Ko/s
Envois "Terminal vers Cloud" (HTTP) - Application | 1 message/s ou 4 Ko/s| 2 messages/s ou 8 Ko/s | 2 messages/s ou 8 Ko/s
Envois "Terminal vers Cloud" (HTTP) - par organisation | 500 messages/s | 600 messages/s | 600 messages/s
Envois "Terminal vers Cloud" (MQTT) - Terminal  |1 message/s |2 messages/s ou 8 Ko/s |2 messages/s ou 8 Ko/s
Envois "Terminal vers Cloud" (MQTT) - Passerelle| 10 messages/s |20 messages/s ou 80 Ko/s  |20 messages/s ou 80 Ko/s  
Envois "Terminal vers Cloud" (MQTT) - Application | 10 messages/s |20 messages/s ou 80 Ko/s |20 messages/s ou 80 Ko/s  
Envois "Terminal vers Cloud" (MQTT) - par organisation |1 000 messages/s | 12 K messages/s|12 K messages/s
Envois "Terminal vers Cloud" (HTTP) - taux de diffusion en rafale par terminal | 30 messages/min| 30 messages/min  | 30 messages/min
Envois "Terminal vers Cloud" (HTTP) - par organisation |  1 000 messages/s|  1 200 messages/s  |  1 200 messages/s
Nombre maximal de messages entrants non reconnus - par terminal | 10 |10 |10
Nombre maximal de messages entrants non reconnus - par passerelle | 1 000 |1 000 |1 000
Nombre maximal de messages entrants non reconnus - par connexion d'application  |2 000 | 2 000|2 000
Nombre maximal de messages sortants non reconnus - par terminal |10  |10 |10
Persistance "Terminal vers Cloud" | 24 heures|24 heures |24 heures
Persistance "Cloud vers terminal" |48 heures (par défaut). Max. 7 jours & lignes de file d'attente| 48 heures (par défaut). Max. 7 jours & lignes de file d'attente  |48 heures (par défaut). Max. 7 jours & lignes de file d'attente
Intervalle maximal entre les nouvelles tentatives pour la livraison de messages QoS 1 | Tentative à la reconnexion |Tentative à la reconnexion |Tentative à la reconnexion
Limite d'inactivité de connexion (intervalle de signal de présence) | Définie par le client lors de la connexion |Définie par le client lors de la connexion  |Définie par le client lors de la connexion
Durée de connexion WebSocket |Définie par le client lors de la connexion | Définie par le client lors de la connexion  |Définie par le client lors de la connexion
Taille maximale de message | 128 Ko |128 Ko |128 Ko
Abonnements max. par terminal |50 |50 |50
Abonnements max. par application | 500 |500 |500
Nombre de lignes de file d'attente - Nombre max. de messages placés en mémoire tampon par abonnement par terminal | 50 |50 |50
Nombre max. de messages placés en mémoire tampon pour les applications 'A' |50 K durables, 50 K non durables |50 K durables, 50 K non durables |50 K durables, 50 K non durables


## API
{: #api_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Appels d'API |10/s |10/s |10/s

## Moteur de règles
{: #rule_engine_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Règles max. par organisation |100|1 000|1 000
Actions par règle|10|10|10

## Gestion des terminaux
{: #device_mgmt_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Taille maximale de journaux de diagnostic par terminal |500K|500K|500K
Nombre max. de versions de journaux de diagnostic conservées|2  |2 |2
Durée max. de conservation des journaux de diagnostic|7 jours| 7 jours|7 jours
Nombre max. d'actions initiées par message en cours |200 |200 |200
Nombre max. de terminaux par action |5 000 |5 000 | 5 000

## Dernier cache d'événement
{: #last_event_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Nombre max. d'ID d'événements par terminal |5|5|5
Nombre max. de formats |2|3|3
Echéance depuis le dernier cache d'événement |7 jours |45 jours | 45 jours

## Extensions
{: #extensions_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Nombre max. de services que vous pouvez lier |12|12|12

## Gestion des informations
{: #information_management_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Niveau d'imbrication max. pour le contenu JSON |5|5|5
Taille max. de chaîne JSON |512|512|512

## Sécurité
{: #security_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Nombre max. de rôles personnalisés |20|20|20
Nombre max. de membres utilisateurs |1 000|1 000|1 000
Nombre max. de terminaux dans un groupe de ressources |300|300|300
Nombre max. de groupes de ressources affectés à une passerelle |10|10|10
Nombre max. de groupes de ressources auquel un terminal peut appartenir |10|10|10

## Interface utilisateur
{: #UI_metrics}

Mesure        | Plan Lite      | Plans Standard & Advanced Security       | Dédié
------------- | -------------|------------- |
Nombre max. de tableaux de bord |50|50|50
Nombre max. de cartes installées |30|30|30
