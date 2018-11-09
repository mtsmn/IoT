---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} Edge-Routing-Regeln konfigurieren (Vorschau)
{: #edge_rules}

{{site.data.keyword.iot_full}} Edge-Routing-Regeln definieren, wie Nachrichten innerhalb des Edge-Knotens weitergeleitet werden.

**Wichtig:** Die {{site.data.keyword.iot_full}} Edge-Komponente steht nur im Rahmen eines eingeschränkten Vorschauprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Sie können mehrere Routing-Regeln für einen Edge-Knoten definieren. Die Regeln werden für jede Nachricht ausgewertet, die ein lokales Gerät oder eine lokale Anwendung direkt auf dem Edge-Knoten publiziert. Nachrichten, die auf dem Edge-Knoten aus der Cloud veröffentlicht werden, werden immer lokal publiziert und sind nicht von der Weiterleitung betroffen.

## Format der Routing-Regeln
{: #edge_rules_format}

Eine Routing-Regel wird als JSON-Objekt im folgenden Format definiert:

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

Eine Routing-Regel wird als Array der JSON-Objekte in der Datei `routing.json` angegeben, die im Ordner `/etc/wiotp-edge` auf dem Edge-Knoten gespeichert ist.

Die Eigenschaften der einzelnen Routing-Regeln sind wie folgt definiert:

Eigenschaft   | Typ     | Beschreibung       
------------- | -----------| -----------
rule_id | Ganze Zahl, obligatorisch | Eine ID für die Regel. Routing-Regeln werden in aufsteigender Reihenfolge entsprechend der Regel-ID ausgewertet. Es sollte nicht mehr als Regel mit derselben ID vorhanden sein.
matching_filter | Zeichenfolge, obligatorisch | Wird verwendet, um zu bewerten, ob die Regel auf eine publizierte Nachricht angewendet werden soll. Die Eigenschaft sollte das Format eines MQTT-Abonnements haben. MQTT-Platzhalterzeichen für eine Ebene (+) und mehrere Ebenen (#) werden unterstützt. Dieser Filter wird anhand des Topics ausgewertet, in dem die Nachricht publiziert wird. Diese Eigenschaft ist im Format des {{site.data.keyword.iot_short_notm}}-Anwendungstopics definiert und enthält die Felder `/type/deviceType/id/deviceId`. Beispiel: `"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
destination | Zeichenfolge, obligatorisch | Definiert, wohin die Nachricht weitergeleitet werden soll. Legen Sie einen der folgenden Werte fest: `"CLOUD"`, `"IM"` (um die Nachricht an die Information Management-Komponente weiterzuleiten) oder `"LOCAL"` (um die Nachricht lokal auf dem Edge-Knoten weiterzuleiten). Beispiel: `"destination" : "CLOUD"`
continue_matching | boolesch, optional, Standardwert = true | Gibt an, ob zusätzliche Nachrichten mithilfe dieser Regel begewertet werden sollen, wenn die Regel ein Mal abgeglichen wird. Diese Eigenschaft bietet eine besser Kontrolle über die Weiterleitung, wenn sich überschneidende Routing-Regeln vorhanden sind.
forward_skip_levels | Ganze Zahl, optional, Standardwert = 0) | Gibt die Anzahl der Ebenen aus dem ursprünglichen Nachrichtentopic an, die entfernt werden sollen, wenn die Nachricht erneut publiziert wird. Der Standardwert ist '0', was bedeutet, dass das ursprüngliche Topic beibehalten wird. Wenn beispielsweise `forward_skip_levels` auf `1` gesetzt ist und eine Nachricht unter dem Topic `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json` publiziert ist, wird die Nachricht unter dem Topic `iot-2/type/T1/id/D1/evt/E1/fmt/json` erneut publiziert.
store | boolesch, optional, Standardwert = true | Definiert, ob die Store-and-forward-Funktion auf Nachrichten angewendet werden soll, die von dieser Regel weitergeleitet werden. Diese Eigenschaft gilt nur, wenn die Eigenschaft 'destination' auf `"CLOUD"` gesetzt ist. Wenn diese Eigenschaft auf 'false' gesetzt ist, werden Nachrichten, die mit der Regel übereinstimmen, nicht gespeichert, wenn der Edge-Knoten von der Cloud getrennt wird.

## Beispiel für eine Datei 'routing.json'
{: #edge_rules_example}

Im folgenden Beispiel werden drei Regeln konfiguriert:
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

Die erste Regel leitet alle Ereignisse des Typs `AlarmEvent` direkt an die Cloud weiter.

Die zweite Regel leitet alle Ereignisse, die in einem Topic publiziert wurden, das mit `sendToCloud/iot-2` beginnt, direkt an die Cloud weiter. Die Nachricht wird in einem Topic, das mit `iot-2` beginnt, weitergeleitet und enthält nicht das Präfix `sendToCloud/`.

Die dritte Regel ist die Standardregel, die alle anderen Nachrichten an den lokalen MQTT-Broker weiterleitet. Diese Nachrichten können dann von jedem lokalen Konsumenten mit einer entsprechenden Subskription verwendet werden. Die Installation des Edge-Knotens enthält die Standardregel für die lokale Weiterleitung von allen lokalen Dateien in `/etc/wiotp-edge/routing.json`.

## Routing-Regeln bearbeiten
{: #edge_rules_edit}

Sie können Regeln in der Datei `/etc/wiotp-edge/routing.json` direkt bearbeiten. Nachdem Sie die Datei bearbeitet haben, speichern Sie die Änderungen und führen Sie anschließend `docker ps` aus, um die ID des Containers 'edge-connector' zu ermitteln. Starten Sie den Docker-Container mit dieser ID erneut.

## Fehlerbehebung bei Routing-Regeln
{: #edge_rules_ts}

Wenn der Container 'edge-connector' nach einer Änderung in `routing.json` immer wieder erneut gestartet wird, bedeutet dies, dass die Routing-Regeldatei Syntaxfehler enthält. Überprüfen Sie die Protokolldatei `/var/wiotp-edge/trace/edgeConnector_trace.log`, um die Fehlernachrichten anzuzeigen.
