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


# Autenticazione e autorizzazione di Cloud IAM per {{site.data.keyword.iot_short_notm}} (Beta)
{: #cloud_iam}

Le API {{site.data.keyword.iot_full}} supportano l'autenticazione e l'autorizzazione degli utenti attraverso IAM (Identity and Access Management).

**Importante:** la funzione Autenticazione e autorizzazione di Cloud IAM per {{site.data.keyword.iot_short_notm}} è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Cloud IAM è costruito nell'IBM Cloud e viene utilizzato per l'autenticazione e l'autorizzazione degli utenti amministratori e sviluppatori che devono configurare e gestire i propri servizi IBM. Gli utenti che necessitano dell'accesso all'interfaccia utente Watson IoT Platform vengono autenticati con IBM Cloud IAM. L'origine dell'identità per Cloud IAM può essere un utente con ID IBM registrato o un servizio di directory del cliente che supporta SAML.  

{{site.data.keyword.iot_short_notm}} supporta anche l'autenticazione degli utenti tramite App ID. App ID viene utilizzato per autenticare gli utenti che devono accedere alle applicazioni ospitate in IBM Cloud. Gli utenti di App ID in genere non eseguono attività amministrative o di sviluppo su un servizio cloud. Per ulteriori informazioni, vedi [Autenticazione di App ID per Watson IoT Platform](app_id.html#app_id).

IAM consente agli utenti di ottenere un token IAM OAuth e di utilizzarlo per autenticare le chiamate API. Ad esempio, un utente che ha installato la CLI {{site.data.keyword.containershort_notm}} potrebbe utilizzare il seguente comando:

`bx iam oauth-tokens`

Il comando restituisce il token IAM dell'utente collegato nel seguente formato:

`IAM Token: Bearer <some token>`

Un utente può anche utilizzare l'endpoint del token IAM per effettuare l'accesso e richiamare il token IAM, come mostrato nel seguente esempio:
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

Questo esempio di API utilizza una chiave API IAM per richiedere il token IAM e restituisce il `Bearer <token>`.

Quindi, quando un utente chiama un'API {{site.data.keyword.iot_short_notm}}, può fornire l'intestazione di autorizzazione `Authorization: Bearer <token>` sulle proprie chiamate API.

## Generazione di un token IAM
{: #iam_generate}

Puoi scegliere il modo in cui automatizzare la creazione del token IAM, a seconda di come esegui l'autenticazione con IBM Cloud e del tipo di ID IBM Cloud che usi.

Se utilizzi un ID non federato, puoi scegliere una delle seguenti opzioni per creare un token IAM:
 - **Nome utente e password IBM Cloud:** puoi creare un token e automatizzare completamente la creazione del token di accesso IAM.
 - **Genera una chiave API IBM Cloud:** utilizza le chiavi API IBM Cloud che dipendono dall'account IBM Cloud per il quale sono state generate. Non puoi combinare la chiave API IBM Cloud con un ID account diverso nello stesso token IAM. Per accedere ai cluster creati con un account diverso da quello su cui è basata la chiave API IBM Cloud, devi accedere all'account per generare una nuova chiave API.

Se utilizzi un ID federato, puoi scegliere una delle seguenti opzioni per creare un token IAM:
 - **Genera una chiave API IBM Cloud:** le chiavi API IBM Cloud dipendono dall'account IBM Cloud per il quale sono state generate. Non puoi combinare la chiave API IBM Cloud con un ID account diverso nello stesso token IAM. Per accedere ai cluster creati con un account diverso da quello su cui è basata la chiave API IBM Cloud, devi accedere all'account per generare una nuova chiave API.
 - **Utilizza un passcode monouso:** se effettui l'autenticazione con IBM Cloud utilizzando un passcode monouso, non puoi automatizzare completamente la creazione del tuo token IAM in quanto il richiamo del tuo passcode monouso richiede un'interazione manuale con il tuo browser web. Per automatizzare completamente la creazione del tuo token IAM, devi creare invece una chiave API IBM Cloud.

Quando crei il token di accesso, le informazioni del corpo incluse nella tua richiesta variano in base al metodo di autenticazione di IBM Cloud che utilizzi. Sostituisci i valori nel seguente modo:
- `<my_username>` = Il tuo nome utente IBM Cloud.
- `<my_password>` = La tua password IBM Cloud.
- `<my_api_key>` = La tua chiave API IBM Cloud.
- `<my_passcode>` = Il tuo passcode monouso di IBM Cloud. Esegui `bx login --sso` e segui le istruzioni nell'output della CLI per richiamare il passcode monouso utilizzando il tuo browser web.

Esempio:
`POST https://iam.<region>.bluemix.net/oidc/token`

Per specificare una regione IBM Cloud, rivedi le abbreviazioni delle regioni così come sono utilizzate negli endpoint API. Per ulteriori informazioni, vedi [Endpoint API delle regioni IBM Cloud[Icona link esterno](../../icons/launch-glyph.svg)(https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions)].

Parametro di input	 | Valori
---------------- | -----------
Intestazione	| Content-Type:application/x-www-form-urlencoded<br>Authorization: Basic Yng6Yng=<br>Nota: ti viene fornito Yng6Yng=, l'autorizzazione con codifica URL per il nome utente bx e la password bx.
Corpo per nome utente e password di IBM Cloud	|	grant_type: password<br>response_type: cloud_iam, uaa<br>username: `<my_username>`<br>password: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Nota: aggiungi la chiave uaa_client_secret senza alcun valore specificato.
Corpo per le chiavi API di IBM Cloud	|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Nota: aggiungi la chiave uaa_client_secret senza alcun valore specificato.
Corpo per il passcode monouso di IBM Cloud	|	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Nota: aggiungi la chiave uaa_client_secret senza alcun valore specificato.

Output API di esempio:

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
Puoi trovare il token IAM nel campo **access_token** dell'output dell'API.

Il seguente esempio mostra come utilizzare il token IAM durante la chiamata delle API.

```
GET https://org.domain/api/v0002/bulk/devices
```

Parametri di input  |	 Valori
----------------- | -----------
Intestazioni		|	Content-Type: application/json<br>Authorization: bearer `<iam_token>`
