---

copyright:
years: 2018
lastupdated: "2018-08-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Authentification via l'ID d'application dans Watson IoT Platform (bêta)
{: #app_id}

L'ID d'application est utilisé pour authentifier les utilisateurs qui ont besoin d'accéder à des applications qui sont hébergées dans IBM Cloud. Ces utilisateurs accèdent au service à travers les applications mobiles ou Web fournies par le service.
{: shortdesc}

**Important :** La fonction d'authentification et d'autorisation via l'ID d'application de {{site.data.keyword.iot_short_notm}} est uniquement disponible dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} prend également en charge l'authentification des utilisateurs via Cloud IAM. Cloud IAM est intégré à IBM Cloud et est utilisé pour authentifier et autoriser les administrateurs et les développeurs qui ont besoin de configurer et de gérer leurs services IBM. Pour en savoir plus sur Cloud IAM, voir [Authentification et autorisation dans Cloud IAM pour Watson IoT Platform](cloud_iam.html#cloud_iam).

Les utilisateurs de l'ID d'application ne réalisent généralement pas d'activités administratives ou de développement sur un service cloud et ils ne peuvent pas se connecter au tableau de bord Web de {{site.data.keyword.iot_short_notm}}. Seuls les utilisateurs de Cloud IAM peuvent se connecter au tableau de bord.

Les API de {{site.data.keyword.iot_short_notm}} prennent en charge l'authentification des utilisateurs à partir du service ID d'application IBM Cloud. Vous pouvez configurer votre organisation {{site.data.keyword.iot_short_notm}} pour utiliser une instance du service ID d'application puis ajouter vos utilisateurs du service ID d'application à votre organisation. Votre application authentifie ces utilisateurs avec l'ID d'application qui peut ensuite être utilisé pour appeler les API de {{site.data.keyword.iot_short_notm}}. Pour en savoir plus sur le service ID d'application IBM Cloud, voir le [tutoriel d'initiation à l'ID d'application IBM Cloud![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}.

La prise en charge actuelle du service ID d'application est pour les API REST hors messagerie, ainsi que pour les API de configuration et de gestion des terminaux, des utilisateurs et autres fonctionnalités de {{site.data.keyword.iot_short_notm}}. L'ID d'application ne peut pas être utilisée pour publier et s'abonner à des messages.

L'ID d'application est généralement utilisé pour authentifier les API REST qui sont appelés pour le compte d'un utilisateur non administrateur. Un développeur peut créer une application qui utilise un jeton d'ID d'application pour appeler une API REST pour le compte d'un utilisateur enregistré, comme un technicien de maintenance, qui n'a pas d'accès administrateur. L'avantage d'utiliser l'ID d'application pour authentifier les utilisateurs non administrateurs plutôt que d'utiliser un jeton d'authentification unique réside dans le fait que la trace d'audit dans {{site.data.keyword.iot_short_notm}} montre l'utilisateur ayant appelé les API REST au lieu d'un jeton d'authentification partagé. L'ID d'application est également utile pour authentifier les utilisateurs non administrateurs qui ont besoin d'utiliser l'interface de ligne de commande (CLI) de {{site.data.keyword.iot_short_notm}}.

**Important :** L'appel des API REST de {{site.data.keyword.iot_short_notm}} doit être fait en utilisant une application d'utilisateur et non directement à partir d'un navigateur ou d'une application mobile. L'application d'utilisateur peut fournir un niveau supplémentaire de contrôle d'accès pour déterminer à quelles fonctions de terminal un utilisateur spécifique est autoriser à accéder, de même que pour la validation des messages MQTT envoyés aux terminaux. L'image suivante montre ce flux d'appel :

![Appel de l'API REST à l'aide d'une application d'utilisateur](images/app_id_user_app.PNG)

Les utilisateurs de l'ID d'application doivent être enregistrés dans {{site.data.keyword.iot_short_notm}} au moyen du même processus que celui qui est utilisé par les utilisateurs de Cloud IAM et les deux apparaissent dans l'interface du tableau de bord.

