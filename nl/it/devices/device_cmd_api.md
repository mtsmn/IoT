---

copyright:
 years: 2015, 2017
lastupdated: "2017-06-08"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API di messaggistica HTTP per i dispositivi (Beta)
{: #api}

**Importante:** l'API di messaggistica HTTP {{site.data.keyword.iot_full}} per la funzione dei dispositivi è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.


## Accesso alla documentazione dell'API di messaggistica HTTP per i dispositivi
{: #rest_messaging_api}

Per accedere alla documentazione dell'API di messaggistica HTTP {{site.data.keyword.iot_short_notm}}, consulta [API di messaggistica HTTP {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Connessioni client
{: #client_connections}

Per informazioni sulla sicurezza client e su come connettere i client ai dispositivi in {{site.data.keyword.iot_short_notm}}, consulta [Connessione di applicazioni, dispositivi e gateway a {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

## Pubblicazione eventi
{: #event_publication}

In aggiunta all'utilizzo del protocollo di messaggistica MQTT, puoi anche configurare i tuoi dispositivi a pubblicare eventi {{site.data.keyword.iot_short_notm}} in HTTP utilizzando i comandi API REST HTTP.

Utilizza uno dei seguenti URL per inviare una richiesta `POST` da un dispositivo collegato a {{site.data.keyword.iot_short_notm}}:

### Richiesta POST non sicura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Richiesta POST sicura

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Nota:** la porta 443, la porta SSL predefinita, può anche essere specificata per le chiamate API HTTP sicure.

### Autenticazione

Tutte le richieste devono includere un'intestazione di autorizzazione. L'autenticazione di base è l'unico metodo supportato. Quando un dispositivo effettua una richiesta HTTP tramite l'API REST HTTP {{site.data.keyword.iot_short_notm}}, sono necessarie le seguenti credenziali:

|Credenziale|Input richiesto|
|:---|:---|
|Nome utente|`use-token-auth`
|Password| Il token di autenticazione che è stato generato automaticamente o specificato manualmente quando hai registrato il dispositivo.


### Intestazioni richiesta per tipo di contenuto

Deve essere fornita un'intestazione della richiesta `Content-Type` con la richiesta. La seguente tabella mostra come i tipi supportati sono associati ai formati interni di {{site.data.keyword.iot_short_notm}}.

|Intestazione Content-Type|Formato in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|testo/semplice|"text"
|applicazione/json| "json"
|applicazione/xml | "xml"
|applicazione/octet-stream|"bin"


## Ricezione di comandi
{: #receive_commands}

Oltre a utilizzare il protocollo di messaggistica MQTT, puoi anche configurare i tuoi dispositivi per ricevere comandi da {{site.data.keyword.iot_short_notm}} su HTTP utilizzando i comandi della API di messaggistica HTTP. Un dispositivo può ricevere i comandi a esso indirizzati.

Utilizza uno dei seguenti URL per inviare una richiesta `POST` da un dispositivo collegato a {{site.data.keyword.iot_short_notm}}:

### Richiesta POST non sicura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Richiesta POST sicura

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Nota:** la porta 443, la porta SSL predefinita, può anche essere specificata per le chiamate API HTTP sicure.

Per ricevere un comando da {{site.data.keyword.iot_short_notm}}, usa la seguente API:

<pre class="pre"><code class="hljs">/device/types/{deviceType}/devices/{deviceId}/commands/{command}/request</code></pre>

Puoi, facoltativamente, includere il parametro *waitTimeSecs* nel corpo della richiesta HTTP per specificare un numero intero che rappresenta il numero massimo di secondi di attesa per un comando:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Note importanti:**
- Il valore di *waitTimeSecs* deve essere un numero intero nell'intervallo 0 - 3600. Il valore predefinito è 0.
- Per ricevere qualsiasi comando, utilizza il carattere jolly "qualsiasi" (+) per il componente {command}. Se viene utilizzato il carattere jolly, l'identificativo del comando è contenuto nel campo dell'intestazione della risposta *X-commandId*.
- Se il codice di stato della risposta HTTP è 200, i dati del comando sono contenuti nel corpo della risposta. Esamina il campo dell'intestazione della risposta *Content-Type* per trovare il tipo di contenuto.
- Se il codice di stato della risposta HTTP è 204, non è disponibile alcun dato del comando.


Per informazioni sullo sviluppo di dispositivi, consulta [Sviluppo dei dispositivi su {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).
