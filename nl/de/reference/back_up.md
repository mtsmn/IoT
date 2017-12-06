---

copyright:

years: 2017
lastupdated: "2017-08-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Datensicherung
{: #back_up}

Verwenden Sie die folgenden Informationen, um sich mit der Datensicherungsstrategie für {{site.data.keyword.iot_full}} vertraut zu machen.

## Welche Datentypen werden gesichert?

Momentan werden im Rahmen der {{site.data.keyword.iot_short_notm}}-Strategie die folgenden Typen von Clientdaten gesichert:

- Informationen zur Organisation
- Gerätedaten
- API-Schlüssel und Tokens
- Benutzerdaten
- Alle Datensätze von Gerätemanagementanforderungen einschließlich des Protokolls der initialisierten Anforderungen, z. B. der aktuelle Status der Anforderung
- Definitionen von Anforderungsbundles für das angepasste Gerätemanagement

## Welche Datentypen werden nicht gesichert?

Die folgenden Datentypen werden in {{site.data.keyword.iot_short_notm}} nicht gesichert:

- Geräteereignisse
- Daten zum transienten Nachrichtenstatus, z. B. für momentan ausgeführte Daten
- Analyseregeln und Alertkonfiguration
- MQTT-Nachrichten, die im Rahmen einer Gerätemanagementanforderung gesendet und empfangen werden

## Wie häufig werden Daten gesichert und wo werden sie gespeichert?

Daten werden alle 24 Stunden gesichert.

Die Daten werden getrennt vom {{site.data.keyword.iot_short_notm}}-Hauptservice an einem anderen Standort gespeichert. Dadurch besteht eine geografische Redundanz und die Services können wiederhergestellt werden, wenn es zu einem größeren Störfall kommt. Die betroffenen Kunden werden kontaktiert, wenn eine Datenwiederherstellung erforderlich wird. Alle Daten werden verschlüsselt und ihre Behandlung entspricht den lokalen Datenschutzanforderungen.

Momentan werden die folgenden Auslagerungsstandorte für die Datensicherung verwendet:

Standort                   | Sicherungsstandort                   
------------- | -------------
Bluemix - Vereinigte Staaten (Süden) (Dallas)| Washington
Bluemix - Großbritannien (London) | Frankfurt
Bluemix - Deutschland (Frankfurt) | London
Bluemix Dedicated | Auf Basis der Kundenanforderung bei Bestellung von {{site.data.keyword.iot_short_notm}} Dedicated

**Hinweis:** Zukünftige Standorte können sich ändern, um den Gesetzen zum Datenschutz Rechnung zu tragen, z. B. in Bezug auf die möglichen Auswirkungen des Brexit auf die Regelungen der EU-Datensouveränität.
