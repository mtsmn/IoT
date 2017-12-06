---

copyright:
  years: 2017
lastupdated: "2017-10-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Übersicht zur Zugriffssteuerung auf Ressourcenebene (Beta)
{: #RLAC_overview}

Die Zugriffssteuerung auf Ressourcenebene ermöglicht Ihnen die Steuerung des Zugriffs von Benutzern und API-Schlüsseln zur Verwaltung von Geräten. Sie können Ressourcengruppen verwenden, um die Geräte in einer Organisation anzugeben, die von den einzelnen Benutzern oder API-Schlüsseln verwaltet werden können. Weitere Informationen zur Vorgehensweise bei der Konfiguration der Zugriffssteuerung auf Ressourcenebene finden Sie in [Zugriffssteuerung auf Ressourcenebene konfigurieren](rlac.html#configure_RLAC).

**Wichtig:** Die {{site.data.keyword.iot_full}}-Funktion für die Zugriffssteuerung auf Ressourcenebene steht nur als Bestandteil eines eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Konzepte der Zugriffssteuerung auf Ressourcenebene 
{: #RLAC_concepts}

Gerätegruppen sind Teil der Zugriffssteuerung auf Ressourcenebene für {{site.data.keyword.iot_short_notm}}. Sie können die Funktion für die Gerätegruppen zum Verwalten der Anzahl der Geräte verwenden, auf die Benutzer auf Basis ihrer Rolle zugreifen können. Vor der Implementierung von Gerätegruppen müssen Sie den Zweck und den Geltungsbereich der Gerätegruppen ermitteln und dokumentieren, um die Effizienz Ihrer Lösung zu verbessern. 

Sie können Gerätegruppen als Teil Ihrer IoT-Lösung implementieren, um einem Benutzer oder einer Anwendung den Zugriff auf eine Untergruppe der Geräte zu ermöglichen und nicht auf alle Geräte, die einer Organisation zugeordnet sind. Um die Effizienz von Gerätegruppen zu erhöhen, sollten Sie Gerätegruppen verwenden, wenn eine Person Zugriff auf mehrere Geräte benötigt.

Beispiel: Ein Unternehmen beschäftigt eine Reihe von Servicemitarbeitern und Systembedienern und möchte eine IoT-Lösung für Großbritannien implementieren. Das Unternehmen konfiguriert eine Gerätegruppe für jede der 69 Städte, 9 Gerätegruppen für jede der geografischen Regionen und eine Gerätegruppe für ganz Großbritannien. Diesen Gruppen werden Geräte zugeordnet und die Gruppen werden 15 Servicemitarbeitern und Systembedienern zugewiesen, wobei einige dieser Personen Zugriff auf mehrere Gruppen haben. Benutzer, die für ein bis zwei Geräte zuständig sind, können weiterhin mobile Anwendungen und Unternehmensarchitekturen verwenden, die in Bezug auf {{site.data.keyword.iot_short_notm}} extern sind, die IoT-Gesamtlösung jedoch ergänzen.

Fragen zu Gerätegruppen können im [IoT-Bereich für Fragen ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window} gestellt werden.

## Ressourcengruppenbeschränkungen für Zugriffssteuerung

Ressourcengruppenbeschränkungen werden erzwungen, um sicherzustellen, dass die Funktion für die Zugriffssteuerung auf Ressourcenebene effektiv arbeitet. Die Beschränkungen grenzen die Anzahl der Ressourcengruppen ein, die einem Thema zugewiesen sind, und außerdem die Anzahl der Geräte in den jeweiligen Gruppen sowie die Anzahl der Gruppen, zu denen eine Ressource gehören kann.

Die folgenden Beschränkungen werden erzwungen:

 - Einem API-Schlüssel, Benutzer oder Gateway können maximal 10 Ressourcengruppen zugewiesen werden.
 - Eine Ressourcengruppe kann maximal 300 Ressourcen umfassen.
 - Eine Ressource kann zu maximal 10 Gruppen gehören.

## Terminologie zur Zugriffssteuerung auf Ressourcenebene

Sie können die folgenden Abschnitte zurate ziehen, um sich mit der Terminologie vertraut zu machen, die in der Funktion für die Zugriffssteuerung auf Ressourcenebene verwendet wird. 

### Themen
{: #subjects}

Bei einem Subjekt handelt es sich um eine authentifizierte Entität, die Zugriff auf eine Plattform anfordert, der autorisiert werden muss. Die folgenden Subjekttypen sind gültig:

- *Benutzer*: Endbenutzer, die mit den Anwendungen arbeiten. Ein Mitglied ist ein Benutzer, dessen Konto kein Ablaufdatum hat. Ein Gast ist ein Benutzer, dessen Konto über ein Ablaufdatum verfügt.
- *Geräte*: Physische Geräte, die auf die {{site.data.keyword.iot_short_notm}}-Plattform zugreifen und den Typ 'Gerät' verwenden.
- *Gateways*: Physische Geräte, die auf die {{site.data.keyword.iot_short_notm}}-Plattform zugreifen und den Typ 'Gateway' verwenden.
- *Anwendungen*: Anwendungen oder Services, die auf die {{site.data.keyword.iot_short_notm}}-Plattform zugreifen. Zur Berechtigung und Zugriffssteuerung werden dabei API-Schlüssel verwendet.

### Ressourcen
{: #resources}

Eine Ressource ist eine Entität, für die ein Subjekt eine bestimmte Aktion ausführt. Das Subjekt fordert den berechtigten Plattformzugriff für die Ressource an. Geräte sind die einzigen Ressourcen, die in der Betaversion der Zugriffssteuerung auf Ressourcenebene unterstützt werden.

### Aktionen
{: #actions}

Die Aktion stellt die vom Subjekt ausgeführte Operation dar. Dabei handelt es sich häufig um Erstellungs-, Lese-, Aktualisierungs-, Lösch- oder Ausführungsoperationen. Operationen werden zum Gruppieren von Aktionen für Ressourcen verwendet.

### Anwendungen
{: #applications}

Eine Anwendung kann im Namen eines Subjekts handeln, das auf der Plattform nicht bekannt ist, z. B. einer mobilen Anwendung, die ein inländisches Gerät steuert, und eine Anwendung kann im Namen eines realen oder simulierten Geräts handeln, das auf der Plattform bekannt oder unbekannt ist. Bei einer Anwendung kann es sich auch um eine reale serverseitige Anwendung handeln, die nicht im Namen eines anderen Gerätetyps handelt.

Die Authentifizierung wird durchgeführt und der Zugriff auf die {{site.data.keyword.iot_short_notm}}-Plattform wird auf Basis eines API-Schlüssels oder Tokens erteilt.

### Operationen
{: #operations}

Eine Operation enthält einzelne oder mehrere Aktionen, die für bestimmte Ressourcentypen mithilfe von Benutzerschnittstellen oder programmgesteuerten Schnittstellen (z. B. REST-Services und MQTT-Schnittstellen) ausgeführt werden. Sie werden von unterschiedlichen Subjekten einschließlich Mitglieder, Anwendungen und Geräten verwendet. Operationen werden anhand ihrer Operations-ID (OID) identifiziert.

### Rollen
{: #roles}

Bei Rollen handelt es sich um eine Gruppe von Operationen, die verwendet werden, um Berechtigungen zur Verwendung der Operationen zu erteilen. Ein Subjekt kann über mehrere Rollen verfügen und die Rolle stellt ein Attribut des zugehörigen Subjekts dar.

**Rollenformat**

Array von 0..n Rollen

    { "roles": [ "PD_ADMIN_APP" ] }

**Rollen-zu-Gruppen-Format**

Zuordnung von 0..n <string, list<string>>-Paaren

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### Berechtigungen
{: #permissions}

Berechtigungen erlauben einem Subjekt die Ausführung von Operationen für eine Ressource oder Ressourcengruppe.

### Ressourcengruppen
{: #resource_groups}

Eine Ressourcengruppe stellt eine Sammlung von Geräten als Ressourcen dar. Geräte können zu Ressourcengruppen hinzugefügt werden und Ressourcengruppen können Subjekten in Rolle-zu-Gruppen-Paaren zugewiesen werden. Diese Beziehungen ermöglichen den Subjekten die Ausführung von Operationen für eine bestimmte Rolle für bestimmte Gerätegruppen.

### Benutzer
{: #users}

Benutzer melden Sie beim Plattformdashboard von {{site.data.keyword.iot_short_notm}} an, indem Sie eine ID verwenden, die Mitglied einer Organisation ist. Benutzern werden Rollen zugewiesen, die ihnen die Berechtigung zum Aufrufen bestimmter Gruppen von HTTP-APIs erteilen. Weitere Informationen zu den Benutzerrollen finden Sie in [Zugriffsebenen für Benutzerrollen](roles_access.html#levels-of-access-for-user-roles).

### API-Schlüssel
{: #API_keys}

API-Schlüssel sind Tokens, die zum Aufrufen der HTTP-APIs für die Plattform von {{site.data.keyword.iot_short_notm}} dienen. API-Schlüsseln werden Rollen zugewiesen, die ihnen die Berechtigung zum Aufrufen bestimmter Gruppen von HTTP-APIs erteilen. Weitere Informationen zu den Rollen von API-Schlüsseln finden Sie in [Zugriffsebenen für Anwendungsrollen](app_roles_access.html#levels-of-access-for-application-roles).

## APIs mit erzwungener Zugriffssteuerung auf Ressourcenebene
{: #RLAC_enforced_APIs}
Wenn die Zugriffssteuerung auf Ressourcenebene aktiviert wurde, dann können die gerätebezogenen APIs in diesem Abschnitt nur arbeiten, wenn auf das angegebene Gerät durch den Aufrufenden zugegriffen werden kann.

### Geräte-APIs (individuell)

**Geräte nach Typ auflisten**

    GET /api/v0002/device/types/${typeId}

Das System gibt nur die Untergruppe der Geräte zurück, die zu den entsprechenden Gruppen gehören.

**Gerät abrufen**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Details zum Gerätemanagement abrufen**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Vom Gateway registrierte Geräte abrufen**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gateway zugreifen kann. Das System gibt nur die Untergruppe der Geräte zurück, die zu den entsprechenden Gruppen gehören. Die Geräte und die Gateways müssen in der Gruppe des Aufrufenden enthalten sein.

**Gerät aktualisieren**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Gerät löschen**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

### Geräte-APIs (Bulk)

**Geräte auflisten**

    GET /api/v0002/bulk/devices

Das System gibt nur die Untergruppe der Geräte zurück, die zu den entsprechenden Gruppen gehören.

**Geräte löschen**

    DELETE /api/v0002/bulk/devices/remove

Das System löscht nur die Untergruppe der Geräte, die zu den entsprechenden Gruppen gehören. Geräte, auf die nicht zugegriffen werden kann, geben dennoch die Meldung zurück, dass die Verarbeitung erfolgreich durchgeführt wurde.

**Geräte aktualisieren**

    PUT /api/v0002/bulk/devices/update

### APIs für Gerätemanagement

**Anforderung einleiten**

    POST /api/v0002/mgmt/requests

Die Verarbeitung schlägt fehl, wenn ein Aufrufender nicht auf ein Gerät zugreifen kann.

**Anforderung löschen**

    DELETE /api/v0002/mgmt/requests/${requestId}

Die Verarbeitung schlägt fehl, wenn ein Aufrufender nicht auf ein Gerät zugreifen kann.

**Anforderung anzeigen**

    GET /api/v0002/mgmt/requests/${requestId}

**Anforderungen auflisten**

    GET /api/v0002/mgmt/requests

**Eine Anforderung abrufen**

    GET /api/v0002/mgmt/requests/${requestId}

**Gerätedetails der Anforderung abrufen**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**Gerätedetails der Anforderung für ein Gerät abrufen**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### APIs für Problembestimmung

**Verbindungsprotokolle für Gerät**

    GET /api/v0002/logs/connection

Das System gibt einen leeren Array zurück, wenn auf das Gerät nicht zugegriffen werden kann.

**Gerätefehlercode hinzufügen**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Gerätefehlercodes auflisten**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Gerätefehlercodes bereinigen**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Diagnoseprotokoll für Gerät hinzufügen**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Diagnoseprotokoll für Gerät auflisten**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Diagnoseprotokoll für Gerät abrufen**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Diagnoseprotokoll für Gerät bereinigen**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Diagnoseprotokoll für ein Gerät löschen**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

###APIs für letzten Ereigniscache

**Ereignisse für Gerät abrufen**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.

**Ereignisse für Gerät abrufen**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

Das System gibt einen Fehler vom Typ '404' zurück, wenn der Aufrufende nicht auf das Gerät zugreifen kann.
