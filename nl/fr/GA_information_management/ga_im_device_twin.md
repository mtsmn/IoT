---

copyright:
years: 2016, 2017
lastupdated: "2017-07-20"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduction à la gestion de données
{: #device_twins}

Un nombre incalculable de terminaux et de capteurs existent partout dans le monde. La plupart de ces terminaux fournissent une fonctionnalité similaire, mais des variations dans la marque, le modèle et la version entraînent une sortie des données dans différents formats. Par exemple, un capteur de température peut enregistrer les températures en degrés Fahrenheit ou Celsius. In est inutile de coder les applications de sorte qu'elles utilisent les données dans tous ces formats. A la place, les données doivent être normalisées afin de créer une vue logique unique pouvant être utilisée par les  applications.
{: shortdesc}

Utilisez la fonction de gestion de données de {{site.data.keyword.iot_full}} pour configurer un terminal jumeau capable d'exposer une vue normalisée des données pour vos applications.

Un terminal jumeau est une représentation digitale dans le Cloud d'un terminal physique ou d'un capteur connecté à {{site.data.keyword.iot_short_notm}}. Un terminal jumeau crée un modèle logique des propriétés et des événements provenant d'un capteur ou d'un terminal spécifique. Une fois que vous avez défini et instancié le terminal jumeau, ce dernier permet d'interagir de manière constante avec un terminal basé sur REST, que le terminal soit en ligne ou hors ligne. Un modèle logique pouvant être partagé par plusieurs terminaux de marques et modèles différents, l'application IoT est maintenant protégée contre les variations et les changements au sein de l'écosystème des terminaux. Les propriétés d'un terminal, notamment les informations sur l'état actuel du terminal (état de terminal), peuvent être récupérées à l'aide d'une requête HTTP ou en souscrivant à une rubrique.

Les avantages liés aux terminaux jumeaux sont les suivants :
- Fournir à vos développeurs d'application des interfaces cohérentes pour leur permettre d'accéder aux données des terminaux déclenchées par les événements à travers des API REST.
- Normaliser les données issues de terminaux de marques et modèles différents qui publient leurs données dans différents formats.

Pour configurer un terminal jumeau à l'aide de la fonction de gestion des données, définissez les informations suivantes lors de la configuration des ressources dans {{site.data.keyword.iot_short_notm}} :
- La structure des événements qui sont envoyés par votre terminal. La structure d'un événement entrant est défini dans l'interface physique, le type d'événement et les ressources de schéma d'événement. 
- Les propriétés que vous souhaitez enregistrer. Ces propriétés définissent la structure logique de l'état du terminal pouvant être utilisé par vos applications. Les propriétés sont définies dans l'interface logique et les ressources de schéma logiques.
- Les modalités de mappage des événements d'interface physique aux propriétés de l'interface logique. Utilisez la ressource des mappages pour mapper les événements aux propriétés.

Le diagramme ci-dessous illustre les données de terminal qui entrent dans {{site.data.keyword.iot_short_notm}} sous différents formats, et qui sont transformées et normalisées en une seule vue logique qui peut être facilement utilisée par les applications de back end.  

![Présentation et gestion des données dans {{site.data.keyword.iot_short_notm}}.](images/ga_im_resources_overview.svg "Présentation et gestion des données dans {{site.data.keyword.iot_short_notm}}")

Pour plus d'informations sur la définition et la configuration d'informations et ressources clés, voir [Comprendre la gestion des données](ga_im_definitions.html). Vous pouvez créer votre propre terminal jumeau dans {{site.data.keyword.iot_short_notm}} en effectuant les étapes décrites dans le document [Initiation à la gestion des données](ga_im_example.html). Pour obtenir des informations détaillées sur chacune des étapes décrites dans le guide, consultez le scénario du document [Guide détaillé : Exemple détaillé de la manière de travailler avec des terminaux via une interface commune](ga_im_index_scenario.html#scenario). 
