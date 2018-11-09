---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configuration de {{site.data.keyword.iot_short_notm}} Edge (prévisualisation)
{: #edge_configure}

**Important :** la fonction {{site.data.keyword.iot_full}} Edge n'est disponible que dans le cadre d'un programme de prévisualisation limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Avant de commencer à recevoir des données de terminaux connectés à votre noeud Edge, vous devez connecter la passerelle à {{site.data.keyword.iot_short_notm}}. Connecter une passerelle à {{site.data.keyword.iot_short_notm}} implique de créer un type de terminal de passerelle et d'enregistrer la passerelle auprès de {{site.data.keyword.iot_short_notm}}. Vous pouvez ensuite utiliser les informations d'enregistrement pour connecter le terminal de passerelle à {{site.data.keyword.iot_short_notm}}.

## Flux de configuration de haut niveau

1. [Créez une organisation](#edge_org) dans l'environnement de test de prévisualisation de {{site.data.keyword.iot_short_notm}} Edge.
2. [Activez la prévisualisation de {{site.data.keyword.iot_short_notm}} ](#edge_enable).
3. Configurez votre organisation pour utiliser des noeuds de passerelle Edge avec {{site.data.keyword.iot_short_notm}} en [créant un type de passerelle avec Edge activé](#edge_gw_type), en [ajoutant des terminaux de passerelle](#edge_gw) et en sélectionnant les services Edge à exécuter sur vos noeuds Edge. Vous pouvez [créer des service personnalisés](WIoTP_edge_dev.html#create_service) si nécessaire.
4. [Configurez les services Edge](#config_service).
5. [Installez Edge sur vos terminaux](#edge_install_device).
6. [Gérer et surveiller les services Edge](#monitor_service).

## Créer une organisation
{: #edge_org}

Pour créer une organisation dans l'environnement de test de prévisualisation de {{site.data.keyword.iot_short_notm}} Edge, procédez comme suit :
1. Souscrivez à un compte {{site.data.keyword.Bluemix_notm}} et créez une instance du service {{site.data.keyword.iot_short_notm}} dans votre organisation {{site.data.keyword.Bluemix_notm}}. Vous pouvez créer une instance {{site.data.keyword.iot_short_notm}} directement depuis la page [{{site.data.keyword.iot_short_notm}} dans le catalogue de services IBM Cloud ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}.  
2. Complétez les informations nécessaires pour la mise à disposition de l'organisation IBM Cloud. **Important :** vous devez déployer votre organisation IBM Cloud dans la région **Sud des Etats-Unis** pour pouvoir utiliser la prévisualisation de {{site.data.keyword.iot_short_notm}} Edge.
3. Cliquez sur **Créer**.
4. Dans l'instance de {{site.data.keyword.iot_short_notm}} qui s'ouvre, cliquez sur **Lancer**.
Une nouvelle organisation {{site.data.keyword.iot_short_notm}} est créée. L'ID d'organisation s'affiche au début de l'URL et dans le tableau de bord en dessous de votre nom d'utilisateur. Vous pouvez maintenant activer {{site.data.keyword.iot_short_notm}} Edge pour l'organisation.

## Activer la prévisualisation de {{site.data.keyword.iot_short_notm}} Edge 
{: #edge_enable}

La prévisualisation de {{site.data.keyword.iot_short_notm}} Edge est désactivée par défaut. Pour activer {{site.data.keyword.iot_short_notm}} Edge :

1. Dans le tableau de bord de {{site.data.keyword.iot_short_notm}} de votre organisation, sélectionnez **Paramètres**.
2. Dans la section **Fonctions expérimentales**, activez les fonctions expérimentales, qui incluent la prévisualisation de {{site.data.keyword.iot_short_notm}} Edge.
3. Actualisez votre navigateur pour afficher la nouvelle fonction **Services Edge** et le raccourci dans le menu de navigation du tableau de bord. **Remarque :** si l'icône **Services Edge** n'apparaît pas dans le menu de navigation du tableau de bord, vérifiez que vous avez bien sélectionné **Sud des Etats-Unis** comme emplacement.

Le drapeau est ajouté au stockage du navigateur local.

## Créer un type de passerelle avec les fonctions Edge activées
{: #edge_gw_type}

Chaque passerelle connectée à {{site.data.keyword.iot_short_notm}} doit être associée à un type de passerelle. Les types de passerelle sont des groupes de terminaux qui partagent des caractéristiques communes.

1. Dans le menu de navigation principal du tableau de bord de {{site.data.keyword.iot_short_notm}}, sélectionnez **Terminaux**.
2. Sur l'onglet **types de terminal**, cliquez sur **Ajouter un type de terminal**.
3. Sur l'onglet **Identité** de la page **Ajouter un type**, sélectionnez le type **Passerelle** et entrez un nom de type de passerelle tel que `my_gateway_type` et une description du type de passerelle.
**Important :** Le nom du type de passerelle ne doit pas contenir plus de 36 caractères et peut uniquement contenir :
<ul>
 <li>des caractères alphanumériques (a-z, A-Z, 0-9)</li>
 <li>des traits d'union (-)</li>
 <li>des traits de soulignement (&lowbar;)</li>
 <li>des points (.)</li>
 </ul>
3. Facultatif : entrez les métadonnées et les attributs du type de passerelle.
4. Activez le bouton **Capacités Edge**.
5. Sélectionnez le type d'architecture du type de passerelle. Le type d'architecture doit correspondre à l'architecture de votre terminal. Par exemple, une terminal Odroid s'exécute sur l'architecture ARM64.
6. Cliquez sur **Suivant**.
7. Facultatif : sur l'onglet **Informations sur le terminal**, entrez des détails et des métadonnées supplémentaires en relation avec le type de terminal de passerelle.
8. Sélectionnez les services Edge que vous souhaitez ajouter à vos terminaux Edge. Le service Core IoT est ajouté à tous les terminaux mais vous pouvez ajouter d'autres services du catalogue en effectuant les étapes suivantes :
 1. Cliquez sur **Ajouter des services**.
 2. Dans la fenêtre **Ajouter des services Edge**, sélectionnez la version appropriée des services que vous souhaitez ajouter puis cliquez sur **Terminé**.
9. Dans la fenêtre **Ajouter un type de passerelle**, cliquez sur **Terminé**.
Vous pouvez procéder à l'[ajout des terminaux de passerelle à ce type de passerelle](#edge_gw).

## Ajout de terminaux de passerelle
{: #edge_gw}

1. Dans le menu de navigation principal du tableau de bord de {{site.data.keyword.iot_short_notm}}, sélectionnez **Terminaux**.
2. Cliquez sur **Ajouter un terminal**.
3. Sur l'onglet **Identité**, sélectionnez le type de terminal de passerelle auquel vous ajoutez des terminaux et entrez un ID unique pour le terminal de passerelle que vous ajoutez.
L'ID de terminal permet d'identifier le terminal de passerelle dans le tableau de bord {{site.data.keyword.iot_short_notm}} et représente également un paramètre requis pour la connexion de votre terminal de passerelle à {{site.data.keyword.iot_short_notm}}.
**Important :** L'ID de terminal ne doit pas dépasser 36 caractères et peut uniquement contenir les caractères suivants :
 <ul>
 <li>Caractères alphanumériques (a-z, A-Z, 0-9)</li>
 <li>Traits d'union (-)</li>
 <li>Traits de soulignement (&lowbar;)</li>
 <li>Points (.)</li>
 </ul>
 **Astuce :** Pour les terminaux connectés à un réseau, l'ID de terminal pourrait être par exemple l'adresse MAC du terminal sans aucun deux-points de séparation.
4. Cliquez sur **Suivant**.
5. Faculatif : sur l'onglet **Informations sur le terminal**, entrez des métadonnées et des détails supplémentaires au terminal de passerelle.
6. Cliquez sur **Suivant**.
7. Sur l'onglet **Sécurité** de la page **Ajouter un terminal**, auto-générez un jeton d'authentification ou fournissez votre propre jeton d'authentification pour ce terminal. Si vous choisissez de créer votre propre jeton, assurez-vous qu'il comporte entre 8 et 36 caractères et qu'il contient une combinaison de minuscules et de majuscules, des nombres, un trait d'union, un tiret de soulignement ou un point. Le jeton ne doit pas contenir des séquences de caractères répétés, des mots de dictionnaire, des noms d'utilisateur ni d'autres séquences prédéfinies.
9. Cliquez sur **Suivant**.
10. Cliquez sur **Terminé**.
11. Sur la page d'information du terminal, copiez et sauvegardez les informations suivantes sur le terminal :
 - ID d'organisation, par exemple, `tubo8x`
 - Type de terminal, par exemple, `my_gateway_type`
 - ID de terminal. **Astuce :** Pour les terminaux connectés à un réseau, cela pourrait être par exemple l'adresse MAC sans aucun deux-points de séparation.
 - Méthode d'authentification, par exemple, `token`
 - Jeton d'authentification tel que `PtBVriRqIg4uh)_-Kl`
  Utilisez ces informations pour connecter la passerelle à {{site.data.keyword.iot_short_notm}} et commencer à recevoir des données des terminaux connectés à la passerelle.

## Configuration des services Edge
{: #config_service}

Après avoir créé un nouveau type de terminal avec les fonctionnalités Edge activées, vous pouvez sélectionner différents services pour les installer dans votre noeud Edge.

1. Dans le menu de navigation principal du tableau de bord de {{site.data.keyword.iot_short_notm}}, sélectionnez **Terminaux**.
2. Cliquez sur **Types de terminal**.
3. Sélectionnez le type de terminal Edge que vous souhaitez configurer.
4. Sur l'onglet **Services Edge**, sélectionnez l'icône du crayon pour éditer l'enregistrement.
6. Cliquez sur **Ajouter des services Edge** pour afficher une liste des services qui ont été téléchargés pour cette organisation.
7. Dans la fenêtre **Ajouter des services Edge**, sélectionnez la version appropriée des services que vous souhaitez ajouter puis cliquez sur **Terminé**.
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## Gestion et configuration des services Edge
{: #monitor_service}

Une fois que les noeuds Edge sont installés, {{site.data.keyword.iot_short_notm}} vous permet de surveiller le statut des services qui s'exécutent sur les noeuds Edge.

1.	Dans le menu de navigation principal du tableau de bord de {{site.data.keyword.iot_short_notm}}, sélectionnez **Terminaux**.
2.	Sélectionnez le terminal correspondant à votre noeud Edge
3.	Cliquez sur **Services Edge** pour afficher le statut des services qui sont installés sur le noeud Edge.

## Recherche d'informations supplémentaires
{: #more_info}

Pour plus d'informations sur la connexion de votre passerelle à {{site.data.keyword.iot_short_notm}}, voir [Connectivité MQTT pour les passerelles](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt).

Pour en savoir plus sur {{site.data.keyword.iot_short_notm}}, voir [Initiation à Watson IoT Platform](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp).

Pour en savoir plus sur {{site.data.keyword.iot_short_notm}} Edge, consultez les rubriques suivantes :
- Aperçu de [{{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge.html#edge_overview)
- [Installation de {{site.data.keyword.iot_short_notm}} Edge sur les terminaux Edge](WIoTP_edge_install.html#edge_install_device)
- [Développement pour {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
