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

# Authentifizierung mit App ID für Watson IoT Platform (Beta)
{: #app_id}

Mit App ID können Benutzer authentifiziert werden, die Zugriff auf Anwendungen benötigen, die in IBM Cloud gehostet werden. Diese Benutzer greifen über mobile Anwendungen oder Webanwendungen, die vom Service bereitgestellt werden, auf den Service zu.
{: shortdesc}

**Wichtig:** Die Funktion für Authentifizierung und Berechtigung mit App ID für {{site.data.keyword.iot_short_notm}} steht nur als Bestandteil des eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} unterstützt die Authentifizierung von Benutzern über Cloud IAM. Cloud IAM ist in IBM Cloud integriert und wird zur Authentifizierung und Berechtigung von Benutzern mit Verwaltungsaufgaben und Entwicklern verwendet, die ihre IBM Services konfigurieren und verwalten müssen. Weitere Informationen zu Cloud IAM finden Sie in [Authentifizierung und Berechtigung mit Cloud IAM für Watson IoT Platform](cloud_iam.html#cloud_iam).

Benutzer von App ID führen in der Regel keine Verwaltungs- oder Entwicklungsaktivitäten für einen Cloud-Service aus und können sich nicht beim {{site.data.keyword.iot_short_notm}}-Web-Dashboard anmelden. Nur Cloud IAM-Benutzer können sich beim Dashboard anmelden.

Die {{site.data.keyword.iot_short_notm}}-APIs unterstützen die Authentifizierung von Benutzern über den Service 'IBM Cloud App ID'. Sie können Ihre {{site.data.keyword.iot_short_notm}}-Organisation so konfigurieren, dass eine Instanz von App ID verwendet wird, und anschließend die App ID-Benutzer zu Ihrer Organisation hinzufügen. Ihre Anwendung authentifiziert diese Benutzer mit App ID; der Service kann dann zum Aufrufen der {{site.data.keyword.iot_short_notm}}-APIs verwendet werden kann. Weitere Informationen zum Service 'IBM Cloud App ID' finden Sie im [Lernprogramm zur Einführung in IBM Cloud App ID ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}.

Die aktuelle Unterstützung für App ID bezieht sich auf REST-APIs, die nicht für das Messaging verwendet werden, einschließlich APIs für die Konfiguration und Verwaltung von Geräten, Benutzern und anderen {{site.data.keyword.iot_short_notm}}-Funktionen. App ID kann nicht dazu verwendet werden, Nachrichten zu publizieren und zu abonnieren.

In der Regel verwenden Sie App ID, um REST-APIs zu authentifizieren, die im Namen eines Benutzers ohne Administratorberechtigung aufgerufen werden. Ein Entwickler kann eine Anwendung erstellen, die ein App ID-Token verwendet, um eine REST-API im Namen eines registrierten Benutzers aufzurufen, z. B. eines Wartungstechnikers, der keinen Administratorzugriff hat. Der Vorteil bei der Verwendung von App ID gegenüber eines einzelnen Authentifizierungstoken zur Authentifizierung von Benutzern ohne Administratorberechtigung ist, dass das Prüfprotokoll in {{site.data.keyword.iot_short_notm}} den Benutzer anzeigt, der die REST-APIs aufgerufen hat, und nicht ein gemeinsam genutztes Authentifizierungstoken. App ID ist auch für die Authentifizierung von Benutzern ohne Administratorberechtigung geeignet, die die {{site.data.keyword.iot_short_notm}}-Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden müssen.

**Wichtig:** Der Aufruf von {{site.data.keyword.iot_short_notm}}-REST-APIs sollte mithilfe einer Benutzeranwendung und nicht direkt über einen Browser oder über mobile Apps erfolgen. Die Benutzeranwendung kann eine zusätzliche Ebene der Zugriffssteuerung bereitstellen, um festzustellen, auf welche Gerätefunktionen ein bestimmter Benutzer zugreifen darf, und um die MQTT-Nachrichten zu validieren, die an Geräte gesendet werden. In der folgenden Abbildung sehen Sie diesen Aufrufablauf:

![REST-API-Aufruf mithilfe einer Benutzeranwendung](images/app_id_user_app.PNG)

App ID-Benutzer müssen bei {{site.data.keyword.iot_short_notm}} in der gleichen Art und Weise wie Cloud IAM-Benutzer registriert werden und beide werden in der Dashboardschnittstelle angezeigt.

Da App ID-Benutzer in der Regel keine Administratoren sind, ist es wichtig, die Ressourcen und Befehle für den Zugriff zu berücksichtigen und dann die {{site.data.keyword.iot_short_notm}}-Zugriffskontrolleinstellungen auf Ressourcenebene entsprechend festzulegen. In den meisten Fällen wird den Benutzern von App ID eine angepasste Rolle zugewiesen, die restriktiver ist als die definierten Standardrollen einer {{site.data.keyword.iot_short_notm}}-Organisation. Weitere Informationen zu Rollen finden Sie in [Benutzer-, Anwendungs- und Gateway-Rollen](../../roles_index.html#user-application-and-gateway-roles).

Standardmäßig können Benutzer von App ID auf alle Geräte zugreifen. Wenn ein App ID-Benutzer nur eine beschränkte Liste von Geräten verwalten darf, weisen Sie den entsprechenden App ID-Benutzern Ressourcengruppen zu.

**Hinweis:** {{site.data.keyword.iot_short_notm}} begrenzt die Gesamtzahl der registrierten Benutzer auf 1.000.

## App ID-Service einrichten
{: #set_up_app_id}

Bevor Sie einen App ID-Service einrichten können, müssen Sie eine Instanz des Service 'IBM Cloud App ID' hinzufügen und Identitätsprovider konfigurieren. Wenn Sie noch nicht über eine Instanz von App ID verfügen, können Sie über den [ IBM Cloud-Services-Katalog ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link") ](https://console.bluemix.net/catalog){: new_window} eine Instanz bereitstellen.

Nachdem Sie den App ID-Service eingerichtet haben, müssen Sie Serviceberechtigungsnachweise anfordern, um App ID als Provider für Ihre {{site.data.keyword.iot_short_notm}}-Organisation zu konfigurieren. Sie können Serviceberechtigungsnachweise über die Service-UI von APP ID anfordern oder erstellen.

1. Wählen Sie im IBM Cloud-Dashboard den App ID-Service aus und klicken Sie auf **Serviceberechtigungsnachweise**.
2. Klicken Sie auf **Berechtigungsnachweise anzeigen**, um nach den Werten für `tenantId`, `clientId` und `secret` in den Berechtigungsnachweisen zu suchen. Diese Werte sind erforderlich, um APP ID zu einem späteren Zeitpunkt zu konfigurieren. Der Aussteller ist der Hostname von `oauthServerUrl`, zum Beispiel `appid-oauth.ng.bluemix.net`.

In der folgenden Abbildung sehen Sie ein Beispiel für das Abrufen der Berechtigungsnachweise:

![Abrufen von APP ID-Berechtigungsnachweisen](images/app_id_credentials.PNG "Beispiel zum Abrufen von APP ID-Berechtigungsnachweisen")


## App ID in {{site.data.keyword.iot_short_notm}} konfigurieren
{: #config_app_id}

Sie müssen APP ID konfigurieren, bevor Sie das APP ID-Token für die Autorisierung verwenden können. Greifen Sie zum Erstellen der APP ID-Konfiguration auf die API `POST /api/v0002/authentication/providers` zurück. Dabei werden `tenantId` und `issuer` im Schritt 2 in [App ID-Service einrichten](#set_up_app_id) abgerufen:

Anforderungshauptteil:

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

Anforderungsantwort: 200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## Benutzer zu {{site.data.keyword.iot_short_notm}} hinzufügen
{: #users_app_id}

Sie müssen Benutzer hinzufügen und ihnen auf der Basis ihrer Rollen Berechtigungen erteilen. Um Benutzer zu erstellen, verwenden Sie die API `POST /api/v0002/authorization/users` mit den folgenden Angaben:

 - Der Wert für `issuer` wurde im Schritt 1 im Abschnitt [APP ID-Service einrichten](#set_up_app_id) abgerufen.
 - `uniqueSecurityName` wird mit den Details für `appIdConfigName`, `amr` und der E-Mail des Benutzers ausgefüllt, die in den Identitätsprovidern des APP ID-Service konfiguriert sind.
 - `appIdConfigName` ist der Name, der im Abschnitt [App ID konfigurieren](##config_app_id) verwendet wird. Beispiel: “TestAppIdConfigName”.
 - `amr`  ist die Authentifizierungsmethodenreferenz, d. h. der Identitätsprovider, der im App ID-Service verwendet wird. App ID unterstützt die folgenden Identitätsprovider:

	 - CloudCloudverzeichnis
	 - SAML 2.0-Föderation
	 - Facebook
	 - Google

	 Basierend auf dem Identitätsprovider, der im App ID-Service verwendet wird, kann `amr` den Wert `cloud_directory`, `saml`, `facebook` oder `google` aufweisen.

Im folgenden Beispiel wird das Cloudverzeichnis (cloud_directory) verwendet:

Anforderungshauptteil:

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

Anforderungsantwort: 200

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

## APP ID-Token generieren
{: #token_app_id}

Nachdem Sie App ID konfiguriert und Benutzer hinzugefügt haben, können Sie mit der Anwendung App ID-Tokens für Ihre Benutzer generieren und mit diesen Plattform-APIs aufrufen. Weitere Informationen zu den ersten Schritten mit APP ID erhalten Sie in dem folgenden Video: [IBM Bluemix App ID ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}.

Ein Benutzer kann sich über den APP ID-Endpunkt anmelden und sein APP-Token durch das Posten der folgenden Beispiele abrufen.

### HTTP-Beispiel für das Generieren eines App ID-Tokens

Um das App ID-Token zu generieren, verwenden Sie die folgende API, indem Sie sich mit den Serviceberechtigungsnachweisen bei IBM Cloud authentifizieren:

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

Dabei werden die Werte für `tenantId`, `issuer`, `clientId` und `secret` aus Schritt 2 in [App ID-Service einrichten](#set_up_app_id) verwendet.

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

Anforderungshauptteil für 'grant_type=password':

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<benutzername>&
  password=<kennwort>&
```

Anforderungshauptteil für 'grant_type= authorization_code':

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

Anforderungsantwort:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Curl-Beispiel für das Generieren eines App ID-Tokens

Das folgende Snippet zeigt ein CURL-Beispiel für die Generierung eines App ID-Tokens:

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

Dabei sind die Werte für `benutzername` und `kennwort` für den Benutzer in App ID definiert.

Anforderungsantwort:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Swagger-Beispiel für das Generieren eines App ID-Tokens

Um mit Swagger ein App ID-Token zu generieren, verwenden Sie das [Swagger-Beispiel ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}.

Füllen Sie alle Details in den Swagger-Parametern aus und klicken Sie am Ende der Methode `/oauth/v3/{tenantId}/token` auf **Try it out**.

Die Antwort auf den Tokenendpunkt enthält die Zugriffs- und ID-Token, das optionale Aktualisierungstoken und die Ablaufinformationen.

## App ID-Token verwenden
{: #use_app_id}

Nachdem das App ID-Token erstellt wurde, kann die Anwendung mit diesem Token Plattform-APIs aufrufen. Das App ID-Token ist die Eigenschaft `id_token` in der Antwort, in der es im Abschnitt [App ID-Token generieren](#token_app_id) erstellt wurde. Wenn das App ID-Token abläuft, muss die Anwendung eines neues Token generieren, um weiterhin Plattform-APIs aufrufen zu können.

Die folgenden Beispiele veranschaulichen die Verwendung des App ID-Tokens beim Aufrufen von APIs.

###	HTTP-Beispiel für die Verwendung eines App ID-Tokens

`GET https://org.domain/api/v0002/bulk/devices`

Eingabeparameter  |	Werte
----------------- | -----------
Header	|	Content-Type: application/json<br>Authorization: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### Curl-Beispiel für die Verwendung eines App ID-Tokens

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
