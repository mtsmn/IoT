---

copyright:
  years: 2017
lastupdated: "2017-06-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Anleitung 3: Gerätedaten überwachen
Nachdem Sie nun eine Verbindung für mindestens ein Gerät hergestellt haben, sollten Sie nun mit der Überwachung der Gerätedaten in Echtzeit beginnen.
{:shortdesc}

## Übersicht und Ziel
{: #overview}  

In der vorliegenden Anleitung werden Sie eine Überwachungsanwendung unter {{site.data.keyword.Bluemix_notm}} bereitstellen, um Daten Ihrer Geräte anzuzeigen.

Wie bereits in Anleitung 1 können Sie einen oder beide der folgenden Lösungswege (Pfade) befolgen:
- Pfad A: [Schritt 1A - Webanwendung für Überwachung bereitstellen und verbinden.](#deploy_app)  
Wenn Sie Pfad A benutzen, dann werden Sie eine einsatzbereite App bereitstellen, die die in Ihrer Organisation ausgeführten Laufbandgeräte überwacht.
- Pfad B: [Schritt 1B - Überwachungsbenutzerschnittstelle mit Widgetbibliothek erstellen.](#widget-library)
Pfad B ist geringfügig komplexer und führt die Widgetbibliothek ein. Sie werden dabei durch einige der grundlegenden Aufgaben zur Erstellung von Benutzerschnittstellen geführt.

## Voraussetzungen
{: #prereqs}  
Bevor Sie beginnen, sollten Sie sich vergewissern, dass die folgenden Voraussetzungen erfüllt sind.

### Lokale Umgebung
- [Node.js ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://nodejs.org){: new_window} Version 6.x oder höher
- Pfad A: [Angular-CLI ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/angular/angular-cli){: new_window} Version 1.x oder höher  
- [Cloud Foundry-CLI ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Verwenden Sie die Cloud Foundry-CLI (cf-CLI) zur Bereitstellung von Apps und Services für {{site.data.keyword.Bluemix_notm}}. Weitere Informationen finden Sie in der [Dokumentation zur Cloud Foundry-CLI![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cloudfoundry.org/cf-cli/){: new_window}.  
- Optional: [Git ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://git-scm.com/downloads){: new_window}  
Wenn Sie sich für den Einsatz von Git zum Herunterladen der Codebeispiele entscheiden, dann benötigen Sie außerdem ein [GitHub.com-Konto ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com){: new_window}. Sie können den Code auch als komprimierte Datei herunterladen. Dazu ist kein GitHub.com-Konto erforderlich.

### Weitere Voraussetzungen
Sie müssen außerdem über ein verbundenes Gerät des Gerätetyps `iot-conveyor-belt` verfügen, das Ereignisse mit dem Ereignisnamen `sensorData` und mit Nachrichtennutzdaten sendet, die die folgenden Eigenschaften umfassen:
```
{
	"d": {
		"id": "belt1",
		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
Weitere Informationen zu den Geräteereignissen und Nachrichtenformaten finden Sie in [Ereignisse publizieren](/docs/services/IoT/devices/mqtt.html#publishing_events).  
Wenn Sie [Anleitung 1: Einführung zu {{site.data.keyword.iot_short_notm}} und zu einem simulierten Laufband](getting-started-iot-conveyor.html) ausgeführt haben, dann haben Sie alle Vorbereitungen getroffen.  
{: tip}

## Schritt 1A - Webanwendung für Überwachung bereitstellen und verbinden
{: #deploy_app}

Die Beispielapp für die Anlagenüberwachung listet alle Geräte vom Typ 'iot-conveyor-belt' auf, die mit Ihrer {{site.data.keyword.iot_short_notm}}-Organisation verbunden sind. Außerdem wird eine Teilmenge der Ereignisdaten wie z. B. Angaben zu RPM, zur letzten Aktualisierung und zur Geräte-ID aufgelistet.

Die Beispielapp wird mithilfe der Node.js-Clientbibliotheken unter [https://github.com/ibm-watson-iot/iot-nodejs ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window} erstellt.

![Node.js-basierte Überwachungsapp](images/app_monitor.png "Node.js-basierte Überwachungsapp")

Im Rahmen dieser Anleitung werden Sie die folgenden Arbeitsschritte ausführen:
- Bereitstellen einer Beispielwebanwendung für die Überwachung auf Basis einer GitHub-Quelle mithilfe von Cloud Foundry.
- Konfigurieren einer Beispielapp zur Herstellung einer Verbindung zu {{site.data.keyword.iot_short_notm}} mithilfe eines API-Schlüssels und eines Authentifizierungstokens.
- Verwenden der Webanwendung zur Überwachung Ihrer verbundenen Laufbandgeräte.  

### Detaillierte Schritte
Die folgenden Schritte führen Sie durch die Erstellung und Bereitstellung der App unter {{site.data.keyword.Bluemix_notm}}. Informationen zur lokalen Ausführung der App finden Sie in der Readme-Datei von GitHub.
1. Klonen Sie das GitHub-Repository der Node.js-Beispielapp für die *Anlagenüberwachung*.  
Verwenden Sie das bevorzugte Git-Tool, um das folgende Repository zu klonen:  
https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
Verwenden Sie in der Git-Shell den folgenden Befehl:
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
  ```
2. Erstellen Sie eine Kombination aus API-Schlüssel und Authentifizierungstoken für Ihre App.  
Sie benötigen diese Komponenten beim Konfigurieren der App für die Verbindung zu Ihrer Organisation. Weitere Informationen zur Registrierung von Geräten finden Sie in [Anwendungen verbinden](/docs/services/IoT/platform_authorization.html).  
 1. Öffnen Sie das {{site.data.keyword.iot_short_notm}}-Dashboard.
 2. Wählen Sie **Apps** aus.
 3. Klicken Sie auf **API-Schlüssel generieren**.
 4. Kopieren Sie die Werte für den API-Schlüssel und das Authentifizierungstoken, die aufgelistet werden.
 5. Wählen Sie als API-Rolle **Visualisierungsanwendung** aus.  
**Tipp:** Wenn Sie weitere Funktionen zur Anwendung hinzufügen, dann müssen Sie eine höhere Rolle angeben.
 6. Fügen Sie einen Kommentar hinzu, sodass Sie die Kombination aus API-Schlüssel und Authentifizierungstoken einfach identifizieren können.
 7. Klicken Sie auf **Generieren**.
3. Konfigurieren Sie Ihre App für die Herstellung einer Verbindung zu {{site.data.keyword.Bluemix_notm}}.
Navigieren Sie zum Stammelement des Repositorys *guide-conveyor-ui-angular* und erstellen Sie die Datei `basicConfig.json`, die den folgenden Inhalt hat:
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
Ersetzen Sie die Parameterwerte durch die entsprechenden Werte für Ihre {{site.data.keyword.Bluemix_notm}}-Organisation: orgID, API-Schlüssel und Authentifizierungstoken, die soeben erstellt wurden.  
Beispiel:
```
 {   
  "org": "3v5whr",    
  "apiKey": "a-3v5whr-jhkmsghlqv",  
  "apiToken": "-q0MkPN2cNYB6+?ISk"  
}
```
4. Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto über die Cloud Foundry-CLI an.  
Weitere Informationen finden Sie in der [Dokumentation zur Cloud Foundry-CLI![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cloudfoundry.org/cf-cli/){: new_window}.  
Geben Sie den folgenden Befehl an der Befehlszeile ein:  
  ```
cf login
  ```
Wenn Sie vom System dazu aufgefordert werden, dann wählen Sie die Organisation und den Bereich aus, in denen Sie die Überwachungsapp bereitstellen wollen.
5. Legen Sie bei Bedarf den API-Endpunkt fest, indem Sie den Befehl 'cf api' ausführen.   
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
6. Wechseln Sie in das Verzeichnis, in dem sich die Beispielapp befindet.  
  ```
cd guide-conveyor-ui-angular
  ```
7. Führen Sie `npm install -g @angular/cli` aus, um die Angular-CLI zu installieren.
8. Führen Sie `npm install` aus.
9. Führen Sie `npm run push` aus, um das Projekt zu erstellen und mit einer Push-Operation an Ihre Organisation zu übertragen.  
Ihre Beispielwebanwendung wird unter {{site.data.keyword.Bluemix_notm}} bereitgestellt.  
Nach Abschluss der Bereitstellung wird eine Nachricht angezeigt, in der Sie darüber informiert werden, dass Ihre App nun aktiv ist.   
Beispiel:  
  ```
Angeforderter Status: started
Instanzen: 1/1
Verwendung: 64M x 1 instances
URLs: iotmonitoringcontrol-undertided-eng.mybluemix.net
Letztes Hochladen: Tue May 16 19:01:13 UTC 2017
Stack: cflinuxfs2
Buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     Status    Seite                    CPU    Speicher   Platte      Details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
10. Öffnen Sie die Web-App.  
Klicken Sie im {{site.data.keyword.Bluemix_notm}}-App-Dashboard auf **App-URL aufrufen**, um die Webanwendung zu öffnen.  
Greifen Sie auf die Anwendungs-URL zu und verwalten Sie diese, indem Sie auf **Routen** klicken.   
Die Standard-URL hat folgendes Format:  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## Schritt 1B - Überwachungsbenutzerschnittstelle mit Widgetbibliothek erstellen
{: #widget-library}

Die auf der Widgetbibliothek basierende Beispielapp umfasst eine Messanzeige für die Motorgeschwindigkeit, für die Beschleunigungssensordaten sowie ein Diagramm für die Motorgeschwindigkeit, das die Daten für ein einzelnes Gerät vom Typ 'iot-conveyor-belt' anzeigt, das mit Ihrer {{site.data.keyword.iot_short_notm}}-Organisation verbunden ist. Sie können den Beispielcode verwenden, um eine vollständige Front-End-Anwendung für Ihre mit {{site.data.keyword.iot_short_notm}} verbundenen Geräte zu erstellen.

![Überwachungsapp auf Basis der Widgetbibliothek](images/app_monitor_b.png "Überwachungsapp auf Basis der Widgetbibliothek")

Im Rahmen dieser Anleitung werden Sie die folgenden Arbeitsschritte ausführen:
- Bereitstellen einer Beispielwebanwendung auf Basis einer GitHub-Quelle mithilfe von Cloud Foundry.
- Konfigurieren einer Beispielapp zur Herstellung einer Verbindung zu {{site.data.keyword.iot_short_notm}} mithilfe eines API-Schlüssels und eines Authentifizierungstokens.
- Konfigurieren von drei Benutzerschnittstellenwidgets zur Anzeige von Gerätedaten wie Messanzeigen und Diagrammen.
- Verwenden der Webanwendung zur Überwachung Ihres verbundenen Laufbandgeräts.  

### Detaillierte Schritte
Die folgenden Schritte führen Sie durch die Erstellung und Bereitstellung der App unter {{site.data.keyword.Bluemix_notm}}. Informationen zur lokalen Ausführung der App finden Sie in der Readme-Datei von GitHub.
1. Klonen Sie das GitHub-Repository der Beispielapp für die *Widgetbibliotheksüberwachung*.  
Verwenden Sie das bevorzugte Git-Tool, um das folgende Repository zu klonen:  
https://github.com/ibm-watson-iot/guide-conveyor-ui-html
Verwenden Sie in der Git-Shell den folgenden Befehl:
```
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-html
```
2. Installieren Sie die Anwendungsabhängigkeiten.  
Navigieren Sie zum Stammelement des Repositorys *guide-conveyor-ui-html* und führen Sie den folgenden Befehl aus:
```
npm install
```
3. Erstellen Sie die Benutzerschnittstelle.  
Zur Erstellung der Anwendungsbenutzerschnittstelle müssen Sie die Widgets als JavaScript-Code zur Datei 'index.html' der Anwendung für jede Benutzerschnittstellenkomponente hinzufügen.  
Jedes Widget verwendet die folgenden JavaScript-Parameter:  
`WIoTPWidget.CreateWIDGET_TYPE("ELEMENT_ID","EVENT_NAME", "DEVICE_TYPE", "DEVICE_ID", "PROPERTY" , {WIDGET_DEFAULT_OVERRIDE}, [WIDGET_SPECIFIC_SETTINGS])`

In der folgenden Tabelle sind Beschreibungen der Parameter aufgeführt:

| Parameter | Beschreibung |    
| ----- | ---- |   
| WIDGET_TYPE | Der Typ des zu erstellenden Widgets. Beispiel: `Messanzeige` oder `Diagramm`. |
| ELEMENT_ID | Die Element-ID des Widgets in der Form, in der sie in der Anwendung angezeigt wird. Beispiel: `RPM`. |
| EVENT_NAME | Der Geräteereignisname, der die anzuzeigende Eigenschaft enthält. Beispiel: `sensorData`. |
| DEVICE_TYPE | Der Gerätetyp. Beispiel: `iot-conveyor-belt`. |
| DEVICE_ID | Die ID des Geräts, das die anzuzeigenden Daten liefert. Beispiel: `belt1`. |
| PROPERTY | Die Eigenschaft für die Gerätenachrichtennutzdaten, die angezeigt werden sollen. Beispiel: `rpm`. |
| WIDGET_DEFAULT_OVERRIDE | Die Widgetkonfigurationseinstellungen, mit denen die Standardeinstellungen überschrieben werden sollen.|
| WIDGET_SPECIFIC_SETTINGS | Mindestens ein zusätzlicher Parameter für das Widget (siehe Beispiele). |

Detaillierte Informationen zum Widgettyp finden Sie in den folgenden Beispielen und der Dokumentation im [IoT Widgets GitHub-Repository ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/iot-widgets){: new_page}.
 1. Fügen Sie eine RPM-Messanzeige hinzu.  
Diese Messanzeige zeigt den RPM-Wert des Laufbands als Messanzeige an, deren Minimum 0 und deren Maximum 5 RPM beträgt.
    1. Öffnen Sie die folgende Vorlage zur Bearbeitung: `public/index.html`.  
    2. Suchen Sie den Platzhalter für die RPM-Messanzeige: `<!--- place holder for rpm gauge  -->`
    3. Fügen Sie das folgende Element 'div' mit einer eindeutigen ID wie folgt hinzu:
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. Suchen Sie den JavaScript-Platzhalter: `/// Hier Scripts hinzufügen`.
    4. Fügen Sie den RPM-JavaScript-Code hinzu.  
Beispiel:  
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iot-conveyor-belt", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. Fügen Sie eine Beschleunigungsmessanzeige hinzu.  
Diese Messanzeige zeigt den Messwert des Beschleunigungssensors als Messanzeige an, deren Messwerte zwischen -1 und 1 liegen.
    1. Suchen Sie den Platzhalter für die Beschleunigungssensorenmessanzeige: `<!--- place holder for accelerometer gauge  -->`
    2. Fügen Sie das folgende Element 'div' mit einer eindeutigen ID wie folgt hinzu:
 ```html
 <div id="aygauge" ></div>
 ```  
    3. Suchen Sie den JavaScript-Platzhalter: `/// Hier Scripts hinzufügen`.
    4. Fügen Sie den JavaScript-Code für den Beschleunigungssensor hinzu.  
Beispiel:   
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iot-conveyor-belt", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
          },
          show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. Fügen Sie ein Diagramm für die Motorgeschwindigkeit hinzu.  
Dieses Diagramm zeigt die Motorgeschwindigkeit als Liniendiagramm an.
    1. Suchen Sie den Platzhalter für die Motorgeschwindigkeitsmessanzeige: `<!--- place holder for motor speed gauge  -->`
    2. Fügen Sie das folgende Element 'div' mit einer eindeutigen ID wie folgt hinzu:
 ```html
 <div id="speedchart" ></div>
 ```  
    3. Suchen Sie den JavaScript-Platzhalter: `/// Hier Scripts hinzufügen`.
    4. Fügen Sie den JavaScript-Code für das Geschwindigkeitsdiagramm ('speedchart') hinzu.  
Beispiel:  
 ```javascript
 WIoTPWidget.CreateChart("speedchart ","sensorData", "iot-conveyor-belt", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. Anwendung für {{site.data.keyword.Bluemix_notm}} bereitstellen  
 1. Aktualisieren Sie die Datei 'manifest.yml' mit Ihrem {{site.data.keyword.iot_short_notm}}-Servicenamen.  
Wenn Sie z. B. einen {{site.data.keyword.iot_short_notm}}-Service im Rahmen von [Anleitung 1: Laufbandgerät verbinden](getting-started-iot-monitoring.html) bereitgestellt haben, dann lautet der Wert für YOUR_PLATFORM_NAME `iotp-for-conveyor`.
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    Bezeichnung: iotf-service  </br>
    Plan: iotf-service-free  </br>
Anwendungen:  </br>
\- Pfad: .  </br>
  Speicher: 128M  </br>
  Instanzen: 1  </br>
  Domäne: mybluemix.net  </br>
  Datenträgerkontingent: 1024M  </br>
  Services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. Melden Sie sich bei Ihrem {{site.data.keyword.Bluemix_notm}}-Konto über die Cloud Foundry-CLI an.  
Weitere Informationen finden Sie in der [Dokumentation zur Cloud Foundry-CLI![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cloudfoundry.org/cf-cli/){: new_window}.  
Geben Sie den folgenden Befehl an der Befehlszeile ein:  
   ```
 cf login
   ```
Wenn Sie vom System dazu aufgefordert werden, dann wählen Sie die Organisation und den Bereich aus, in denen Sie die Überwachungsapp bereitstellen wollen.
 5. Legen Sie bei Bedarf den API-Endpunkt fest, indem Sie den Befehl 'cf api' ausführen.   
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
 6. Wechseln Sie in das Verzeichnis, in dem sich die Beispielapp befindet.  
   ```
 cd guide-conveyor-ui-html
   ```
 2. Führen Sie den Befehl 'cf push' aus, um die Anwendung mit einer Push-Operation an {{site.data.keyword.Bluemix_notm}} zu übertragen:  
Ordnen Sie der Anwendung einen eindeutigen Namen zu.
```
cf push YOUR_APP_NAME
```
5. Öffnen Sie die widgetbibliotheksbasierte Web-App.  
Klicken Sie im {{site.data.keyword.Bluemix_notm}}-App-Dashboard auf **App-URL aufrufen**, um die Webanwendung zu öffnen.  
Greifen Sie auf die Anwendungs-URL zu und verwalten Sie diese, indem Sie auf **Routen** klicken.   
Die Standard-URL hat folgendes Format:  
`https://YOUR_APP_NAME.mybluemix.net`

## Schritt 2 - Verbundene Geräte anzeigen
{: #view_devices}

Nachdem die Webkonsole nun betriebsbereit ist, können Sie die verbundenen Laufbandgeräte anzeigen.
1. Überprüfen Sie in der Webkonsole im Abschnitt **Geräte**, ob Ihre Laufbänder aufgelistet werden und ob der korrekte RPM- und Laufstatus angezeigt wird.
2. Ändern Sie den RPM-Wert für das Laufbandgerät und überprüfen Sie, ob der Wert wie erwartet in der Überwachungsapp aktualisiert wird.


## Weitere Schritte
{: @whats_next}  
Fahren Sie mit der nächsten Anleitung fort oder springen Sie zu einem anderen Thema, das für Sie von Interesse ist:
- Pfad A: Überwachungsapp Ihren Anforderungen anpassen.  
Technische Details finden Sie unter:
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md){: new_window}
 - [Node.js-Clientbibliotheken ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- Pfad B: Widgetbibliotheksapp Ihren Anforderungen anpassen.  
Technische Details finden Sie unter:
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md){: new_window}
- [Anleitung 4: Große Anzahl von Geräten simulieren](getting-started-iot-large-scale-simulation.html)  
Erweitern Sie die Basissimulation, indem Sie eine große Anzahl selbstausführender Simulatoren zu Ihrer Umgebung hinzufügen. Diese Erweiterung ermöglicht Ihnen die Ausführung der grundlegenden Analyse- und Überwachungsfunktionen aus den vorherigen Anleitungen in einer realistischeren Umgebung mit mehreren Geräten.
- [Weitere Informationen zu {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Weitere Informationen zu {{site.data.keyword.iot_short_notm}}-APIs](/docs/services/IoT/reference/api.html){:new_window}
