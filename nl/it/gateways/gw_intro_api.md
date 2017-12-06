---

copyright:
 years: 2015, 2017
lastupdated: "2017-10-04"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# API di messaggistica HTTP per i dispositivi gateway
{: #api}

## Accesso alla documentazione dell'API di messaggistica HTTP per i dispositivi gateway
{: #rest_messaging_api}

Per accedere alla documentazione dell'API di messaggistica HTTP {{site.data.keyword.iot_short_notm}} e trovare più informazioni sull'invio degli eventi da dispositivi gateway, consulta [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![Icona link esterno](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Connessioni client
{: #client_connections}

Per informazioni sulla sicurezza client e su come connettere i client ai dispositivi client e gateway in {{site.data.keyword.iot_short_notm}}, consulta [Connessione di applicazioni, dispositivi e gateway a {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).


## Pubblicazione eventi
{: #event_publication}

In aggiunta all'utilizzo del protocollo di messaggistica MQTT, puoi anche configurare i tuoi dispositivi a pubblicare eventi {{site.data.keyword.iot_short_notm}} tramite HTTP utilizzando o i comandi dell'API di messaggistica HTTP.

Per inoltrare una richiesta ``POST`` da un dispositivo collegato a {{site.data.keyword.iot_short_notm}}, utilizza uno dei seguenti URL:

### Richiesta POST non sicura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Richiesta POST sicura
<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Note importanti:**
- Puoi inoltrare gli eventi del dispositivo gateway solo utilizzando la messaggistica HTTP. Utilizza il protocollo di messaggistica MQTT per inviare richieste per la gestione di un altro dispositivo gateway o per le funzioni di controllo.
- La porta 443, la porta SSL predefinita, può anche essere specificata per le chiamate API HTTP sicure.
- Se un gateway non è assegnato al ruolo *Gateway standard*, può pubblicare gli eventi al posto di tutti i dispositivi nell'organizzazione. Se il dispositivo collegato al gateway non è registrato, il gateway automaticamente registra tale dispositivo.
- Assegna il ruolo *Gateway standard* se desideri controllare i livelli di autorizzazione.

Per ulteriori informazioni sul ruolo dei gateway e dei gruppi di risorse, consulta [Controllo dell'accesso al gateway (Beta)](../gateways/gateway-access-control.html).

### Autenticazione

Tutte le richieste devono includere un'intestazione di autorizzazione. L'autenticazione di base è l'unico metodo supportato. Quando un dispositivo effettua una richiesta HTTP utilizzando l'API REST HTTP {{site.data.keyword.iot_short_notm}}, sono necessarie le seguenti credenziali:

|Credenziale|Input richiesto|
|:---|:---|
|Nome utente| `g/{orgId}/{gwType}/{gwDevId}` or `g-{orgId}-{gwType}-{gwDevId}`
|Password| Il token di autenticazione che è stato generato automaticamente o specificato manualmente quando hai registrato il dispositivo gateway.

Dove:

<dl>
<dt>orgId</dt>  
<dd>Il nome dell'organizzazione, che deve corrispondere al nome specificato nell'intestazione host.</dd>

<p></p>
<dt>gwType</dt>  
<dd>Il tipo del gateway. </dd>
<dd>Se utilizzi il carattere trattino "-" come delimitatore nel nome utente, questo valore non deve includere un carattere trattino. </dd>
<p></p>
<dt>gwDevId</dt>  
<dd>L'identificativo del dispositivo del gateway. </dd>
</dl>


### Intestazioni richiesta per tipo di contenuto

Deve essere fornita un'intestazione della richiesta `Content-Type` con la richiesta se il contenuto non è JSON. La seguente tabella mostra come i tipi supportati sono associati ai formati interni di {{site.data.keyword.iot_short_notm}}:

|Intestazione Content-Type|Formato in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|testo/semplice|"text"
|applicazione/json| "json"
|applicazione/xml | "xml"
|applicazione/octet-stream|"bin"

### QOS (quality of service)

Simile al livello di sicurezza di distribuzione 0 di QOS (quality of service) MQTT "at most once", la messaggistica REST HTTP fornisce la fornitura del messaggio non persistente ma verifica che la richiesta sia corretta e che possa essere distribuita al server prima che invii la risposta HTTP. Un risposta che contiene un codice di stato HTTP di 200 conferma che il messaggio è stato distribuito al server. Quando utilizzi il livello di QOS (quality of service) MQTT "at most once" o l'equivalente HTTP per distribuire i messaggi dell'evento, il dispositivo o l'applicazione deve implementare la logica del nuovo tentativo per garantire la distribuzione.

Per ulteriori informazioni sul protocollo MQTT e sui livelli di QOS (quality of service) per {{site.data.keyword.iot_short_notm}}, consulta [Messaggistica MQTT](../reference/mqtt/index.html).

Per informazioni sulla gestione dei dispositivi gateway utilizzando le API, consulta [API REST HTTP per dispositivi gateway](../gateways/gw_api.html).

## Ricezione di comandi
{: #receive_commands}

Oltre a utilizzare il protocollo di messaggistica MQTT, puoi anche configurare i tuoi dispositivi gateway per ricevere comandi da {{site.data.keyword.iot_short_notm}} su HTTP utilizzando i comandi della API di messaggistica HTTP. Un dispositivo gateway può ricevere i comandi indirizzati ai dispositivi nel suo gruppo di risorse associato. Per ulteriori informazioni sui gruppi di risorse dei gateway, consulta [Controllo dell'accesso al gateway (Beta)](../gateways/gateway-access-control.html).

Usa uno dei seguenti URL per inoltrare una richiesta ``POST`` da un gateway connesso a {{site.data.keyword.iot_short_notm}}:

### Richiesta POST non sicura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Richiesta POST sicura

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
