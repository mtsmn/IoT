---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Visualisation des données en temps réel à l'aide de tableaux et de cartes
{: #boards_and_cards}


**Important :** Nous lançons un nouveau programme bêta proposant une nouvelle façon de définir des règles pour vos données de terminal IoT dans le cadre d'un programme plus vaste de modifications visant à améliorer la manière dont {{site.data.keyword.iot_full}} distribue des règles et des actions.

Pour en savoir plus, consultez l'article de blogue [Une approche alternative pour définir des règles sur les données IoT ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}.

Pour commencer à définir vos propres règles, voir la documentation [Création de règles imbriquées (bêta)](information_management/im_rules.html).

## A propos des tableaux et des cartes

Créez des tableaux et des cartes afin de créer et partager vos propres tableaux de bord permettant de visualiser vos données de terminal en temps réel.

En utilisant des tableaux et des cartes, vous pouvez visualiser, sous forme
graphique, les valeurs de jeux de données d'un ou de plusieurs terminaux afin de bénéficier
d'un aperçu rapide et d'une meilleure compréhension des données. Créez des tableaux et ajoutez des cartes qui affichent les données sous forme de nombres bruts, de graphiques en temps réel, de jauges, etc. Ajoutez des membres à vos tableaux pour les partager avec d'autres utilisateurs dans votre organisation. Organisez les cartes et ajoutez des diviseurs de texte explicatif pour peaufiner votre présentation  

Vous pouvez également développer l'ensemble par défaut de cartes [en créant vos propres cartes personnalisées](custom_cards/custom-cards.html).

![Affichage de données en temps réel avec des cartes.](images/boards_and_cards.svg "Affichage de données en temps réel avec des cartes.")

## Cartes par défaut
{: #default_boards}
Le tableau de bord {{site.data.keyword.iot_short_notm}} comporte les tableaux par défaut suivants :

|Nom de tableau | Description | Cartes incluses
|:---|:---|:---|  
|Présentation de l'utilisation  | Statistiques d'utilisation pour votre organisation. Répertorie les types de terminaux, ainsi que les données consommées. | <ul><li>Types de terminaux<li>Données transférées</ul>
|Analyse centrée sur la règle | Règles pour votre organisation. Des cartes supplémentaires recensent les alertes déclenchées, les terminaux associés, les propriétés de terminal et les informations d'alerte. | <ul><li>Règles que je gère<li>Alertes de règle<li>Informations sur l'alerte de règle<li>Terminaux associés<li>Informations sur le terminal<li>Propriétés de terminal</ul>  
|Analyse centrée sur le terminal | Les terminaux connectés à votre organisation. Des cartes supplémentaires affichent des alertes pour un terminal sélectionné, des informations relatives à un terminal sélectionné, et des informations d'alerte. | <ul><li>Terminaux qui m'intéressent<li>Informations sur le terminal<li>Alertes de règle pour ce terminal<li>Informations sur l'alerte de règle<li>Propriétés de terminal</ul>
|Présentation de la sécurité et des risques (bêta) | L'état général de la sécurité de votre organisation. Les opérateurs système et les analystes de sécurité peuvent afficher des détails sur la conformité, l'état de connexion des terminaux, les causes des échecs de connexion et les terminaux bloqués par liste noire ou autorisés par liste blanche.  À partir de la carte Conformité à la sécurité de connexion, l'utilisateur peut obtenir un rapport détaillé sur les terminaux non conformes et exporter le rapport vers Excel. | <ul><li>Conformité à la politique<li>Sécurité de connexion<li>Conformité liste noire/liste blanche</ul>

Vous pouvez mettre à jour ces tableaux en ajoutant, en mettant à jour et en retirant des cartes.

Pour ramener un tableau par défaut à son état d'origine, vous pouvez le supprimer. Le tableau est ensuite recréé avec les cartes d'origine.
{: tip}

## Création de tableaux et de cartes
{: #visualizing_data}

{{site.data.keyword.iot_short_notm}} fournit un tableau intégré que vous pouvez utiliser pour afficher les données en temps réel renvoyées par votre terminal. Par défaut, la page de présentation affiche les informations d'utilisation relatives à votre organisation {{site.data.keyword.iot_short_notm}}, telles que les données et l'espace de stockage consommés. Pour afficher le flux des données de terminal en temps réel, ajoutez des cartes spécifiques des terminaux à cette page.

Pour des instructions pas à pas sur la façon d'afficher les données temps réel de terminaux, consultez la recette [Configuring Boards & Cards in the new Watson IoT Dashboard ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/configuring-the-cards-in-the-new-watson-iot-dashboard/){: new_window}.
{: tip}

Pour créer un tableau et y ajouter une carte :
1. Dans le tableau de bord {{site.data.keyword.iot_short_notm}}, sélectionnez **Tableaux**.
2. Sélectionnez un tableau sur lequel vous possédez des droits d'édition ou créez un nouveau tableau.
3. Dans le tableau, cliquez sur **Ajouter une nouvelle carte**.
3. Sélectionnez un type de carte.  
**Astuce :** Si vous hésitez quant au choix de visualisation d'une carte de type Terminaux, sélectionnez **Visualisation générique**. Vous pouvez modifier le type de carte ultérieurement.
<dl>
<dt>Terminaux</dt>
<dd><table>
<thead>
<tr>
<th>Type</th>
<th>Données affichées.</th>
</tr>
</thead>
<tbody>
<tr>
<td>Visualisation générique</td>
<td>Valeur d'un ou de plusieurs jeux de données. </br>**Astuce :** Pour voir jusqu'à trois valeurs de points de données dans un petit tableau, choisissez la grande taille de widget. </td>
</tr>
<tr>
<td>Line chart (graphique à courbes)</td>
<td>Un ou plusieurs jeux de données dans un graphique temps réel défilant. Utilisez le menu Paramètres pour définir la plage et la conservation des données, la présentation des graphiques, etc. </td>
<tr>
<td>Graphique à barres</td>
<td>Valeurs des jeux de données dans des barres libellées. Utilisez le menu Paramètres pour passer de l'affichage horizontal à l'affichage vertical et inversement pour les barres.</td>
</tr>
<tr>
<td>Graphique en anneau</td>
<td>Au moins deux jeux de données dans une représentation circulaire.</td>
</tr>
<tr>
<td>Valeur</td>
<td>Valeur brute d'un ou de plusieurs jeux de données.</td>
</tr>
<tr>
<td>Jauge</td>
<td>Valeur d'un jeu de données affichée sous forme de jauge. Utilisez le menu Paramètres pour définir éventuellement des seuils de jauge pour les plages de données inférieures, intermédiaires et supérieures.  </td>
</tr>
<tr>
<td>Propriétés de terminal</td>
<td>Propriétés spécifiques pour un ou plusieurs terminaux.</td>
</tr>
<tr>
<td>Toutes les propriétés de terminal</td>
<td>Toutes les propriétés pour un ou plusieurs terminaux.</td>
</tr>
<tr>
<td>Liste de terminaux</td>
<td>Liste permettant de surveiller plusieurs terminaux. Elle peut être utilisée comme source de données d'autres cartes. </br>Vous pouvez
filtrer une liste par ID et type de terminal dans les paramètres de la carte. Les listes de terminaux de taille L ou plus grande peuvent aussi être filtrées interactivement. Pour cela,
cliquez sur l'icône de filtre dans la carte. Les entrées du filtre peuvent être ajoutées une à une, sous forme de plage
(x-y) ou de liste d'éléments séparés par une virgule.</br> Par défaut, une liste affiche l'ID et le type de chaque terminal. Vous pouvez configurer les paramètres de la carte pour qu'elle affiche d'autres métadonnées des terminaux dans la liste.  </td>
</tr>
<tr>
<td>Informations sur le terminal</td>
<td>Informations de base pour un terminal.</td>
<tr>
<td>Mappe de terminaux</td>
<td>Emplacement des terminaux d'une liste.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Analyse</dt>
<dd>
<table>
<thead>
<tr>
<th>Type</th>
<th>Données affichées.</th>
</tr>
</thead>
<tbody>
<tr>
<td>Règles</td>
<td>Liste de règles auxquelles sont associées des alertes.</td>
</tr>
<tr>
<td>Alertes de règle</td>
<td>Liste d'alertes pour un terminal.</td>
</tr>
<tr>
<td>Informations d'alerte</td>
<td>Informations de base pour une alerte.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Gestion des risques (bêta)</dt>
<dd>Disponible uniquement pour les organisations avec plan de [Sécurité avancée](reference/security/RM_security.html).
<table>
<thead>
<tr>
<th>Type</th>
<th>Données affichées.</th>
</tr>
</thead>
<tbody>
<tr>
<td>Conformité à la politique</td>
<td>Vue d'ensemble de la sécurité de connexion et des terminaux sur liste noire et sur liste blanche.</td>
</tr>
<tr>
<td>Conformité liste noire/liste blanche</td>
<td>Nombre de terminaux sur liste noire et sur liste blanche.</td>
</tr>
<tr>
<td>Sécurité de connexion</td>
<td>Le nombre de terminaux ayant échoué aux vérifications de la sécurité de connexion.</td>
</tr>
</tbody>
</table>
</dd>
<dt>Utilisation</dt>
<dd>
<table>
<thead>
<tr>
<th>Type</th>
<th>Données affichées.</th>
</tr>
</thead>
<tbody>
<tr>
<td>Types de terminaux</td>
<td>Graphique à secteurs affichant le nombre de terminaux enregistrés par type de terminal, pour votre organisation.</td>
</tr><tr>
<td>Données transférées</td>
<td>Statistiques d'utilisation relatives aux données transférées pour votre organisation.</td>
</tr>
</tbody>
</table>
</dd>
<dt>De base</dt>
<dd>
<table>
<thead>
<tr>
<th>Type</th>
<th>Données affichées.</th>
</tr>
</thead>
<tbody>
<tr>
<td>Séparateur</td>
<td>Séparateur horizontal permettant de structurer et de regrouper des cartes sur le tableau.</td>
</tr>
</tbody>
</table>
</dd>
</dl>

4.	Sélectionnez la ou les sources de données de la carte.  
Sélectionnez une ou plusieurs sources de données de carte, puis cliquez sur **Suivant**.  
Les sources de données peuvent être des terminaux enregistrés ou d'autres cartes. L'utilisation d'une autre carte comme source de données exige la
présence d'une carte de type liste ou mappe dans le tableau.  
5. Ajoutez un ou plusieurs jeux de données pour chacune de vos sources de données.
 - Terminaux
    2. Sélectionnez un événement qui inclut le point de données que vous souhaitez afficher.
    3.	Sélectionnez la propriété qui représente le point de données.
    1.	Donnez un nom d'identification au jeu de données.
    4.	Définissez le type, l'unité, la précision, et les valeurs minimum et maximum pour le point de données.  
    Lorsque vous avez terminé, vous pouvez soit cliquer sur **Nouveau jeu de données** pour ajouter d'autres jeux de données, soit cliquer sur **Suivant**.
 - Listes
    2. Sélectionnez un type de terminal ou choisissez **Tout type de terminal**.
    2. Sélectionnez un événement qui inclut le point de données que vous souhaitez afficher.
    3.	Sélectionnez la propriété qui représente le point de données.
    1.	Donnez un nom d'identification au jeu de données.
    4.	Définissez le type, l'unité, la précision, et les valeurs minimum et maximum pour le point de données.  
    Lorsque vous avez terminé, vous pouvez soit cliquer sur **Nouveau jeu de données** pour ajouter d'autres jeux de données, soit cliquer sur **Suivant**.
5.	Personnalisez la visualisation de la carte dans son aperçu.  
 7. Sélectionnez la taille de la présentation.  
Vous pouvez régler la taille de la carte sur votre tableau, mais aussi contrôler
d'autres variables de présentation telles que le nombre de terminaux à lister ou les
métadonnées du graphique affiché.   
**Astuce :** Cliquez sur les différentes étiquettes de taille pour obtenir un aperçu des cartes dans ces différentes tailles.
 8. Configurez les autres réglages.  
Si la carte le prévoit, cliquez sur **Paramètres** pour accéder aux autres réglages tels que
les plages de données pour les cartes de type compteur (ou jauge) et les options de filtrage pour les
cartes de type liste de terminaux.
6. Mettez à jour les informations de la carte.  
 1. Entrez un titre et une description pour la carte et, en option, choisissez une combinaison de couleurs.   
 2. Cliquez sur **Soumettre** pour créer la carte.
7.	Positionnez la nouvelle carte sur votre tableau en la faisant glisser vers l'emplacement approprié.  
