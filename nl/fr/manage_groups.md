---

copyright:
years: 2018
lastupdated: "2018-06-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Gestion des groupes (bêta)
{: #groups_overview}

Les utilisateurs possédant le rôle d'administrateur peuvent utiliser les groupes {{site.data.keyword.iot_full}} pour accorder à des membres et à des clés d'API un accès à des terminaux spécifiques. Après avoir créé un groupe et y avoir ajouté des terminaux, ajoutez des membres et des clés d'API au groupe et affectez-leur des rôles à l'intérieur du groupe. La combinaison de rôles et de groupes détermine à quels terminaux les utilisateurs et les clés d'API peuvent accéder, et les actions qu'ils peuvent réaliser sur les terminaux.
{: shortdesc}

Vous pouvez gérer ces groupes à l'aide de l'interface utilisateur du tableau de bord {{site.data.keyword.iot_short_notm}} ou à l'aide des API de contrôle d'accès {{site.data.keyword.iot_short_notm}}.

Pour obtenir des renseignements supplémentaires sur la gestion des rôles, voir [Gestion des rôles d'utilisateurs](managing_user_roles.html#managing-user-roles).

**Important :** La fonction de de groupes de l'interface utilisateur {{site.data.keyword.iot_short_notm}} est disponible uniquement dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Pour plus d'informations sur le contrôle d'accès et les groupes et pour des instructions d'utilisation des API de contrôle d'accès {{site.data.keyword.iot_short_notm}} pour gérer des groupes, voir [Présentation du contrôle d'accès au niveau de la ressource](reference/rlac_overview.html#RLAC_overview).

## Ajout de groupes

**Remarque :** Pour commencer à ajouter des groupes, activez la fonction **Groupes** sur la page **Paramètres**. 

1. Depuis votre tableau de bord {{site.data.keyword.iot_short_notm}}, sélectionnez **Gestion des accès** dans la barre de navigation de gauche.
2. Sélectionnez l'onglet **Groupes** pour afficher la liste des groupes.
3. Cliquez sur **Ajouter un groupe**.
4. Dans la fenêtre **Ajouter un groupe**, entrez le nom du groupe et, éventuellement, une description.
5. Cliquez sur **Suivant**.
6. Cliquez sur **Ajouter des terminaux à un groupe**.
7. Sélectionnez les terminaux à ajouter au groupe, puis cliquez sur **Terminé**.
8. Cliquez sur **Terminer** pour créer le groupe.
Vous pouvez éditer le groupe en ajoutant d'autres terminaux ou en en supprimant et ajouter d'autres groupes.

