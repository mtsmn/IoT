---

copyright:

years: 2017
lastupdated: "2017-11-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Kontingente
Im vorliegenden Abschnitt erhalten Sie Informationen zu den Grenzwerten, die für die {{site.data.keyword.iot_full}}-Services nach Plantyp festgelegt wurden. Die Grenzwerte wurden im Rahmen der fairen Nutzungsrichtlinie definiert, um sicherzustellen, dass die Leistung für alle Benutzer des Mulitenenant-Systems nicht durch Missbrauch nachteilig beeinflusst wird. Die Grenzwerte dienen auch zur Vermeidung von Arbeitslasten realer Benutzer, die versehentlich als Denial-of-Service-Attacke identifiziert wurden.

## Einführung
{: #metrics_about}

Kontingente geben die Obergrenzen an, die für eine Ressource festgelegt wurden. Wenn die Nutzung den angegebenen Grenzwert überschreitet, dann kann die Verarbeitung von Benutzeranforderungen verzögert oder verweigert werden. In besonderen Situationen kann es durch das Überschreiten der Grenzwerte, durch das die Systemstabilität beeinträchtigt wird, dazu kommen, dass die {{site.data.keyword.iot_short_notm}}-Organisation ausgesetzt wird, um eine Beeinträchtigung anderer Benutzer zu verhindern.

Die folgenden Tabellen enthalten die Grenzwerte für {{site.data.keyword.iot_short_notm}}. Die meisten der Grenzwerte, die hier angegeben werden, gelten pro {{site.data.keyword.iot_short_notm}}-Organisation, sofern nicht explizit angegeben ist, dass sie pro Gerät gelten. Einige der Grenzwerte verfügen über ein Maximum, dessen Einhaltung erzwungen wird, während andere Grenzwerte auf Anforderung erhöht werden können. Wenn Sie höhere Grenzwerte als die angegebenen benötigen, dann reichen Sie ein [Ticket ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://support.ng.bluemix.net/gethelp/){: new_window} ein.

## Messaging
{: #messaging_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dedizierter Plan
------------- | -------------|------------- |
Maximale Anzahl der Gerätetypen |50 |1000 |1000
Maximale Anzahl der gleichzeitig verbundenen Geräte | 500| 500K. Wenn Sie mehr als 50.000 Geräte verbinden wollen, dann sollten Sie sich an die zuständige Unterstützungsfunktion wenden, um Ihre Pläne zu erörtern. | 500K
Maximale Anzahl registrierter Geräte |500 | 1 Million | 100K pro Inkrement
Maximale Anzahl von ['A'-Anwendungen - gemeinsam genutzt](../applications/mqtt.html#scalable_apps) |1 | 10 gemeinsam genutzte Subskriptionen, jede mit 50 Instanzen |10 gemeinsam genutzte Subskriptionen, jede mit 50 Instanzen 
Maximale Anzahl von ['a'-Anwendungen - nicht gemeinsam genutzt](../applications/mqtt.html#client_connections) |500 API-Schlüssel | 2000 API-Schlüssel | 2000 API-Schlüssel
Maximales Datenvolumen pro Monat | 200 MB | unbegrenzt|unbegrenzt
Geräteverbindungen|10/Sek.| 300/Sek.|300/Sek.
Gerät-zu-Cloud-Sendeoperationen (MQTT) - Gerät | 5 Nachrichten/Sek. oder 20 KB/Sek. | 10 Nachrichten/Sek. oder 40 KB/Sek. |10 Nachrichten/Sek. oder 40 KB/Sek. 
Gerät-zu-Cloud-Sendeoperationen (MQTT) - Gateway | 50 Nachrichten/Sek. oder 200 KB/Sek.| 100 Nachrichten/Sek. oder 400 KB/Sek.| 100 Nachrichten/Sek. oder 400 KB/Sek.
Gerät-zu-Cloud-Sendeoperationen (MQTT) - Anwendung | 10 Nachrichten/Sek. oder 40 KB/Sek. | 20 Nachrichten/Sek. oder 80 KB/Sek.| 20 Nachrichten/Sek. oder 80 KB/Sek.
Gerät-zu-Cloud-Sendeoperationen (MQTT) - pro Organisation | 5000 Nachrichten/Sek. | 6000 Nachrichten/Sek. | 6000 Nachrichten/Sek. 
Gerät-zu-Cloud-Sendeoperationen (HTTP) - Gerät | 30 Nachrichten/Min. oder 2 KB/Sek. | 1 Nachricht/Sek. oder 4 KB/Sek. | 1 Nachricht/Sek. oder 4 KB/Sek.
Gerät-zu-Cloud-Sendeoperationen (HTTP) - Gateway | 5 Nachrichten/Sek. oder 20 KB/Sek. | 10 Nachrichten/Sek. oder 40 KB/Sek. | 10 Nachrichten/Sek. oder 40 KB/Sek. 
Gerät-zu-Cloud-Sendeoperationen (HTTP) - Anwendung | 1 Nachricht/Sek. oder 4 KB/Sek.| 2 Nachrichten/Sek. oder 8 KB/Sek. | 2 Nachrichten/Sek. oder 8 KB/Sek. 
Gerät-zu-Cloud-Sendeoperationen (HTTP) - pro Organisation | 500 Nachrichten/Sek. | 600 Nachrichten/Sek. | 600 Nachrichten/Sek. 
Cloud-zu-Gerät-Sendeoperationen (MQTT) - Gerät |1 Nachricht/Sek. |2 Nachrichten/Sek. oder 8 KB/Sek. |2 Nachrichten/Sek. oder 8 KB/Sek. 
Cloud-zu-Gerät-Sendeoperationen (MQTT) - Gateway | 10 Nachrichten/Sek. |20 Nachrichten/Sek. oder 80 KB/Sek.|20 Nachrichten/Sek. oder 80 KB/Sek. 
Cloud-zu-Gerät-Sendeoperationen (MQTT) - Anwendung | 10 Nachrichten/Sek. |20 Nachrichten/Sek. oder 80 KB/Sek.|20 Nachrichten/Sek. oder 80 KB/Sek.
Cloud-zu-Gerät-Sendeoperationen (MQTT) - pro Organisation |1000 Nachrichten/Sek. | 12K Nachrichten/Sek.|12K Nachrichten/Sek.
Cloud-zu-Gerät-Sendeoperationen (HTTP) - Burstrate pro Gerät | 30 Nachrichten/Min. | 30 Nachrichten/Min. | 30 Nachrichten/Min. 
Cloud-zu-Gerät-Sendeoperationen (HTTP) - pro Organisation |  1000 Nachrichten/Sek. |  1200 Nachrichten/Sek. |  1200 Nachrichten/Sek. 
Maximale Anzahl eingehender nicht bestätigter Nachrichten - pro Gerät | 10 |10 |10
Maximale Anzahl eingehender nicht bestätigter Nachrichten - pro Gateway | 1000 |1000 |1000
Maximale Anzahl eingehender nicht bestätigter Nachrichten - pro Anwendungsverbindung |2000 | 2000 |2000 
Maximale Anzahl ausgehender nicht bestätigter Nachrichten - pro Gerät |10 |10 |10
Gerät-zu-Cloud-Persistenz | 24 Stunden|24 Stunden|24 Stunden
Cloud-zu-Gerät-Persistenz |48 Std. (Standard). Max. 7 Tage & 50 Warteschlangenlänge| 48 Std. (Standard). Max. 7 Tage & 50 Warteschlangenlänge|48 Std. (Standard). Max. 7 Tage & 50 Warteschlangenlänge
Maximal zulässiges Wiederholungsintervall für QoS bei Zustellung - 1 Nachricht| Wiederholung bei Wiederherstellung der Verbindung |Wiederholung bei Wiederherstellung der Verbindung |Wiederholung bei Wiederherstellung der Verbindung 
Verbindungsinaktivitätslimit (Keepalive-Intervall) | Angabe durch Client bei Verbindung |Angabe durch Client bei Verbindung |Angabe durch Client bei Verbindung 
WebSocket-Verbindungsdauer |Angabe durch Client bei Verbindung | Angabe durch Client bei Verbindung |Angabe durch Client bei Verbindung 
Maximal zulässige Nachrichtengröße | 128 KB |128 KB |128 KB 
Maximale Anzahl von Subskriptionen pro Gerät |50 |50 |50 
Maximale Anzahl von Subskriptionen pro Anwendung | 500 |500 |500
Warteschlangenlänge - Maximale Anzahl gepufferter Nachrichten pro Subskription pro Gerät | 50 |50 |50 
Maximale Anzahl gepufferter Nachrichten für 'A'-Anwendungen |50K dauerhaft, 50K nicht dauerhaft |50K dauerhaft, 50K nicht dauerhaft |50K dauerhaft, 50K nicht dauerhaft 


## APIs
{: #api_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
API-Aufrufe |10/Sek.|10/Sek.|10/Sek.

## Regelengine
{: #rule_engine_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
Maximale Anzahl Regeln pro Organisation |100|1000|1000
Aktionen pro Regel|10|10|10

## Gerätemanagement
{: #device_mgmt_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
Maximale Größe der Diagnoseprotokolle pro Gerät |500K|500K|500K
Maximale Anzahl archivierter Versionen der gespeicherten Diagnoseprotokolle |2  |2 |2
Maximale Zeitdauer zur Speicherung der Diagnoseprotokolle |7 Tage| 7 Tage|7 Tage
Maximale Anzahl der Aktionen, die pro Inflight-Nachricht initialisiert werden kann |200 |200 |200
Maximale Anzahl der Geräte pro Aktion |5000 |5000 | 5000

## Cache für zuletzt gemeldete Ereignisse
{: #last_event_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
Maximale Anzahl der Ereignis-IDs pro Gerät |5|5|5
Maximale Anzahl der Formate |2|3|3
Ablauf des letzten Ereigniscaches |12 Monate |12 Monate | 12 Monate 

## Erweiterungen
{: #extensions_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
Maximale Anzahl der Services, zu denen eine Bindung hergestellt werden kann |12|12|12

## Informationsmanagement
{: #information_management_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
Maximale Verschachtelungstiefe für JSON-Nutzdaten |5|5|5
Maximale Größe der JSON-Zeichenfolge |512|512|512

## Sicherheit
{: #security_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
Maximale Anzahl der angepassten Rollen |20|20|20
Maximale Anzahl der Benutzermitglieder |1000|1000|1000
Maximale Anzahl der Geräte in einer Ressourcengruppe |300|300|300
Maximale Anzahl der Ressourcengruppen, die einem Gateway zugewiesen sind |10|10|10
Maximale Anzahl der Ressourcengruppen, zu denen ein Gerät gehören kann |10|10|10

## Benutzerschnittstelle
{: #UI_metrics}

Metrik        | Lite-Plan      | Standardmäßige & erweiterte Sicherheitspläne | Dediziert 
------------- | -------------|------------- |
Maximale Anzahl der Dashboards |50 |50 |50 
Maximale Anzahl der Karten auf dem Board |30|30|30
