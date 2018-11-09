---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Configuration des politiques de sécurité
{: #set_up_policies.md}

Lorsqu'un plan de sécurité avancée est utilisé pour une organisation, un analyste de sécurité peut configurer des politiques de sécurité de connexion et des listes noires ou des listes blanches. Lorsqu'un plan standard est utilisé, l'analyste bénéficie d'un nombre plus restreint d'options.
Il ne peut pas non plus configurer de listes noires ni de listes blanches.

Pour des informations sur l'utilisation des API
de gestion des politiques, consultez
[IBM Watson IoT Platform Risk Management APIs ![Icône de lien externe](../../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/riskmgmt.html){: new_window}.

## Configuration des politiques sécurité de connexion pour une sécurité avancée
{: #config_connect}

Vous pouvez définir le niveau de sécurité par défaut qui est appliqué à tous les terminaux. Vous pouvez ensuite ajouter des paramètres de sécurité personnalisés pour des terminaux spécifiques.

1. Sur la page **Politiques**, cliquez sur **Configurer** en regard de **Sécurité de connexion**.
2. Sous **Sécurité d connexion par défaut**, sélectionnez le niveau de sécurité de connexion par défaut dans la liste déroulante. La valeur que vous sélectionnez ici s'applique à tous les terminaux, à l'exception des terminaux dotés de paramètres de connexion personnalisés. Ces politiques affectent la façon dont les terminaux se connectent au serveur. Elles ne changent
aucun réglage sur le terminal même et n'envoient aucun message à celui-ci. Vous pouvez sélectionner l'un des niveaux de sécurité suivants comme niveau par défaut :
    - TLS optionnel
    - TLS avec authentification par jeton
    - TLS avec authentification par certificat de client
    - TLS avec authentification par jeton et par certificat de client
    - TLS avec certificat de client ou jeton
3. Si nécessaire, cliquez sur **Ajouter une connexion personnalisée** et sélectionnez les types de terminal et les niveaux de sécurité personnalisés.
3. Cliquez sur **Actualiser la conformité**. En fonction du
niveau de sécurité sélectionné, le tableau actualisé indique le nombre de terminaux
concernés et le niveau de conformité prévu au niveau de sécurité défini.
4. Cliquez sur **Sauvegarder**.

**Remarque :** Vous pouvez également accéder aux paramètres de sécurité de connexion sur la page **Général** sous **Paramètres**. Cliquez sur **Ouvrir la politique de sécurité de connexion**.

## Configuration des politiques de connexion pour une sécurité standard
{: #config_connect_standard}

Pour les organisations qui utilisent une sécurité standard, vous modifiez les paramètres de sécurité sur la page **Général** sous **Paramètres**. Vous pouvez définir le niveau de sécurité par défaut qui est appliqué à tous les terminaux.

1. Sous **Paramètres**, sélectionnez **Général**.
2. Sous **Sécurité de connexion**, sélectionnez le niveau de sécurité de connexion par défaut dans la liste déroulante. La valeur que vous sélectionnez ici s'applique à tous les terminaux. Ces politiques affectent la façon dont les terminaux se connectent au serveur. Elles ne changent
aucun réglage sur le terminal même et n'envoient aucun message à celui-ci. Vous pouvez sélectionner l'un des niveaux de sécurité suivants comme niveau par défaut :
    - TLS optionnel
    - TLS avec authentification par jeton
    - TLS avec authentification par jeton et par certificat de client
4. Cliquez sur **Sauvegarder**.

## Configuration des listes noires et des listes blanches
{: #config_black_white}

Les organisations utilisant une sécurité avancée peuvent empêcher l'accès au serveur par certains terminaux en
mettant en place une liste noire. Elles peuvent sinon opter pour une liste blanche désignant spécifiquement
les terminaux autorisés à accéder au serveur. Vous pouvez utiliser soit une liste noire, soit une liste blanche, mais pas les deux ensemble.

### Configuration d'une liste noire
{: #config_blacklist}

1. Sur la page **Politiques** de sécurité, dans la section **Liste noire**, cliquez sur **Configurer**.
2. Sur la page **Liste noire**, cliquez sur **Ajouter à la liste noire**.
3. Dans la fenêtre **Ajouter à la liste noire**, effectuez l'une des opérations suivantes :
    - Dans l'onglet **Adresse IP/plage**, entrez les adresses IP que vous souhaitez bloquer ou les plages d'adresses IP pour plusieurs terminaux consécutifs.
    - Dans l'onglet **CIDR**, entrez un bloc noté Classless Inter-Domain Routing (CIDR).
    - Dans l'onglet **Pays**, entrez ou sélectionnez les pays dont vous souhaitez bloquer tous les terminaux.
4. Dans la fenêtre **Ajouter à la liste noire**, cliquez sur **Sauvegarder**.
5. Consultez la liste des terminaux bloqués. Aucun des terminaux figurant dans la liste (selon leur adresse IP, routage CIDR ou pays) ne pourra se connecter au
serveur {{site.data.keyword.iot_short_notm}}.
6. Cliquez sur **Sauvegarder**.
7. Activez la liste noire. Si une liste blanche était activée, elle est désactivée.

### Configuration d'une liste blanche
{: #config_whitelist}

1. Sur la page **Politiques** de sécurité, cliquez sur **Configurer** en regard de **Liste blanche**.
2. Sur la page **Liste blanche**, cliquez sur **Ajouter à la liste blanche**.
3. Dans la fenêtre **Ajouter à la liste blanche**, effectuez l'une des opérations suivantes :
    - Dans l'onglet **Adresse IP/plage**, entrez les adresses IP auxquelles vous souhaitez accorder l'accès ou les plages d'adresses IP pour plusieurs terminaux consécutifs.
    - Dans l'onglet **CIDR**, entrez un bloc noté Classless Inter-Domain Routing (CIDR).
    - Dans l'onglet **Pays**, entrez ou sélectionnez les pays dont vous souhaitez autoriser l'accès de tous les terminaux.
4. Dans la fenêtre **Ajouter à la liste blanche**, cliquez sur **Sauvegarder**.
5. Consultez la liste des terminaux autorisés. Tous les terminaux figurant dans la liste (selon leur adresse IP, routage CIDR ou pays) pourront se connecter au
serveur {{site.data.keyword.iot_short_notm}}.
6. Cliquez sur **Sauvegarder**.
7. Activez la liste blanche. Si une liste noire était activée, elle est désactivée.
