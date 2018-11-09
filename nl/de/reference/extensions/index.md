---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Externe Services integrieren
{: #ref-index}

Die Integration von externen Services ermöglicht Ihnen den Zugriff auf Daten und Operationen von Drittanbietern oder externen Services innerhalb Ihrer {{site.data.keyword.iot_full}}-Organisation.

## Jasper
{: #jasper}

Jasper ist eine Verwaltungs- und Managementplattform für SIM-Geräte. Jasper ist in das Dashboard von {{site.data.keyword.iot_short_notm}} integriert, sodass es nun möglich ist, Jasper-Geräte über Ihr Organisationsdashboard von {{site.data.keyword.iot_short_notm}} zu verwalten.

### Unterstützte Operationen für Jasper

Die von Ihrer Plattform bereitgestellte Integration von Jasper bietet Unterstützung für folgende Jasper-Operationen:

- Gesamte Jasper-Daten anzeigen
  - Folgendes wird angezeigt: Status, Tarifplan, Datennutzung für den Monat bisher, SMS-Nutzung für den Monat bisher, Telefonnutzung für den Monat bisher, Überschreitungsgrenzwerte, Hinzufügungsdatum und Änderungsdatum.
- SIM-Aktivierungsstatus ändern.
  - Folgendes kann ausgewählt werden: Bestand, Aktivierungsbereit, Aktiviert, Inaktiviert und Ruhezustand.
- SIM-Nutzung anzeigen
  - Folgendes wird angezeigt: Zyklusstartdatum, abrechnungsfähige und gesamte Datennutzung, abrechnungsfähige und gesamte SMS-Nutzung, abrechnungsfähige und gesamte Telefonnutzung.
  - Das Zyklusstartdatum kann im Format JJJJ-MM-TT festgelegt werden.
- SMS an SIM senden
- Tarifplan ändern

Sie können auf diese Operationen im Geräte-Drilldown eines verbundenen Jasper-Geräts zugreifen, nachdem die nachfolgend beschriebenen Konfigurationsschritte abgeschlossen sind.

### REST-APIs für Jasper
Informationen für den Zugriff auf die REST-API für Jasper finden Sie im Abschnitt 'Jasper-Erweiterung' in der Dokumentation für die [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/ext-jasper.html){: new_window}.

### Konfiguration für Jasper

Damit eine Verbindung zwischen Ihrem Jasper-Service mit Ihrer {{site.data.keyword.iot_short_notm}}-Organisation hergestellt werden kann, müssen zuvor zwei Konfigurationsphasen ausgeführt werden. Zunächst muss {{site.data.keyword.iot_short_notm}} mit Ihrem Jasper-Service verbunden werden, anschließend müssen Ihre {{site.data.keyword.iot_short_notm}}-Geräte konfiguriert werden.


1. Jasper-Erweiterung aktivieren. Führen Sie folgende Schritte aus, um die Integration von Jasper in Ihre {{site.data.keyword.iot_short_notm}}-Organisation zu ermöglichen.
  1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard die Option **Erweiterungen** aus.
  2. Klicken Sie auf der Seite **Erweiterungen** auf **Erweiterung hinzufügen**.
  3. Klicken Sie neben 'Jasper' auf **Hinzufügen**.
  4. Geben Sie Ihren Benutzernamen, das Kennwort, den Zugriffsschlüssel und die Domänen-ID für Jasper ein.
  5. Klicken Sie auf **Fertig**.

2. Geräte konfigurieren
Sie können die Geräte, die sowohl mit Ihrer {{site.data.keyword.iot_short_notm}}-Organisation als auch mit Ihrem Jasper-Konto verbunden sind, so konfigurieren, dass Daten von Jasper im Dashboard von {{site.data.keyword.iot_short_notm}} angezeigt werden.  
**Wichtig:** Die Jasper-Konfiguration kann nicht im Rahmen des Prozesses zum Hinzufügen von Geräten angewendet werden, nur zuvor bereits verbundene Geräte können mit Jasper konfiguriert werden.  
Führen Sie folgende Schritte aus, um Ihre mit Jasper verbundenen Geräte zu konfigurieren:
 1. Suchen Sie auf der Registerkarte 'Geräte' in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard nach dem zu konfigurierenden und mit Jasper verbundenen Gerät.
 2. Wählen Sie das Gerät aus, um die Ansicht *Drilldown für Geräte* zu öffnen.
 3. Blättern Sie abwärts zur Option *Erweiterungskonfiguration*.
 4. Geben Sie die Erweiterungskonfiguration ein, indem Sie das folgende JSON-Format verwenden, und klicken Sie anschließend auf **Änderungen bestätigen**, um Ihre Konfiguration zu speichern.  

```json  
    {
        "jasper": {
            "iccid": "string"
        }
    }

```

Wenn die Organisation erfolgreich konfiguriert wurde, wird der Abschnitt *Erweiterungen* unterhalb des Abschnitts zur *Erweiterungskonfiguration* in der Ansicht *Drilldown für Geräte* angezeigt.

## AT&T
{: #att}

### Unterstützte Operationen für AT&T

Die AT&T-Erweiterung ermöglicht folgende AT&T-Operationen:

- Gesamte AT&T-Daten anzeigen
  - Folgendes wird angezeigt: Status, Tarifplan, Datennutzung für den Monat bisher, SMS-Nutzung für den Monat bisher, Telefonnutzung für den Monat bisher, Überschreitungsgrenzwerte, Hinzufügungsdatum und Änderungsdatum.
- SIM-Aktivierungsstatus ändern.
  - Folgendes kann ausgewählt werden: Bestand, Aktivierungsbereit, Aktiviert, Inaktiviert und Ruhezustand.
- SIM-Nutzung anzeigen
  - Folgendes wird angezeigt: Zyklusstartdatum, abrechnungsfähige und gesamte Datennutzung, abrechnungsfähige und gesamte SMS-Nutzung, abrechnungsfähige und gesamte Telefonnutzung.
  - Das Zyklusstartdatum kann im Format JJJJ-MM-TT festgelegt werden.
- SMS an SIM senden
- Tarifplan ändern

### REST-APIs für AT&T
Informationen für den Zugriff auf die REST-API für AT&T finden Sie im Abschnitt 'AT&T-Erweiterung' in der Dokumentation für die [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/ext-atnt.html){: new_window}.

### Konfiguration für AT&T

Um Ihre {{site.data.keyword.iot_short_notm}}-Organisation mit AT&T zu verbinden, müssen Sie eine Organisationskonfiguration und eine Gerätekonfiguration ausführen.

Führen Sie folgende Schritte aus, um Ihre {{site.data.keyword.iot_short_notm}}-Plattform zu konfigurieren.

1. AT&T-Erweiterung aktivieren. Führen Sie folgende Schritte aus, um die Integration von AT&T und der {{site.data.keyword.iot_short_notm}}-Organisation zu aktivieren:
  1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard die Option **Erweiterungen** aus.
  2. Klicken Sie auf der Seite **Erweiterungen** auf **Erweiterung hinzufügen**.
  3. Klicken Sie neben AT&T auf die Option **Hinzufügen**.
  4. Geben Sie Ihren Benutzernamen, das Kennwort, den Zugriffsschlüssel und die Domänen-ID für AT&T ein.
  5. Klicken Sie auf **Fertig**.

Zum Verbinden Ihrer {{site.data.keyword.iot_short_notm}}-Organisation mit Ihrem AT&T-Konto müssen zunächst zwei Konfigurationsphasen ausgeführt werden. Führen Sie die Konfiguration der Organisation aus und konfigurieren Sie anschließend Ihre Geräte.


2. Geräte konfigurieren
Sie können die Geräte, die sowohl mit Ihrer {{site.data.keyword.iot_short_notm}}-Organisation als auch mit Ihrem AT&T-Konto verbunden sind, so konfigurieren, dass Daten von AT&T im {{site.data.keyword.iot_short_notm}}-Dashboard angezeigt werden.  
**Wichtig:** Die AT&T-Konfiguration kann nicht im Rahmen des Prozesses zum Hinzufügen von Geräten angewendet werden, nur zuvor bereits verbundene Geräte können mit AT&T konfiguriert werden.  
Führen Sie folgende Schritte aus, um Ihre mit AT&T verbundenen Geräte zu konfigurieren:
 1. Suchen Sie auf der Registerkarte 'Geräte' in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard nach dem zu konfigurierenden und mit AT&T verbundenen Gerät.
 2. Wählen Sie das Gerät aus, um die Ansicht *Drilldown für Geräte* zu öffnen.
 3. Blättern Sie abwärts zur Option *Erweiterungskonfiguration*.
 4. Geben Sie die Erweiterungskonfiguration ein, indem Sie das folgende JSON-Format verwenden, und klicken Sie anschließend auf **Änderungen bestätigen**, um Ihre Konfiguration zu speichern.  

```json  
    {
        "atnt": {
            "iccid": "string"
        }
    }

```

Wenn die Organisation erfolgreich konfiguriert wurde, wird der Abschnitt *Erweiterungen* unterhalb des Abschnitts zur *Erweiterungskonfiguration* in der Ansicht *Drilldown für Geräte* angezeigt.

## Arm Mbed-Bridge
{: #arm}

Mit der Bridge können Arm Mbed-Geräte in IBM Watson IoT Platform integriert und bidirektional Nachrichten ausgetauscht werden. Um diese Integration zu aktivieren, müssen Sie sich zunächst für ein Arm Mbed-Cloud-Konto anmelden und dann die angeforderten Verbindungsinformationen für Ihre Watson IoT-Konfiguration bereitstellen.

### Konfiguration für die Einrichtung


1. Aktivieren Sie die Erweiterung für die Arm Mbed-Bridge. Führen Sie dazu die folgenden Schritte aus:
  1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard die Option **Erweiterungen** aus.
  2. Klicken Sie auf der Seite **Erweiterungen** auf **+Erweiterung hinzufügen**.
  3. Klicken Sie neben der Erweiterung für die Arm Mbed-Bridge auf **Hinzufügen**.
  4. Geben Sie den Zugriffsschlüssel für Arm Mbed ein. Sie können diesen Schlüssel über das Arm Mbed-Portal unter 'https://portal.mbedcloud.com' erstellen.
  5. Prüfen Sie, ob die Berechtigungsnachweise richtig sind, indem Sie auf die Schaltfläche **Verbindung überprüfen** klicken.
  6. Klicken Sie auf **Fertig**.

### Nutzdatenformat

Die von der ARM Mbed-Plattform eingehenden Nachrichten haben den Typ 'Benachrichtigung' oder 'asynchrone Antwort'. {{site.data.keyword.iot_short_notm}} kann Befehle an Geräte senden, die mit der Arm Mbed-Plattform verbunden sind.

#### Benachrichtigungen

Benachrichtigungen werden durch Änderungen in den Geräte- oder Sensordaten generiert. Nachdem {{site.data.keyword.iot_short_notm}} die Nachricht verarbeitet hat, wird sie auf dieselbe Weise an das Ereignistopic des Geräts gesendet wie bei einem Gerät, das direkt mit {{site.data.keyword.iot_short_notm}} verbunden ist. Der Ereignistyp, der für Benachrichtigungen verwendet wird, die von Geräten stammen, die mit der Arm Mbed-Plattform verbunden sind, lautet `notify`.

Das folgende Codebeispiel zeigt das Nutzdatenformat für eine Benachrichtigung, die von der API der Arm Mbed-Plattform gesendet wurde:

```
{
  "ep":<Endpunkt/Geräte-ID>,
  "path":<Ressourcenpfad>,
  "ct":<Inhaltstyp>,
  "payload":<Nutzdaten mit Base64-Codierung>,
  "max-age":<Gültigkeit der Nutzdaten in Sekunden>
}
```

#### Asynchrone Antworten

Wenn {{site.data.keyword.iot_short_notm}} einen Befehl an ein Gerät sendet, das mit der Arm Mbed-Plattform verbunden ist, sendet das Gerät eine Bestätigungsnachricht zurück an {{site.data.keyword.iot_short_notm}}. Diese Bestätigungsnachricht wird als _asynchrone Antwort_ bezeichnet und verwendet den Ereignistyp `asyncResponse`.

Das folgende Codebeispiel zeigt das Nutzdatenformat für eine asynchrone Antwort, die vom Arm Mbed-Cloud-Service gesendet wird:

```
{
  "id":<Transaktions-ID>,
  "status":<200, falls Befehl erfolgreich ausgeführt wurde. Überprüfen Sie weitere HTTP-Antwortcodes>,
  "ct":<Inhaltstyp>,
  "max-age":<Gültigkeit der Nutzdaten in Sekunden>,
  "payload":<Nutzdaten mit Base64-Codierung>,
  "ep":<Endpunkt/Geräte-ID, der/die vom Befehl betroffen ist>,
  "path":<Ressourcenpfad, der vom Befehl betroffen ist>
}
```

#### Befehle an die Arm Mbed-Plattform senden

{{site.data.keyword.iot_short_notm}} kann Befehle an Geräte senden, die mit der Arm Mbed-Plattform verbunden sind. An die Arm Mbed-Plattform gesendete Befehle müssen das folgende JSON-Format aufweisen.

```
{
  "method":<PUT oder POST>,
  "deviceId":<Endpunkt/Geräte-ID>,
  "resourceId":<Ressourcenpfad>,
  "payload": <Nutzdaten mit Base64-Codierung>
}
```
Bei der gewählten Methode muss die Groß-/Kleinschreibung beachtet werden. Die Angabe '/' vor dem Ressourcenpfad muss übersprungen werden.


Die Nutzdaten müssen im folgenden Topic publiziert werden:

```
iot-2/type/<Gerätetyp>/id/<Geräte-ID>/cmd/<Befehlstyp>/fmt/<Befehlsformat>
```


## Orange
{: #orange}

Die Orange-Erweiterung ermöglicht Ihnen die Daten der SIM-Karte von Geräten anzuzeigen, die mit {{site.data.keyword.iot_short_notm}} verbunden sind und in die eine Orange-SIM-Arte installiert ist.

https://developer.ibm.com/iotplatform/2016/03/30/watson-iot-platform-integration-with-orange-beta/

### Unterstützte Operationen für Orange

Wenn Sie ein Gerät haben, das mit Ihrem {{site.data.keyword.iot_short_notm}}-Service verbunden ist und eine Orange-SIM-Karte hat, können Sie die Orange-Erweiterung verwenden, um die folgenden Daten der SIM-Karte anzuzeigen:

- SIM-Seriennummer
- Aktivierungsstatus
- Letzter Änderungsstatus
- Letzter Aktualisierungsstatus
- Standortstatus

### REST-APIs für Orange
Informationen für den Zugriff auf die REST-API für Orange finden Sie im Abschnitt 'Orange-Erweiterung' in der Dokumentation für die [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/ext-orange.html){: new_window}.

### Konfiguration für Orange

Gehen Sie wie folgt vor, die Orange-Erweiterung zu aktivieren:

1. Wählen Sie im {{site.data.keyword.iot_short_notm}}-Dashboard die Option **Erweiterungen** aus.
2. Klicken Sie auf der Seite **Erweiterungen** auf **Erweiterung hinzufügen**.
3. Klicken Sie neben der Orange-Erweiterung auf **Hinzufügen**.
4. Geben Sie Ihren Benutzernamen und das Kennwort für Orange ein.
6. Klicken Sie auf **Fertig**.

Nach dem Aktivieren der Orange-Erweiterung muss jedes Gerät mit einer Orange-SIM-Karte für die Anzeige von Daten der Orange-SIM-Karte konfiguriert werden.

1. Suchen Sie auf der Registerkarte 'Geräte' in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard nach dem zu konfigurierenden Orange-SIM-Gerät.
2. Wählen Sie das Gerät aus und blättern Sie abwärts zu *Erweiterungskonfiguration*.
3. Geben Sie die Erweiterungskonfiguration ein, indem Sie das folgende JSON-Format verwenden, und klicken Sie anschließend auf **Änderungen bestätigen**, um Ihre Konfiguration zu speichern.

```  
    {
        "orange": {
            "serialnumber": "<Seriennummer der Orange-SIM>"
        }
    }

```
Wenn die Organisation erfolgreich konfiguriert wurde, wird der Abschnitt *Erweiterungen* unterhalb des Abschnitts zur *Erweiterungskonfiguration* in der Ansicht *Drilldown für Geräte* angezeigt.

## Speicherung archivierter Daten
{: #historical_data}

Die Erweiterung für die Speicherung archivierter Daten ermöglicht Ihnen, kompatible Services für das Speichern von Nachrichten wie beispielsweise [{{site.data.keyword.cloudantfull}}](../../cloudant_connector.html) oder [{{site.data.keyword.messagehub_full}}](../../message_hub.html) für Ihre IoT-Daten zu lokalisieren und zu konfigurieren.

## Angepasste Pakete für das Gerätemanagement
{: #device_mgmt}

Das Gerätemanagement ist eine zentrale Funktion von {{site.data.keyword.iot_short_notm}}, die jedoch erweitert werden kann, um zusätzliche Funktionen zu entwickeln. Angepasste Pakete für das Gerätemanagement müssen gültiges JSON-Format enthalten und mindestens eine angepasste Gerätemanagementaktion definieren.

Weitere Informationen zu angepassten Gerätemanagementfunktionen sowie ein Beispiel für das erforderliche JSON-Format finden Sie in [Angepasste Erweiterungen für das Gerätemanagement](../../devices/device_mgmt/custom_actions.html){: new_window}.

### Angepasstes Gerätemanagementpaket hinzufügen

Sie können angepasste Pakete für das Gerätemanagement mit dem {{site.data.keyword.iot_short_notm}}-Dashboard hinzufügen oder mithilfe der API.

Gehen Sie wie folgt vor, um ein angepasstes Paket für das Gerätemanagement mithilfe des {{site.data.keyword.iot_short_notm}}-Dashboards hinzuzufügen:

1. Klicken Sie in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard in der Navigationsleiste auf **Einstellungen**.
2. Klicken Sie auf **Angepasste Gerätemanagementpakete**.
3. Klicken Sie auf die Schaltfläche **Paket hinzufügen**.
4. Wählen Sie Ihre Paketdatei aus und klicken Sie auf **Öffnen**.

Informationen zum Hinzufügen eines angepassten Gerätemanagementpakets mithilfe der API finden Sie in der Dokumentation für die [{{site.data.keyword.iot_short_notm}}-API ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/orgAdmin.html){: new_window}.

<!-- ## The Weather Company
{: #weathercompany}

The Weather Company extension combines weather data with your existing {{site.data.keyword.iot_short_notm}} devices. Weather data from The Weather Company appears in the device details view if an update location request has been made by using the API, or if the device has already set its location by using a device management message.

**Note:** Only managed devices can set their own locations. All unmanaged devices must have their locations set manually by using the API. For more information on setting a device location, see [Update Location requests](../../devices/device_mgmt/index.html#update-location).

### REST APIs for The Weather Company
To access the REST API for The Weather Company, see the
Device Location Weather section in the [{{site.data.keyword.iot_short_notm}} HTTP REST API ![External link icon](../../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/swagger/v0002.html#!/Device_Location_Weather){: new_window} documentation.

### Weather Data

To view the weather data retrieved for a device location, find the device in the **Devices** pane and click it. In the detailed device view scroll down to the **Extensions** section. The following weather data is listed:

- Current weather.
- Current temperature.
- Predicted maximum and minimum temperature.
- Relative humidity.
- Pressure.
- Visibility.
- Wind speed.
- Wind direction.
- Latitude.
- Longitude.
-->

<!-- Weather data from The Weather Company extension can be retrieved by using the API. For information on the Weather Company API, see [The Weather Company API documentation ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/swagger/ext-twc.html){: new_window}. -->

## E-Mail
{: #email}

Benutzer können mithilfe von E-Mail-Einladungen zu {{site.data.keyword.iot_short_notm}} hinzugefügt werden. Informationen finden Sie in [Benutzerzugriff verwalten](../../add_users.html).

Damit die Funktion für E-Mail-Einladungen verwendet werden kann, muss eine E-Mail-Erweiterung für die Verwendung des SendGrid-Onlineservice oder eines SMTP-Service (Simple Mail Transfer Protocol) konfiguriert sein. Die Erweiterung kann auch die {{site.data.keyword.Bluemix_notm}}-Anwendung SendGrid verwenden.

### SendGrid-Onlineservice

Führen Sie die folgenden Schritte aus, um die E-Mail-Erweiterung für die Verwendung mit dem SendGrid-Onlineservice zu konfigurieren:

1. Rufen Sie den berechtigten API-Schlüssel aus Ihrem SendGrid-Onlinekonto ab.
2. Klicken Sie in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard in der Navigationsleiste auf **Erweiterungen**.
3. Klicken Sie im Abschnitt **E-Mail** auf **Einrichten**.
4. Wählen Sie **SendGrid mit API-Schlüssel** aus.
5. Geben Sie den Namen und die E-Mail-Adresse Ihres Siteadministrators und den berechtigten API-Schlüssel ein.

### SMTP-Service

Führen Sie die folgenden Schritte aus, um die E-Mail-Erweiterung für die Verwendung mit einem SMTP-Service zu konfigurieren:

1. Klicken Sie in Ihrem {{site.data.keyword.iot_short_notm}}-Dashboard in der Navigationsleiste auf **Erweiterungen**.
2. Klicken Sie im Abschnitt **E-Mail** auf **Einrichten**.
3. Wählen Sie **SMTP** aus.
4. Geben Sie die Konfigurationsdetails Ihres SMTP-Service ein.

### {{site.data.keyword.Bluemix_notm}}-Anwendung SendGrid

Führen Sie die folgenden Schritte aus, um die E-Mail-Erweiterung für die Verwendung der {{site.data.keyword.Bluemix_notm}}-Anwendung SendGrid zu konfigurieren:

1. Erstellen Sie eine Dummy-Anwendung und binden Sie sie an den SendGrid-Service.  
Fügen Sie den SendGrid-Service zu einer Dummy-App hinzu und binden Sie ihn an sie, um die Konfigurationsberechtigungsnachweise abzurufen.

 1. Klicken Sie in Ihrem {{site.data.keyword.Bluemix_notm}}-Dashboard auf **Service erstellen**.
 2. Wählen Sie den SendGrid-Service aus dem Katalog aus und klicken Sie auf **Erstellen**.
 3. Fügen Sie im {{site.data.keyword.Bluemix_notm}}-Dashboard die Anwendung {{site.data.keyword.sdk4nodefull}} hinzu.
 4. Klicken Sie im {{site.data.keyword.Bluemix_notm}}-Dashboard auf die Anwendung {{site.data.keyword.sdk4nodefull}} und klicken Sie auf **Service oder API binden**.
 5. Wählen Sie den SendGrid-Service aus und klicken Sie auf **Hinzufügen**.
 6. Für die Anwendung {{site.data.keyword.sdk4nodefull}} muss nun ein erneutes Staging ausgeführt werden.
2. Bereiten Sie die Konfiguration des {{site.data.keyword.iot_short_notm}}-Service vor.  
{{site.data.keyword.iot_short_notm}} kann mithilfe des {{site.data.keyword.iot_short_notm}}-Dashboards oder mithilfe der {{site.data.keyword.iot_short_notm}}-API konfiguriert werden.  
 1. Klicken Sie im {{site.data.keyword.Bluemix_notm}}-Dashboard auf die Anwendung {{site.data.keyword.sdk4nodefull}}.
 2. Klicken Sie in der Navigationsleiste auf **Umgebungsvariablen**.
 3. Kopieren Sie die angezeigte JSON in eine temporäre Textdatei.  
 Die JSON sollte folgendes Format aufweisen:
```
{
  "name": "SendGridServiceName",
  "label": "user-provided",
  "credentials": {
    "password": "xxx",
    "hostname": "smtp.sendgrid.net",
    "username": "username"
  }
}
```
3. Fügen Sie die Konfigurationsdaten zur {{site.data.keyword.iot_short_notm}}-Organisation hinzu.
 1. Öffnen Sie das {{site.data.keyword.iot_short_notm}}-Dashboard.
 2. Klicken Sie in der Navigationsleiste auf **Erweiterungen**.
 3. Klicken Sie unter dem Symbol **E-Mail** auf **Einrichten**.
 4. Wählen Sie **SendGrid mit Benutzername** aus.
 5. Geben Sie die Konfigurationsdaten aus der temporären Textdatei ein.
 6. Klicken Sie auf **Fertig**.
