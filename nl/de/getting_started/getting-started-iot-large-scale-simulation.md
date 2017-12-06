---

copyright:
  years: 2017
lastupdated: "2017-11-08"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Anleitung 4: Große Anzahl von Geräten simulieren
In der ersten Anleitung haben Sie einen Basisgerätesimulator konfiguriert, um manuell einzelne oder mehrere Laufbänder zu simulieren. In dieser Anleitung wird auf dieser Simulation aufgebaut, indem eine große Anzahl selbstausführender Simulatoren zu Ihrer Umgebung hinzugefügt wird, um die Analyse und die Überwachung in einer realistischeren Umgebung mit mehreren Geräten zu testen.
{:shortdesc}

**Wichtig:** Für die Anwendung sind 512 MB Speicherplatz erforderlich, was das standardmäßig zugeordnete Speicherplatzvolumen und auch das Speicherplatzvolumen überschreitet, das für kostenlose Testkonten (einschließlich Bluemix-Testkonto und -Standardkonto) bereitgestellt wird. Inhaber von Subskriptionskonten und nutzungsabhängigen Konten können den zugeordneten Speicherplatz auf 512 MB erhöhen. Inhaber kostenloser Testkonten müssen ihr System auf ein Subskriptionskonto oder ein nutzungsabhängiges Konto aufrüsten. Weitere Informationen zu den {{site.data.keyword.Bluemix_notm}}-Kontotypen finden Sie in [Kontotypen](/docs/pricing/index.html#pricing).

## Übersicht und Ziel
{: #overview}

In dieser Anleitung werden Sie eine {{site.data.keyword.Bluemix_notm}}-Anwendung für die Simulation mehrerer Laufbandgeräte mit einem Back-End-System von [Node-RED ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://nodered.org){: new_window} einrichten.

Die Anwendung enthält drei Abläufe zur Durchführung der folgenden Operationen:   
1. Erstellen mehrerer Geräte.   
2. Simulieren der parallelen Ereignispublikation.
3. Löschen der Geräte.   

![Geräte erstellen und löschen](images/device_creation.png "Geräte erstellen und löschen")

![Simulation von Geräteereignissen](images/device_event_simulation.png "Simulation von Geräteereignissen")

Wenn Sie die Anweisungen in der vorliegenden Anleitung ausführen, dann gehen Sie wie folgt vor:
- Verwenden Sie Cloud Foundry zur Bereitstellung einer Node-RED-basierten und webhookfähigen Anwendung für einen Gerätesimulator.
- Verwenden Sie API-Aufrufe zur Erstellung und Registrierung von Geräten, zum Publizieren von Geräteereignissen und zum Löschen von Geräten.

**Wichtig:** Die Simulation einer großen Anzahl von Geräten, die gleichzeitig Gerätedaten an {{site.data.keyword.iot_short_notm}} senden, kann mit dem Verbrauch großer Datenvolumen verbunden sein. Sie können das Dashboard {{site.data.keyword.iot_short_notm}} *Nutzung* verwenden, um zu überwachen, welche Datenvolumen von Ihren Geräten und Anwendungen verbraucht werden. Die Metriken werden in Intervallen von zwei Stunden aktualisiert.

## Voraussetzungen
{: #prereqs}  
Sie benötigen die folgenden Konten und Tools:

* [{{site.data.keyword.Bluemix_notm}}-Konto](https://console.ng.bluemix.net/registration/) mit folgenden Merkmalen:    
 - Mehr als 512 MB Arbeitsspeicher (RAM)   
 - Mehr als 1 GB Datenträgerkontingent  
 - Zwei verfügbare Services
* [Cloud Foundry Command Line Interface (cf CLI) ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Verwenden Sie die Cloud Foundry-CLI (cf CLI) zum Bereitstellen und Verwalten Ihrer {{site.data.keyword.Bluemix_notm}}-Anwendungen.
* Optional: [Git ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://git-scm.com/downloads){: new_window}  
Wenn Sie sich für den Einsatz von Git zum Herunterladen der Codebeispiele entscheiden, dann benötigen Sie außerdem ein [GitHub.com-Konto ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com){: new_window}. Sie können den Code auch als komprimierte Datei herunterladen. Dazu ist kein GitHub.com-Konto erforderlich.

Für alle in den nachfolgenden Schritten durchgeführten REST-Aufrufe können Sie entweder cURL oder das Add-on-Plug-in des REST-Clients verwenden, das in Mozilla verfügbar ist.  
{: tip}

## Schritt 1 - Anwendung für {{site.data.keyword.Bluemix_notm}} bereitstellen
{: #step1}

Führen Sie die folgenden Schritte aus, um Ihre App manuell zu erstellen und bereitzustellen.   

1. Klonen Sie das GitHub-Repository der Beispielapp, das in *Lektion 4* beschrieben wird.  
Verwenden Sie das bevorzugte Git-Tool, um das folgende Repository zu klonen:  
https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
Verwenden Sie in der Git-Shell den folgenden Befehl:
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
```
3. Konfigurieren Sie die Anwendung für Ihre Umgebung, indem Sie die Datei 'manifest.yml' bearbeiten.  
Zu bearbeitende Elemente:
 - Zur Verwendung eines bereits vorhandenen {{site.data.keyword.iot_short_notm}}-Service müssen Sie alle Instanzen von `lesson4-simulate-iotf-service` aktualisieren, sodass dort der entsprechende Servicename angegeben wird. Wenn Sie z. B. den {{site.data.keyword.iot_short_notm}}-Service aus Anleitung 1 verwenden, dann benutzen Sie als Servicenamen `iotp-for-conveyor`.    
 - Legen Sie den Gerätenamen und den Host fest.   
Ändern Sie im Anwendungsabschnitt die Einträge für `name` und `host` und geben Sie dabei einen eindeutigen Wert an. Beispiel: `YOUR_NAME-lesson4-simulate`.   
**Tipp:** Die Angabe für ROUTES URL, die Sie für den Zugriff auf die App verwenden, wird mithilfe des Eintrags `host` erstellt. Beispiel: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     Bezeichnung: cloudantNoSQLDB
     Plan: Lite
    lesson4-simulate-iotf-service:
     Bezeichnung: iotf-service
     Plan: iotf-service-free
Anwendungen:  </br>
  -  Pfad: .
      Speicher: 512M
      Instanzen: 1
      Domäne: mybluemix.net
      Name: lesson4-simulate
      Host: lesson4-simulate
      Datenträgerkontingent: 1024M</br>
  Services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
1. Legen Sie über die Befehlszeile den API-Endpunkt fest, indem Sie den Befehl 'cf api' ausführen.   
Ersetzen Sie dabei den Wert für `API-ENDPOINT` durch den API-Endpunkt Ihrer Region.
   ```
cf api API-ENDPOINT
   ```
Beispiel: `cf api https://api.ng.bluemix.net`  
<table>
<tr>
<th>Region</th>
<th>API-Endpunkt</th>
</tr>
<tr>
<td>Vereinigte Staaten (Süden)</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>Großbritannien</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto an.
  ```
cf login
  ```
Wenn Sie vom System dazu aufgefordert werden, dann wählen Sie die Organisation und den Bereich, in denen Sie {{site.data.keyword.iot_short_notm}} bereitstellen wollen, und außerdem die Beispielapp aus.
6. Erstellen Sie die erforderlichen Services in {{site.data.keyword.Bluemix_notm}}.   
 1. Verwenden Sie den folgenden Befehl, um den Lite-Service 'cloudantNoSQLDB' zu erstellen:
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. Verwenden Sie den folgenden Befehl, um den {{site.data.keyword.iot_short_notm}}-Service zu erstellen.
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**Wichtig:** Wenn Sie einen vorhandenen {{site.data.keyword.iot_short_notm}}-Service verwenden und die Datei 'manifest.yml' entsprechend aktualisiert haben, dann vergewissern Sie sich, dass Sie den bereits vorhandenen Servicenamen verwenden. Verwenden Sie z. B. aus Anleitung 1 anstelle von `lesson4-simulate-iotf-service` im cf-Aufruf die Angabe `iotp-for-conveyor`.
7. Führen Sie den Befehl `cf push` aus, um das Projekt zu erstellen und mit einer Push-Operation an Ihre Organisation zu übertragen.  
Ihre Beispielanwendung wird unter {{site.data.keyword.Bluemix_notm}} bereitgestellt.  
Nach Abschluss der Bereitstellung wird eine Nachricht angezeigt, in der Sie darüber informiert werden, dass Ihre App nun aktiv ist.   
Beispiel:  
  ```
Angeforderter Status: started
Instanzen: 1/1
Verwendung: 512M x 1 instances
URLs: lesson4-simulate.mybluemix.net
Letztes Hochladen: Tue May 30 13:22:17 UTC 2017
Stack: cflinuxfs2
Buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     Status    Seite                    CPU     Speicher         Platte
Details
#0   aktiv     2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. Rufen Sie die Serviceberechtigungsnachweise für Ihre App ab.
 1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an:  
 [https://bluemix.net ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://bluemix.net){: new_window}.
 2. Wählen Sie das Konto und den Bereich aus, unter denen die App bereitgestellt wurde.
 3. Wählen Sie im Menü **Apps** und dann **Dashboard** aus.
 4. Klicken Sie unter 'Cloud Foundry-Apps' auf den Namen der Anwendung, die Sie soeben bereitgestellt haben.
 4. Wählen Sie **Verbindungen** aus.
 5. Suchen Sie das Element 'iotf-service', das Sie für Ihre App benutzen, und klicken Sie dann auf **Berechtigungsnachweise anzeigen**.  
 6. Suchen Sie auf der Seite für die Serviceberechtigungsnachweise nach `apiKey` und `apiToken`.   
Der API-Schlüssel und das Authentifizierungstoken werden als Berechtigungsnachweise eingesetzt, wenn Sie die API der App zum Erstellen und Ausführen der simulierten Geräte verwenden.

## Schritt 2 - Node-RED-Ablauf sichern
{: #step2}

Obwohl dieser Schritt optional ist, hat es sich bewährt, den Node-RED-Ablauf zu sichern.

1. Sichern Sie den Editor, sodass nur berechtigte Benutzer darauf zugreifen können.
2. Rufen Sie zum Sichern des Node-RED-Ablaufs das {{site.data.keyword.Bluemix_notm}}-Dashboard auf und wählen Sie die Anwendung aus, die Sie zuvor bereitgestellt haben.
3. Klicken Sie auf **Laufzeit** und dann auf **Umgebungsvariablen**.
4. Fügen Sie die folgenden benutzerdefinierten Umgebungsvariablen und die entsprechenden Werte hinzu:
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. Speichern Sie den Ablauf.

Wenn Sie den Einsatz von REST-API-Befehlen zum Erstellen, Löschen oder Simulieren mehrerer Geräte planen, dann müssen Sie nach der Sicherung Ihres Node-RED-Ablaufs den Node-RED-Benutzernamen und das entsprechende Kennwort während der Basisauthentifizierung als Berechtigungsnachweise angeben.

## Schritt 3 - Geräte erstellen und verbinden
{: #step3}

Sie können die Schnittstelle des Node-RED-Ablaufs oder die REST-API der Anwendung benutzen, um die folgenden Tasks auszuführen.

### Node-RED  
Gehen Sie wie folgt vor, um mehrere Geräte zu registrieren:  
1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an:  
[https://bluemix.net ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://bluemix.net){: new_window}.
2. Wählen Sie das Konto und den Bereich aus, unter denen die App bereitgestellt wurde.
3. Wählen Sie im Menü **Apps** und dann **Dashboard** aus.
4. Klicken Sie unter 'Cloud Foundry-Apps' auf die URL **ROUTE** der Anwendung, die Sie soeben bereitgestellt haben.  
Die Angabe für ROUTE_URL wird auf Basis des Eintrags `host` erstellt, den Sie in der Datei 'manifest.yml' verwendet haben: `HOST.mybluemix.net`  
Beispiel: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
Die Node-RED-Schnittstelle wird geöffnet.
5. Klicken Sie auf **Zu Node-RED-Ablaufeditor wechseln**.
6. Wählen Sie im Ablaufeditor die Registerkarte für **Gerätetyp und Instanz** aus.
7. Klicken Sie zum Registrieren von fünf Geräten auf den Einfügeknoten mit der Kennzeichnung **Register 5 motorController devices**.
8. Überprüfen Sie, ob Ihre Geräte registriert wurden.
 1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard im Menü die Option **Boards** aus.
 3. Wählen Sie das Board **Device Centric Analytics** aus.
 4. Suchen Sie die Karte **Für mich relevante Geräte**.  
Die Gerätenamen werden angezeigt.

### REST-API  
Gehen Sie wie folgt vor, um mehrere Geräte zu registrieren:  

1. Erstellen Sie eine HTTP POST-Anforderung an die folgende URL: `ROUTE_URL/rest/devices`.  
Beispiel: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - Geben Sie für 'Content-Type' und 'Accept' den Wert 'application/json' an.  
 - Verwenden Sie die folgenden JSON-Nutzdaten:  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
  Dabei gilt Folgendes:  
    - 'numberDevices' gibt die Anzahl der Geräte an, die erstellt und registriert werden sollen.
    - Optional: 'typeId' gibt den Gerätetyp an, unter dem die Geräte registriert werden. Wird 'typeId' nicht angegeben, dann wird standardmäßig "iotp-for-conveyor" verwendet. **Tipp:** Sie können einen beliebigen Gerätetypnamen eingeben, die anderen Anleitungen in der vorliegenden Reihe erwarten jedoch Geräte mit dem Gerätetyp `iotp-for-conveyor`. Wenn Sie einen anderen Gerätetyp verwenden wollen, dann müssen Sie die Einstellungen in den Anleitungen entsprechend ändern.
    - Optional: 'authToken' gibt das Berechtigungstoken an, mit dem die Geräte registriert werden.
    - Optional: Wird 'chunkSize' nicht angegeben, dann wird standardmäßig die Einstellung '500' verwendet. Für 'chunksize' muss ein Wert kleiner als 'numberDevices' angegeben werden, der außerdem einen Faktor von 'numberDevices' darstellt.
    - 'deviceName' gibt das Muster für 'deviceID' für die erstellten Geräte an.
2. Überprüfen Sie, ob Ihre Geräte registriert wurden.
 1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard im Menü die Option **Boards** aus.
 3. Wählen Sie das Board **Device Centric Analytics** aus.
 4. Suchen Sie die Karte **Für mich relevante Geräte**.  
Die Gerätenamen werden angezeigt.

## Schritt 4 - Geräteereignisse simulieren
{: #step4}

Da die simulierten Geräte bei {{site.data.keyword.iot_short_notm}} registriert sind, können Sie den Simulator jetzt ausführen, um mit dem Senden von Geräteereignissen zu beginnen.

### Node-RED  
Gehen Sie wie folgt vor, um Geräteereignisse zu senden:  
1. Wählen Sie im Node-RED-Ablaufeditor die Registerkarte zum **Simulieren mehrerer Geräte** aus.
7. Klicken Sie zum Simulieren von fünf Geräten auf den Einfügeknoten mit der Kennzeichnung **Simulate 5 devices**.
8. Überprüfen Sie, ob Ihre Geräte Daten senden.
 1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard im Menü die Option **Boards** aus.
 3. Wählen Sie das Board **Device Centric Analytics** aus.
 4. Suchen Sie die Karte **Für mich relevante Geräte**.  
 5. Wählen Sie eines der Geräte aus und überprüfen Sie, ob die aktualisieren Gerätedatenpunkte, die der publizierten Nachricht entsprechen, auf der Karte **Geräteeigenschaften** angezeigt werden.
  


### REST-API  
Gehen Sie wie folgt vor, um Geräteereignisse zu senden:

1. Erstellen Sie eine HTTP POST-Anforderung an die folgende URL: `ROUTE_URL/rest/runtest`.  
Beispiel: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - Geben Sie für 'Content-Type' und 'Accept' den Wert 'application/json' an.  
 - Verwenden Sie die folgenden JSON-Nutzdaten:   
<pre><code>{  </br>
"numberDevices":5, </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
Dabei gilt:
    - 'numberDevices' gibt die Anzahl der Geräte an, für die Daten simuliert werden sollen.
    - 'numberEvents' gibt die Anzahl der Ereignisse an, die von jedem der simulierten Geräte gesendet werden.
    - 'timeInterval' gibt den Abstand der Ereignisse in Millisekunden an.
    - 'deviceType' ist der Gerätetyp, für den Sie simulierte Geräte erstellt haben.
    - 'deviceName' ist das Muster für 'deviceID' für die erstellten Geräte.
8. Überprüfen Sie, ob Ihre Geräte Daten senden.
 1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard im Menü die Option **Boards** aus.
 3. Wählen Sie das Board **Device Centric Analytics** aus.
 4. Suchen Sie die Karte **Für mich relevante Geräte**.  
 5. Wählen Sie eines der Geräte aus und überprüfen Sie, ob die aktualisieren Gerätedatenpunkte, die der publizierten Nachricht entsprechen, auf der Karte **Geräteeigenschaften** angezeigt werden.   

## Schritt 5 - Geräte löschen
{: #deleting}

### Node-RED  
Gehen Sie wie folgt vor, um Geräte zu löschen:  
1. Wählen Sie im Node-RED-Ablaufeditor die Registerkarte für **Gerätetyp und Instanz** aus.
2. Klicken Sie zum Löschen von fünf Geräten auf den Einfügeknoten mit der Kennzeichnung **Delete 5 devices**.
3. Überprüfen Sie, ob Ihre Geräte gelöscht wurden.
 1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard im Menü die Option **Boards** aus.
 3. Wählen Sie das Board **Device Centric Analytics** aus.
 4. Suchen Sie die Karte **Für mich relevante Geräte**.  
 5. Überprüfen Sie, dass die Geräte nicht mehr aufgelistet werden.  


### REST-API  

1. Erstellen Sie eine HTTP POST-Anforderung an die folgende URL: `ROUTE_URL/rest/deleteDevices`.  
Beispiel: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - Geben Sie für 'Content-Type' und 'Accept' den Wert 'application/json' an.  
 - Verwenden Sie die folgenden JSON-Nutzdaten:      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName": "belt"  
}</code></pre>
2. Überprüfen Sie, ob Ihre Geräte gelöscht wurden.
 1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard im Menü die Option **Boards** aus.
 3. Wählen Sie das Board **Device Centric Analytics** aus.
 4. Suchen Sie die Karte **Für mich relevante Geräte**.  
 5. Überprüfen Sie, dass die Geräte nicht mehr aufgelistet werden.  


## Weitere Schritte
{: @whats_next}  
Springen Sie zu einem anderen Thema, das für Sie von Interesse ist:
- [Anleitung 2: Grundlegende Echtzeitregeln und -aktionen verwenden](getting-started-iot-rules.html)  
Nachdem Sie das Laufband erfolgreich konfiguriert, es mit {{site.data.keyword.iot_short_notm}} verbunden und Daten gesendet haben, sollten Sie mit der Nutzung der Daten beginnen, indem Sie Regeln und Aktionen verwenden.

- [Anleitung 3: Gerätedaten überwachen](getting-started-iot-monitoring.html)  
Nachdem Sie nun mindestens ein Gerät verbunden und mit der effizienten Nutzung der Gerätedaten begonnen haben, sollten Sie nun eine Gruppe von Geräten überwachen.

- [Weitere Informationen zu {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Weitere Informationen zu {{site.data.keyword.iot_short_notm}}-APIs](/docs/services/IoT/reference/api.html){:new_window}
