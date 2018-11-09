---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API HTTP per i dispositivi gateway
{: #api_link}

Esistono due tipi di API HTTP che è possibile utilizzare con {{site.data.keyword.iot_full}}. Puoi utilizzare le API REST HTTP per
creare, aggiornare, elencare ed eliminare i dispositivi gateway. Utilizza le API di messaggistica HTTP per inviare informazioni sugli eventi dai dispositivi gateway al cloud e per accettare i comandi dalle applicazioni nel cloud. 


## Connessioni client
{: #client_connections}

Per informazioni sulla sicurezza client e su come connettere i client ai dispositivi client e gateway in {{site.data.keyword.iot_short_notm}}, consulta [Connessione di applicazioni, dispositivi e gateway a {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

**Nota:** la porta HTTP 1883 è disabilitata per impostazione predefinita. Per informazioni sulla modifica dell'impostazione predefinita, vedi [Configurazione delle politiche di sicurezza](../reference/security/set_up_policies.html#set_up_policies.md).


## Autenticazione

Tutte le richieste devono includere un'intestazione di autorizzazione. L'autenticazione di base è l'unico metodo supportato. Quando un dispositivo effettua una richiesta HTTP utilizzando l'API REST HTTP {{site.data.keyword.iot_short_notm}}, sono necessarie le seguenti credenziali:

|Credenziale|Input richiesto|
|:---|:---|
|Nome utente| `g/{orgId}/{gwType}/{gwDevId}` or `g-{orgId}-{gwType}-{gwDevId}`
|Password| Il token di autenticazione che è stato generato automaticamente o specificato manualmente quando hai registrato il dispositivo gateway.

dove:

<dl>
<dt>orgId</dt>  
<dd>Il nome dell'organizzazione, che deve corrispondere al nome specificato nell'intestazione host.</dd>

<p></p>
<dt>gwType</dt>  
<dd>Il tipo del gateway.</dd> 
<dd>Se utilizzi il carattere trattino "-" come delimitatore nel nome utente, questo valore non deve includere un carattere trattino. </dd>
<p></p>
<dt>gwDevId</dt>  
<dd>L'identificativo del dispositivo del gateway. </dd>
</dl>


## Intestazioni richiesta per tipo di contenuto

Deve essere fornita un'intestazione della richiesta `Content-Type` con la richiesta. La seguente tabella mostra come i tipi supportati sono associati ai formati interni di {{site.data.keyword.iot_short_notm}}:

|Intestazione Content-Type|Formato in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|testo/semplice|"text"
|applicazione/json| "json"
|applicazione/xml | "xml"
|applicazione/octet-stream|"bin"

## QOS (quality of service)

Simile al livello di sicurezza di distribuzione 0 di QOS (quality of service) MQTT "at most once", la messaggistica REST HTTP fornisce la fornitura del messaggio non persistente ma verifica che la richiesta sia corretta e che possa essere distribuita al server prima che invii la risposta HTTP. Un risposta che contiene un codice di stato HTTP di 200 conferma che il messaggio è stato distribuito al server. Quando utilizzi il livello di QOS (quality of service) MQTT "at most once" o l'equivalente HTTP per distribuire i messaggi dell'evento, il dispositivo o l'applicazione deve implementare la logica del nuovo tentativo per garantire la distribuzione.

Per ulteriori informazioni sul protocollo MQTT e sui livelli di QOS (quality of service) per {{site.data.keyword.iot_short_notm}}, consulta [Messaggistica MQTT](../reference/mqtt/index.html).

Puoi utilizzare le API di messaggistica HTTP per pubblicare eventi dai dispositivi gateway al cloud e per ricevere i comandi dalle applicazioni nel cloud.

## Pubblicazione eventi
{: #event_publication}

Oltre a utilizzare il protocollo di messaggistica MQTT, puoi anche configurare i tuoi dispositivi gateway per pubblicare eventi su {{site.data.keyword.iot_short_notm}} tramite HTTP utilizzando o i comandi dell'API di messaggistica HTTP.

Per inoltrare una richiesta ``POST`` da un dispositivo collegato a {{site.data.keyword.iot_short_notm}}, utilizza uno dei seguenti URL:

### Richiesta POST non sicura per la pubblicazione di eventi

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Richiesta POST sicura per la pubblicazione di eventi

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Note importanti:**
- La porta 443, la porta SSL predefinita, può anche essere specificata per le chiamate API HTTP sicure.
- Se un gateway non è assegnato al ruolo *Gateway standard*, può pubblicare gli eventi al posto di tutti i dispositivi nell'organizzazione. Se il dispositivo collegato al gateway non è registrato, il gateway automaticamente registra tale dispositivo.
- Assegna il ruolo *Gateway standard* se desideri controllare i livelli di autorizzazione.

Per ulteriori informazioni sul ruolo dei gateway e dei gruppi di risorse, consulta [Controllo dell'accesso al gateway](../gateways/gateway-access-control.html).

## Ricezione di comandi
{: #receive_commands}

Oltre a utilizzare il protocollo di messaggistica MQTT, puoi anche configurare i tuoi dispositivi gateway per ricevere comandi da {{site.data.keyword.iot_short_notm}} tramite HTTP utilizzando i comandi dell'API di messaggistica HTTP. Un dispositivo gateway può ricevere i comandi indirizzati ai dispositivi nel suo gruppo di risorse associato. Per ulteriori informazioni sui gruppi di risorse dei gateway, consulta [Controllo dell'accesso al gateway](../gateways/gateway-access-control.html).

Usa uno dei seguenti URL per inoltrare una richiesta ``POST`` da un gateway connesso a {{site.data.keyword.iot_short_notm}}:

### Richiesta POST non sicura per la ricezione di comandi

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Richiesta POST sicura per la ricezione di comandi

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Nota:** la porta 443, la porta SSL predefinita, può anche essere specificata per le chiamate API HTTP sicure.

Puoi, facoltativamente, includere il parametro *waitTimeSecs* nel corpo della richiesta HTTP per specificare un numero intero che rappresenta il numero massimo di secondi di attesa per un comando:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Note importanti:**
- Il valore di *waitTimeSecs* deve essere un numero intero nell'intervallo 0 - 3600. Il valore predefinito è 0.
- Per ricevere comandi da qualsiasi tipo di dispositivo, utilizza il carattere jolly "qualsiasi" (+) per il componente `typeId`. Se viene utilizzato il carattere jolly, il tipo di dispositivo è contenuto nel campo dell'intestazione della risposta *X-deviceType*.
- Per ricevere comandi per da qualsiasi dispositivo, utilizza il carattere jolly "qualsiasi" (+) per il componente `deviceId`. Se viene utilizzato il carattere jolly, l'identificativo del dispositivo è contenuto nel campo dell'intestazione della risposta *X-deviceId*.
- Per ricevere qualsiasi comando, utilizza il carattere jolly "qualsiasi" (+) per il componente `command`. Se viene utilizzato il carattere jolly, l'identificativo del comando è contenuto nel campo dell'intestazione della risposta *X-commandId*.
- Se il codice di stato della risposta HTTP è 200, i dati del comando sono contenuti nel corpo della risposta. Esamina il campo dell'intestazione della risposta *Content-Type* per trovare il tipo di contenuto.
- Se il codice di stato della risposta HTTP è 204, non è disponibile alcun dato del comando.

Per accedere alle API REST HTTP di {{site.data.keyword.iot_short_notm}}, vedi [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icona link esterno](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

Per accedere alle API di messaggistica HTTP di {{site.data.keyword.iot_short_notm}}, vedi [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![Icona link esterno](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

## Ultima cache evento
{: #last-event-cache}

Puoi utilizzare l'ultima cache evento per memorizzare le informazioni relative all'ultimo evento inviato a {{site.data.keyword.iot_short_notm}} da un dispositivo connesso. Utilizzando un'[API ![Icona link esterno](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}, puoi richiamare l'ultimo valore registrato di un id-evento specifico per un dispositivo o l'ultimo valore registrato per ogni id-evento segnalato da un dispositivo specifico. 

### Configurazione dell'ultima cache evento

La funzione di ultima cache evento è disabilitata per impostazione predefinita, ma puoi abilitare la funzionalità nei seguenti modi: 

-	Imposta il parametro *enabled* su **true** utilizzando un'API.
-	Nella pagina *Settings* del dashboard {{site.data.keyword.iot_short_notm}}, imposta **Enable LEC** su **On**.

Il periodo di tempo in cui i dati dell'evento sono memorizzati nella cache è specificato dal valore TTL (time to live). Per impostazione predefinita, i dati dell'ultimo evento per ogni evento specifico vengono memorizzati per sette giorni.  Puoi modificare la durata utilizzando un'API per aggiornare il parametro **ttlDays** o selezionando un valore per il campo **Event Data TTL** nella pagina *Settings* del dashboard {{site.data.keyword.iot_short_notm}}.

Per i piani Lite, puoi memorizzare le informazioni per un minimo di un giorno e un massimo di sette giorni. Per gli altri piani, puoi memorizzare le informazioni per un minimo di un giorno e un massimo di 45 giorni.

Utilizza le seguenti informazioni come supporto per configurare le impostazioni dell'ultima cache evento utilizzando le API.

Per richiamare la configurazione corrente dell'ultima cache evento per un'organizzazione, utilizza la seguente API:

```
    GET /api/v0002/config/lec
```   

   
Il seguente esempio mostra una risposta positiva al metodo GET:
```
{
    "enabled": false,
    "ttlDays": 7
}
```
Qualsiasi persona o applicazione in un'organizzazione è autorizzata ad accedere a questo endpoint. 

Se l'ultima cache evento viene disabilitata per un'organizzazione dall'amministratore di {{site.data.keyword.iot_short_notm}}, viene restituito il seguente codice di stato HTTP 403 FORBIDDEN in risposta al metodo GET:

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: Last event cache is disabled for this organization. For more information, contact your support team."
}
```

Se l'ultima cache evento non è abilitata, viene restituito un codice di stato HTTP 404 in risposta al metodo GET. Per abilitare la funzione di ultima cache evento per un'organizzazione, utilizza la seguente API:

```
    PUT /api/v0002/config/lec
```

con il seguente payload di esempio: 
```
{
    "enabled": true,
    "ttlDays": 5
}
```
dove: 

- **enabled** è obbligatorio e specifica se l'ultima cache evento è abilitata per un'organizzazione. Il valore deve essere un valore booleano. 

- **ttlDays** è facoltativo e specifica il numero di giorni in cui un evento viene conservato nell'ultima cache evento per un'organizzazione. Il valore deve essere un numero intero compreso tra 1 e 45 inclusi.
   
 
Solo l'amministratore, gli utenti operatore o le applicazioni dell'operatore sono autorizzati ad accedere a questo endpoint. Se i parametri o il JSON non sono validi, viene restituito un codice di stato HTTP 401 BAD REQUEST in risposta al metodo PUT. 

**Nota:** una modifica della configurazione può richiedere fino a 30 secondi.

### Richiamo di informazioni dall'ultima cache evento

Utilizzando un'API, puoi richiamare l'ultimo evento che è stato inviato da un dispositivo. Puoi richiamare lo stato del dispositivo indipendentemente dalla sua ubicazione fisica o dal suo stato di utilizzo. Puoi richiamare l'ultimo valore registrato di un ID evento per un dispositivo specifico o l'ultimo valore registrato per ogni ID evento che è stato riportato da un dispositivo specifico. Gli ultimi dati evento possono essere richiamati per ogni evento specifico verificatosi negli ultimi 45 giorni.

Per richiedere il valore più recente di un ID evento specifico, utilizza la seguente richiesta API, che restituisce l'ultimo valore registrato per l'ID evento “power”.

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

La risposta viene restituita nel seguente formato JSON:

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**Nota:** mentre la risposta API è nel formato JSON, i payload dell'evento possono essere scritti in qualsiasi formato. I payload che vengono restituiti dall'API Ultima cache evento sono codificati in base64.

Per richiedere il valore più recente di ogni ID evento che è stato riportato da un dispositivo, utilizza la seguente richiesta API:

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

La risposta include tutti gli ID evento che sono stati inviati dal dispositivo. Nel seguente esempio, i valori vengono restituiti per gli eventi “power” e “temperature”.

```
[
    {
        "deviceId": "<device-id>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

Per accedere alle API Ultima cache evento di {{site.data.keyword.iot_short_notm}}, vedi [{{site.data.keyword.iot_short_notm}} HTTP Information Management API ![Icona link esterno](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}.
