---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Aperçu de {{site.data.keyword.iot_short_notm}} Edge (prévisualisation)
{: #edge_overview}

**Important :** la fonction {{site.data.keyword.iot_full}} Edge n'est disponible que dans le cadre d'un programme de prévisualisation limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} Edge vous permet d'étendre les fonctions de {{site.data.keyword.iot_short_notm}} aux terminaux Edge. Les terminaux Edge résident à la périphérie d'un réseau dans une organisation. Les exemples incluent des capteurs et des contrôleurs industriels à un emplacement physique, comme par exemple une usine. Avec {{site.data.keyword.iot_short_notm}} Edge, les données peuvent être traitées à l'intérieur des terminaux avant d'être envoyés au cloud.

Pour utiliser {{site.data.keyword.iot_short_notm}} Edge, vous devez créer des noeuds Edge et configurer ces noeuds avec les services qui doivent être exécutés à leur intérieur. WIoTP Edge déploie ensuite ces services dans les noeuds. Lorsque les services Edge s'exécutent sur eux, les noeuds Edge peuvent envoyer des messages à partir des terminaux via la passerelle Edge. La passerelle traite les messages, transforme les données et peut envoyer ensuite les données au cloud, suivant la manière dont les interfaces sont configurées.

La prévisualisation de {{site.data.keyword.iot_short_notm}} offre le service par défaut Core IoT ainsi que la possibilité de créer des services personnalisés, comme un service permettant de prévoir des pannes dans un robot de production et d'envoyer des notifications de panne. Un exemple de terminal Edge est un capteur qui surveille l'utilisation du l'unité centrale et envoie une alerte lorsque celle-ci dépasse un certain pourcentage.

## Composants de {{site.data.keyword.iot_short_notm}} Edge

**Noeud Edge**
Un noeud Edge se compose d'une passerelle et d'un terminal qui réside à la périphérie du réseau.

**Terminal Edge**
Un terminal, comme par exemple un capteur, qui réside à la périphérie d'un réseau et exécute des services qui traitent les données avant qu'elles soient envoyées au cloud. Un exemple de terminal Edge est un capteur qui surveille l'utilisation du l'unité centrale et envoie une alerte lorsque celle-ci dépasse un certain pourcentage.

**Passerelle Edge**
Les passerelles sont des terminaux spécialisés qui ont les capacités combinées d'une application et d'un terminal, ce qui leur permet d'agir en tant que points d'accès pour d'autres terminaux. Lorsque Edge est activé, les passerelles Edge activent les terminaux qui ne peuvent pas se connecter directement à Internet pour accéder au service {{site.data.keyword.iot_short_notm}} en se connectant d'abord au terminal de passerelle à la périphérie.

**Type de passerelle Edge**
Un type de passerelle regroupe des terminaux de passerelle qui partagent des attributs, comme un numéro de modèle ou un emplacement. Lorsque les fonctions de Edge sont activées et un terminal de passerelle de périphérie est ajouté à {{site.data.keyword.iot_short_notm}}, les attributs du type de passerelle de périphérie sont utilisés comme modèle pour le nouveau terminal de passerelle de périphérie.

**Services Edge**
Les services sont des fonctions qui sont ajoutées à une passerelle {{site.data.keyword.iot_short_notm}} Edge et qui effectuent des processus tels que la manipulation des données. Vous générez des applications qui utilisent les services.

**Catalogues des services Edge**
Le service par défaut Core IoT est inclus dans le catalogue et ajouté à tous les noeuds Edge. Vous pouvez ajouter des services personnalisés en fonctions de vos besoins commerciaux. Le catalogue des services Edge contient tous les services personnalisés disponibles.

**Référentiel du serveur de fichier**
Le référentiel du serveur de fichier contient des conteneurs Docker et des images. Toutes les fonctions de traitement ou services Edge travaillent à l'intérieur des conteneurs Docker. Vous sélectionnez les images Docker à exécuter à l'intérieur des terminaux.

## Recherche d'informations supplémentaires
{: #more_info}

Pour en savoir plus sur la configuration, l'installation et le développement de {{site.data.keyword.iot_short_notm}} Edge, consultez les rubriques suivantes :
 - [Configuration de {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
 - [Installation de {{site.data.keyword.iot_short_notm}} Edge sur les terminaux Edge](WIoTP_edge_install.html#edge_install_device)
 - [Développement pour {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
