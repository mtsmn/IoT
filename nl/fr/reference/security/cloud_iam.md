---

copyright:
  years: 2018
lastupdated: "2018-07-16"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Authentification et autorisation de Cloud IAM pour {{site.data.keyword.iot_short_notm}} (bêta)
{: #cloud_iam}

Les API {{site.data.keyword.iot_full}} prennent en charge l'authentification et l'autorisation des utilisateurs via la gestion d'identité et d'accès (IAM).

**Important :** La fonction d'authentification et d'autorisation Cloud IAM pour {{site.data.keyword.iot_short_notm}} est uniquement disponible dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Cloud IAM est intégré à IBM Cloud et est utilisé pour authentifier et autoriser les administrateurs et les développeurs qui ont besoin de configurer et de gérer leurs services IBM. Les utilisateurs nécessitant un accès à l'interface utilisateur de Watson IoT Platform UI sont authentifiés avec IBM Cloud IAM. La source d'identité de Cloud IAM peut être des utilisateurs IBM ID enregistrés ou un service de répertoire de clients prenant en charge SAML.  

{{site.data.keyword.iot_short_notm}} prend également en charge l'authentification des utilisateurs via l'ID d'application. L'ID d'application est utilisé pour authentifier les utilisateurs qui ont besoin d'accéder à des applications qui sont hébergées dans IBM Cloud. Les utilisateurs de l'ID d'application ne réalisent généralement pas d'activités administratives ou de développement sur un service cloud. Pour en savoir plus, voir [Authentification via l'ID d'application dans Watson IoT Platform](app_id.html#app_id).

IAM permet aux utilisateurs d'obtenir un jeton OAuth IAM et de l'utiliser pour authentifier les appels d'API. Par exemple, un utilisateur avec l'interface de ligne de commande {{site.data.keyword.containershort_notm}} installée peut utiliser la commande suivante :

`bx iam oauth-tokens`

La commande renvoie le jeton IAM de l'utilisateur connecté dans le format suivant :

`IAM Token: Bearer <some token>`

Un utilisateur peut également utiliser le point de terminaison du jeton IAM pour se connecter et extraire son jeton IAM, comme illustré dans l'exemple suivant :
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

Cet exemple d'API utilise une clé d'API IAM pour demander le jeton IAM et renvoie `Bearer <token>`.

Ensuite, lorsqu'un utilisateur appelle une API {{site.data.keyword.iot_short_notm}}, il peut fournir l'en-tête d'autorisation `Authorization: Bearer <token>` sur ses appels d'API.

## Génération d'un jeton IAM
{: #iam_generate}

Vous pouvez choisir comment automatiser la création du jeton IAM suivant la manière dont vous vous authentifiez auprès d'IBM Cloud et le type d'ID IBM Cloud que vous utilisez.

Si vous utilisez un ID non fédéré, vous pouvez choisir l'une des options suivantes pour créer un jeton IAM :
 - **Nom d'utilisateur et mot de passe IBM Cloud :** Vous pouvez créer un jeton pour automatiser entièrement la création de votre jeton d'accès IAM.
 - **Générer une clé d'API IBM Cloud :** Utilisez les clés d'API IBM Cloud qui dépendent du compte IBM Cloud pour lequel elles sont générées. Vous ne pouvez pas combiner votre clé d'API IBM Cloud à un ID de compte différent dans le même jeton IAM. Pour accéder à des clusters créés avec un compte autre que celui sur lequel votre clé d'API IBM Cloud est basée, vous devez vous connecter au compte pour générer une nouvelle clé d'API. 

Si vous utilisez un ID fédéré, vous pouvez choisir l'une des options suivantes pour créer un jeton IAM :
 - **Générer une clé d'API IBM Cloud :** Les clés d'API IBM Cloud dépendent du compte IBM Cloud pour lequel elles sont générées. Vous ne pouvez pas combiner votre clé d'API IBM Cloud à un ID de compte différent dans le même jeton IAM. Pour accéder à des clusters créés avec un compte autre que celui sur lequel votre clé d'API IBM Cloud est basée, vous devez vous connecter au compte pour générer une nouvelle clé d'API. 
 - **Utiliser un code d'accès à un usage **:  Si vous vous authentifiez avec IBM Cloud à l'aide d'un code d'accès à usage unique, vous ne pouvez pas entièrement automatiser la création de votre jeton IAM car l'extraction de votre code d'accès à usage unique nécessite une interaction manuelle avec votre navigateur Web. Pour automatiser complètement la création de votre jeton IAM, vous devez créer une clé d'API IBM Cloud à la place. 

Lorsque vous créez votre jeton d'accès, les informations de corps incluses dans votre demande varient en fonction de la méthode d'authentification IBM Cloud que vous utilisez. Remplacez les valeurs comme ceci :
- `<my_username>` = votre nom d'utilisateur IBM Cloud.
- `<my_password>` = votre mot de passe IBM Cloud.
- `<my_api_key>` = votre clé d'API IBM Cloud.
- `<my_passcode>` = votre code d'accès à usage unique IBM Cloud. Exécutez `bx login --sso` et suivez les instructions de la sortie de l'interface de ligne de commande pour extraire votre code d'accès à usage unique à l'aide de votre navigateur Web.

Exemple :
`POST https://iam.<region>.bluemix.net/oidc/token`

Pour spécifier une région IBM Cloud, passez en revue les abréviations de région utilisées dans les points de terminaison d'API. Voir [IBM Cloud region API endpoints [Icône de lien externe](../../icons/launch-glyph.svg)](https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions) pour plus d'informations.

Paramètre d'entrée  | Valeurs
---------------- | -----------
En-tête 	| Content-Type:application/x-www-form-urlencoded<br>Authorization: Basic Yng6Yng=<br>Remarque : vous obtenez Yng6Yng=, l'autorisation avec codage d'URL pour le nom d'utilisateur bx et le mot de passe bx.
Corps du nom d'utilisateur et du mot de passe IBM Cloud |	grant_type: password<br>response_type: cloud_iam, uaa<br>username: `<my_username>`<br>password: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Remarque : ajoutez la clé uaa_client_secret sans aucun valeur spécifiée.
Corps des clés d'API IBM Cloud 	|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Remarque : ajoutez la clé uaa_client_secret sans aucune valeur spécifiée.
Corps du code d'accès à usage unique IBM Cloud |	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Remarque : ajouter la clé uaa_client_secret dans aucune valeur spécifiée.

Exemple de sortie d'API :

```
{
"access_token": "<iam_token>",
"refresh_token": "<iam_refresh_token>",
"uaa_token": "<uaa_token>",
"uaa_refresh_token": "<uaa_refresh_token>",
"token_type": "Bearer",
"expires_in": 3600,
"expiration": 1493747503
}
```
Le jeton IAM figure dans la zone **access_token** de la sortie de votre API. 

L'exemple suivant montre comment utiliser le jeton IAM lorsque vous appelez des API.

```
GET https://org.domain/api/v0002/bulk/devices
```

Paramètres d'entrée  |	Valeurs
----------------- | -----------
En-têtes	|	Content-Type: application/json<br>Authorization: bearer `<iam_token>`