Puisque les utilisateurs d'ID d'application ne sont généralement pas des administrateurs, il est important de prendre en considération à quelles ressources et commandes ils doivent être autorisés à accéder, puis définir en conséquent les paramètres de contrôle d'accès au niveau de la ressource dans {{site.data.keyword.iot_short_notm}}. Dans la plupart des cas, les utilisateurs de l'ID d'application obtiennent un rôle personnalisé qui est plus restrictif que les rôles définis par défaut d'une organisation {{site.data.keyword.iot_short_notm}}. Pour en savoir plus sur les rôles, voir [Rôles d'utilisateur, d'application et de passerelle](../../roles_index.html#user-application-and-gateway-roles).

Par défaut, les utilisateurs d'ID d'application peuvent accéder à tous les terminaux. Si vous souhaitez qu'un utilisateur d'ID d'application soit uniquement capable de gérer une liste limitée de terminaux, affectez-leur des groupes de ressources.

**Remarque :** {{site.data.keyword.iot_short_notm}} limite le nombre total d'utilisateurs enregistrés à 1000.

## Configuration d'un service d'ID d'application
{: #set_up_app_id}

Avant de configurer un service ID d'application, vous devez ajouter une instance du service ID d'application IBM Cloud et configurer des fournisseurs d'identité. Si vous n'avez pas encore d'instance d'ID d'application, vous pouvez en obtenir une dans le [Catalogue des services d'IBM Cloud![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/catalog){: new_window}.

Après avoir configuré le service ID d'application, vous devez obtenir les données d'identification du service pour configurer l'ID d'application en tant que fournisseur pour votre organisation {{site.data.keyword.iot_short_notm}}. Vous pouvez obtenir ou créer des données d'identification de service dans l'interface utilisateur du service ID d'application. 

1. Dans le tableau de bord d'IBM Cloud, sélectionnez votre service ID d'application et cliquez sur **Données d'identification pour le service**.
2. Cliquez sur **Afficher les données d'identification** pour localiser le `tenantId`, le `clientId` et le `secret` dans les données d'identification. Ces valeurs sont requises pour configurer l'ID d'application par la suite. L'émetteur sera le nom d'hôte de `oauthServerUrl`, par exemple `appid-oauth.ng.bluemix.net`.

L'image suivante montre un exemple de la manière dont vous pouvez extraire les données d'identification :

![Extraction des données d'identification d'ID d'application](images/app_id_credentials.PNG "Exemple d'extraction des données d'identification d'ID d'application")


## Configuration d'ID d'application dans {{site.data.keyword.iot_short_notm}}
{: #config_app_id}

Vous devez configurer ID d'application avant de pouvoir utiliser le jeton d'ID d'application à des fins d'autorisation. Pour créer la configuration d'ID d'application, utilisez l'API `POST /api/v0002/authentication/providers`, où `tenant_Id` et `issuer` sont obtenus à l'étape 2 de [Configuration d'un service ID d'application](#set_up_app_id) :

Corps de demande :

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

Réponse à la demande : 200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## Ajout d'utilisateurs à {{site.data.keyword.iot_short_notm}}
{: #users_app_id}

Vous devez ajouter des utilisateurs et leur donner des autorisations en fonction de leurs rôles. Pour créer des utilisateurs, utilisez l'API `POST /api/v0002/authorization/users` avec les données suivantes : 

 - `issuer` est obtenu à l'étape 2 de [Configuration d'un service d'ID d'application](#set_up_app_id).
 - `uniqueSecurityName` est complété avec les détails de `appIdConfigName`, `amr` et le courrier électronique de l'utilisateur qui sont configurés dans les fournisseurs d'identité du service ID d'application.
 - `appIdConfigName` est le nom utilisé dans [Configuration d'ID d'application](##config_app_id), par exemple “TestAppIdConfigName”.
 - `amr` est la référence de la méthode d'authentification, qui est le fournisseur d'identité utilisé dans le service ID d'application. Le service ID d'application prend en charge les fournisseurs d'identité suivants :

	 - Cloud Directory
	 - SAML 2.0 Federation
	 - Facebook
	 - Google

	 En fonction du fournisseur d'identité utilisé dans le service ID d'application, `amr` peut être `cloud_directory`, `saml`, `facebook` ou `google`.

Dans l'exemple suivant, Cloud Directory est utilisé :

Corps de demande :

```
{
    "uniqueSecurityName": "TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
    "PD_ADMIN_USER"
  ]
}
```

Réponse à la demande : 200

```
{
    "uniqueSecurityName": “TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
        "PD_ADMIN_USER"
    ]
}
```

## Génération d'un jeton d'ID d'application
{: #token_app_id}

Après avoir configuré le service ID d'application et ajouté des utilisateurs, votre application peut commencer à générer des jetons d'ID d'application pour vos utilisateurs et les utiliser pour appeler les API de plateforme. Pour en savoir plus sur la mise en route du service ID d'application, consultez la vidéo suivante : [IBM Bluemix App ID ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}.

Un utilisateur peut utiliser le point de terminaison du service ID d'application pour se connecter et extraire son jeton d'ID d'application en publiant les exemples suivants.

### Exemple HTTP de génération d'un jeton d'ID d'application

Pour générer un jeton d'ID d'application, utilisez l'API suivante en vous authentifiant auprès d'IBM Cloud avec les données d'identification du service :

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

Où `tenantId`, `issuer`, `clientId` et `secret` sont obtenu à l'étape 2 de la section [Configuration d'un service ID d'application](#set_up_app_id).

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

Corps de demande pour grant_type=password :

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

Corps de demande pour grant_type= authorization_code :

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

Réponse à la demande :

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Exemple de génération d'un jeton d'ID d'application avec Curl

Le fragment suivant montre un exemple de génération d'un jeton d'ID d'application avec CURL :

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

où `username` et `password` sont définis pour l'utilisateur dans App ID.

Réponse à la demande :

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Exemple de génération d'un jeton d'ID d'application avec Swagger

Pour utiliser Swagger pour générer un jeton d'ID d'application, utilisez l'[exemple Swagger ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}.

Entrez tous les détails dans les paramètres Swagger puis cliquez sur **Essayez** à la fin de la méthode `/oauth/v3/{tenantId}/token`.

La réponse du point de terminaison du jeton contient le jeton d'accès et le jeton d'ID, le jeton d'actualisation facultatif et des informations relatives à la date d'expiration.

## Utilisation du jeton d'ID d'application
{: #use_app_id}

Une fois que le jeton d'ID d'application a été créé, votre application peut commencer à l'utiliser pour appeler les API de plateforme. Le jeton d'ID d'application est la propriété `id_token` dans la réponse où il a été généré dans [Génération d'un jeton d'ID d'application](#token_app_id). Lorsque le jeton d'ID d'application expire, votre application doit générer un nouveau jeton pour continuer à appeler les API de plateforme.

Les exemples suivants montrent comment utiliser le jeton d'ID d'application lorsque vous appelez des API.

###	Exemple d'utilisation d'un jeton d'ID d'application dans HTTP

`GET https://org.domain/api/v0002/bulk/devices`

Paramètres d'entrée  |	Valeurs
----------------- | -----------
En-têtes	|	Content-Type: application/json<br>Authorization: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### Exemple d'utilisation d'un jeton d'ID d'application dans Curl

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
