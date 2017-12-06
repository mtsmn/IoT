---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Gestion des risques et de la sécurité
{: #RM_security}

Vous pouvez améliorer la sécurité pour activer la création, l'application et la génération de rapports sur la sécurité de connexion des terminaux. Avec cette sécurité avancée, les certificats et l'authentification TLS (Transport Layer
security) sont utilisés, en plus des ID utilisateur et des jetons utilisés par
{{site.data.keyword.iot_short_notm}}, pour déterminer comment et où les terminaux
se connectent à la plateforme.

## Certificats
{: #certificates}

Lorsque l'utilisation de certificats est en vigueur, pendant la communication entre les terminaux
et le serveur, les terminaux qui n'ont pas de certificat valide
configuré dans leurs paramètres de sécurité se voient refuser l'accès, même s'ils
utilisent un ID utilisateur et un mot de passe valides.

Pour configurer les certificats et l'accès au serveur par les terminaux,
l'opérateur du système enregistre les certificats de l'autorité de certification (AC) associée et,
en option, les certificats des serveurs de messagerie dans Watson IoT Platform.
Pour configurer les certificats de client et l'accès au serveur par les terminaux, l'opérateur du
système importe les certificats de l'AC associée et les certificats des serveurs de messagerie dans la {{site.data.keyword.iot_short_notm}}. L'analyste de sécurité configure ensuite les politiques de sécurité de connexion par défaut entre les terminaux et la plateforme. L'analyste peut ajouter différentes politiques pour différents types de terminaux.

Pour plus d'informations sur la configuration des certificats, voir [Configuration des certificats](set_up_certificates.html).

## Paramètres de sécurité
{: #connect_policy}

Les paramètres de sécurité, qui comprennent l'utilisation de politiques de connexion, de certificats d'AC et de certificats
de serveur de messagerie, déterminent de quelle manière les terminaux se connectent à la plateforme. Vous pouvez configurer des politiques de connexion par défaut pour tous les types de
terminaux et créer des configurations personnalisées pour des types de terminaux
spécifiques. La
politique peut être définie pour autoriser les connexions non chiffrées,
pour imposer l'emploi de connexions sécurisées par TLS et pour
permettre aux terminaux de s'authentifier avec un certificat côté client.

Pour plus d'informations sur la configuration des politiques de sécurité de connexion, consultez [Configuration des politiques de sécurité](set_up_policies.html).

Les politiques de liste noire et de liste blanche permettent de contrôler les lieux
à partir desquels les terminaux sont autorisés à se connecter au compte de
l'organisation. Une liste noire identifie l'ensemble des adresses IP, CIDR ou pays auxquels l'accès au
serveur est refusé. Une liste blanche donne un accès explicite à des adresses
IP spécifiques.

Pour plus d'informations sur la configuration des politiques de liste noire et de liste blanche, voir [Configuration des listes noires et des listes blanches](set_up_policies.html#config_black_white).

## Plans d'organisation et politiques de sécurité
Les politiques de sécurité avancée permettent aux organisations de déterminer la façon dont elles souhaitent que les terminaux se connectent
et s'authentifient auprès de la plateforme. Elles peuvent à cet effet utiliser des politiques de connexion et des politiques de liste noire et de liste blanche. Les options de politique de sécurité dont dispose une organisation dépendent du
type de plan de cette dernière :

**Plan standard :**
- Les opérateurs système peuvent configurer des politiques de connexion à l'aide des options suivantes :
    - TLS optionnel
    - TLS avec authentification par jeton
    - TLS avec authentification par jeton et par certificat de client

**Plan de sécurité avancée ou plan léger :**
- Les opérateurs système peuvent configurer des politiques de connexion à l'aide des options suivantes :
    - TLS optionnel
    - TLS avec authentification par jeton
    - TLS avec authentification par certificat de client
    - TLS avec authentification par jeton et par certificat de client
    - TLS avec certificat de client ou jeton
- Les opérateurs système peuvent configurer des listes noires ou des listes blanches.

## Tableau de bord et rapports de gestion des risques et de la sécurité
{: #dashboard}

L'opérateur du système et l'analyste de sécurité peuvent utiliser le tableau de bord de Gestion des risques et de la sécurité pour
consulter l'état global de sécurité. Les cartes du tableau de bord peuvent leur fournir une vue d'ensemble complète de la conformité, ainsi que l'état de connexion des terminaux.

Les cartes de tableau de bord suivantes sont disponibles pour l'analyse du risque et de la conformité :
 - **Conformité à la sécurité de connexion** - Affiche le niveau de conformité des terminaux connectés au serveur.
 - **Conformité liste noire/liste blanche** - Affiche le niveau de conformité des terminaux en fonction de la politique de liste noire ou liste blanche active.
 - **Conformité globale à la politique** (bêta) - Affiche le niveau global de conformité à toutes les politiques en place.
 - **Violations de la politique** (bêta) - Affiche les violations de chaque politique.

**Important** : Les cartes **Conformité globale à la politique** et
**Violations de la politique** ne sont disponibles que dans le cadre d'un programme bêta limité. Pour en bénéficier, activez les **Fonctions expérimentales** sur la
page **Paramètres**.

### Rapports détaillés sur les politiques (bêta)
{: #drill_down}

**Important** : La fonction Rapports détaillés n'est disponible que dans le cadre d'un programme bêta limité. Pour en bénéficier, activez les **Fonctions expérimentales** sur la
page **Paramètres**.

Avec la fonctionnalité bêta, l'opérateur du système peut approfondir son examen en visualisant, pour chaque rapport,
les détails spécifiques des terminaux et identifier ceux qui sont conformes aux politiques et ceux qui sont en violation de celles-ci.

Vous pouvez accéder aux rapports détaillés à partir des cartes du tableau de bord, à partir de la page
**Politiques** sur laquelle sont listées toutes les politiques de sécurité et à partir
de la page des détails de chaque politique. Vous pouvez descendre à trois niveaux de détails dans les rapports :
1. Le premier niveau dresse la liste de toutes les politiques du type sélectionné. Par exemple, la politique de connexion par
défaut et les éventuelles politiques de connexion personnalisées.
2. Le deuxième niveau montre les terminaux non conformes à la politique, avec la cause de la violation.
3. Le troisième niveau présente les détails relatifs à tout terminal non conforme à la politique.
