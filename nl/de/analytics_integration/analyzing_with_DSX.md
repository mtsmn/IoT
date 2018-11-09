---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-01"
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
){: new_window}, Zugriff auf den Service [Apache Spark ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} und Zugriff auf ein [DSX-Konto ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window} verfügen.


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
2. Warten Sie, bis das System die Bereitstellung abgeschlossen hat, und navigieren Sie dann zum IBM Cloud-Dashboard.
3. Starten Sie den {{site.data.keyword.iot_short_notm}}-Service 'wiotp-for-weather-sensors-simulator', der vom Bereitstellungsprozess erstellt wurde.
4. Fahren Sie mit [Schritt 2. Datenbankconnector konfigurieren](#DSX_config_db) fort.


## Schritt 2. Datenbankconnector konfigurieren
{: #DSX_config_db}

Gerätedaten können abhängig von der ausgewählten Option für das Bucketintervall in den Cloudant-Datenbanken gespeichert werden, die auf der Grundlage 'Tag', 'Woche' oder 'Monat' arbeiten. Die Daten, die mit dem gleichen Bucketintervall (Tag, Woche oder Monat) von allen Geräten erfasst werden, werden in derselben Datenbank gespeichert.

Unabhängig von der verwendeten Bucketintervallkonfiguration werden vom Connector automatisch drei Datenbanken erstellt. Eine dieser Datenbanken wird für das aktuelle Bucketintervall, eine für das nächste Intervall und eine für die Konfigurationsdatenbank erstellt. Am Ende des Intervalls werden die Gerätedaten in der Bucketdatenbank für das neue Intervall gespeichert und eine neue Datenbank wird für das nachfolgende Bucket erstellt.

Zur Verwendung von {{site.data.keyword.cloudant_short_notm}} mit DSX müssen Sie den Plattformdatenspeicher so konfigurieren, dass als Archivierungsfunktion Cloudant NoSQL DB verwendet wird.

1. Klicken Sie im {{site.data.keyword.cloudant_short_notm}}-Dashboard in der Navigationsleiste auf **Erweiterungen**.
2. Klicken Sie unter **Speicherung archivierter Daten** auf **Einrichtung**. Daraufhin werden im Abschnitt **Speicherung archivierter Daten konfigurieren** alle Cloudant NoSQL DB-Services aufgelistet, die innerhalb desselben IBM Cloud-Bereichs wie {{site.data.keyword.cloudant_short_notm}} verfügbar sind.
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
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. Vorkonfiguriertes Jupyter-Notizbuch einrichten
{: #setup_jupyter_notebook}

Bei einem Jupyter-Notizbuch handelt es sich um eine Webanwendung, mit der Sie Dokumente erstellen und gemeinsam nutzen können, die ausführbaren Code, mathematische Formeln, Grafiken und Visualisierungen (matplotlib) und erläuternden Text enthalten.

Gehen Sie wie folgt vor, um ein vorkonfiguriertes Jupyter-Notizbuch zur Gewinnung von Einblicken in Ihre Daten einzurichten und um Anomalien festzustellen:
1. Verwenden Sie einen unterstützten Browser, um sich bei [DSX ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://datascience.ibm.com/){: new_window} mit Ihrer IBM Cloud-ID anzumelden.
2. Klicken Sie auf **+ Neues Projekt**, um ein neues Projekt zu erstellen, und wählen Sie die Kachel **Jupyter-Notizbücher** aus. Mit Projekten kann ein Bereich erstellt werden, in dem Sie Notizbücher erfassen und gemeinsam nutzen, Verbindungen zu Datenquellen herstellen, Pipelines erstellen und Datasets hinzufügen können.
3. Klicken Sie auf das Dropdown-Menü **+ Zu Projekt hinzufügen** und wählen Sie **Verbindung** aus. Wählen Sie in der Liste der Services **Cloudant** aus und rufen Sie anschließend Ihre Cloudant-URL, den Benutzernamen und das Kennwort von der Registerkarte 'Serviceberechtigungsnachweise' der Seite für den {{site.data.keyword.cloudant_short_notm}}-Service ab und geben Sie diese Informationen in den angezeigten Felder ein. Geben Sie einen Namen für die Verbindung an und klicken Sie auf **Erstellen**.
4. Überprüfen Sie, ob die Verbindung unter dem Abschnitt 'Datenassets' des Dashboards verfügbar ist.
5. Klicken Sie auf **+ Neues Notizbuch**, um ein neues Jupyter-Notizbuch zu erstellen.
6. Wählen Sie in der Dropdown-Liste 'Laufzeit auswählen' unter 'Services' die Option **spark-iw** aus. Wenn diese Option nicht vorhanden ist, bedeutet dies, dass der Apache Spark-Service nicht ordnungsgemäß bereitgestellt wurde. Stellen Sie im IBM Cloud-Dashboard sicher, dass der Service vorhanden ist; ist dies nicht der Fall, wechseln Sie zur Seite mit den Services, um den Service bereitzustellen.
7. Definieren Sie die Sprache als **Python 2** und die Spark-Version als **2.1**.
8. Wählen Sie die **Absender-URL** aus, um ein vorhandenes Notizbuch zu laden, und geben Sie dann einen beschreibenden Namen für das Notizbuch und die folgende URL an, um das Beispielnotizbuch zu öffnen:
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. Klicken Sie auf **Notizbuch erstellen**. Überprüfen Sie, ob das Notizbuch mit Metadaten und Code erstellt wurde.
10. Wählen Sie die Zelle aus, die mit '!pip install --upgrade pixiedust,,' beginnt, und klicken Sie anschließend auf **Ausführen**, um den Code auszuführen.
11. Wenn die Installation abgeschlossen ist, starten Sie den Spark-Kernel erneut, indem Sie auf das Symbol **Kernel erneut starten** klicken oder **Erneut starten** im Kernel-Menü auswählen.
12. Warten Sie ca. zehn Sekunden, bis der Kernel neu gestartet wird, und klicken Sie dann auf die leere Codezelle mit dem Kommentar, um sie auszuwählen.
13. Importieren Sie die Cloudant-Berechtigungsnachweise in diese Zelle, indem Sie die folgenden Schritte ausführen:

    1. Klicken Sie au ![Symbol zum Suchen und Hinzufügen von Daten](images/find_add_data_icon.png).
    2. Wählen Sie die Registerkarte **Verbindungen** aus.
    3. Klicken Sie auf **In Code einfügen** aus.
Daraufhin wird ein Wörterverzeichnis mit dem Namen 'credentials_1' mit Ihren Cloudant-Berechtigungsnachweisen erstellt. Wenn als Name nicht 'credentials_1' angegeben ist, dann benennen Sie das Wörterverzeichnis in 'credentials_1' um, da dies der Name ist, der erforderlich ist, damit der Notizbuchcode ausgeführt werden kann.
14. Geben Sie in der Zelle mit dem Datenbanknamen (dbname) den Namen der Cloudant-Datenbank ein, die als Datenquelle genutzt wird, z. B. `iotp_yourWIoTPorgId_default_Year-month-day`.
15. Speichern Sie das Notizbuch. Das Notizbuch kann nun ausgeführt werden.


### 2. Analyse ausführen
{: #run_analysis}

1.	Wählen Sie die Zelle aus, die Ihre Cloudant-Berechtigungsnachweise enthält.
2.	Klicken Sie auf **Wiedergeben**, um den Zellencode auszuführen.
3.	Überprüfen Sie die Ausführungsergebnisse und analysieren Sie den Python-Code, der in den jeweiligen Zellen verwendet wird.
4.	Wiederholen Sie die Schritte 2 und 3 für jede Zelle. Für Zellen mit dem Vermerk **Benutzereingabe erforderlich** müssen Sie die neuen Eingabewerte für die Variable angeben, die in der nächsten Codezelle definiert ist, bevor die Ausführung beginnen kann.

**Hinweis:** Bestimmte Zellen dienen zur Ausführung von Spark-Hintergrundjobs und benötigen eventuell mehr Zeit als gewöhnlich. Wenn die Codeausführung in einer Zelle abgeschlossen ist, dann wird der Stern (`*`) in eine Nummer geändert. Beispiel: 'In `[*]`' wird in 'In `[1]`' geändert. Nach Abschluss der Schritte können Sie die Cloud-Regeln in {{site.data.keyword.iot_short_notm}} erstellen, sodass das System automatisch Alerts generiert, wenn Anomalien festgestellt werden.


<!-- ### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails. -->


## Weitere Schritte

Weitere Informationen zu DSX finden Sie in den folgenden Ressourcen:

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [DSX Community Notebooks und Lernprogramme ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} unter den Links für die weiterführenden Informationen zu Jupyter-Notizbüchern
 - [Analytics-Anleitungen im Watson IoT Platform-Cookbook ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
