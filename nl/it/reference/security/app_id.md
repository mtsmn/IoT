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

# Autenticazione di App ID per Watson IoT Platform (Beta)
{: #app_id}

App ID viene utilizzato per autenticare gli utenti che devono accedere alle applicazioni ospitate in IBM Cloud. Questi utenti accedono al servizio tramite applicazioni mobili o web fornite dal servizio.
{: shortdesc}

**Importante:** la funzione Autenticazione e autorizzazione di App ID per {{site.data.keyword.iot_short_notm}} è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} supporta anche l'autenticazione degli utenti tramite Cloud IAM. Cloud IAM è integrato in IBM Cloud e viene utilizzato per l'autenticazione e l'autorizzazione degli utenti amministratori e sviluppatori che devono configurare e gestire i propri servizi IBM. Per ulteriori informazioni su Cloud IAM, vedi [Autenticazione e autorizzazione di Cloud IAM per Watson IoT Platform](cloud_iam.html#cloud_iam).

Gli utenti di App ID in genere non eseguono attività amministrative o di sviluppo su un servizio cloud e non possono accedere al dashboard web {{site.data.keyword.iot_short_notm}}. Solo gli utenti di Cloud IAM possono accedere al dashboard.

Le API {{site.data.keyword.iot_short_notm}} supportano l'autenticazione di utenti dal servizio IBM Cloud App ID.  Puoi configurare la tua organizzazione {{site.data.keyword.iot_short_notm}} per utilizzare un'istanza del servizio App ID e quindi aggiungere gli utenti di App ID alla tua organizzazione. L'applicazione autentica questi utenti con App ID che può quindi essere utilizzato per richiamare le API {{site.data.keyword.iot_short_notm}}. Per ulteriori informazioni sul servizio IBM Cloud App ID, vedi [Esercitazione introduttiva di IBM Cloud App ID ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}.

L'attuale supporto per App ID è per le API REST non di messaggistica, comprese le API per la configurazione e la gestione di dispositivi, utenti e altre funzionalità di {{site.data.keyword.iot_short_notm}}. App ID non può essere utilizzato per pubblicare e sottoscrivere i messaggi.

In genere, App ID è utilizzato per autenticare le API REST richiamate per conto di un utente non amministrativo. Uno sviluppatore può creare un'applicazione che utilizza un token App ID per richiamare un'API REST per conto di un utente registrato, ad esempio un tecnico di manutenzione, che non dispone di accesso amministrativo. Il vantaggio di utilizzare App ID per autenticare gli utenti non amministrativi piuttosto che utilizzare un singolo token di autenticazione è che l'audit trail in {{site.data.keyword.iot_short_notm}} mostra l'utente che ha richiamato le API REST anziché un token di autenticazione condiviso. App ID è utile anche per autenticare gli utenti non amministratori che devono utilizzare l'interfaccia riga di comando (CLI) {{site.data.keyword.iot_short_notm}}.

**Importante:** la chiamata delle API REST {{site.data.keyword.iot_short_notm}} deve essere eseguita utilizzando un'applicazione utente e non direttamente da un browser o da applicazioni mobili. L'applicazione utente può fornire un ulteriore livello di controllo dell'accesso per determinare a quali funzioni del dispositivo può accedere un determinato utente, oltre a convalidare i messaggi MQTT inviati ai dispositivi. La seguente immagine mostra questo flusso di chiamata:

![Chiamata dell'API REST utilizzando un'applicazione utente](images/app_id_user_app.PNG)

Gli utenti di ID App devono essere registrati in {{site.data.keyword.iot_short_notm}} utilizzando lo stesso processo usato dagli utenti di Cloud IAM ed entrambi vengono visualizzati nell'interfaccia del dashboard. 

