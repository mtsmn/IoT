---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Anleitung 1: Laufbandgerät verbinden  
Erstellen Sie ein Basislaufband mit einem IoT-Gerät, das Überwachungsdaten an {{site.data.keyword.iot_full}} unter {{site.data.keyword.Bluemix_notm}} sendet.
{:shortdesc}

## Übersicht und Ziel
{: #overview}  
Diese Anleitung stellt das erste Element in einer Reihe von Anleitungen dar, die Sie schrittweise durch den Prozess zur Verbindung von Geräten mit {{site.data.keyword.iot_short_notm}}, den Prozess zum Überwachen und Bearbeiten von Gerätedaten und durch weitere Aufgaben führen. Jede Anleitung baut dabei auf der vorherigen Anleitung auf und der Beispielcode der einzelnen Anleitungen dient als Eingabe für die jeweils nächste Anleitung. Sie können diese Anleitungen auch eigenständig nutzen, indem Sie den Beispielcode Ihren individuellen Zielsetzungen anpassen.

In der ersten Anleitung wird ein verbundenes Laufband eingerichtet und zum Senden von IoT-Daten an {{site.data.keyword.iot_short_notm}} verwendet.
Abhängig von Ihrem persönlichen Kenntnisstand können Sie einen der folgenden Lösungswege (Pfade) oder aber beide verwenden, um Ihr Laufband einzurichten:
- Pfad A  
Dieser Pfad bietet eine schnelle Lösung, indem eine Simulatorapp für ein Laufband unter {{site.data.keyword.Bluemix_notm}} installiert wird. Die App führt eine Selbstregistrierung eines Geräts bei {{site.data.keyword.iot_short_notm}} durch und sendet automatisch korrekt formatierte Daten an die Plattform. Anweisungen für diesen Pfad finden Sie in [Schritt 2A - Simulator-Beispielapp von GitHub verwenden](#deploy_app).  
- Pfad B  
Dieser Pfad ist fachlich anspruchsvoller und erfordert zusätzliche Hardware, Programmierkenntnisse in Python und die manuelle Registrierung Ihres Geräts bei {{site.data.keyword.iot_short_notm}}. Anweisungen für diesen Pfad finden Sie in [Schritt 2B - Physisches Laufband mit Raspberry Pi und elektrischem Motor erstellen](#raspberry).

Im Rahmen dieser Anleitung werden Sie die folgenden Arbeitsschritte ausführen:
- Erstellen und Bereitstellen einer {{site.data.keyword.iot_short_notm}}-Organisation mithilfe der Cloud Foundry-CLI.
- Erstellen und Bereitstellen eines Beispiels für ein Laufbandgerät.
- Verbinden des simulierten Laufbandgeräts mit {{site.data.keyword.iot_short_notm}}.

Um mit der Nutzung eines anderen IoT-Geräts durch {{site.data.keyword.iot_short_notm}} zu beginnen, sollten Sie sich mit dem [Lernprogramm 'Einführung'](/docs/services/IoT/getting-started.html) vertraut machen.
{: tip}

## Voraussetzungen
{: #prereqs}

Sie benötigen die folgenden Konten und Tools:
* [{{site.data.keyword.Bluemix_notm}}-Konto ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.ng.bluemix.net/registration/){: new_window}
* [Cloud Foundry Command Line Interface (cf CLI) ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Verwenden Sie die Cloud Foundry-CLI (cf CLI) zum Bereitstellen von Apps und Services für {{site.data.keyword.Bluemix_notm}}. Weitere Informationen finden Sie in der [Dokumentation zur Cloud Foundry-CLI![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cloudfoundry.org/cf-cli/){: new_window}.
* Optional: [Git ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://git-scm.com/downloads){: new_window}  
Wenn Sie sich für den Einsatz von Git zum Herunterladen der Codebeispiele entscheiden, dann benötigen Sie außerdem ein [GitHub.com-Konto ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com){: new_window}. Sie können den Code auch als komprimierte Datei herunterladen. Dazu ist kein GitHub.com-Konto erforderlich.
* Optional: Ein Mobiltelefon zur Ausführung der Beispielwebanwendung für das *Laufband*, mit der Beschleunigungssensordaten gesendet werden.

## Schritt 1 - {{site.data.keyword.iot_short_notm}} bereitstellen  
{: #deploy_watson_iot_platform_service}
Die nachfolgenden Schritte dienen zur Bereitstellung einer Instanz des {{site.data.keyword.iot_short_notm}}-Service mit dem Namen `iotp-for-conveyor` in Ihrer {{site.data.keyword.Bluemix_notm}}-Umgebung. Wenn Sie bereits über eine aktive Serviceinstanz verfügen, dann können Sie diese Instanz mit der Anleitung benutzen und den ersten Schritt überspringen. Vergewissern Sie sich, dass Sie den korrekten Servicenamen und {{site.data.keyword.Bluemix_notm}}-Bereich verwenden, wenn Sie die Anleitungen durcharbeiten.
{: tip}
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
3. Stellen Sie den {{site.data.keyword.iot_short_notm}}-Service für {{site.data.keyword.Bluemix_notm}} bereit.  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
   ```
Verwenden Sie für YOUR_IOT_PLATFORM_NAME *iotp-for-transport*.  
Beispiel: `cf create-service iotf-service iotf-service-free iotp-for-conveyor`
3. Erstellen eines Beispiels für ein Laufbandgerät.  
 - Pfad A: [Schritt 2A - Simulator-Beispielapp von GitHub verwenden](#deploy_app).  
 - Pfad B: [Schritt 2B - Physisches Laufband mit Raspberry Pi und elektrischem Motor erstellen](#raspberry).  

## Schritt 2A - Beispielwebanwendung für Laufband bereitstellen  
{: #deploy_app}

Die Beispielapp ermöglicht Ihnen die Simulation eines verbundenen {{site.data.keyword.Bluemix_notm}}-Laufbands für den industriellen Einsatzbereich. Sie können das Band starten und stoppen und die Geschwindigkeit des Bands anpassen. Jede Änderung am Band wird in Form einer MQTT-Nachricht an {{site.data.keyword.Bluemix_notm}} gesendet, die in der App angezeigt wird. Sie können das Verhalten des Bands mithilfe der standardmäßigen Dashboardkarten überwachen.
Die Beispielapp wird mithilfe der Node.js-Clientbibliotheken unter [https://github.com/ibm-watson-iot/iot-nodejs ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window} erstellt.

![Laufband-App](images/app_conveyor_belt.png "Conveyor belt app")

1. Verwenden Sie das bevorzugte Git-Tool, um das folgende Repository zu klonen:  
https://github.com/ibm-watson-iot/guide-conveyor-simulator
Verwenden Sie in der Git-Shell den folgenden Befehl:
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. Wechseln Sie in der Befehlszeile in das Verzeichnis, in der sich die Beispielapp befindet.
  ```bash
cd guide-conveyor-simulator
  ```
3. 3. Senden Sie, während Sie sich im Verzeichnis *guide-conveyor-simulator* befinden, Ihre App mit einer Push-Operation an {{site.data.keyword.Bluemix_notm}} und ordnen Sie ihr einen neuen Namen zu, indem Sie *YOUR_APP_NAME* im Befehl 'cf push' ersetzen. Verwenden Sie die Option `--no-start`, weil Sie die App in der nächsten Phase starten werden, nachdem Sie an {{site.data.keyword.iot_short_notm}} gebunden wurde.
**Hinweis:** Die Bereitstellung Ihrer Anwendung kann einige Minuten in Anspruch nehmen.
   ```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. Binden Sie im Verzeichnis *guide-conveyor-simulator* Ihre App an Ihre Instanz von {{site.data.keyword.iot_short_notm}} und verwenden Sie dazu die Namen, die Sie für die Elemente angegeben haben.
  ```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
Weitere Informationen zum Binden von Anwendungen finden Sie in [Anwendungen verbinden](/docs/services/IoT/platform_authorization.html#bluemix-binding).
2. Starten Sie Ihre Anwendung, damit die Bindung wirksam wird.
  ```bash
cf start YOUR_APP_NAME
  ```
In dieser Phase wird die Beispielwebanwendung in {{site.data.keyword.Bluemix_notm}} bereitgestellt.
Nach Abschluss der Bereitstellung wird eine Nachricht angezeigt, in der Sie darüber informiert werden, dass Ihre App nun aktiv ist.   
Beispiel:
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
Startbefehl:       ./vendor/initial_startup.rb

     state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
Um sowohl den Bereitstellungsstatus der App als auch die URL der App anzuzeigen, müssen Sie den folgenden Befehl ausführen:
  ```bash
cf apps
  ```
Beheben Sie Fehler im Bereitstellungsprozess mithilfe des Befehls 'cf logs YOUR_APP_NAME --recent'.
{: tip}
1. Greifen Sie in einem Browser auf die App zu.  
Öffnen Sie die folgende URL: 'https://YOUR_APP_NAME.mybluemix.net'    
Beispiel: 'https://conveyorbelt.mybluemix.net/'.
2. Geben Sie eine Geräte-ID und ein Token für Ihr Gerät ein.  
Die Beispielapp registriert automatisch ein Gerät des Typs 'iot-conveyor-belt' mit der von Ihnen angegebenen Geräte-ID und dem entsprechenden Token. Weitere Informationen zur Registrierung von Geräten finden Sie in [Geräte verbinden](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
4. Fahren Sie mit [Schritt 3 - Unformatierte Daten in {{site.data.keyword.iot_short_notm}} anzeigen] fort(#see_live_data).

## Schritt 2B - Raspberry Pi-Laufband erstellen
{: #raspberry}

Die Raspberry Pi-Lösung wird anhand der Python-Clientbibliotheken erstellt unter: [https://github.com/ibm-watson-iot/iot-python ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/iot-python){: new_window}

### Schematische Darstellung für den Kreis

![Schaltbild](images/raspi_circuit.png)

### Erforderliche Verbindungen

Die folgenden Verbindungen zwischen Raspberry Pi und dem L298N Dual H Bridge Motor Driver Board sind erforderlich. Verbinden Sie die folgenden Komponenten:

- Pin 2 von Raspberry Pi mit +5v des L298N-Boards
- Pin 6 von Raspberry Pi mit GND des L298N-Boards
- Pin 13 von Raspberry Pi mit IN1 des L298N-Boards
- Pin 15 von Raspberry Pi mit IN2 des L298N-Boards
- +9v der Batterie mit +12v des L298N-Boards
- -9v der Batterie mit GND des L298N-Boards
- +ve-Knoten des Motors mit OUT1 des L298N-Boards
- -ve-Knoten des Motors mit OUT2 des L298N-Boards

### Hardwarevoraussetzungen

1. [Raspberry Pi(2/3) ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.raspberrypi.org/){: new_window} mit Raspbian Jessie (8.x)
2. [L298N Dual H Bridge Motor Driver Board ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. 9-V-Gleichstrommotor
4. 9-V-Batterie

### Raspberry Pi-Softwarevoraussetzungen
- Python 2.7.9 und höher, installiert mit Raspbian Jessie (8.x)
- [Watson IoT Python-Bibliothek ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- Optional: Git  
Wenn unter Raspberry Pi Git nicht installiert ist, dann können Sie diese Komponente mit dem folgenden Befehl installieren:  
```bash
$ sudo apt-get install git
```

### Detaillierte Schritte

1. Öffnen Sie das Terminal oder SSH für Raspberry Pi.
2. Verwenden Sie das bevorzugte Git-Tool, um das folgende Repository für Raspberry Pi zu klonen:
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
Verwenden Sie in der Git-Shell den folgenden Befehl:
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. Registrieren Sie das Gerät bei {{site.data.keyword.iot_short_notm}}.  
Weitere Informationen zur Registrierung von Geräten finden Sie in [Geräte verbinden](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
	 1. Klicken Sie in der {{site.data.keyword.Bluemix_notm}}-Konsole auf der Detailseite des {{site.data.keyword.iot_short_notm}}-Service auf **Starten**.
	     Die {{site.data.keyword.iot_short_notm}}-Webkonsole wird in einer neuen Browserregisterkarte unter der folgenden URL geöffnet:
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     Hierbei steht ORG_ID für eine aus sechs Zeichen bestehende eindeutige ID [Ihrer {{site.data.keyword.iot_short_notm}}-Organisation](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}.
	 2. Wählen Sie im Dashboard 'Überblick' im Menüteilfenster die Option **Geräte** aus und klicken Sie anschließend auf **Gerät hinzufügen**.
	 3. Erstellen Sie einen Gerätetyp für das Gerät, das Sie hinzufügen.
	     1. Klicken Sie auf **Gerätetyp erstellen**.
	     2. Geben Sie den Namen des Gerätetyps `iotp-for-transport` und eine Beschreibung für den Gerätetyp ein.  
	**Tipp:** Sie können einen beliebigen Gerätetypnamen eingeben, die anderen Anleitungen in der vorliegenden Reihe erwarten jedoch Geräte mit dem Gerätetyp `iotp-for-conveyor`. Wenn Sie einen anderen Gerätetyp verwenden, dann müssen Sie die Einstellungen in den anderen Anleitungen entsprechend anpassen.
	     3. Optional: Geben Sie Attribute und Metadaten zu dem Gerätetyp ein.
	 4. Klicken Sie auf **Weiter**, um mit dem Hinzufügen Ihres Geräts mit dem ausgewählten Gerätetyp zu beginnen.
	 5. Geben Sie eine Geräte-ID ein, z. B. `conveyor_belt`.
	 5. Klicken Sie auf **Weiter**, um den Prozess abzuschließen.
	 6. Stellen Sie ein Authentifizierungstoken bereit oder akzeptieren Sie ein automatisch generiertes Token.
	 7. Überprüfen Sie die Übersichtsinformationen auf ihre Richtigkeit und klicken Sie anschließend auf **Hinzufügen**, um die Verbindung hinzuzufügen.
	 8. Kopieren Sie auf der Seite mit den Geräteinformationen die folgenden Einzelangaben und speichern Sie sie:
	     * Organisations-ID
	     * Gerätetyp
	     * Geräte-ID
	     * Authentifizierungsmethode
	     * Authentifizierungstoken
	     Sie benötigen die Werte für die Organisations-ID, den Gerätetyp, die Geräte-ID und das Authentifizierungstoken, um Ihr Gerät für die Verbindung mit {{site.data.keyword.iot_short_notm}} zu konfigurieren.
4. Navigieren Sie zum Stammelement von *guide-conveyor-rasp-pi* des geklonten Repositorys.
5. Konvertieren Sie das Shell-Script in eine ausführbare Datei. Verwenden Sie dazu den Befehl `sudo chmod +x setup.sh`.
6. Führen Sie die Datei *setup.sh* aus und geben Sie die Details ein, die Sie von der Seite mit den Gerätedaten kopiert haben, wenn Sie vom System dazu aufgefordert werden.
```bash  
./setup.sh
```  

7. Führen Sie das Programm 'deviceClient' aus.  
Wenn Sie das Programm ausführen, dann startet Raspberry Pi den Motor. Er wird abhängig von den Parametereinstellungen für maximal 2 Minuten ausgeführt.
```bash
python deviceClient.py -t 2
```

Während der Motor ausgeführt wird, publiziert das Programm Ereignisse des Ereignistyps `sensorData`, die die folgende Beispielnutzdatenstruktur aufweisen:  
```
{
"d": {
  "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. Fahren Sie mit [Schritt 3 - Rohdaten in {{site.data.keyword.iot_short_notm}} anzeigen](#see_live_data) fort.

## Schritt 3 - Rohdaten in {{site.data.keyword.iot_short_notm}} anzeigen
{: #see_live_data}
1. Überprüfen Sie, ob das Gerät bei {{site.data.keyword.iot_short_notm}} registriert wurde.
 1. Melden Sie sich beim {{site.data.keyword.Bluemix_notm}}-Dashboard unter 'https://bluemix.net' an.
 2. Klicken Sie in der [Liste der Services ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://bluemix.net/dashboard/services){: new_window} auf den {{site.data.keyword.iot_short_notm}}-Service *iotp-for-conveyor*.
 3. Klicken Sie auf die Option zum *Starten*, um das {{site.data.keyword.iot_short_notm}}-Dashboard in einer neuen Browserregisterkarte zu öffnen.  
Sie können die URL mit einem Lesezeichen versehen, um sie später einfacher wiederzufinden.   
Beispiel: `https://*iot-org-id*.internetofthings.ibmcloud.com`.
 4. Wählen Sie im Menü **Geräte** aus und stellen Sie sicher, dass Ihr neues Gerät angezeigt wird.
2. Senden sie Sensordaten an die Plattform.   
Das Gerät sendet Daten an {{site.data.keyword.iot_short_notm}}, wenn die Sensormesswerte sich ändern. Sie können diesen Datenversand simulieren, indem Sie das Laufband stoppen oder starten oder die Geschwindigkeit des Laufbands ändern.   
**Pfad A:** Wenn Sie auf die App über einen mobilen Browser zugreifen, dann schütteln Sie das Smartphone, um die Beschleunigungssensordaten für das Laufband auszulösen.
{: tip}

## Weitere Schritte
{: @whats_next}  
Fahren Sie mit der nächsten Anleitung fort oder springen Sie zu einem anderen Thema, das für Sie von Interesse ist:
- Pfad A: Ändern Sie die Laufband-App entsprechend Ihren Anforderungen.  
Technische Details finden Sie unter:
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Node.js-Clientbibliotheken ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **Hinweis:** Bluemix ist jetzt IBM Cloud. Weitere Informationen finden Sie im [IBM Cloud-Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link").
 
- Pfad B: Raspberry Pi-Konfiguration Ihren Anforderungen anpassen.  
Technische Details finden Sie unter:
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}


**Hinweis:** Bluemix ist jetzt IBM Cloud. Weitere Informationen finden Sie im [IBM Cloud-Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link").

- [Anleitung 2: Gerätedaten überwachen](getting-started-iot-monitoring.html)  
Nachdem Sie nun mindestens ein Gerät verbunden und mit der effizienten Nutzung der Gerätedaten begonnen haben, sollten Sie nun eine Gruppe von Geräten überwachen.
- [Anleitung 3: Große Anzahl von Geräten simulieren](getting-started-iot-large-scale-simulation.html)  
Die Beispielapp für das Laufband in Pfad A ermöglicht Ihnen die manuelle Simulation einzelner oder mehrerer Laufbandgeräte. Diese Anleitung ermöglicht Ihnen die Konfiguration einer simulierten Umgebung, die eine große Anzahl von Geräten enthält.
- [Weitere Informationen zu {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html)
- [Weitere Informationen zu {{site.data.keyword.iot_short_notm}}-APIs](/docs/services/IoT/reference/api.html)
