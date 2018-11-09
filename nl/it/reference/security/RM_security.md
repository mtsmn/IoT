---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-03"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Gestione della sicurezza e del rischio
{: #RM_security}

Puoi migliorare la sicurezza per abilitare la creazione, la definizione e l'invio di notifiche sulla sicurezza della connessione del dispositivo. Con questa sicurezza migliorata, vengono utilizzati i certificati e l'autenticazione TLS (transport layer security) in aggiunta all'utilizzo di token e ID utente utilizzati da {{site.data.keyword.iot_short_notm}} per determinare come e dove i dispositivi si connettono alla piattaforma.

## Elenchi di revoche di certificati (CRL) e certificati client
{: #CRLs}

Gli elenchi di revoche di certificati (CRL) sono supportati per le configurazioni che utilizzano i certificati client per autenticare i dispositivi. L'ubicazione specificata come punto di distribuzione del CRL deve supportare l'accesso HTTP o HTTPS e il sito deve essere raggiungibile. La connessione client viene rifiutata se non è possibile raggiungere il punto di distribuzione del CRL o se il CRL non è valido o non è stato trovato nel punto di distribuzione. 
 

I certificati client devono essere firmati dai certificati dell'autorità di certificazione (CA, Certificate Authority) con "Certificate Sign" specificato nella sezione "X509v3 Key Usage" in "X509v3 extensions". Per utilizzare i CRL per revocare i certificati client, il certificato CA che firma un certificato client deve includere anche "CRL Sign" nella sezione "X509v3 Key Usage" in "X509v3 extensions". Questa configurazione consente al certificato CA di emettere e firmare aggiornamenti del CRL quando richiesto. Solo il CRL specificato nel punto di distribuzione del certificato client viene utilizzato per determinare se un certificato è stato revocato.

Assicurati di verificare i certificati e i CRL, in particolare se generi i tuoi propri certificati e CRL invece di utilizzare i certificati da un'autorità di certificazione riconosciuta.


## Configurazione dei certificati
{: #certificates}

Quando i certificati vengono abilitati, durante la comunicazione tra i dispositivi e il server, a tutti i dispositivi che non dispongono di certificati validi configurati nelle impostazioni di sicurezza non viene consentito l'accesso, anche se utilizzano password e ID utente validi.

Per configurare i certificati e l'accesso al server per i dispositivi, l'operatore di sistema registra i certificati dell'autorità di certificazione (CA, Certificate Authority) e i certificati del server di messaggistica nella piattaforma Watson IoT Platform.
Per configurare i certificati client e l'accesso al server per i dispositivi, l'operatore di sistema importa i certificati dell'autorità di certificazione (CA, Certificate Authority) e i certificati del server di messaggistica in {{site.data.keyword.iot_short_notm}}. L'analista della sicurezza configura quindi le politiche di sicurezza predefinite per le connessioni tra i dispositivi e la piattaforma. L'analista può aggiungere diverse politiche per diversi tipi di dispositivo.

Per informazioni su come configurare i certificati, consulta [Configurazione dei certificati](set_up_certificates.html).

## Impostazioni della sicurezza
{: #connect_policy}

Le impostazioni di sicurezza, compreso l'uso di impostazioni delle politiche di connessione, certificati CA e certificati di server di messaggistica, implementano la modalità di connessione dei dispositivi alla piattaforma. Puoi impostare le politiche di connessione predefinite e creare le impostazioni personalizzate per i tipi di dispositivo specifici. La politica può essere impostata per consentire le connessioni non crittografate, per definire solo le connessioni TLS (transport layer security) e per consentire ai dispositivi di autenticarsi con i certificati lato client.

Per informazioni su come configurare le politiche di connessione, consulta [Configurazione delle politiche di sicurezza](set_up_policies.html).

Le politiche whitelist e whitelist forniscono la possibilità di controllare le ubicazioni da cui i dispositivi possono collegarsi all'account dell'organizzazione. Una blacklist identifica tutti gli indirizzi IP, i CIDR o i paesi a cui è negato l'accesso al server. Una whitelist dà un accesso esplicito a specifici indirizzi IP.

Per informazioni su come configurare le politiche blacklist e whitelist, consulta [Configurazione delle blacklist e delle whitelist](set_up_policies.html#config_black_white).

## Piani dell'organizzazione e politiche di sicurezza
Le politiche di sicurezza migliorate abilitano le organizzazioni a determinare come desiderano che i dispositivi si connettono e come vengono autenticati con la piattaforma, utilizzando le politiche di connessione e di blacklist e whitelist. Le opzioni della politica di sicurezza disponibili per un'organizzazione dipendono dal tipo di piano dell'organizzazione:

**Piano standard:**
- Gli operatori di sistema possono configurare le politiche di connessione con le seguenti opzioni:
    - TLS facoltativo
    - TLS con autenticazione token
    - TLS con autenticazione certificato client e token

**Piano di sicurezza avanzato (ASP) o piano lite:**
- Gli operatori di sistema possono configurare le politiche di connessione con le seguenti opzioni:
    - TLS facoltativo
    - TLS con autenticazione token
    - TLS con autenticazione certificato client
    - TLS con autenticazione certificato client e token
    - TLS con certificato client o token
- Gli operatori di sistema possono configurare le blacklist o whitelist

## Dashboard e creazione di report di Gestione della sicurezza e del rischio
{: #dashboard}

L'operatore di sistema e l'analista delle sicurezza possono utilizzare il dashboard di Gestione della sicurezza e del rischio per visualizzare lo stato di sicurezza generale. Le schede nel dashboard posso fornire una panoramica di conformità completa e lo stato di connessione dei dispositivi.

Le seguenti schede del dashboard sono disponibili per l'analisi del rischio e della conformità:
 - **Connection Security Compliance**- mostra il livello di conformità dei dispositivi connessi al server.
 - **Blacklist/Whitlelist Compliance** - mostra il livello di conformità dei dispositivi sulla base della politica di blacklist o whitelist attiva.
 - **Overall Policy Compliance** (Beta) - mostra il livello globale di conformità sulla base di tutte le politiche implementate.
 - **Policy Violations** (Beta) - mostra le violazioni globali per ciascuna politica.

**Importante**: le schede **Overall Policy Compliance** e **Policy Violations** sono disponibili solo come parte di un programma Beta limitato. Per abilitare queste schede del dashboard, attiva **Experimental Features** nella pagina **Settings**.

### Drill-down policy reporting (Beta)
{: #drill_down}

**Importante**: la funzione di creazione di report di drill-down per le politiche Risk Management è disponibile solo come parte di un programma Beta limitato. Per abilitare la creazione di report di drill-down, attiva **Experimental Features** nella pagina **Settings**.

Come parte della funzione Beta, l'operatore di sistema può eseguire il drill-down in ciascun report per visualizzare gli specifici dettagli dei dispositivi conformi alle politiche o che le violano.

Puoi accedere ai report di drill-down dalle schede del dashboard, dalla pagina **Policies** dove sono elencate tutte le politiche di sicurezza e dalla pagina Details per ciascuna politica. Puoi eseguire il drill-down a tre livelli di dettagli nei report:
1. Il primo livello elenca tutte le politiche contenute nel tipo di politica selezionato, ad esempio la politica di connessione predefinita e le eventuali politiche di connessione personalizzate.
2. Il secondo livello mostra i dispositivi che violano la politica e la causa della violazione.
3. Il terzo livello mostra i dettagli specifici di un singolo dispositivo che viola la politica.
