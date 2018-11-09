---
copyright:
  years: 2016, 2018
lastupdated: "2018-02-23"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Abrechnung

Bezahlte {{site.data.keyword.iot_full}}-Servicepläne (andere Pläne als 'Lite') basieren auf dem Konzept des Megabytevolumens, das im Verlauf eines Monats ausgetauscht wird. Das vorliegende Dokument enthält detaillierte Informationen zur Vorgehensweise von {{site.data.keyword.iot_short_notm}} bei der Messung der Daten für die Erstellung von Nutzungsinformationen, mit denen die Kosten für die Nutzung des Service ermittelt werden.  Die Nutzungsinformationen können verwendet werden, um die Kosten der Nutzung von {{site.data.keyword.iot_short_notm}} auf Basis des Designs und der Anzahl der Geräte, Anwendungen und Gateways zu schätzen.

Informationen zu den Kosten für jedes Megabyte an ausgetauschten Daten finden Sie unter dem {{site.data.keyword.iot_short_notm}}-Service im {{site.data.keyword.Bluemix_notm}}-Katalog für die erforderliche Region.

Sie können den [Preisrechner ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://iot-cost-calculator.ng.bluemix.net/) verwenden, um die Kosten eines {{site.data.keyword.iot_short_notm}}-Service zu berechnen.

Die folgenden Elemente werden nach Nutzung gemessen und in Rechnung gestellt: 

## Ausgetauschte Daten
Die Berechnung der *ausgetauschten Daten* umfasst die Kontodaten, die von Anwendungen, Geräten und Gateways mithilfe des MQTT- oder HTTP-Messagings ausgetauscht werden, sowie die Daten, die von Anwendungen mithilfe der HTTP-API ausgetauscht werden.

### Ausgetauschte MQTT-Daten
Bei Verwendung von MQTT zählt der TCP-Datenstrominhalt zu den *ausgetauschten Daten*.  Die Nutzdaten der MQTT-Nachrichten werden in die Kumulation der ausgetauschten Daten mithilfe des MQTT-Protokolls einbezogen.  Die Kumulation des Nutzdatenvolumens wird durchgeführt, wenn die Nachricht publiziert wird, und auch wenn die Nachricht an einen Subskribenten zugestellt wird.

Das MQTT-Protokoll wurde in Bezug auf seine Lightweight-Eigenschaften optimiert.  Obwohl {{site.data.keyword.iot_short_notm}} die Größe der MQTT-Pakete einschließt, wenn die *ausgetauschten Daten* kumuliert werden, ist der dabei entstehende Aufwand aufgrund des Lightweight-Protokolls gering.  Die folgende Tabelle gibt Aufschluss über die Mindestgröße der MQTT-Pakete:

|MQTT-PAKET                    |Mindestgröße (Byte)  |Variablen und zugehörige Mindestgröße (in Byte)|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |Client-ID (12 wenn d:xxxxxx:t:i), dabei Thema (0) und Nachricht (0), wenn definiert, Benutzername (14 für Geräte - 'use-token-auth') und Kennwort (18 bei automatischer Generierung)|
|CONNACK                        |4                   |Keine Angabe|
|PUBLISH                        |21                  |Themenname (17 bei iot-2/evt/i/fmt/f) und Nutzdaten (0 ist das Minimum).  Gilt für ein- und ausgehende PUBLISH-Pakete.|
|PUBACK                         |4                   |Keine Angabe|
|PUBREC                         |4                   |Keine Angabe|
|PUBREL                         |4                   |Keine Angabe|
|PUBCOMP                        |4                   |Keine Angabe|
|SUBSCRIBE                      |26                  |Kann mehrere der beiden Themenfilter umfassen (19 bei iot-2/evt/i/fmt/f) und QoS (1)|
|SUBACK                         |5                   |(1) für jeden Themenfilter in SUBSCRIBE|
|UNSUBSCRIBE                    |23                  |Kann mehrere Themenfilter umfassen (19 bei iot-2/evt/i/fmt/f)|
|UNSUBACK                       |4                   |Keine Angabe|
|PINGREQ                        |2                   |Keine Angabe|
|PINGRESP                       |2                   |Keine Angabe|
|DISCONNECT                     |2                   |Keine Angabe|

Bei Verwendung von TLS wird auch der sichere Handshake berücksichtigt. Der Handshake umfasst ca. 8 KB. Dadurch ist es kosteneffizienter, MQTT-Verbindungen so lange wie möglich geöffnet zu halten. Offene Verbindungen verursachen nur den PINGREQ- und den PINGRESP-Aufwand (4 Byte) pro Keepalive-Intervall, was kundenspezifisch ist und von dem Keepalive-Zeitraum abhängt, der angegeben ist.  Das erneute Öffnen einer TLS-Verbindung mithilfe einer vorhandenen TLS-Sitzung reduziert die Anzahl der Byte, die ausgetauscht werden, weil die Zertifikate nicht in beide Richtungen gesendet werden.  Zahlreiche TLS-Clients unterstützen dies standardmäßig, aber die Sitzung weist eine begrenzte Lebensdauer auf.  Durch die Verwendung von Clientzertifikaten wird die Größe des TLS-Handshakes abhängig von der Größe des Clientzertifikats erhöht. 

TLS-Datensätze und TCP- und IP-Aufwände werden nicht berücksichtigt.

**Hinweis** - Wenn Sie das {{site.data.keyword.iot_short_notm}}-Dashboard zum Anzeigen eines Geräts verwenden, dann wird eine MQTT-Subskription erstellt, sodass das Dashboard Liveereignisse anzeigen kann.  Diese MQTT-Subskription wird auch bei der Gesamtmenge der ausgetauschten Daten berücksichtigt.

### Ausgetauschte Daten für das HTTP-Messaging
Bei Verwendung des HTTP-Messagings sind die Nachrichtennutzdaten in der Kumulation der *ausgetauschten Daten* enthalten, und zwar sowohl beim Publizieren von Ereignissen als auch bei Empfangen von Befehlen.

Wie bei MQTT werden auch bei HTTP die Aufwände mitgezählt.  Der HTTP-Aufwand ist erheblich größer als der MQTT-Aufwand. Der HTTP-Aufwand pro Anforderung umfasst ca. 300 Byte. Die Nachrichtennutzdatengröße muss ebenfalls hinzuaddiert werden.

Bei Verwendung von TLS wird der sichere Handshake berücksichtigt.  Der Handshake umfasst ca. 8 KB.  Dadurch ist es kosteneffizienter, HTTPS-Verbindungen so lange wie möglich geöffnet zu halten.  Offene Verbindungen führen nicht zu zusätzlichem Aufwand in Bezug auf die *ausgetauschten Daten*.  Das erneute Öffnen einer TLS-Verbindung mithilfe einer vorhandenen TLS-Sitzung reduziert die Anzahl der Byte, die ausgetauscht werden, weil die Zertifikate nicht in beide Richtungen gesendet werden.  Zahlreiche TLS-Clients unterstützen dies standardmäßig, aber die Sitzung weist eine begrenzte Lebensdauer auf.  Durch die Verwendung von Clientzertifikaten wird die Größe des TLS-Handshakes abhängig von der Größe des Clientzertifikats erhöht.

TLS-Datensätze und TCP- und IP-Aufwände werden nicht berücksichtigt.

### Ausgetauschte Daten der HTTP-API
Wenn eine API von einer Anwendung aufgerufen wird, dann werden der Anforderungs- und der Antworthauptteil in Bezug auf die *ausgetauschten Daten* kumuliert.  Die jeweiligen Größen können abhängig von der verwendeten API erheblich variieren.

Anders als beim MQTT- und HTTP-Messaging werden weder der HTTP- noch der TLS-Aufwand bei den ausgetauschten Daten der HTTP-API berücksichtigt.

**Hinweis** - Wenn Sie das {{site.data.keyword.iot_short_notm}}-Dashboard verwenden, dann werden die HTTP-APIs vom Dashboard zum Auflisten der Informationen einschließlich der Daten zu den Geräten, Gerätetypen und Geräteverbindungsprotokollen verwendet.  Die HTTP-API-Aufrufe zählen zu den *ausgetauschten Daten*.

<!-- ## Data Analyzed
The *data analyzed* calculation measures event data that is processed by the rules engine within the platform.  Data is considered processed by the rules engine when device events are evaluated by one or more rules, based on a specific device and event type. 
## Edge Data Analyzed
The *edge data analyzed* calculation measures event data that is processed on a gateway device by the {{site.data.keyword.iot_short_notm}} Edge Analytics Agent.  Data is considered processed by the edge agent when device events are evaluated by one or more edge rules, based on a specific device and event type.  -->
