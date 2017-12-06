---

copyright:
  years: 2017
lastupdated: "2017-10-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Panoramica sul controllo dell'accesso al livello della risorsa (Beta)
{: #RLAC_overview}

Il controllo dell'accesso al livello della risorsa ti permette di controllare l'accesso della chiave API e dell'utente per gestire i dispositivi. Utilizza i gruppi di risorse per specificare i dispositivi in un'organizzazione che ogni utente o chiave API possono gestire. Per informazioni su come configurare il controllo dell'accesso al livello della risorsa, consulta [Configurazione del controllo dell'accesso al livello della risorsa](rlac.html#configure_RLAC).

**Importante:** la funzione di controllo dell'accesso al livello della risorsa {{site.data.keyword.iot_full}} è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Concetti sul controllo dell'accesso al livello della risorsa 
{: #RLAC_concepts}

I gruppi di dispositivi sono parte dei controlli dell'accesso al livello della risorsa per {{site.data.keyword.iot_short_notm}}. Puoi utilizzare la funzione dei gruppi di risorse per gestire il numero di dispositivi a cui il personale può accedere in base ai rispettivi ruoli. Prima di implementare i gruppi di dispositivi, determinane e registrane lo scopo e l'ambito per aumentare l'efficienza della soluzione. 

Puoi implementare i gruppi di dispositivi come parte della tua soluzione IoT per consentire a un utente o un'applicazione di accedere a un sottoinsieme di dispositivi invece che a tutti i dispositivi associati con un'organizzazione. Per aumentare l'efficienza dei gruppi di dispositivi, utilizzali quando una persona deve accedere a molti dispositivi.

Ad esempio, un'azienda che impiega molti tecnici o personale operativo che vuole implementare una soluzione IoT nel Regno Unito. L'azienda configura un gruppo di dispositivi per ognuna delle 69 città, 9 gruppi di dispositivi per ogni regione geografica e uno per l'intero Regno Unito. I dispositivi sono assegnati a questi gruppi e i gruppi ai 15 tecnici e personale operativo, con alcuni di loro con l'accesso a più di un gruppo. Gli utenti che sono responsabili di solo uno o due dispositivi possono continuare ad utilizzare le applicazioni mobili e l'architettura aziendale che è esterna a {{site.data.keyword.iot_short_notm}} ma completare la soluzione IoT generale.

Possono essere fatte delle domande sui gruppi di dispositivi in [Questions in Internet of Things space ![Icona link esterno](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window}.

## Limiti del gruppo di risorse per il controllo dell'accesso

I limiti del gruppo di risorse sono imposti per garantire che la funzione di controllo dell'accesso al livello della risorsa funzioni efficacemente. Le restrizioni limitano il numero di gruppi di risorse assegnati a un soggetto, il numero di dispositivi all'interno dei gruppi e il numero di gruppi a cui può appartenere una risorsa.

I seguenti limiti sono imposti:

 - Può essere assegnato un massimo di 10 gruppi di risorse a una chiave API, utente o gateway.
 - Un gruppo di risorse può avere un massimo di 300 risorse.
 - Una risorsa può appartenere a un massimo di 10 gruppi.

## Terminologia del controllo dell'accesso al livello della risorsa

Puoi utilizzare le seguenti sezioni per comprendere la terminologia utilizzata nella funzione di controllo dell'accesso al livello della risorsa. 

### Soggetti
{: #subjects}

Un soggetto è un'entità autenticata che richiede l'accesso alla piattaforma, che deve essere autorizzato. I seguenti tipi di soggetto sono validi:

- *Utenti*: gli utenti finali sono le persone che utilizzano le applicazioni. Un membro è un utente il cui account non ha data di scadenza mentre un ospite ha la data di scadenza.
- *Dispositivi*: i dispositivi fisici che accedono alla piattaforma {{site.data.keyword.iot_short_notm}} e utilizzano il tipo Dispositivo.
- *Gateway*: i dispositivi fisici che accedono alla piattaforma {{site.data.keyword.iot_short_notm}} e utilizzano il tipo Gateway.
- *Applicazioni*: le applicazioni o i servizi che accedono alla piattaforma {{site.data.keyword.iot_short_notm}} utilizzando le chiavi API per l'autorizzazione e il controllo dell'accesso.

### Risorse
{: #resources}

Una risorsa è un'entità su cui il soggetto esegue le azioni. Il soggetto richiede l'accesso alla piattaforma autorizzato per la risorsa. I dispositivi sono le sole risorse supportate dalla release beta del controllo dell'accesso al livello della risorsa.

### Azioni
{: #actions}

L'azione è cosa esegue il soggetto, spesso si tratta di creare, leggere, aggiornare, eliminare o eseguire. Le operazioni sono utilizzare per raggruppare le azioni nelle risorse.

### Applicazioni
{: #applications}

Un applicazione può agire per conto di un soggetto sconosciuto sulla piattaforma, come un'applicazione mobile che controlla un'applicazione domestica e un'applicazione può agire per conto di un dispositivo simulato o reale conosciuto o sconosciuto dalla piattaforma. Un'applicazione può anche essere un'applicazione lato server vera che non agisce per conto di un altro tipo di dispositivo.

L'autenticazione e l'accesso vengono forniti dalla piattaforma {{site.data.keyword.iot_short_notm}} in base a un token o una chiave API.

### Operazioni
{: #operations}

Un'operazione contiene una o più azioni eseguite su alcuni tipi di risorsa utilizzando le interfacce utente e programmatiche, come i servizi REST e le interfacce MQTT. Sono utilizzate da diversi soggetti inclusi i membri, le applicazioni e i dispositivi. Le operazioni sono identificate dai loro ID operazione (OID).

### Ruoli
{: #roles}

I ruoli sono serie di operazioni utilizzati per fornire l'autorizzazione ad utilizzare le operazioni. Un soggetto può avere più ruoli e il ruolo è un attributo di un soggetto.

**Formato ruoli**

Array di 0..n ruoli

    { "roles": [ "PD_ADMIN_APP" ] }

**Formato ruoli per gruppi**

Associazione di 0..n coppie <string, list<string>>

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### Autorizzazioni
{: #permissions}

Le autorizzazioni consentono a un soggetto di eseguire le operazioni per una risorsa o un gruppo di risorse.

### Gruppi di risorse
{: #resource_groups}

Un gruppo di risorse è una raccolta di dispositivi come le risorse. I dispositivi possono essere aggiunti ai gruppi di risorse e i gruppi di risorse possono essere assegnati ai soggetti nelle coppie ruolo per gruppi. Queste relazioni consentono ai soggetti di eseguire le operazioni per un ruolo selezionato su gruppi di dispositivi specifici.

### Utenti
{: #users}

Gli utenti accedono al dashboard della piattaforma {{site.data.keyword.iot_short_notm}} utilizzando un ID che è un membro di un'organizzazione. Gli utenti sono ruoli assegnati che concedono loro l'autorizzazione a chiamare alcune serie di API HTTP. Per ulteriori informazioni sui ruoli utente, vedi [Livelli di accesso per i ruoli utente](roles_access.html#levels-of-access-for-user-roles).

### Chiavi API
{: #API_keys}

Le chiavi API sono token utilizzati per richiamare le API HTTP della piattaforma {{site.data.keyword.iot_short_notm}}. Le chiavi API sono ruoli assegnati che concedono loro l'autorizzazione a chiamare alcune serie di API HTTP. Per ulteriori informazioni sui ruoli della chiave API, vedi [Livelli di accesso per i ruoli applicazione](app_roles_access.html#levels-of-access-for-application-roles).

## API in cui viene imposto il controllo dell'accesso al livello della risorsa
{: #RLAC_enforced_APIs}
Quando il controllo dell'accesso al livello della risorsa è abilitato, le API correlate al dispositivo incluse in questa sezione funzionano solo se il dispositivo specificato è accessibile dal chiamante.

### API dispositivo (individuali)

**Elenca i tipi di dispositivo**

    GET /api/v0002/device/types/${typeId}

Viene restituito solo il sottoinsieme di dispositivi che appartengono al gruppo appropriato.

**Richiama dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Richiama dettagli di gestione del dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Richiama i dispositivi registrati a un gateway**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante. Viene restituito solo il sottoinsieme di dispositivi che appartengono al gruppo appropriato. I dispositivi e i gateway devono essere nel gruppo del chiamante.

**Aggiorna dispositivo**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Elimina dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

### API dispositivo (in blocco)

**Elenca i dispositivi**

    GET /api/v0002/bulk/devices

Viene restituito solo il sottoinsieme di dispositivi che appartengono al gruppo appropriato.

**Elimina dispositivi**

    DELETE /api/v0002/bulk/devices/remove

Viene eliminato solo il sottoinsieme di dispositivi che appartengono al gruppo appropriato. I dispositivi che non sono accessibili restituiscono ancora success = true.

**Aggiorna dispositivi:**

    PUT /api/v0002/bulk/devices/update

### API di gestione dispositivo 

**Avvia richiesta**

    POST /api/v0002/mgmt/requests

Non possibile se un qualsiasi dispositivo non è accessibile al chiamante.

**Elimina richiesta**

    DELETE /api/v0002/mgmt/requests/${requestId}

Non possibile se un qualsiasi dispositivo non è accessibile al chiamante.

**Visualizza richiesta**

    GET /api/v0002/mgmt/requests/${requestId}

**Elenca richieste**

    GET /api/v0002/mgmt/requests

**Richiama una richiesta**

    GET /api/v0002/mgmt/requests/${requestId}

**Richiama dettagli del dispositivo della richiesta**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**Richiama dettagli del dispositivo della richiesta per un dispositivo**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### API di determinazione del problema

**Log di connessione di un dispositivo**

    GET /api/v0002/logs/connection

Restituisce un array vuoto se il dispositivo non è accessibile.

**Aggiunge il codice di errore del dispositivo**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Elenca i codici di errore del dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Cancella i codici di errore del dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Aggiunge il log di diagnostica del dispositivo**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Elenca il log di diagnostica del dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Richiama un log di diagnostica del dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Cancella il log di diagnostica del dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Elimina un log di diagnostica del dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

###API ultima cache evento

**Richiama gli eventi per il dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.

**Richiama gli eventi per il dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

Restituisce un errore 404 se il dispositivo non è accessibile al chiamante.
