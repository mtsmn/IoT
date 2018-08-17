---

copyright:
  years: 2016, 2018
lastupdated: "2017-03-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Analyse de données IoT en temps réel
{: #analytics}  

**Important :** Nous lançons un nouveau programme bêta proposant une nouvelle façon de définir des règles pour vos données de terminal IoT dans le cadre d'un programme plus vaste de modifications visant à améliorer la manière dont {{site.data.keyword.iot_full}} distribue des règles et des actions.

Pour en savoir plus, consultez l'article de blogue [Une approche alternative pour définir des règles sur les données IoT ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}.

Pour commencer à définir vos propres règles, voir la documentation [Création de règles imbriquées (bêta)](information_management/im_rules.html).

## A propos de l'analyse de données IoT en temps réel

Utilisez Watson {{site.data.keyword.iot_short_notm}} Analytics pour obtenir les informations d'analyse en temps réel dont vous avez besoin à partir des données brutes générées par vos terminaux.  
{: shortdesc}

En utilisant des [tableaux et des cartes](data_visualization.html), vous pouvez visualiser, sous forme
graphique, les valeurs de jeux de données d'un ou de plusieurs terminaux afin de bénéficier
d'un aperçu rapide et d'une meilleure compréhension des données.

Les règles d'analyse vous permettent de spécifier les conditions qui déclenchent des actions. Par exemple, vous pouvez créer une règle garantissant qu'une alerte est envoyée au tableau de bord du terminal de l'utilisateur et qu'un courrier électronique est envoyé à l'administrateur lorsque le terminal est supprimé ou que la température du terminal augmente.

Utilisez des [règles cloud](cloud_analytics.html) afin de déclencher des règles pour des terminaux qui sont connectés directement à {{site.data.keyword.iot_short_notm}} dans le cloud, et des [règles Edge](edge_analytics.html) afin de déclencher des règles pour les terminaux qui sont connectés à des passerelles activées via Edge Analytics.
