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


# Authentifizierung und Berechtigung mit Cloud IAM für {{site.data.keyword.iot_short_notm}} (Beta)
{: #cloud_iam}

{{site.data.keyword.iot_full}}-APIs unterstützen die Authentifizierung und Berechtigung von Benutzern durch die Verwendung von Identity and Access Management (IAM).

**Wichtig:** Die Funktion für Authentifizierung und Berechtigung mit Cloud AIM für {{site.data.keyword.iot_short_notm}} steht nur als Bestandteil des eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Cloud IAM ist in der IBM Cloud integriert und wird zur Authentifizierung und Berechtigung von Benutzern mit Verwaltungsaufgaben und Entwicklern verwendet, die ihre IBM Services konfigurieren und verwalten müssen. Benutzer, die Zugriff auf die Benutzerschnittstelle von Watson IoT Platform benötigen, werden mit IBM Cloud IAM authentifiziert. Die Identitätsquelle für Cloud IAM können registrierte IBM ID-Benutzer oder der Verzeichnisservice eines Kunden sein, der SAML unterstützt.  

{{site.data.keyword.iot_short_notm}} unterstützt auch die Authentifizierung von Benutzern über App ID. Mit App ID können Benutzer authentifiziert werden, die Zugriff auf Anwendungen benötigen, die in IBM Cloud gehostet werden. Benutzer der App ID führen in der Regel keine Verwaltungs- oder Entwicklungsaktivitäten für einen Cloud-Service aus. Weitere Informationen finden Sie in [Authentifizierung mit App ID für Watson IoT Platform](app_id.html#app_id).

Mit IAM können Benutzer ein IAM-OAuth-Token anfordern und für die Authentifizierung von API-Aufrufen verwenden. Ein Benutzer mit installierter {{site.data.keyword.containershort_notm}}-CLI kann beispielsweise den folgenden Befehl verwenden:

`bx iam oauth-tokens`

Der Befehl gibt das IAM-Token des angemeldeten Benutzers im folgenden Format zurück:

`IAM Token: Bearer <some token>`

Ein Benutzer kann auch den IAM-Tokenendpunkt verwenden, um sich anzumelden und sein IAM-Token abzurufen, wie im folgenden Beispiel gezeigt:
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

Dieses API-Beispiel verwendet einen IAM-API-Schlüssel, um das IAM-Token anzufordern und gibt `Bearer <token>` zurück.

Wenn dann ein Benutzer eine {{site.data.keyword.iot_short_notm}}-API aufruft, kann der Berechtigungsheader `Authorization: Bearer <token>` für die API-Aufrufe bereitgestellt werden.

## IAM-Token generieren
{: #iam_generate}

Sie können auswählen, wie die Erstellung des IAM-Tokens automatisiert werden soll. Dies hängt von Ihrer Authentifizierung mit IBM Cloud und dem Typ der IBM Cloud-ID ab, die Sie verwenden.

Wenn Sie eine nicht eingebundene ID verwenden, können Sie eine der folgenden Optionen für die Erstellung eines IAM-Tokens auswählen:
 - **Benutzername und Kennwort für den IBM Cloud-Benutzer:** Sie können ein Token erstellen und die Erstellung Ihres IAM-Zugriffstokens vollständig automatisieren.
 - **IBM Cloud-API-Schlüssel generieren:** Verwenden Sie IBM Cloud-API-Schlüssel, die von dem IBM Cloud-Konto abhängig sind, für das sie generiert werden. Sie können Ihren IBM Cloud-API-Schlüssel nicht mit einer anderen Konto-ID in demselben IAM-Token kombinieren. Wenn Sie auf Cluster zugreifen möchten, die mit einem anderen Konto als dem Konto erstellt wurden, auf dem der IBM Cloud-API-Schlüssel basiert, müssen Sie sich bei dem entsprechenden Konto anmelden, um einen neuen API-Schlüssel zu generieren.

Wenn Sie eine eingebundene ID verwenden, können Sie eine der folgenden Optionen für die Erstellung eines IAM-Tokens auswählen:
 - **IBM Cloud-API-Schlüssel generieren:** IBM Cloud-API-Schlüssel sind von dem IBM Cloud-Konto abhängig, für das sie generiert werden. Sie können Ihren IBM Cloud-API-Schlüssel nicht mit einer anderen Konto-ID in demselben IAM-Token kombinieren. Wenn Sie auf Cluster zugreifen möchten, die mit einem anderen Konto als dem Konto erstellt wurden, auf dem der IBM Cloud-API-Schlüssel basiert, müssen Sie sich beim Konto anmelden, um einen neuen API-Schlüssel zu generieren.
 - **Einmaligen Kenncode verwenden:** Wenn Sie sich mit IBM Cloud authentifizieren, indem Sie einen einmaligen Kenncode verwenden, können Sie die Erstellung Ihres IAM-Tokens nicht vollständig automatisieren, da der Abruf Ihres einmaligen Kenncodes eine manuelle Interaktion mit Ihrem Web-Browser erfordert. Wenn Sie die Erstellung Ihres IAM-Tokens vollständig automatisieren möchten, müssen Sie stattdessen einen IBM Cloud-API-Schlüssel erstellen.

Wenn Sie Ihr Zugriffstoken erstellen, hängen die in der Anforderung enthaltenen Hauptteilinformationen von der von Ihnen verwendeten IBM Cloud-Authentifizierungsmethode ab. Ersetzen Sie die Werte wie folgt:
- `<my_username>` = Ihr IBM Cloud-Benutzername.
- `<my_password>` = Ihr IBM Cloud-Kennwort.
- `<my_api_key>` = Ihr IBM Cloud-API-Schlüssel.
- `<my_passcode>` = Ihr einmaliger IBM Cloud-Kenncode. Führen Sie den Befehl `bx login --sso` aus und befolgen Sie die Anweisungen in der CLI-Ausgabe, um Ihren einmaligen Kenncode über den Web-Browser abzurufen.

Beispiel:
`POST https://iam.<region>.bluemix.net/oidc/token`

Um eine IBM Cloud-Region anzugeben, überprüfen Sie die Regionsabkürzungen, wie sie in den API-Endpunkten verwendet werden. Weitere Informationen finden Sie in der Veröffentlichung zu [API-Endpunkten für IBM Cloud-Regionen [Symbol für externen Link](../../icons/launch-glyph.svg)](https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions).

Eingabeparameter | Werte
---------------- | -----------
Header	| Content-Type:application/x-www-form-urlencoded<br>Authorization: Basic Yng6Yng=<br>Hinweis: Sie erhalten Yng6Yng=, die URL-codierte Berechtigungen für den Benutzernamen bx und das Kennwort bx.
Hauptteil für IBM Cloud-Benutzername und -Kennwort	|	grant_type: password<br>response_type: cloud_iam, uaa<br>username: `<my_username>`<br>password: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Hinweis: Fügen Sie den Schlüssel 'uaa_client_secret' hinzu, ohne dass ein Wert angegeben ist.
Hauptteil für IBM Cloud-API-Schlüssel	|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Hinweis: Fügen Sie den Schlüssel 'uaa_client_secret' hinzu, ohne dass ein Wert angegeben ist.
Hauptteil für einmaligen IBM Cloud-Kenncode	|	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Hinweis: Fügen Sie den Schlüssel 'uaa_client_secret' hinzu, ohne dass ein Wert angegeben ist.

Beispiel für API-Ausgabe:

```
{
"access_token": "<iam-token>",
"refresh_token": "<iam-aktualisierungstoken>",
"uaa_token": "<uaa-token>",
"uaa_refresh_token": "<uaa-aktualisierungstoken>",
"token_type": "Bearer",
"expires_in": 3600,
"expiration": 1493747503
}
```
Sie finden das IAM-Token im Feld **access_token** der API-Ausgabe.

Das folgende Beispiel zeigt, wie das IAM-Token beim Aufrufen von APIs verwendet wird.

```
GET https://org.domain/api/v0002/bulk/devices
```

Eingabeparameter  |	Werte
----------------- | -----------
Header 	|	Content-Type: application/json<br>Authorization: bearer `<iam_token>`
