---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung zu {{site.data.keyword.iot_short_notm}}
{: #gettingstartedtemplate}

{{site.data.keyword.iot_full}} für {{site.data.keyword.Bluemix_notm}} ist ein vielseitiges Toolkit, das Gateway-Geräte, Gerätemanagement und leistungsfähigen Anwendungszugriff zur Verfügung stellt. Durch Verwendung von {{site.data.keyword.iot_short_notm}} können Sie Daten zu verbundenen Geräten erfassen und Echtzeitdaten aus Ihrer Organisation analysieren.
{:shortdesc}

## Vorbereitende Schritte
{: #byb}

Bevor Geräte verbunden und Daten verwendet werden können, müssen Sie sich für ein {{site.data.keyword.Bluemix_notm}}-Konto registrieren und eine Instanz des {{site.data.keyword.iot_short_notm}}-Service in Ihrer {{site.data.keyword.Bluemix_notm}}-Organisation erstellen. Sie können eine {{site.data.keyword.iot_short_notm}}-Instanz direkt über die [{{site.data.keyword.iot_short_notm}}-Seite im IBM Cloud-Servicekatalog ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window} erstellen.  

Detaillierte Informationen zur Vorgehensweise beim Anmelden eines Kontos in {{site.data.keyword.Bluemix_notm}}, beim Konfigurieren von Regionen sowie andere Einstellungen für die Kontoverwaltung finden Sie in [IBM Cloud-Konto verwalten](https://console.ng.bluemix.net/docs/admin/account.html#signup).

Sie können Ihre {{site.data.keyword.iot_short_notm}}-Instanz im Dashboard einrichten und konfigurieren. Zum Öffnen des Dashboards wechseln Sie zu Ihrer Instanz des {{site.data.keyword.iot_short_notm}}-Service in {{site.data.keyword.Bluemix_notm}} und klicken Sie anschließend auf **Starten**.

## Informationen zu diesem Vorgang

In den folgenden Schritten wird beschrieben, wie Sie sich schnell mit dem {{site.data.keyword.iot_short_notm}}-Service vertraut machen können.

Eine ausführlichere Gruppe mit einführenden Anleitungen und Beispielanwendungen, die Sie durch die Grundlagen der Entwicklung eines produktionsbereiten End-to-End-IoT-Prototypsystms mit {{site.data.keyword.iot_short_notm}} führt, ist ebenfalls verfügbar. Wenn Sie ein Entwickler sind, der zum ersten Mal mit {{site.data.keyword.iot_short_notm}} arbeitet, dann verwenden Sie die in einzelne Arbeitsschritte aufgegliederte Vorgehensweise im Abschnitt [Einführende Anleitungen](https://console.bluemix.net/docs/services/IoT/getting_started/getting-started-iot-overview.html#getting-started).

## Schritt 1: Geräte verbinden
{: #up_and_running}

Um den Service betriebsbereit zu machen, müssen Sie folgende Optionen betrachten, deren Auswahl von Ihrer Situation abhängt:

|  |   Service ist bereitgestellt | Service ist nicht bereitgestellt
 | -------------| ------------- | -------------
  |**Ich verfüge über ein Gerät, das ich verbinden kann** | [Verbinden Sie Ihr Gerät mit {{site.data.keyword.iot_short_notm}}](iotplatform_task.html#iotplatform_task).| Erkunden Sie das Herstellen von Verbindungen für Geräte in [{{site.data.keyword.iot_short_notm}} ausprobieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play){:new_window}.
  |**Ich verfüge nicht über ein Gerät, das ich verbinden kann** | [Erstellen und verbinden Sie einen Node-RED-Gerätesimulator](nodereddevice_sample.html){:new_window}. Oder [stellen Sie eine Verbindung für Ihr Smartphone her ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play/device/smartphone){:new_window}. | Beginnen Sie mit [Watson IoT Platform Starter](https://console.bluemix.net/docs/starters/IoT-starter/iot500.html).
  
Weitere Informationen zur Vorgehensweise beim Verbinden bestimmter Gerätetypen mit {{site.data.keyword.iot_short_notm}} finden Sie in [developerWorks-Anleitungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){:new_window}.  

Entwicklerdokumentation für Geräteverbindungen siehe
- [MQTT-Konnektivität für Geräte](devices/mqtt.html).
- [MQTT-Konnektivität für Gateways](gateways/mqtt.html).

<!--
## Step 2: Analyze your device data
{: #analyzing_data}
Start exploring the real-time data that the devices are sending to {{site.data.keyword.iot_short_notm}}.
{{site.data.keyword.iot_short_notm}} includes the following analytics tools:  
- [Boards and cards](data_visualization.html) to visualize your real-time device data.
- [Rules and actions](analytics.html) that are triggered by real-time device data.
For a quick getting started example, see the [Using Rules and Actions with IBM Watson IoT Platform Cloud Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/using-rules-and-actions-with-ibm-watson-iot-platform-cloud-analytics/){:new_window} developerWorks recipe.
-->

## Schritt 2: Anwendungen erstellen, mit denen Ihre Gerätedaten verarbeitet werden
{: #develop_applications}

Erstellen Sie eigene Anwendungen und verbinden Sie sie, um Gerätedaten zu verarbeiten.

Weitere Informationen finden Sie in den folgenden Abschnitten:   
- Lesen Sie die [Dokumentation für Anwendungsentwickler](applications/api.html) und die [{{site.data.keyword.iot_short_notm}}-API-Dokumentation](reference/api.html).
- Lesen Sie die [{{site.data.keyword.iot_short_notm}}-Clientbibliotheken](iot_platform_client_lib.html), die Tools und Dateien zum Erstellen und Entwickeln von Code für die Integration und das Verbinden Ihrer Geräte und Anwendungen bereitstellen.
- [{{site.data.keyword.cloudantfull}}-Service](cloudant_connector.html) mit Ihrer {{site.data.keyword.iot_short_notm}}-Instanz verbinden, um Langzeitdaten zum Gerät zu speichern.
- Erstellen Sie eigene Regeln mithilfe des neuen Features für [eingebettete Regeln (Beta)](information_management/im_rules.html).
