---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Übersicht über die Features von {{site.data.keyword.iot_short_notm}}
{: #feature_overview}

{{site.data.keyword.iot_full}} basiert auf den folgenden zentralen Bereichen:

  1. Verbindung - Verbinden von Geräten und Entwickeln von Anwendungen.
  2. Informationsmanagement - Speichern, Normalisieren, Umwandeln und Überprüfen von Gerätedaten und Integration von {{site.data.keyword.iot_short_notm}} mit anderen Services.
  3. Analyse - Visualisieren von Gerätedaten in Echtzeit durch Verwendung des {{site.data.keyword.iot_short_notm}}-Dashboards.
  4. Risikomanagement - Konfigurieren einer sicheren Konnektivität und Architektur mit Zugriffssteuerung für Benutzer und Anwendungen.

## Verbindung
{: #connect}

Die {{site.data.keyword.iot_short_notm}}-Verbindungsoption ist der Ausgangspunkt aller {{site.data.keyword.iot_short_notm}}-Services. Unter der {{site.data.keyword.iot_short_notm}}-Verbindungsoption stehen das Herstellen von Geräteverbindungen, das Erstellen von Anwendungen, die Steuerung Ihrer Geräte und die Interaktion mit Services von Drittanbietern zur Verfügung.

### Gateway-Geräte

Durch Verwendung eines Gateways können Sie für Geräte Verbindungen zu {{site.data.keyword.iot_short_notm}} herstellen, die andernfalls keine Verbindung zum Internet herstellen könnten. Gateways verfügen über die gesamte Funktionalität eines Geräts, können jedoch auch im Namen der mit ihnen verbundenen Geräte Publizierungen und Subkribierungen durchführen. Gateway-Geräte ermöglichen es Gruppen von Sensoren, die nicht mit dem Internet verbunden werden können, eine Verbindung zu {{site.data.keyword.iot_short_notm}} herzustellen, indem sie ihre Daten an ein Gateway senden. Weitere Informationen finden Sie in der Veröffentlichung zum [Entwickeln von Gateways](https://console.ng.bluemix.net/docs/services/IoT/gateways/gw_dev_index.html).

### Gerätemanagement

Gerätemanagementfunktionen können über die Gerätemanagement-API und einen für Geräte installierten Gerätemanagementagenten bereitgestellt werden. Geräte mit einem Gerätemanagementagenten können Gerätemanagementaktionen ausführen, die über das Hauptdashboard von {{site.data.keyword.iot_short_notm}} oder die Gerätemanagement-API ausgelöst werden können. Das Gerätemanagement gibt Ihnen die Möglichkeit, einen Neustart auszuführen, Firmware-Updates herunterzuladen und zu installieren sowie Geräte auf Werkseinstellungen zurückzusetzen. Das Gerätemanagement kann so erweitert werden, dass es angepasste Gerätemanagementaktionen umfasst. Weitere Informationen hierzu finden Sie in der [Dokumentation für das Gerätemanagement](https://console.ng.bluemix.net/docs/services/IoT/devices/device_mgmt/index.html).

### Erweiterungen und Serviceintegrationen

Im Rahmen von Erweiterungen und der Serviceintegration können sowohl externe Services als auch benutzerdefinierte Erweiterungen von Kernservices zu einer Instanz von {{site.data.keyword.iot_short_notm}} hinzugefügt werden. Zu den externen Services, die mit {{site.data.keyword.iot_short_notm}} integriert werden können, zählen standortbezogene Wetterservices von 'The Weather Company', die Ihnen ermöglichen, das aktuelle Wetter an einer Geräteposition zu ermitteln, Jasper-SIM-Daten und {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.ssoshort}}. Weitere Informationen zu den Serviceintegrationen von Drittanbietern sowie Erweiterungen finden Sie in der Veröffentlichung [Externe Services integrieren](https://console.ng.bluemix.net/docs/services/IoT/reference/extensions/index.html).


---

## Informationsmanagement
{: #information_management}

Das {{site.data.keyword.iot_short_notm}}-Informationsmanagement steuert die Daten, die von Geräten gesendet werden, nachdem sie Ihren {{site.data.keyword.iot_short_notm}}-Service erreicht haben. Zum Informationsmanagement gehört die Datenspeicherung und die Umwandlung von Daten.

### Cache für zuletzt gemeldete Ereignisse des Geräts

Durch Verwendung der {{site.data.keyword.iot_short_notm}}-API für zuletzt gemeldete Ereignisse können Sie das zuletzt von einem Gerät gesendete Ereignis abrufen. Dies funktioniert unabhängig davon, ob das Gerät online oder offline ist, wodurch Sie den Gerätestatus unabhängig vom physischen Standort des Geräts oder dem Status seiner Verwendung abrufen können. Daten zum letzten Ereignis eines Geräts können für jedes Ereignis abgerufen werden, das nicht später als 45 Tage zurückliegt.

### Datenspeicherung von Geräteereignissen

Geräteereignisdaten von Ihrem {{site.data.keyword.iot_short_notm}}-Service können zur späteren Verwendung gespeichert werden. Die Datenspeicherung ist ein wichtiger erster Schritt zur Ausführung einer fundierten Analyse, um über diese Daten Einblick zu gewinnen.  Sie können beispielsweise Änderungen über große Zeiträume verfolgen und Datengruppen für die Verwendung mit leistungsfähigen Analysetools speichern, einschließlich Verwendung von Watson-APIs und der kognitiven Datenverarbeitung. Weitere Informationen finden Sie in der Veröffentlichung zum [Verbinden eines {{site.data.keyword.cloudant_short_notm}}-Archivierungsservice](https://console.ng.bluemix.net/docs/services/IoT/cloudant_connector.html) bzw. [Verbinden eines {{site.data.keyword.messagehub}}-Archivierungsservice](https://console.ng.bluemix.net/docs/services/IoT/message_hub.html).

### Datenmanagement

Unterschiedliche Marken und Modelle von Geräten führen das Publizieren von Daten in unterschiedlichen Formaten durch. Die Funktion für das Datenmanagement ermöglicht Ihnen das Umwandeln und Normalisieren dieser Daten in einer einzigen, logischen Ansicht (sog. *Gerätestatus*), die von Anwendungen nachvollzogen und verarbeitet werden kann. Mit der Funktion für das Datenmanagement kann die Anwendungsentwicklung erheblich vereinfacht werden, da die Anwendung die unterschiedlichen Formate der Ereignisdaten, die von den einzelnen Geräten gesendet werden, nicht mehr interpretieren können muss. Wenn Geräte Ereignisse in {{site.data.keyword.iot_short_notm}} publizieren, dann kann der Inhalt der Ereignisse anhand von Zuordnungen benutzerdefinierten Statuseigenschaften zugewiesen werden. Wenn das eingehende Ereignis zu einer Änderung des Status eines Geräts führt, dann werden die Werte der Gerätestatuseigenschaften aktualisiert und in {{site.data.keyword.iot_short_notm}} gespeichert. Die Werte werden der Anwendung auf Anforderung zur Verfügung gestellt. Dazu wird eine HTTP-Anwendungsprogrammierschnittstelle (API) verwendet oder es wird eine Subskription für ein Thema durchgeführt.

Weitere Informationen zur Verwendung dieser Funktion finden Sie in [Einführung zum Datenmanagement](GA_information_management/ga_im_device_twin.html).

---
## Analyse
{: #analytics}

### Echtzeitdaten von Geräten visualisieren

Sie können mithilfe von Dashboardkarten Gerätedaten in Echtzeit visualisieren und anzeigen. Dashboardkarten überwachen Gerätedaten in Echtzeit und zeigen diese an, wodurch Sie wichtige Geräte oder Gerätedaten verfolgen können. Diese Visualisierungen werden im Hauptdashboard von {{site.data.keyword.iot_short_notm}} angezeigt, um Ihnen einen schnellen Zugriff auf den Kontext und den Status von Gerätedaten in Echtzeit zu ermöglichen. Weitere Informationen finden Sie in der Veröffentlichung zum [Visualisieren von Echtzeitdaten](https://console.ng.bluemix.net/docs/services/IoT/data_visualization.html).

### Edge Analytics und Cloud Analytics

Mithilfe von Cloud Analytics für {{site.data.keyword.iot_short_notm}} geben Sie Regelbedingungen an, die auf Echtzeitdaten von Geräten basieren, und die Alerts und optionale Aktionen auslösen, wenn die Bedingungen erfüllt sind. Beispielsweise können Sie eine Regel erstellen, um sicherzustellen, dass ein Alert an das Dashboard auf dem Gerät eines Benutzers und eine E-Mail an den Administrator gesendet wird, wenn die Verbindung zum Gerät verloren geht oder die Temperatur des Geräts einen Höchststand erreicht. Weitere Informationen finden Sie in der [Dokumentation zu Cloud Analytics](https://console.ng.bluemix.net/docs/services/IoT/cloud_analytics.html).

Mit Edge Analytics verschieben Sie den regelauslösenden Prozess der Analyse von der Cloud in ein für Edge Analytics aktiviertes Gateway, das den Gerätedatenverkehr zur Cloud stark reduzieren kann, indem die Analyseverarbeitung gerätenah ausgeführt wird. Weitere Informationen finden Sie in der [Dokumentation zu Edge Analytics](https://console.ng.bluemix.net/docs/services/IoT/edge_analytics.html).

---

## Risikomanagement
{: #risk_management}

### Sichere Konnektivität und Architektur

Die Architektur von {{site.data.keyword.iot_short_notm}} wurde so entworfen, dass es Geräten nicht möglich ist, die Identität eines anderen Geräts anzunehmen, wodurch die Integrität Ihrer Gerätedaten gewahrt bleibt. Geräte stellen zu {{site.data.keyword.iot_short_notm}} eine Verbindung über eine Kombination aus einer Client-ID und einem Authentifizierungstoken her, das nur Ihnen bekannt ist. Nach der Registrierung der Geräte oder der Generierung der API-Schlüssel, wird das Authentifizierungstoken zufalls- und hashverschlüsselt, um die Sicherheit Ihrer Berechtigungsnachweise aufrecht zu erhalten.

Mit dem Add-on für das Risiko- und Sicherheitsmangement können Sie die Sicherheit von {{site.data.keyword.iot_short_notm}} verbessern, um sicherzustellen, dass alle Verbindungspunkte zwischen dem Server und den Geräten mit bewährten Berechtigungsnachweisen authentifiziert werden. Im Rahmen dieses Add-ons werden neben den von {{site.data.keyword.iot_short_notm}} verwendeten Benutzer-IDs und Tokens Zertifikate und die TLS-Authentifizierung (TLS, Transport Layer Security) verwendet, um festzulegen, wie und wo Geräte mit der Plattform eine Verbindung herstellen. Während der Kommunikation zwischen Geräten und dem Server wird allen Geräten, die nicht über gültige Zertifikate mit Serverzugriff verfügen, wie im Add-on für Risiko- und Sicherheitsmanagement festgelegt, der Zugriff verweigert, selbst dann, wenn sie gültige Benutzer-IDs und Kennwörter verwenden.

---
