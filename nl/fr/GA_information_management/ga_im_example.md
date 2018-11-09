---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Initiation à la gestion des données via les API REST
{: #im_example}

Utilisez les étapes suivantes pour vous aider à configurer les ressources dont vous avez besoin pour pour commencer à utiliser les fonctions de terminal jumeau et d'actif jumeau du composant de gestion des données de {{site.data.keyword.iot_full}}.

Pour des détails sur les API, consultez la documentation en anglais [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Flux de travaux de niveau supérieur
{: #workflow}

Utilisez les étapes suivantes pour vous aider à configurer les ressources dont vous avez besoin pour commencer le mappage de vos données de terminal ou d'objet à l'aide de la fonction de terminal jumeau ou d'actif jumeau.

**Astuce :** Pour des informations plus détaillées sur chacune de ces étapes, consultez les exemples de scénario ou utilisez les liens pour accéder directement à une étape spécifique. Le [guide détaillé 1](ga_im_index_scenario.html#scenario) vous fait découvrir les étapes de création d'une interface logique de type de terminal pour les terminaux de thermomètres hétérogènes. Le [guide détaillé 2](../information_management/im_index_scenario_thing.html#scenario) développe le scénario en décrivant comment créer une interface logique qui permet à votre application de consommer des données provenant de deux types de terminaux climatiques différents regroupés dans une salle de type d'objet.

Le processus de création et de consommation des interfaces logiques varie quelque peu selon que vous créez une interface logique qui est associé à un type de terminal ou à un type d'objet.

### Avant de commencer
Pour créer une interface logique associée à un type de terminal ou à un type d'objet, vous devez avoir [au moins un terminal enregistré auprès de {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) et envoyant des événements avec des propriétés d'état.  


### Procédure

1. 	Définissez les propriétés d'état entrantes.  
Commencez par définir les propriétés d'état entrantes que votre interface logique devra mettre à la disposition de vos applications.  
Selon le type d'interface logique que vous créez, effectuez l'une ou l'autre des procédures suivantes :
<dl>
<dt>Pour une interface Type de terminal : créez une interface physique.</dt>
<dd>
<ol>
<li>[Créez un fichier schéma d'événement](ga_im_index_scenario.html#step1). Il s'agit d'un fichier .JSON local qui définit la structure et le format d'un événement entrant.
<li>[Créez une ressource de schéma d'événement pour votre type d'événement](ga_im_index_scenario.html#step2). Cette ressource de schéma d'événement est une construction programmatique utilisée par {{site.data.keyword.iot_short_notm}}.
<li>[Créez un type d'événement qui fasse référence au schéma d'événement](ga_im_index_scenario.html#step3). Le type d'événement est utilisé par
{{site.data.keyword.iot_short_notm}} pour mapper une ou plusieurs ressources de schéma d'événement à une interface physique.
<li>[Créez une interface physique](ga_im_index_scenario.html#step7).
<li>[Ajoutez le type d'événement à l'interface physique](ga_im_index_scenario.html#step8).
<li>[Ajoutez votre interface physique à votre type de terminal](ga_im_index_scenario.html#step9).
</ol>
</dd>
<dt>Type d'objet : Définissez un type d'objet.</dt>
<dd>
<ol>
<li>[Créez un fichier schéma de type d'objet](../information_management/im_index_scenario_thing.html#crt_composition_file).  
Un fichier schéma de type d'objet est un fichier .JSON local qui définit la composition du type d'objet en pointant sur des interfaces logiques existantes.
<li>[Créez la ressource de schéma de type d'objet](../information_management/im_index_scenario_thing.html#crt_composition_resource).  
Téléchargez le fichier .JSON vers {{site.data.keyword.iot_short_notm}}.
<li>[Créez un type d'objet](../information_management/im_index_scenario_thing.html#crt_thing_type). Un type d'objet sert le même but qu'un type de terminal dans le sens qu'il représente une classe d'objets.
</ol>
</dd>
</dl>
4. 	Créez l'interface logique
 1. 	Créez un fichier schéma d'interface logique pour le [type de terminal](ga_im_index_scenario.html#step4) ou le [type d'objet](../information_management/im_index_scenario_thing.html#crt_ai_schema_file).  
Un fichier schéma d'interface logique est un fichier .JSON local qui définit l'état du terminal mis à la disposition de vos applications.
 2. 	Créez une ressource de schéma d'interface logique pour le [type de terminal](ga_im_index_scenario.html#step5) ou le [type d'objet](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource).
 3.	Créez une interface logique pour le [type de terminal](ga_im_index_scenario.html#step6) ou le [type d'objet](../information_management/im_index_scenario_thing.html#crt_thing_ai).
 4.	Ajoutez l'interface logique au [type de terminal](ga_im_index_scenario.html#step10) ou au [type d'objet](../information_management/im_index_scenario_thing.html#add_thing_ai).
5. 	Définissez les mappages du [type de terminal](ga_im_index_scenario.html#step11) ou du [type d'objet](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings).   
Les mappages servent à mapper les propriétés entrantes aux propriétés de l'interface logique.  
6. 	Validez et activez la configuration associée au [type de terminal](ga_im_index_scenario.html#step15) ou au [type d'objet](../information_management/im_index_scenario_thing.html#activate) provisoire.
7. 	Extrayez l'état du [terminal](ga_im_index_scenario.html#step13) ou de l'[objet](../information_management/im_index_scenario_thing.html##verify_Thing_state).  
Vérifiez que vos abonnements montrent les données de terminal mises à jour ou que des données de terminal mises à jour sont renvoyées à l'aide d'un appel REST ou en vous abonnant à une chaîne de rubrique.  



