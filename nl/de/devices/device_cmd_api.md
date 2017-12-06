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

# HTTP-Messaging-APIs für Geräte
{: #api}


## Zugriff auf die Dokumentation für 'HTTP-Messaging-API für Geräte'
{: #rest_messaging_api}

Weitere Informationen zum Zugriff auf die Dokumentation der {{site.data.keyword.iot_short_notm}} HTTP-Messaging-API finden Sie unter [{{site.data.keyword.iot_short_notm}} HTTP-Messaging-API ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Clientverbindungen
{: #client_connections}

Informationen zur Clientsicherheit und zur Vorgehensweise beim Herstellen von Verbindungen zwischen Clients und Geräten in {{site.data.keyword.iot_short_notm}} finden Sie in [Anwendungen, Geräte und Gateways mit {{site.data.keyword.iot_short_notm}} verbinden](../reference/security/connect_devices_apps_gw.html).

## Ereignisse publizieren
{: #event_publication}

Zusätzlich zum MQTT-Nachrichtenprotokoll können Sie Ihre Geräte auch so konfigurieren, dass Ereignisse in {{site.data.keyword.iot_short_notm}} über HTTP mithilfe von HTTP-REST-API-Befehlen publiziert werden.

Verwenden Sie eine der folgenden URLs, um eine ``POST``-Anforderung von einem Gerät zu übergeben, das mit {{site.data.keyword.iot_short_notm}} verbunden ist:

### Nicht sichere POST-Anforderung
<pre class="pre"><code class="hljs">http://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/events/<var class="keyword varname">Ereignis-ID</var></code></pre>

### Sichere POST-Anforderung

<pre class="pre"><code class="hljs">https://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/events/<var class="keyword varname">Ereignis-ID</var></code></pre>

**Hinweis:** Port 443, der SSL-Standardport, kann auch für sichere HTTP-API-Aufrufe angegeben werden.

### Authentifizierung

Alle Anforderungen müssen einen Berechtigungsheader enthalten. Die Basisauthentifizierung ist die einzige Methode, die unterstützt ist. Wenn ein Gerät eine HTTP-Anforderung über die {{site.data.keyword.iot_short_notm}}-HTTP-REST-API übergibt, sind folgende Berechtigungsnachweise erforderlich:

|Berechtigungsnachweis|Erforderliche Eingabe|
|:---|:---|
|Benutzername|`use-token-auth`
|Kennwort| Das Authentifizierungstoken, das beim Registrieren des Geräts entweder automatisch generiert oder manuell angegeben wurde.


### Anforderungsheader des Typs 'Content-Type'

Zusammen mit der Anforderung muss ein Anforderungsheader des Typs `Content-Type` ('Inhaltstyp') angegeben werden, wenn es sich nicht um JSON-Inhalt handelt. Die folgende Tabelle zeigt die Zuordnung der unterstützten Typen zu den internen {{site.data.keyword.iot_short_notm}}-Formaten.

|Header des Typs 'Content-Type'|Format in {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"


## Befehle empfangen
{: #receive_commands}

Zusätzlich zur Verwendung des MQTT-Nachrichtenprotokolls können Sie Ihre Geräte auch so konfigurieren, dass Befehle von {{site.data.keyword.iot_short_notm}} über HTTP mithilfe von HTTP-Messaging-API-Befehlen empfangen werden. Ein Gerät kann Befehle empfangen, die an das Gerät selbst gerichtet sind.

Verwenden Sie eine der folgenden URLs, um eine ``POST``-Anforderung von einem Gerät zu übergeben, das mit {{site.data.keyword.iot_short_notm}} verbunden ist:

### Nicht sichere POST-Anforderung
<pre class="pre"><code class="hljs">http://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/commands/<var class="keyword varname">Befehl</var>/request</code></pre>

### Sichere POST-Anforderung

<pre class="pre"><code class="hljs">https://<var class="keyword varname">Organisations-ID</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">Typ-ID</var>/devices/<var class="keyword varname">Geräte-ID</var>/commands/<var class="keyword varname">Befehl</var>/request</code></pre>

**Hinweis:** Port 443, der SSL-Standardport, kann auch für sichere HTTP-API-Aufrufe angegeben werden.

Optional können Sie den Parameter *waitTimeSecs* in den Hauptteil der HTTP-Anforderung einschließen, um eine Ganzzahl für die maximale Anzahl Sekunden anzugeben, für die auf einen Befehl gewartet werden soll:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Wichtige Hinweise:**
- Der Wert von *waitTimeSecs* muss eine Ganzzahl zwischen 0 - 3600 Sekunden sein. Der Standardwert ist '0'.
- Um alle Befehle empfangen zu können, verwenden Sie für die Komponente {Befehl} das Platzhalterzeichen für "beliebig" (+). Wird das Platzhalterzeichen verwendet, so ist die Befehls-ID im Antwortheaderfeld *X-commandId* enthalten.
- Wenn der Statuscode der HTTP-Antwort 200 ist, dann sind die Befehlsdaten im Hauptteil der Antwort enthalten. Suchen Sie im Antwortheaderfeld *Inhaltstyp* nach dem Inhaltstyp.
- Wenn der Statuscode der HTTP-Antwort 204 ist, dann sind keine Befehlsdaten verfügbar.


Informationen von Entwickeln von Geräten finden Sie unter [In {{site.data.keyword.iot_short_notm}}Geräte entwickeln](../devices/device_dev_index.html).
