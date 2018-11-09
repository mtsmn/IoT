---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutoriel d'initiation
{: #getting-started-with-iotp}

Dans ce tutoriel d'initiation à {{site.data.keyword.iot_full}}, nous connectons un terminal
IoT à {{site.data.keyword.iot_short_notm}} et mettons en place une analyse pour explorer les données temps réel.
{:shortdesc}

<div id="prerequisites"></div>

## Avant de commencer
{: #prereqs}

Vous aurez besoin d'un [compte IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/registration/){: new_window},
d'une instance du service {{site.data.keyword.iot_short_notm}} et d'un terminal remplissant les conditions suivantes :

*	Votre terminal doit pouvoir communiquer à l'aide des protocoles HTTP ou MQTT.

* Les messages du terminal doivent obéir aux règles concernant la charge utile des messages {{site.data.keyword.iot_short_notm}}.

Pour plus d'informations, consultez [Développement de terminaux sur Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Etape 1 : enregistrez votre terminal

Vous pouvez ajouter les terminaux un à un à partir du tableau de bord [{{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://internetofthings.ibmcloud.com){: new_window} ou utiliser l'[API Watson IoT Platform ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} pour ajouter plusieurs terminaux à la fois.

Pour ajouter un terminal depuis le tableau de bord {{site.data.keyword.iot_short_notm}} :

1. Dans la console {{site.data.keyword.Bluemix_notm}}, cliquez sur **Lancer** dans la page
de détails du service {{site.data.keyword.iot_short_notm}}.

    La console web {{site.data.keyword.iot_short_notm}} s'ouvre dans un nouvel onglet de navigateur à l'URL suivante :

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Où *org_id* est l'ID de [votre organisation {{site.data.keyword.iot_short_notm}} ](iotplatform_overview.html#organizations){: new_window}.

2. Dans le tableau de bord Présentation, à partir du panneau de menu, sélectionnez **Terminaux**, puis cliquez sur **Ajouter un terminal**.

3. Créez un type de terminal pour le terminal que vous ajoutez.

    Chaque terminal connecté à {{site.data.keyword.iot_short_notm}} doit être associé à un type de terminal. Les types de terminal sont des groupes de terminaux ayant des caractéristiques communes.

    1. Cliquez sur **Créer un type de terminal**.
    2. Entrez un nom de type de terminal, par exemple, `my_device_type`, et une description pour
le type de terminal.
    
        > **Remarque :** Le nom du type de terminal ne doit pas dépasser 36 caractères et ne doit contenir que des caractères alphanumériques (a-z, A-Z, 0-9) et les caractères suivants : `_`, `.`, `-`.

    3. Facultatif : Entrez des métadonnées et des attributs de type de terminal.

        Vous pourrez ajouter et éditer des attributs et des métadonnées ultérieurement.
        {: tip}

4. Cliquez sur **Suivant** pour commencer le processus d'ajout de votre terminal avec le type de terminal sélectionné.

5. Entrez un ID de terminal. Par exemple, `my_first_device`.

    L'ID de terminal permet d'identifier le terminal dans le tableau de bord {{site.data.keyword.iot_short_notm}}. Il constitue
aussi un paramètre indispensable à la connexion de votre terminal à {{site.data.keyword.iot_short_notm}}.

    > **Remarque :** Le nom du type de terminal ne doit pas dépasser 36 caractères et ne doit contenir que des caractères alphanumériques (a-z, A-Z, 0-9) et les caractères suivants : `_`, `.`, `-`.

    Pour les terminaux connectés à un réseau, l'ID de chaque terminal pourrait être son adresse MAC sans les signes deux-points de séparation.

5. Cliquez sur **Suivant** pour finaliser le processus.

6. Fournissez un jeton d'authentification ou acceptez celui qui est généré automatiquement. Si vous choisissez de créer votre propre jeton, assurez-vous qu'il comporte entre 8 et 36 caractères composés uniquement de caractères alphanumériques et des caractères suivants : `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)` et `-`.

    Le jeton ne doit pas contenir des séquences de caractères répétés, des mots de dictionnaire, des noms d'utilisateur ni d'autres séquences prédéfinies.

7. Vérifiez que les informations récapitulatives sont correctes, puis cliquez sur **Ajouter** pour ajouter la connexion.

8. Sur la page Informations sur le terminal, copiez et sauvegardez les détails suivants :

    * ID d'organisation
    * Type de terminal
    * ID de terminal
    * Méthode d'authentification
    * Jeton d'authentification

    Vous aurez besoin d'un ID d'organisation, d'un type de terminal, d'un ID de terminal et d'un jeton d'authentification pour configurer votre terminal afin qu'il se connecte à {{site.data.keyword.iot_short_notm}}.

## Etape 2 : connectez votre terminal à {{site.data.keyword.iot_short_notm}}

1. Configurer votre terminal pour la messagerie MQTT et de sorte qu'il soit authentifié avec
l'ID d'organisation, le type de terminal, l'ID de terminal et le jeton d'authentification.

2. Envoyer des messages du terminal à votre organisation {{site.data.keyword.iot_short_notm}} en utilisant le protocole MQTT.

    Les informations suivantes sont requises lors de la connexion de votre terminal :

    * URL : *org_id*.messaging.internetofthings.ibmcloud.com

      Où *org_id* est l'ID de votre organisation {{site.data.keyword.iot_short_notm}}.

    * Port :
      * 1883
      * 8883 (chiffré)
      * 443 (websockets)
    * ID de terminal : d:_org_id:device_type:device_id_
    * Nom d'utilisateur : use-token-auth
    * Mot de passe : _jeton d'authentification_
    * Format de sujet d'événement : iot-2/evt/_event_id/fmt/format_string_

      Où _event_id_ spécifie le nom d'événement affiché dans {{site.data.keyword.iot_short_notm}} et _format_string_ est le format de l'événement, par exemple, JSON.

    * Format de message : JSON

  Pour plus d'informations, consultez [Connectivité MQTT pour les terminaux](/docs/services/IoT/devices/mqtt.html).

## Etape 3 : créez des tableaux et des cartes pour suivre les données des terminaux

En utilisant des tableaux et des cartes, vous pouvez visualiser, sous forme
graphique, les valeurs de jeux de données d'un ou de plusieurs terminaux afin de bénéficier
d'un aperçu rapide et d'une meilleure compréhension des données.

1. Dans votre tableau de bord {{site.data.keyword.iot_short_notm}}, cliquez sur **Créer un nouveau tableau**.

2. Entrez un nom et une description pour le tableau.

3. Cliquez sur **Suivant**, puis sur **Créer**.

4. Cliquez sur le tableau que vous venez de créer, puis sur **Ajouter une nouvelle carte**. Sélectionnez Terminaux comme type de carte. Le tableau suivant décrit les options de visualisation que vous pouvez choisir.

| Type | Données affichées. |
|---------------|---------------|
| Visualisation générique | Valeur d'un ou de plusieurs jeux de données. Pour voir jusqu'à trois valeurs de points de données dans un petit tableau, choisissez la grande taille de widget. |
| Line chart (graphique à courbes) | Un ou plusieurs jeux de données dans un graphique temps réel défilant. Utilisez le menu Paramètres pour définir la plage et la conservation des données, la présentation des graphiques, etc. |
| Graphique à barres | Valeurs des jeu de données dans des barres libellées. Utilisez le menu Paramètres pour passer de l'affichage horizontal à l'affichage vertical et inversement pour les barres. |
| Graphique en anneau | Au moins deux jeux de données dans une représentation circulaire. |
| Valeur | Valeur brute d'un ou de plusieurs jeux de données. |
| Jauge | Valeur d'un jeu de données affichée sous forme de jauge. Utilisez le menu Paramètres pour définir éventuellement des seuils de jauge pour les plages de données inférieures, intermédiaires et supérieures. |
| Propriétés de terminal | Propriétés spécifiques pour un ou plusieurs terminaux. |
| Toutes les propriétés de terminal | Toutes les propriétés pour un ou plusieurs terminaux. |
| Liste de terminaux | Liste permettant de surveiller plusieurs terminaux. Elle peut être utilisée comme source de données d'autres cartes. |
| Informations sur le terminal | Informations de base pour un terminal. |
| Mappe de terminaux | Emplacement des terminaux d'une liste. |

{: caption="Tableau 1. Options de visualisation" caption-side="top"}

## Etape 4 : créez un schéma de type de terminal

A ce stade, vous devez créer un schéma de type de terminal et mapper les propriétés du terminal pour ensuite
créer des règles qui seront déclenchées en fonction des points de données de vos propriétés mappées.

1. Dans le tableau de bord {{site.data.keyword.iot_short_notm}}, allez à
**Terminaux > Gérer les schémas**, puis cliquez sur **Ajouter un schéma**.

2. Sélectionnez un type de terminal à associer au schéma de message. Un seul schéma peut être défini pour un type de terminal.

3. Cliquez sur **Ajouter une propriété** et ajoutez une ou plusieurs propriétés.

    Vous pouvez sélectionner des propriétés d'un terminal connecté, créer des propriétés virtuelles qui modifient ou combinent des propriétés existantes, ou ajouter
des propriétés manuellement.

    Les propriétés disponibles sont définies dans la charge utile des messages émis par un terminal.
    {: tip}

    * Pour ajouter une propriété manuellement, sélectionnez l'onglet **Manuel** et définissez les détails suivants :
        * Nom
        * Type de données
        * Propriété
        * Unité de mesure (optionnel)
        * Nombre de décimales (optionnel)

    * Pour créer une propriété virtuelle, sélectionnez l'onglet **Propriété virtuelle** et définissez les détails suivants :
        * Nom
        * Type de données
        * Propriété
        * Calcul
        * Unité de mesure (optionnel)
        * Nombre de décimales

    * Pour sélectionner des propriétés d'un terminal connecté, choisissez l'onglet **A partir de terminaux connectés**, puis sélectionnez une ou plusieurs propriétés à ajouter au schéma. Les propriétés sélectionnées sont ajoutées et le nom de la propriété est affecté à la description.

4. Cliquez sur **Terminer** pour créer les propriétés.

## Etape 5 : créez des règles et des actions

Les règles sont des points de décision basés sur une condition qui correspondent aux données de terminal en temps réel avec des valeurs de seuil prédéfinies ou d'autres données de propriété afin de déclencher une alerte si une condition est remplie. Outre l'alerte qui s'affiche dans le tableau de bord {{site.data.keyword.iot_short_notm}}, vous pouvez ajouter une ou plusieurs actions afin d'exécuter une logique métier lorsqu'une règle est déclenchée.

1. Dans le tableau de bord {{site.data.keyword.iot_short_notm}}, allez à **Règles** et cliquez
sur **Créer une règle Cloud**.

2. Entrez un nom et une description pour la règle, sélectionnez un type de terminal auquel s'applique la règle et cliquez sur **Suivant**.

3. Pour configurer la logique de règle, ajoutez une ou plusieurs conditions IF à utiliser en tant que déclencheurs pour la règle.

    Vous pouvez ajouter les conditions sur des lignes parallèles afin qu'elles s'appliquent selon leur somme logique (OU), ou bien vous pouvez les ajouter dans des colonnes séquentielles pour qu'elles s'appliquent selon leur produit logique (ET). Examinez les exemples suivants :

      * Une règle simple peut déclencher une alerte si une valeur de paramètre est supérieure à une valeur spécifiée :
Condition = `temp_cpu>80`
      * Une règle plus complexe peut être déclenchée lorsque plusieurs seuils sont tous franchis :
Condition = `temp_cpu>60 ET cpu_load>90`

    Pour déclencher une condition comparant deux propriétés ou pour déclencher
plusieurs conditions de propriété combinées séquentiellement par un ET logique,
incluez les points de données déclencheurs dans le même
message de terminal. Si les données sont reçues dans plusieurs messages, les conditions censées être évaluées ensemble seront évaluées séparément et les actions prévues ne seront pas déclenchées.
    {: tip}

4. Configurez les exigences de déclenchement conditionnel pour votre règle.

    Pour contrôler le nombre d'alertes déclenchées pour une règle pendant une période donnée, vous pouvez utiliser des exigences de déclenchement conditionnel. Le déclenchement conditionnel agit sur n'importe quelle condition dans la règle. Par
exemple, si une règle a cinq conditions parallèles liées par l'opérateur OU, chaque fois que l'une d'entre elles
se vérifie, elle est décomptée du nombre de
déclenchements conditionnels.

      1. Dans l'éditeur de règles, cliquez sur le lien par défaut
**Déclencher chaque fois que les conditions sont remplies** pour
ouvrir la boîte de dialogue Définir une exigence de fréquence.
      2. Sélectionnez et configurez le déclencheur conditionnel que vous souhaitez utiliser dans la règle.

        * Déclencher chaque fois que les conditions sont remplies
        * Déclencher si des conditions sont remplies N fois en M _unité de temps_

5. Créez ou sélectionnez une ou plusieurs actions qui se déclenchent si les conditions de règle sont remplies.

    Par exemple, une action pourrait consister à envoyer un e-mail ou à poster un webhook.

6. Optionnel : sélectionnez une priorité d'alerte pour la règle.

    La priorité sert à classer les alertes affichées dans **Tableaux > Analyse centrée sur la règle**. La priorité par défaut est Low (faible).

7. Cliquez sur **Sauvegarder** pour sauvegarder la règle sans l'activer ou cliquez sur **Activer** pour sauvegarder la règle et l'activer.

    Lorsque vous activez la règle, si ses conditions viennent à se vérifier, une alerte est ajoutée au tableau **Analyse centrée sur la règle** et toute action associée à la règle est exécutée.

## Etapes suivantes

Etendez les fonctions d'analyse de données en créant et en connectant vos propres applications afin qu'elles consomment les données temps réel et historiques des terminaux.

  * Consultez les [bibliothèques client](/docs/services/IoT/iot_platform_client_lib.html) en quête d'outils de construction du
code pour l'intégration et la connexion de vos terminaux et applis.

  * Explorez les [recettes Watson IoT Platform ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} pour d'autres
tutoriels expliquant comment incorporer des analyses avancées dans vos terminaux connectés et applications.
