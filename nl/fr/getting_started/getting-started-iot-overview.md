---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guides d'initiation à {{site.data.keyword.iot_short_notm}}
{: #getting-started}

Les guides d'initiation ont été conçus pour enseigner les principes de base du développement d'un prototype IoT de bout en bout prêt à être utilisé en production avec {{site.data.keyword.iot_full}} et s'adressent aux développeurs qui sont novices dans l'utilisation de {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

Chaque guide offre des instructions étape par étape qui vous permettent d'atteindre une solution rapidement déployée et opérationnelle avec une ou plusieurs fonctionnalités {{site.data.keyword.iot_short_notm}}.

## Utilisation des guides  
{: #using-guides}
Dans chaque guide, vous trouverez au moins un exemple GitHub codé selon les valeurs recommandées de {{site.data.keyword.iot_short_notm}}. Ces exemples peuvent être ajustés, sécurisés et intégrés aux systèmes, et s'adaptent généralement directement à vos processus de construction et de développement devOps.

Les utilisateurs avancés peuvent étendre cette solution rapide en ajoutant l'exemple de code à leur propre environnement ou en la complétant avec des exemples matériels qui simulent mieux les situations susceptibles d'être rencontrées dans votre environnement de production. Les guides offrent des liens supplémentaires à la documentation adéquate, aux documents liés aux API, aux bibliothèques client (SDK), aux vidéos de démonstration et autre matériel d'apprentissage.

Vous pouvez parcourir les guides dans l'ordre dans lequel ils sont fournis, le premier guide fournissant la base de tous les autres guides. Si vous souhaitez passer une documentation et préférer chosir votre propre ordre de lecture, chaque guide offre la liste des exigences. Les exigences vous signalent le format des données de terminal requis pour que vous puissiez définir votre propre environnement en conséquence.
{: tip}

## Liste des guides
{: #list-of-guides}  

Les guides mis à votre disposition sont les suivants :

| Guide | Description |    
| ----- | ---- |   
| [1. Connexion d'un terminal de tapis roulant](getting-started-iot-conveyor.html) | Commencez par installer une organisation {{site.data.keyword.iot_short_notm}} et connectez une application de simulation de tapis roulant basée sur GitHub, ou si vous préférez quelque chose de plus physique, un simulateur de tapis roulant basé sur votre propre Raspberry Pi. </br> Configurez ensuite quelques cartes basiques de tableau de bord {{site.data.keyword.iot_short_notm}} pour visualiser et surveiller les données de votre terminal. |   
| [2. Surveillance des données de votre terminal](getting-started-iot-monitoring.html) | Connectez une application de surveillance basée sur une bibliothèque de widgets ou Node.js pour voir les données du tapis roulant en temps réel.  
| [3. Simulation d'un grand nombre de terminaux](getting-started-iot-large-scale-simulation.html) | Etendez la simulation de terminal simple en ajoutant un grand nombre de simulateurs de terminaux à votre environnement. </br>**Important :** l'application nécessite 512 Mo de mémoire, ce qui est plus que la valeur allouée par défaut et que la quantité disponible pour les comptes d'essai gratuits. Vous devez par conséquent mettre à niveau votre application vers un abonnement ou un compte de type Paiement à la carte. |   