Poiché gli utenti di App ID generalmente non sono amministratori, è importante considerare le risorse e i comandi a cui devono essere autorizzati ad accedere e quindi impostare le impostazioni di controllo dell'accesso al livello di risorsa {{site.data.keyword.iot_short_notm}} in modo appropriato. Nella maggior parte dei casi, agli utenti di App ID viene assegnato un ruolo personalizzato più restrittivo rispetto ai ruoli predefiniti di un'organizzazione {{site.data.keyword.iot_short_notm}}. Per ulteriori informazioni sui ruoli, vedi [Ruoli utente, applicazione e gateway](../../roles_index.html#user-application-and-gateway-roles).

Per impostazione predefinita, gli utenti di App ID possono accedere a tutti i dispositivi. Se un utente di App ID deve essere in grado di gestire solo un elenco limitato di dispositivi, assegna gruppi di risorse agli utenti di App ID.

**Nota:** {{site.data.keyword.iot_short_notm}} limita il numero totale di utenti registrati a 1.000.

## Impostazione di un servizio App ID
{: #set_up_app_id}

Prima di impostare un servizio App ID, devi aggiungere un'istanza del servizio IBM Cloud App ID e configurare i provider di identità. Se non disponi già di un'istanza di App ID, puoi eseguirne il provisioning dal [catalogo dei servizi IBM Cloud ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog){: new_window}.

Dopo aver impostato il servizio App ID, devi ottenere le credenziali del servizio per configurare App ID come provider della tua organizzazione {{site.data.keyword.iot_short_notm}}. Puoi ottenere o creare le credenziali nell'interfaccia utente del servizio App ID.

1. Nel dashboard IBM Cloud, seleziona il tuo servizio App ID e fai clic su **Service Credentials**.
2. Fai clic su **View Credentials** per individuare i `tenantId`, `clientId` e `secret` nelle credenziali. Questi valori sono necessari per configurare App ID nei passi successivi. L'emittente sarà il nome host di `oauthServerUrl`, ad esempio `appid-oauth.ng.bluemix.net`.

La seguente immagine mostra un esempio di come richiamare le credenziali:

![Richiamo delle credenziali di App ID](images/app_id_credentials.PNG "Esempio di richiamo delle credenziali di App ID")


## Configurazione di App ID in {{site.data.keyword.iot_short_notm}}
{: #config_app_id}

Per poter utilizzare il token App ID per l'autenticazione, devi prima configurare App ID. Per creare la configurazione di App ID, utilizza l'API `POST /api/v0002/authentication/providers` dove `tenant_Id` e `issuer` vengono ottenuti attraverso il passo 2 in [Impostazione di un servizio App ID](#set_up_app_id):

Corpo della richiesta:

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

Risposta della richiesta: 200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## Aggiunta di utenti a {{site.data.keyword.iot_short_notm}}
{: #users_app_id}

Devi aggiungere gli utenti e assegnare loro le autorizzazioni in base ai rispettivi ruoli. Per creare gli utenti, utilizza l'API `POST /api/v0002/authorization/users` con i seguenti dettagli:

 - `issuer` viene ottenuto nel passo 2 in [Impostazione di un servizio App ID](#set_up_app_id).
 - `uniqueSecurityName` viene compilato con i dettagli di `appIdConfigName`, `amr` e email dell'utente che vengono configurati nei provider di identità del servizio App ID.
 - `appIdConfigName` è il nome utilizzato in [Configurazione di App ID](##config_app_id), ad esempio “TestAppIdConfigName”.
 - `amr` è il riferimento al metodo di autenticazione, che è il provider di identità utilizzato nel servizio App ID. App ID supporta i seguenti provider di identità:

	 - Cloud Directory
	 - Federazione SAML 2.0
	 - Facebook
	 - Google

	 In base al provider di identità utilizzato nel servizio App ID, `amr` sarà `cloud_directory`, `saml`, `facebook` o `google`.

Nel seguente esempio viene utilizzato Cloud Directory:

Corpo della richiesta:

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

Risposta della richiesta: 200

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

## Generazione di un token APP ID
{: #token_app_id}

Dopo aver configurato App ID e aggiunto gli utenti, la tua applicazione può iniziare a generare token App ID per gli utenti e utilizzarli per chiamare le API della piattaforma. Per ulteriori informazioni su come iniziare con App ID, guarda il seguente video: [IBM Bluemix App ID ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}.

Un utente può utilizzare l'endpoint App ID per effettuare l'accesso e richiamare il proprio token App ID pubblicando i seguenti esempi.

### Esempio HTTP di generazione di un token App ID

Per generare il token App ID, utilizza la seguente API effettuando l'autenticazione a IBM Cloud con le credenziali del servizio:

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

Dove `tenantId`, `issuer`, `clientId` e `secret` vengono ottenuti nel passo 2 in [Impostazione di un servizio App ID](#set_up_app_id).

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

Corpo della richiesta per grant_type=password:

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

Corpo della richiesta per grant_type= authorization_code:

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

Risposta della richiesta:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Esempio Curl di generazione di un token App ID

Il seguente frammento mostra un esempio CURL di generazione di un token App ID:

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

Dove `username` e `password` sono definiti per l'utente in App ID.

Risposta della richiesta:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Esempio Swagger di generazione di un token App ID

Per utilizzare Swagger per generare un token App ID, utilizza l'[esempio Swagger ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}.

Completa tutti i dettagli nei parametri Swagger e fai clic su **Try it out** alla fine del metodo `/oauth/v3/{tenantId}/token`.

La risposta dell'endpoint del token contiene i token ID e di accesso, il token di aggiornamento facoltativo e le informazioni sulla scadenza.

## Utilizzo del token App ID
{: #use_app_id}

Una volta che il token App ID è stato creato, la tua applicazione può iniziare a utilizzarlo per chiamare le API della piattaforma. Il token App ID è la proprietà `id_token` nella risposta in cui è stato generato in [Generazione di un token App ID](#token_app_id). Alla scadenza del token App ID, la tua applicazione deve generare un nuovo token per continuare a chiamare le API della piattaforma.

I seguenti esempi mostrano come utilizzare il token App ID durante la chiamata delle API.

###	Esempio HTTP di utilizzo di un token App ID

`GET https://org.domain/api/v0002/bulk/devices`

Parametri di input  |	 Valori
----------------- | -----------
Intestazioni		|	Content-Type: application/json<br>Authorization: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### Esempio Curl di utilizzo di un token App ID

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
