---

copyright:
years: 2017
lastupdated: "2017-08-10"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Initiation à la gestion des données à l'aide de l'interface Web (bêta)
{: #gs_web}

**Important :** La fonction de l'interface Web Gestion des données {{site.data.keyword.iot_full}} est disponible uniquement dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} fournit des outils en ligne pour vous permettre de normaliser et de transformer des données dans le cadre de la fonctionnalité de gestion des données. Outre l'[API de gestion des données Watson IoT Platform![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} fournie, vous pouvez utiliser le Créateur d'interface simple ou le Créateur d'interface avancée pour configurer des ressources et commencer à mapper vos données de terminal. Les créateurs d'interface en ligne vous guide tout au long de la procédure de l'interface utilisateur.

Pour en savoir plus sur la fonctionnalité de gestion des données et ses avantages, voir [Introduction à la gestion des données](../GA_information_management/ga_im_device_twin.html#device_twins).

Pour plus d'informations sur les ressources que vous devez configurer, voir [Comprendre la gestion des données](../GA_information_management/ga_im_definitions.html#definitions_resource).

A l'aide du Créateur d'interface simple, lorsque vous ajoutez une interface physique, une interface logique est automatiquement créée et les deux interfaces sont mappées ensemble. Lorsque vous utilisez le Créateur d'interface avancée, vous disposez de davantage d'options de configuration. Vous ajoutez une interface physique, une ou plusieurs interfaces logiques, puis vous ajoutez des mappages entre les deux.

Les interfaces et les mappages sont ajoutés de manière provisoire. Une fois que vous les avez ajoutés et validés, vous devez les activer. Pour plus d'informations sur les ressources actives et provisoires, voir [Création, mise à jour, activation et désactivation de vos ressources](../GA_information_management/ga_im_definitions.html#draft_active_resources).



## Flux de niveau supérieur
{: #interface_flow}

Utilisez les étapes décrites ci-après pour vous aider à configurer les ressources dont vous avez besoin pour commencer à utiliser la fonction de gestion des données.

**Avant de commencer**
La procédure ci-dessous suppose que vous disposez d'un type de terminal avec au moins un terminal connecté et enregistré. Pour en savoir plus sur la configuration requise et sur les modalités de création d'un type de terminal et d'enregistrement d'un terminal, voir [Configuration des terminaux à utiliser avec la fonction Gestion des données à l'aide de l'interface Web](im_config_devices.html).

1. Sélectionnez le type de terminal pour lequel vous souhaitez normaliser et transformer les données.
2. Sélectionnez le Créateur d'interface simple ou avancée :

a. Flux simple  
   i. [Créez une interface physique provisoire](#create_physical_interface_simple).  
   
b. Flux avancé  
   i. [Créez une interface physique provisoire](#create_physical_interface_advanced).  
   ii. [Créez une ou plusieurs interfaces logiques provisoires](#create_logical_interface). (Cette étape est automatique lorsque vous utilisez le flux simple.)  
   iii. [Mappez l'interface physique aux interfaces logiques](#create_interface_mappings).  
     
     
3. [Définissez vos préférences de notification](#set_notifications).
4. [Activez la configuration](#validate_activate).

*Remarque :* Vous pouvez également configurer la fonction de gestion des données à l'aide des API. Pour plus d'informations sur la configuration de la fonction de gestion des données à des fins de normalisation et de transformation des données de votre terminal à l'aide des API, voir [Initiation à la gestion des données]{ga_im_example.html#im_example}. Le document [Guide détaillé : Exemple détaillé de la manière de travailler avec des terminaux via une interface commune](../GA_information_management/ga_im_index_scenario.html#scenario) vous fait découvrir la procédure de création d'une interface logique de type de terminal pour différents thermomètres hétérogènes.

Pour des détails sur les API, consultez la documentation en anglais [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icône de lien externe](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Création d'une interface physique provisoire (Flux simple)
{: #create_physical_interface_simple}

1. Dans le menu de navigation principal, sélectionnez **Terminaux**.
2. Sélectionnez **Types de terminaux** et choisissez le type de terminal pour lequel vous souhaitez créer une interface. Vous pouvez également [créer un nouveau type de terminal].
3. Examinez et mettez à jour les informations relatives au type de terminal, si besoin, et sauvegardez les modifications.
4. Cliquez sur **Interface**.
5. Cliquez sur **Flux simple** pour ouvrir le Créateur d'interface simple.
6. Cliquez sur **Créer une interface**.
7. Cliquez sur **Ajouter une propriété** pour commencer à ajouter des événements et des propriétés à l'interface physique.  
 a. Dans la fenêtre **Ajouter des propriétés à l'interface**, sélectionnez les événements que vous voulez ajouter à l'interface. Le système écoute les événements actifs des terminaux connectés pour le type de terminal que vous avez sélectionné. Vous pouvez sélectionner l'ID de terminal du dernier événement mis en cache, ou sélectionner un autre événement à partir de la liste.  
  b. Sélectionnez les propriétés à partir de l'événement et cliquez sur **Ajouter**.  
  c. Ajoutez d'autres propriétés si nécessaire.
8. Cliquez sur **Terminé**. L'interface physique est créée. Si vous souhaitez ajouter d'autres informations, mettez à niveau l'interface en sélectionnant le lien **Utiliser le créateur d'interface avancée**.


## Création d'une interface physique provisoire (Flux avancé)
{: #create_physical_interface_advanced}

1. Dans le menu de navigation principal, sélectionnez **Terminaux**.
2. Sélectionnez **Types de terminaux** et choisissez le type de terminal pour lequel vous souhaitez créer une interface. Vous pouvez également [créer un nouveau type de terminal].
2. Consultez les informations relatives au type de terminal et cliquez sur **Interface**.
4. Cliquez sur **Flux avancé** pour ouvrir le Créateur d'interface avancée.
5. Cliquez sur **Créer une interface**
7. Sur l'onglet **Identité** de la page **Créer une interface physique**, entrez un nom et une description de l'interface physique.
7. Facultatif : Activez Edge Analytics si vous utilisez des terminaux Edge avec l'interface.
8. Cliquez sur **Suivant**.
9. Définissez l'interface physique en ajoutant des types d'événements et du contenu :
 a. Cliquez sur **+ Créer un type d'événement**. 
 b. Sélectionnez le dernier événement mis en cache, ou ajoutez-en un manuellement.
10. Cliquez sur **Terminé**. L'interface physique est créée en version provisoire. Vous pouvez continuer en ajoutant une ou plusieurs interfaces logiques.

## Création d'une interface logique provisoire
{: #create_logical_interface}

1. Une fois que vous avez créé l'interface physique provisoire, cliquez sur **Ajouter une interface logique**.
2. Sur l'onglet **Identité** de la page **Créer une interface logique**, entrez un nom et une description de l'interface logique.
3. Cliquez sur **Suivant**.
4. Cliquez sur **Ajouter une propriété** pour commencer à définir les propriétés de l'interface logique.
5. Dans la fenêtre **Créer une propriété**, sélectionnez les propriétés de l'interface physique que vous souhaitez ajouter à l'interface logique et sauvegardez vos modifications.
6. Ajoutez d'autres interfaces logiques si nécessaire, et cliquez sur **Suivant**.
7. {: #set_notifications}Définissez les préférences de notification de l'interface. Sélectionnez l'une des options suivantes pour déterminer à quel moment envoyer les notifications d'état du terminal :
 - Pas de notifications d'événement - Aucune notification n'est envoyée. Vous pouvez vous servir de l'API pour extraire les informations relatives au changement d'état du terminal.
 - En cas de changement d'état - Une notification est envoyée en cas de changement d'état du terminal.
 - Pour chaque événement - Une notification est envoyée chaque fois que la plateforme traite un événement pour le terminal, même si l'événement n'entraîne pas de changement d'état.
8. Cliquez sur **Terminé**. Une version provisoire des interfaces est créée.
9. {: #validate_activate} Cliquez sur **Déployer** pour valider la configuration.
10. Cliquez sur **Déployer** pour activer la configuration. La configuration prend le statut déployé.  

