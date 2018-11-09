---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} Edge Edge - Übersicht (Vorschau)
{: #edge_overview}

**Wichtig:** Die {{site.data.keyword.iot_full}} Edge-Komponente steht nur im Rahmen eines eingeschränkten Vorschauprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Mit {{site.data.keyword.iot_short_notm}} Edge können Sie {{site.data.keyword.iot_short_notm}}-Funktionen auf Edge-Geräte erweitern. Edge-Geräte befinden sich am Rand ('Edge') eines Netzes innerhalb einer Organisation. Beispiele sind Sensoren und industrielle Steuereinheiten an einem physischen Standort, z. B. in einer Fabrik. Mit {{site.data.keyword.iot_short_notm}} Edge können Daten innerhalb der Geräte verarbeitet werden, bevor sie an die Cloud gesendet werden.

Um {{site.data.keyword.iot_short_notm}} Edge zu verwenden, erstellen Sie Edge-Knoten und konfigurieren diese Knoten mit den Services, die in ihnen ausgeführt werden sollen. WIoTP Edge stellt diese Services dann auf den Knoten bereit. Die Edge-Knoten können dann mit diesen aktiven Services Nachrichten von Geräten mit dem Edge-Gateway senden. Das Gateway verarbeitet die Nachrichten, wandelt die Daten um und kann die Daten dann, je nachdem, wie die Schnittstellen konfiguriert sind, an die Cloud senden.

Die {{site.data.keyword.iot_short_notm}} Edge-Vorschau stellt den Core IoT-Standardservice zusammen mit der Möglichkeit bereit, angepasste Services zu erstellen, z. B. einen Service zur Vorhersage von Fehlern bei einem Roboter für die Fertigungsbodenroboter und zum Senden von Fehlerbenachrichtigungen. Ein Beispiel für ein Edge-Gerät ist ein Sensor, der die CPU-Auslastung überwacht und einen Alert sendet, wenn die Nutzung einen bestimmten Prozentsatz überschreitet.

## {{site.data.keyword.iot_short_notm}} Edge-Komponenten

**Edge-Knoten**
Ein Edge-Knoten besteht aus einem Gateway und einem Gerät, das sich am Rand des Netzes befindet.

**Edge-Gerät**
Ein Gerät, z. B. ein Sensor, das sich am Rand eines Netzes befindet und Services ausführt, die Daten verarbeiten, bevor diese an die Cloud gesendet werden. Ein Beispiel für ein Edge-Gerät ist ein Sensor, der die CPU-Auslastung überwacht und einen Alert sendet, wenn die Nutzung einen bestimmten Prozentsatz überschreitet.

**Edge-Gateway**
Gateways sind spezialisierte Geräte, die über die kombinierten Funktionen einer Anwendung und eines Gerätes verfügen, wodurch sie als Zugriffspunkt für andere Geräte dienen können. Wenn Edge aktiviert ist, ermöglichen Edge-Gateways Geräten, die nicht direkt mit dem Internet verbunden werden können, auf den {{site.data.keyword.iot_short_notm}}-Service zuzugreifen, indem sie zunächst eine Verbindung zum Gateway-Gerät auf dem Edge-System herstellen.

**Edge-Gateway-Typ**
Ein Gateway-Typ gruppiert Gateway-Geräte mit gemeinsamen Attributen, wie Modellnummer oder Standort. Wenn Edge-Funktionen aktiviert sind und ein Edge-Gateway-Gerät zu {{site.data.keyword.iot_short_notm}} hinzugefügt wird, werden die Attribute aus dem Edge-Gateway-Typ als Vorlage für das neue Edge-Gateway-Gerät verwendet.

**Edge-Services**
Services sind Funktionen, die zu einem {{site.data.keyword.iot_short_notm}} Edge-Gateway hinzugefügt werden und Prozesse wie die Datenbearbeitung ausführen. Sie erstellen Anwendungen, die die Services verwenden.

**Edge-Servicekatalog**
Der Core IoT-Standardservice ist im Katalog enthalten und wird zu allen Edge-Knoten hinzugefügt. Sie können angepasste Services auf der Basis Ihrer Geschäftsanforderungen hinzufügen. Der Edge-Servicekatalog enthält alle verfügbaren angepassten Services.

**Dateiserver-Repository**
Das Dateiserver-Repository enthält Docker-Container und -Images. Alle Edge-Verarbeitungsfunktionen oder Services funktionieren in Docker-Containern. Sie können die Docker-Images auswählen, die innerhalb der Geräte ausgeführt werden sollen.

## Weitere Informationen
{: #more_info}

Weitere Informationen zum Konfigurieren, Installieren und Entwickeln von {{site.data.keyword.iot_short_notm}} Edge finden Sie in den folgenden Themen:
 - [{{site.data.keyword.iot_short_notm}} Edge konfigurieren](WIoTP_edge_config.html#edge_configure)
 - [{{site.data.keyword.iot_short_notm}} Edge auf Edge-Geräten installieren](WIoTP_edge_install.html#edge_install_device)
 - [Für {{site.data.keyword.iot_short_notm}} Edge entwickeln](WIoTP_edge_dev.html#edge_dev)
