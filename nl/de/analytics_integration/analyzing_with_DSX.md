---

copyright:
  years: 2017
lastupdated: "2017-09-18"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Daten anhand von Data Science Experience analysieren
{: #DSX_integration}

Sie können {{site.data.keyword.iot_full}} mit Data Science Experience (DSX) verwenden, um die Daten, die von Geräten gesendet werden, die mit der Plattform verbunden sind, zu visualisieren und zu untersuchen.
{: shortdesc}

## Übersicht und Ziele

Die folgende Anleitung führt Sie in einzelnen Arbeitsschritten durch den Prozess zum Visualisieren der {{site.data.keyword.iot_short_notm}}-Geräteereignisdaten mithilfe des Analysetools DSX (Data Science Experience).

DSX stellt Ihnen eine interaktive, bereichsübergreifende cloudbasierte Umgebung zur Verfügung, in der Sie mehrere Tools verwenden können, um die entsprechenden Einblicke zu erhalten. Verwenden Sie das in IBM DSX verfügbare Jupyter-Notizbuch, um {{site.data.keyword.iot_short_notm}}-Archivdaten zu laden. Diese Daten können Sie dann zur Erstellung von Visualisierungen benutzen, um so die Datenanalyse und die Identifikation von Anomalien zu vereinfachen. Sie können von archivierten Daten Grenzwerte ableiten und diese Werte anschließend zum Erstellen von Cloud-Regeln in {{site.data.keyword.iot_short_notm}} verwenden. Cloud-Regeln dienen zur Benachrichtigung von Benutzern, wenn ein IoT-Gerät, das einer Regel zugeordnet ist, einen Messwert publiziert, der sich außerhalb der konfigurierten Grenzwerte befindet.

Die Gerätedaten, die an {{site.data.keyword.iot_short_notm}} gesendet werden, können erfasst und mithilfe des {{site.data.keyword.cloudantfull}} NoSQL DB-Service in {{site.data.keyword.Bluemix}} gespeichert werden. Zum Erfassen der Daten müssen Sie zuerst eine Verbindung zwischen {{site.data.keyword.iot_short_notm}} und dem {{site.data.keyword.cloudant_short_notm}}-Service herstellen. Gerätedaten werden in {{site.data.keyword.cloudant_short_notm}}-Datenbanken gespeichert, die auf der Grundlage 'Tag', 'Woche' oder 'Monat' arbeiten. Die zur Speicherung verwendete Datenbank hängt vom konfigurierten Bucketintervall ab.


![Übersicht zur Verwendung von DSX zur Datenanalyse](images/DSX_overview.png)

In der vorliegenden Anleitung erhalten Sie Informationen zu den folgenden Themen:

 - Vorgehensweise zum Konfigurieren des Plattformdatenspeichers, um Cloudant NoSQL DB als Archivierungsfunktion einsetzen zu können.
 - Vorgehensweise zum Verwenden des Wettersensorensimulators zum Generieren von Daten, die von der Plattform verwendet werden.
 - Vorgehensweise zum Exportieren der Daten und zum anschließenden Importieren dieser Daten in DSX zur Datenanalyse.
 - Vorgehensweise zur Visualisierung von IoT-Daten (IoT = Internet of Things; Internet der Dinge), zum Feststellen von Anomalien und zum Konfigurieren von Alerts.


## Voraussetzungen

Zur Ausführung dieser Schritte müssen Sie über Zugriff auf [{{site.data.keyword.iot_short_notm}} ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} mit [Cloudant NoSQL DB ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window} und über Zugriff auf ein [DSX-Konto ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://datascience.ibm.com/docs/content/getting-started/get-started.html){: new_window} verfügen.


## Schritt 1. Simulator einrichten
{:#DSX_sensor_data}

Zur Durchführung einer aussagekräftigen Analyse benötigen Sie aussagekräftige Daten. Sie können reale Sensordaten simulieren, um sich mit der Vorgehensweise zur Analyse der Watson IoT Platform-Gerätedaten mithilfe von DSX vertraut zu machen. Dieser Schritt enthält Anweisungen für die folgenden Arbeitsschritte:
 - [Einrichten des Simulators mit einer **vorhandenen Instanz von {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Einrichten des Simulators mit einer **neuen Instanz von {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)


### Wettersensorensimulator mit vorhandener Instanz von {{site.data.keyword.iot_short_notm}} einrichten
{: #sim_existing_platform}

Zum Simulieren realer Sensordatenereignisse für Ihre Organisationen mithilfe des Wettersensorensimulators müssen Sie zuerst den Simulator einrichten. In den auszuführenden Schritten wird davon ausgegangen, dass Sie bereits über eine funktionsbereite Instanz von {{site.data.keyword.iot_short_notm}} verfügen.

1. [Generieren Sie den API-Schlüssel und das Token, die zur Ausführung des Simulators erforderlich sind ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}.
2. [Stellen Sie die Web-App für den Wettersensorensimulator ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} bereit und befolgen Sie die detaillierten Anweisungen.

   Weitere Informationen zu den Wettersensoren finden Sie im [Leitfaden zum Wettersensorensimulator ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Fahren Sie mit [Schritt 2. Datenbankconnector konfigurieren](#DSX_config_db) fort.


### Wettersensorensimulator mit neuer Instanz von {{site.data.keyword.iot_short_notm}} einrichten
{: #sim_new_platform}

Zum Simulieren realer Sensordatenereignisse für Ihre Organisationen mithilfe des Wettersensorensimulators müssen Sie zuerst den Simulator einrichten. Diese Schritte enthalten Anweisungen zur Erstellung einer {{site.data.keyword.iot_short_notm}}-Instanz mit dem Simulator.

1. [Stellen Sie die Web-App des Wettersensorensimulators mit einer Instanz von {{site.data.keyword.iot_short_notm}} ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} bereit und befolgen Sie die detaillierten Anweisungen.

   Weitere Informationen zu den Wettersensoren finden Sie im [Leitfaden zum Wettersensorensimulator ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Warten Sie, bis das System die Bereitstellung abgeschlossen hat, und navigieren Sie dann zum Bluemix-Dashboard.
3. Starten Sie den {{site.data.keyword.iot_short_notm}}-Service 'wiotp-for-weather-sensors-simulator', der vom Bereitstellungsprozess erstellt wurde.
4. Fahren Sie mit [Schritt 2. Datenbankconnector konfigurieren](#DSX_config_db) fort.


## Schritt 2. Datenbankconnector konfigurieren
{: #DSX_config_db}

Gerätedaten können abhängig von der ausgewählten Option für das Bucketintervall in den Cloudant-Datenbanken gespeichert werden, die auf der Grundlage 'Tag', 'Woche' oder 'Monat' arbeiten. Die Daten, die mit dem gleichen Bucketintervall (Tag, Woche oder Monat) von allen Geräten erfasst werden, werden in derselben Datenbank gespeichert.

Unabhängig von der verwendeten Bucketintervallkonfiguration werden vom Connector automatisch drei Datenbanken erstellt. Eine dieser Datenbanken wird für das aktuelle Bucketintervall, eine für das nächste Intervall und eine für die Konfigurationsdatenbank erstellt. Am Ende des Intervalls werden die Gerätedaten in der Bucketdatenbank für das neue Intervall gespeichert und eine neue Datenbank wird für das nachfolgende Bucket erstellt.

Zur Verwendung von {{site.data.keyword.cloudant_short_notm}} mit DSX müssen Sie den Plattformdatenspeicher so konfigurieren, dass als Archivierungsfunktion Cloudant NoSQL DB verwendet wird.

1. Klicken Sie im {{site.data.keyword.cloudant_short_notm}}-Dashboard in der Navigationsleiste auf **Erweiterungen**.
2. Klicken Sie unter **Speicherung archivierter Daten** auf **Einrichtung**. Daraufhin werden im Abschnitt **Speicherung archivierter Daten konfigurieren** alle Cloudant NoSQL DB-Services aufgelistet, die innerhalb desselben Bluemix-Bereichs wie {{site.data.keyword.cloudant_short_notm}} verfügbar sind.
3. Wählen Sie den Cloudant NoSQL DB-Service aus, zu dem eine Verbindung hergestellt werden soll.
4. Geben Sie die folgenden Cloudant NoSQL DB-Konfigurationsoptionen an:
  - Bucketintervall = Tag
  - Zeitzone = UTC
  - Datenbankname = default (Standard)
5. Klicken Sie nun auf **Fertig** und bestätigen Sie die Berechtigung zur Herstellung einer Verbindung zum Cloudant-Service. Vergewissern Sie sich, dass die Anzeige von Popups im Browser aktiviert ist, damit Sie Zugriff auf das Bestätigungsfenster haben. Wenn Sie Cloudant NoSQL DB erfolgreich konfiguriert haben, dann wird der Status für die Speicherung archivierter Daten in 'Konfiguriert' geändert und die Gerätedaten werden in {{site.data.keyword.cloudant_short_notm}} NoSQL DB gespeichert.
6. Fahren Sie mit [Schritt 3. Simulator ausführen](#run_simulator) fort.


## Schritt 3. Simulator ausführen
{: #run_simulator}

Der Simulator publiziert reale Wettersensorendaten, die aus 17 Wetterstationen in der Gegend von Haifa stammen, in Ihrer {{site.data.keyword.iot_short_notm}}-Organisation.

1. Navigieren Sie zum Simulator.
2. Wenn Sie den Simulator in einer gebundenen {{site.data.keyword.iot_short_notm}}-Instanz bereitgestellt haben, dann fahren Sie mit Schritt 3 fort. Wenn Sie eine eigenständige Version des Simulators bereitgestellt haben, dann geben Sie die folgenden Einzelangaben ein:
   - Watson IoT Platform-Organisation
   - API-Schlüssel
   - Authentifizierungstoken

3. Klicken Sie auf die Option zum **Ausführen des Simulators**. Das Generieren der Daten kann einige Minuten in Anspruch nehmen.
4. Rufen Sie Watson IoT Platform auf, während der Simulator ausgeführt wird, und überprüfen Sie, ob die Geräte erstellt wurden und ob die Ereignisse auf diesen Geräten eingehen.
5. Fahren Sie mit [Schritt 4. DSX einrichten und Daten visualisieren](#DSX_visualize_data) fort.


## Schritt 4. DSX einrichten und Daten visualisieren
{: #DSX_visualize_data}

Gehen Sie wie folgt vor, um DSX einzurichten und mit dem Visualisieren von Daten zu beginnen:

1. [Richten Sie ein vorkonfiguriertes Jupyter-Notizbuch ein](#setup_jupyter_notebook), um Einblicke in Ihre Daten zu erhalten und Anomalien feststellen zu können.
2. [Führen Sie die Analyse aus.](#run_analysis)
3. [Konfigurieren Sie Alerts für Sensoranomalien.](#config_alerts)


### 1. Vorkonfiguriertes Jupyter-Notizbuch einrichten
{: #setup_jupyter_notebook}

Bei einem Jupyter-Notizbuch handelt es sich um eine Webanwendung, mit der Sie Dokumente erstellen und gemeinsam nutzen können, die ausführbaren Code, mathematische Formeln, Grafiken und Visualisierungen (matplotlib) und erläuternden Text enthalten.

Gehen Sie wie folgt vor, um ein vorkonfiguriertes Jupyter-Notizbuch zur Gewinnung von Einblicken in Ihre Daten einzurichten und um Anomalien festzustellen:
1. Verwenden Sie einen unterstützten Browser, um sich bei [DSX ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://datascience.ibm.com/){: new_window} mit Ihrer Bluemix-ID anzumelden.
2. Klicken Sie auf das Pluszeichen (+) und wählen Sie dann **Projekt erstellen** aus, um ein neues Projekt zu erstellen. Mit Projekten kann ein Bereich erstellt werden, in dem Sie Notizbücher erfassen und gemeinsam nutzen, Verbindungen zu Datenquellen herstellen, Pipelines erstellen und Datasets hinzufügen können.
3. Geben Sie den Projektnamen an und klicken Sie dann auf **Erstellen**. Während der Einrichtung des DSX-Kontos erstellt das System automatisch den Spark-Service und die Objektspeicherinstanz. Alternativ hierzu können Sie sie auch mithilfe der Bluemix-Schnittstelle erstellen und sie dann in einer späteren Phase Ihrem DSX-Projekt zuordnen.
4. Importieren Sie Ihre Cloudant-Berechtigungsnachweise, indem Sie Ihr Projekt im Menü **DSX** auswählen und dann auf ![Symbol zum Suchen und Hinzufügen von Daten](images/find_add_data_icon.png) klicken.
5. Klicken Sie auf die Registerkarte **Verbindungen**.
6. Klicken Sie auf **Verbindung erstellen**, um die Cloudant-Berechtigungsnachweise zu importieren, damit diese in allen Notizbüchern Ihres Projektes zur Verfügung stehen.
7. Füllen Sie die Anzeige **Neue Verbindungen** aus, indem Sie die folgenden Informationen eingeben:
   - Verbindungsname
   - Geben Sie für die **Servicekategorie** die Einstellung `Datenservice` an.
   - Wählen Sie den Cloudant-Service im Dropdown-Menü **Zielserviceinstanz** aus.
   - Wählen Sie die Cloudant-Datenbank aus, die dem aktuellen Datum entspricht.
8. Klicken Sie auf **Erstellen**.
9. Klicken Sie auf **Notizbücher hinzufügen**, um ein neues Jupyter-Notizbuch zu erstellen.
10. Wählen Sie die **Absender-URL** aus, um ein vorhandenes Notizbuch zu laden, und geben Sie dann einen beschreibenden Namen für das Notizbuch und die folgende URL an, um das Beispielnotizbuch zu öffnen:
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```
11. Klicken Sie auf **Notizbuch erstellen**. Überprüfen Sie, ob das Notizbuch mit Metadaten und Code erstellt wurde.
12. Wählen Sie die Zelle aus, die mit der Zeichenfolge '!pip install --upgrade pixiedust, ,' beginnt und klicken Sie dann auf **Wiedergeben**, um den Code auszuführen.
13. Nach Abschluss der Installation müssen Sie für den Spark-Kernel einen Neustart ausführen. Klicken Sie dazu auf das Symbol **Kernel erneut starten**.
14. Warten Sie ca. zehn Sekunden, um dem Kernel den Neustart zu ermöglichen, und klicken Sie dann auf die Zelle mit dem Kommentar, um sie auszuwählen.
15. Importieren Sie die Cloudant-Berechtigungsnachweise in diese Zelle, indem Sie die folgenden Schritte ausführen:

    1. Klicken Sie auf ![Symbol zum Suchen und Hinzufügen von Daten](images/find_add_data_icon.png).
    2. Wählen Sie die Registerkarte **Verbindungen** aus.
    3. Klicken Sie auf **In Code einfügen** aus. Daraufhin wird ein Wörterverzeichnis mit dem Namen 'credentials_1' mit Ihren Cloudant-Berechtigungsnachweisen erstellt. Wenn als Name nicht 'credentials_1' angegeben ist, dann benennen Sie das Wörterverzeichnis in 'credentials_1' um, da dies der Name ist, der erforderlich ist, damit der Notizbuchcode ausgeführt werden kann.
16. Geben Sie in der Zelle mit dem Datenbanknamen (dbname) den Namen der Cloudant-Datenbank ein, die als Datenquelle genutzt wird, z. B. `iotp_yourWIoTPorgId_default_Year-month-day`.
17. Speichern Sie das Notizbuch. Das Notizbuch kann nun ausgeführt werden.


### 2. Analyse ausführen
{: #run_analysis}

1.	Wählen Sie die Zelle aus, die Ihre Cloudant-Berechtigungsnachweise enthält.
2.	Klicken Sie auf **Wiedergeben**, um den Zellencode auszuführen.
3.	Überprüfen Sie die Ausführungsergebnisse und analysieren Sie den Python-Code, der in den jeweiligen Zellen verwendet wird.
4.	Wiederholen Sie die Schritte 2 und 3 für jede Zelle. Für Zellen mit dem Vermerk **Benutzereingabe erforderlich** müssen Sie die neuen Eingabewerte für die Variable angeben, die in der nächsten Codezelle definiert ist, bevor die Ausführung beginnen kann.

**Hinweis:** Bestimmte Zellen dienen zur Ausführung von Spark-Hintergrundjobs und benötigen eventuell mehr Zeit als gewöhnlich. Wenn die Codeausführung in einer Zelle abgeschlossen ist, dann wird der Stern (`*`) in eine Nummer geändert. Beispiel: 'In `[*]`' wird in 'In `[1]`' geändert. Nach Abschluss der Schritte können Sie die Cloud-Regeln in {{site.data.keyword.iot_short_notm}} erstellen, sodass das System automatisch Alerts generiert, wenn Anomalien festgestellt werden.


### 3. Alerts für Sensoranomalien konfigurieren
{: #config_alerts}


Sie können Cloud-Regeln in {{site.data.keyword.iot_short_notm}} erstellen. Diese Regeln können Alerts generieren, wenn Anomalien festgestellt werden, die verursacht werden, wenn publizierte Ereignisse die Schwellenwerte überschreiten, die Sie im Notizbuch abgeleitet haben.

Im vorliegenden Beispiel verwenden Sie Stickstoffdioxid (NO2 = Nitrogen Dioxide) und die oberen/unteren Grenzwerte für ein bestimmtes Gerät. Es wird eine E-Mail-Aktion erstellt, sodass eine E-Mail an eine bestimmte E-Mail-Adresse gesendet wird, sobald der NO2-Wert die definierten Grenzwerte überschreitet.

Gehen Sie wie folgt vor, um Cloud-Regeln zu erstellen:

1. Erstellen Sie ein Schema:
    1. Wählen Sie auf der Registerkarte **Geräte** Ihres {{site.data.keyword.iot_short_notm}}-Dashboards die Registerkarte **Schemas verwalten** aus.
    2. Klicken Sie auf **Schema hinzufügen**.
    3. Wählen Sie den Gerätetyp (DeviceType) 'WS' aus, für den das Schema erstellt wird, und klicken Sie dann auf **Weiter**.
    4. Klicken Sie auf **Eigenschaft hinzufügen**, um den Datenpunkt der verbundenen Geräte hinzuzufügen.
    5. Legen Sie auf der Registerkarte **Manuell** für das Datentypfeld die Angabe `Gleitkommawert` und für das Eigenschaftsfeld die Angabe `NO2` fest.
    6. Klicken Sie auf **OK**.
    7. Klicken Sie auf **Fertigstellen**.

2. Erstellen Sie eine Aktion:
    1. Wählen Sie die Registerkarte **Regeln** im {{site.data.keyword.iot_short_notm}}-Dashboard aus.
    2. Wählen Sie die Registerkarte **Aktionen** aus.
    3. Klicken Sie auf **+Eine Aktion erstellen**.
    4. Geben Sie in der Anzeige **Neue Aktion erstellen** einen Namen ein und wählen Sie dann als Aktionstyp 'E-Mail senden' aus.
    5. Klicken Sie auf **Weiter**.
    6. Aktivieren Sie in der Anzeige **Aktion bearbeiten** den Umschalter **Daten einfügen**.
    7. Klicken Sie auf **Fertigstellen**.
    8. Wählen Sie auf der Registerkarte **Regeln** die Registerkarte **Durchsuchen** aus.
    9. Klicken Sie auf **+Cloud-Regel erstellen**.
    10. Geben Sie in der Anzeige **Neue Cloud-Regel hinzufügen** einen Namen für Ihre Regel ein und wählen Sie Ihren Schemanamen im Feld **Gültig für** aus. Im vorliegenden Beispiel lautet der Schemaname 'WS'.
    11. Klicken Sie auf **Weiter**.

3. Legen Sie die Bedingung fest - Die Grenzwerte, die Sie in diesen Schritten verwenden, befinden sich im letzten Codeblock, der im Notizbuch ausgeführt wird, und zwar neben dem NO2-Diagramm:
    1. Klicken Sie auf 'Neue Bedingung' und legen Sie die erste Bedingung fest:
    - Geben Sie im Feld **Eigenschaft** den Wert `Nitrogen Dioxide` (Stickstoffdioxid) ein.
    - Wählen Sie im Feld **Operator** das Symbol für 'größer als' (`>`) aus.
    - Geben Sie im Feld **Wert** den oberen Grenzwert ein.
    - Klicken Sie auf **OK**.
    2. Wählen Sie OR aus und fügen Sie dann die zweite Bedingung hinzu:
    - Geben Sie im Feld **Eigenschaft** den Wert `Nitrogen Dioxide` (Stickstoffdioxid) ein.
    - Wählen Sie im Feld **Operator** das Symbol für 'kleiner als' (`) aus.<`.
    - Geben Sie im Feld **Wert** den unteren Grenzwert ein.
    - Klicken Sie auf **OK**. Die Bedingungen zur Auslösung der Regel sind nun festgelegt.
4. Legen Sie als Aktion 'E-Mail senden' fest und klicken Sie dann auf **OK**, um die Regel zu aktivieren. Daraufhin wird immer dann ein E-Mail-Alert generiert, wenn der Messwert des Stickstoffdioxids, der von einem Gerät publiziert wird, einen der Grenzwerte überschreitet. Sie können den Simulator ausführen, um die Alert-E-Mails anzuzeigen.


## Weitere Schritte

Weitere Informationen zu DSX finden Sie in den folgenden Ressourcen:

 - [WIoTP-Cloud-Regeln ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window}
 - [DSX Community Notebooks und Lernprogramme ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} unter den Links für die weiterführenden Informationen zu Jupyter-Notizbüchern
 - [Analytics-Anleitungen im Watson IoT Platform-Cookbook ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
