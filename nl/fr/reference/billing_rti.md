---
copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Facturation

Les plans de service {{site.data.keyword.iot_full}} payés (autres que le plan 'Lite') sont basés sur la notion de mégaoctets échangés et analysés sur une période d'un mois.  Ce document explique comment {{site.data.keyword.iot_short_notm}} mesure les données pour créer des informations qui déterminent le coût d'utilisation du service.  Ces informations peuvent ensuite servir à faire une estimation du coût d'utilisation de {{site.data.keyword.iot_short_notm}} en fonction de la conception et du nombre de terminaux, des applications et des passerelles.

Pour obtenir des informations sur le coût de chaque mégaoctet de données échangé ou analysé, consultez le service {{site.data.keyword.iot_short_notm}} dans le catalogue {{site.data.keyword.Bluemix_notm}} de la région requise.

Vous pouvez utiliser la [Calculatrice de prix ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://iot-cost-calculator.ng.bluemix.net/) pour vous aider à estimer le coût d'un service {{site.data.keyword.iot_short_notm}}.

Les trois éléments ci-dessous sont mesurés séparément et facturés en fonction de leur utilisation : 
- les données échangées
- les données analysées
- les données Edge analysées

## Echange de données
Le calcul des *données échangées* inclut les données de compte qui sont échangées par les applications, les terminaux et les passerelles via messagerie MQTT ou HTTP, ainsi que les données qui sont échangées par les applications via l'API HTTP.

### Données échangées via MQTT
Lorsque vous utilisez MQTT, le contenu de flux TCP est pris en compte dans les *données échangées*.  Le contenu d'un message MQTT est inclus dans le cumul des données échangées via le protocole MQTT.  Le cumul des données utiles intervient lors de la publication du message, ainsi que de la distribution du message à un abonné.

Le protocole MQTT a été conçu pour être le plus léger possible.  Bien que {{site.data.keyword.iot_short_notm}} intègre la taille des paquets MQTT lors du cumul des *données échangées*, vu qu'il s'agit d'un protocole léger, la charge reste faible.  Le tableau ci-dessous donne une indication de la taille minimale de chaque paquet MQTT :

|Paquet MQTT                    |Taille minimale en octets  |Variable et taille minimale associée en octets|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |ID client (12 si d:xxxxxx:t:i), avec rubrique (0) et message (0) si défini, nom d'utilisateur (14 pour les terminaux - 'use-token-auth') et mot de passe (18 en cas de génération automatique)|
|CONNACK                        |4                   |Néant|
|PUBLISH                        |21                  |Nom de rubrique (17 si iot-2/evt/i/fmt/f) et charge utile (mimimum 0).  S'applique aux paquets PUBLISH entrants et sortants.|
|PUBACK                         |4                   |Néant|
|PUBREC                         |4                   |Néant|
|PUBREL                         |4                   |Néant|
|PUBCOMP                        |4                   |Néant|
|SUBSCRIBE                      |26                  |Peut inclure plusieurs filtre de rubrique (19 si iot-2/evt/i/fmt/f) et QoS (1)|
|SUBACK                         |5                   |(1) pour chaque filtre de rubrique dans SUBSCRIBE|
|UNSUBSCRIBE                    |23                  |Peut inclure plusieurs filtres de rubrique (19 si iot-2/evt/i/fmt/f)|
|UNSUBACK                       |4                   |Néant|
|PINGREQ                        |2                   |Néant|
|PINGRESP                       |2                   |Néant|
|DISCONNECT                     |2                   |Néant|

Lorsque vous utilisez le protocole TLS, l'établissement de liaison sécurisée est également pris en compte. L'établissement de liaison représente environ 8 kilooctets. Par conséquent, il est préférable de garder les connexions MQTT ouvertes le plus longtemps possible. Les connexions ouvertes acceptent uniquement les surcharges PINGREQ et PINGRESP (4 octets) par intervalle de signal de présence qui dépend de chaque client et de la période de présence spécifiée.  La réouverture d'une connexion TLS à l'aide d'une session TLS existante diminue le nombre d'octets échangés, car les certificats ne sont pas échangés de manière bidirectionnelle.  Un grand nombre de clients TLS prennent en charge cette fonction par défaut, mais la session a une durée de vie limitée.  L'utilisation de certificats client augmentent la taille de l'établissement de liaison TLS, en fonction de la taille du certificat client. 

Les enregistrements TLS et les surcharges TCP et IP ne sont pas pris en compte.

**Remarque** - Lorsque vous utilisez le tableau de bord {{site.data.keyword.iot_short_notm}} pour voir un terminal, un abonnement MQTT est effectué pour que le tableau de bord puisse afficher des événements en direct.  Cet abonnement MQTT est également pris en compte pour le calcul du volume total de données échangées.

### Données échangées via la messagerie HTTP
Lorsque vous utilisez la messagerie HTTP, la charge utile des messages est incluse dans le cumul des *données échangées* lors de la publication d'événements et/ou de la réception de commandes.

A l'instar de MQTT, la surcharge HTTP est comptabilisée.  La surcharge HTTP est beaucoup plus importante que la surcharge MQTT. La surcharge HTTP par requête représente environ 300 octets. La taille de la charge utile des messages doit également être ajoutée.

Lorsque vous utilisez le protocole TLS, l'établissement de la liaison sécurisée est pris en compte.  L'établissement de liaison représente environ 8 kilooctets.  Par conséquent, il est préférable de garder les connexions HTTP ouvertes le plus longtemps possible.  Les connexions ouvertes n'entraînent aucune charge additionnelle en termes d'*échange de données*.  La réouverture d'une connexion TLS à l'aide d'une session TLS existante diminue le nombre d'octets échangés, car les certificats ne sont pas échangés de manière bidirectionnelle.  Un grand nombre de clients TLS prennent en charge cette fonction par défaut, mais la session a une durée de vie limitée.  L'utilisation de certificats client augmentent la taille de l'établissement de liaison TLS, en fonction de la taille du certificat client.

Les enregistrements TLS et les surcharges TCP et IP ne sont pas pris en compte.

### Données échangées via l'API HTTP
Lorsqu'une API est appelée par une application, la demande et la réponse sont pris en compte dans le cumul des *données échangées*.  Leur taille peuvent varier grandement en fonction de l'API utilisée.

Contrairement aux messageries MQTT et HTTP, ni la surcharge HTTP ni la surcharge TLS ne sont prises en compte pour l'échange de données via l'API HTTP.

**Remarque** - Lorsque vous utilisez le tableau de bord {{site.data.keyword.iot_short_notm}}, les API HTTP sont utilisées par le tableau de bord pour répertorier des informations, notamment les terminaux, les types de terminal et les journaux de connexion des terminaux.  Ces appels d'API HTTP sont pris en compte dans le calcul des *données échangées*.

## Données analysées
Le calcul des *données analysées* tient compte des données traitées par le moteur de règles dans la plateforme.  Les données sont considérées comme étant traitées par le moteur de règles lorsque des événements de terminaux sont évalués par une ou plusieurs règles, selon le terminal et le type d'événement définis. 

## Données Edge analysées
Le calcul des *données Edge analysées* tient compte des données d'événement traitées sur un terminal de passerelle par l'agent {{site.data.keyword.iot_short_notm}} Edge Analytics.  Les données sont considérées comme étant traitées par l'agent Edge lorsque les événements de terminaux sont évalués par un ou plusieurs règles Edge, selon le terminal et le type d'événement définis. 
