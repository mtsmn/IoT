---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-07"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Simulation de données de terminal 
{: #sim_device_data}

Le simulateur de terminal {{site.data.keyword.iot_full}} permet de configurer les événements simulés de terminaux. Vous pouvez exploiter les données d'événements simulés pour découvrir, tester et démontrer pleinement le fonctionnement de {{site.data.keyword.iot_short_notm}}.
{: shortdesc}

Vous pouvez vous servir des terminaux et types de terminal existants ou vous pouvez générer de nouveaux terminaux pour des types existants. Vous avez la possibilité de configurer les détails d'événement de chaque terminal ou d'appliquer une configuration par défaut à l'ensemble des terminaux. Enfin, vous pouvez exporter une configuration d'événement simulé afin de la réutiliser ou de la partager à des fins de configuration d'autres simulations.

Pour simuler des données de terminal : 

1. Connectez-vous à {{site.data.keyword.iot_short_notm}}.
2. Dans le panneau de navigation principal, sélectionnez **Paramètres**.
3. Dans la section **Fonctions expérimentales**, activez le simulateur de terminal.
4. Dans le panneau de navigation principal, sélectionnez **Terminaux**. Un message dans le coin inférieur droit de l'écran indique qu'aucune simulation n'est en cours d'exécution.
5. Cliquez sur le message "0 simulation en cours d'exécution".
6. Dans la fenêtre en incrustation **Simulations**, cliquez sur **Ajouter une première simulation**.
7. Sélectionnez le type de terminal pour lequel vous souhaitez simuler des données.
8. Sélectionnez l'une des options suivantes :
  - Pour ajouter un ou plusieurs nouveaux terminaux, sélectionnez le nombre de votre choix et cliquez sur **Nouveau terminal**. La liste des nouveaux terminaux s'affiche.
  - Pour ajouter un terminal existant, cliquez sur **Utiliser un terminal enregistré** et sélectionnez un terminal dans la liste.
  - Pour importer une simulation existante, cliquez sur **Importation/Exportation**, sélectionnez l'onglet **Importer**, puis importez un fichier JSON ou copiez  une configuration précédemment exportée à partir du presse-papiers.
9. Cliquez sur ![Icône Paramètres](images/settings_icon.png) et configurez les détails de simulation d'un type de terminal :
   1. Sélectionnez le type de terminal dans la liste et cliquez sur **+ Type d'événement** pour ajouter un type d'événement au type de terminal.
   2. Sélectionnez un type d'événement dans la liste et cliquez sur **Programmer** pour définir la fréquence de l'événement.
   3. Editez les détails du type d'événement au format JSON et sauvegardez le type d'événement mis à jour.
   
   <p> Vous pouvez utiliser les deux variables spéciales ci-dessous lors de la configuration d'événements :  
        - `range` :  Cette variable est remplacée par un nombre aléatoire compris entre les deux valeurs fournies. Par exemple : `{ "random": range(300, 100) }`  
        - `$counter` : Cette variable est remplacée par le nombre de terminaux qui sont ajoutés dans le simulateur pour le type actuel. Par exemple : `{"total": $counter}`</p>
   {: tip}
   
   4. Indiquez si vous voulez utiliser ces paramètres comme valeurs par défaut pour tous les terminaux de ce type ou si vous voulez utiliser des valeurs personnalisées pour des terminaux individuels. 
   5. Répétez les étapes de configuration pour chaque terminal auquel vous souhaitez appliquer les paramètres personnalisés. Le message situé dans le coin inférieur droit de l'écran affiche le nombre de simulations actives.
10. **Facultatif :** Après avoir configuré les simulations pour les terminaux, exportez les détails de sorte qu'ils puissent être réutilisés ou partagés :
    1. Dans la fenêtre en incrustation **Simulations**, cliquez sur **Importation/Exportation**.
    2. Sélectionnez l'onglet **Exporter**.
    3. Copiez les détails de la simulation dans le presse-papiers ou téléchargez-les dans un fichier JSON.
11. Sur la page **Terminaux**, accédez à l'un des terminaux simulés et sélectionnez l'onglet **Evénements récents** pour vérifier que les événements simulés sont opérationnels.
