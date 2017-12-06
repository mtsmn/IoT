---

copyright:
  years: 2017
lastupdated: "2017-06-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guides d'initiation à {{site.data.keyword.iot_short_notm}}
{: #getting-started}

Les guides d'initiation ont été conçus pour enseigner les principes de base du développement d'un prototype IoT de bout en bout prêt à être utilisé en production avec {{site.data.keyword.iot_short_notm}} et s'adressent aux développeurs qui sont novices dans l'utilisation de {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

Chaque guide offre des instructions étape par étape qui vous permettent d'atteindre une solution rapidement déployée et opérationnelle avec une ou plusieurs fonctionnalités {{site.data.keyword.iot_short_notm}}.

## Utilisation des guides  
{: #using-guides}
Dans chaque guide, vous trouverez au moins un exemple GitHub codé selon les valeurs recommandées de {{site.data.keyword.iot_short_notm}}. Ces exemples peuvent être ajustés, sécurisés et intégrés aux systèmes, et s'adaptent généralement directement à vos processus de construction et de développement devOps.

Les utilisateurs avancés peuvent étendre cette solution rapide en ajoutant l'exemple de code à leur propre environnement ou en la complétant avec des exemples matériels qui simulent mieux les situations susceptibles d'être rencontrées dans votre environnement de production. Les guides offrent des liens supplémentaires à la documentation adéquate, aux documents liés aux API, aux bibliothèques client (SDK), aux vidéos de démonstration et autre matériel d'apprentissage.

Vous pouvez parcourir les guides dans l'ordre dans lequel ils sont fournis, le premier guide fournissant la base de tous les autres guides. Si vous souhaitez passer une documentation et préférer chosir votre propre ordre de lecture, chaque guide offre la liste des exigences. Par exemple, si vous avez déjà une organisation {{site.data.keyword.iot_short_notm}} configurée, vous pouvez passer directement aux guides 2 et 3 pour commencer à découvrir les règles, les actions, ainsi qu'un modèle d'application de surveillance. Les exigences vous signalent le format des données de terminal requis pour que vous puissiez définir votre propre environnement en conséquence.
{: tip}

## Liste des guides
{: #list-of-guides}  

Les guides mis à votre disposition sont les suivants :

| Guide | Description |    
| ----- | ---- |   
| [1. Connexion d'un terminal de tapis roulant](getting-started-iot-conveyor.html) | Commencez par installer une organisation {{site.data.keyword.iot_short_notm}} et connectez une application de simulation de tapis roulant basée sur GitHub, ou si vous préférez quelque chose de plus physique, un simulateur de tapis roulant basé sur votre propre Raspberry Pi. </br> Configurez ensuite quelques cartes basiques de tableau de bord {{site.data.keyword.iot_short_notm}} pour visualiser et surveiller les données de votre terminal. |   
| [2. Utilisation des règles et actions de base en temps réel](getting-started-iot-rules.html) | Lorsque un ou plusieurs terminaux sont connectés à votre organisation {{site.data.keyword.iot_short_notm}} et qu'ils envoient des données, vous pouvez définir des règles métier avec les actions à déclencher, par exemple avertir l'opérateur de l'usine si la vitesse du tapis roulant passe en dessous d'un certain seuil.  
| [3. Surveillance des données de votre terminal](getting-started-iot-monitoring.html) | Etendez les tableaux de bord de surveillance du terminal {{site.data.keyword.iot_short_notm}} intégré en connectant une application de surveillance basée sur Node.js ou une bibliothèque de widgets pour voir les données du tapis roulant en temps réel.  
| [4. Simulation d'un grand nombre de terminaux](getting-started-iot-large-scale-simulation.html) | Etendez la simulation d'un seul terminal en ajoutant un grand nombre de simulateurs de terminaux à votre environnement pour tester les analyses et la surveillance des guides précédents dans un environnement plus réaliste comportant plusieurs terminaux. </br>**Important :** L'application nécessite 512 Mo de mémoire, ce qui est plus que la valeur allouée par défaut et que la quantité disponible pour les comptes d'essai gratuits. Vous devez par conséquent mettre à niveau votre application vers un abonnement ou un compte de type Paiement à la carte. |   
