---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-08"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Gerätemanagementprotokoll
{: #index}

## Einführung
{: #introduction}

{{site.data.keyword.iot_full}} erkennt Geräte und Gateways als die beiden Klassen des Geräts. Die Geräteklasse wird anhand des Felds 'classId' identifiziert. Geräte in der Geräteklasse können verwaltete oder nicht verwaltete Geräte sein.

**Verwaltete Geräte** sind als Geräte definiert, die über einen Gerätemanagementagenten verfügen. Ein Gerätemanagementagent ist eine Logikeinheit, die es dem Gerät ermöglicht, über das Gerätemanagementprotokoll mit dem {{site.data.keyword.iot_short_notm}}-Gerätemanagementservice zu interagieren. Verwaltete Geräte können Gerätemanagementoperationen einschließlich von Positionsaktualisierungen, Firmware-Downloads und Aktualisierungen, Neustarts und das Zurücksetzen auf Werkseinstellungen ausführen.

Das Gerätemanagementprotokoll definiert eine Gruppe von unterstützten Operationen. Ein Gerätemanagementagent kann eine Untergruppe der Operationen unterstützen, muss aber die Anforderung 'Gerät verwalten' vornehmen. Ein Gerät, das Operationen für Firmwareaktionen unterstützt, muss auch Beobachtungen unterstützen.

Ein Gerätemanagementprotokoll wird auf der Grundlage eines MQTT-Nachrichtenprotokolls erstellt. Weitere Informationen zu der Vorgehensweise, wie das Gerätemanagementprotokoll mit MQTT interagiert, finden Sie in [MQTT-Konnektivität für Geräte](../mqtt.html).


### Lebenszyklus im Gerätemanagement

1. Ein Gerät und sein zugeordneter Gerätetyp werden in {{site.data.keyword.iot_short_notm}} mithilfe des Dashboards oder der REST-API erstellt.
2. Ein Gerät stellt eine Verbindung zu {{site.data.keyword.iot_short_notm}} her und verwendet die Anforderung 'Gerät verwalten', um ein verwaltetes Gerät zu werden.
3. Sie können die Metadaten eines Geräts anzeigen und bearbeiten, indem Sie die {{site.data.keyword.iot_short_notm}}-REST-API verwenden. Diese Operationen - beispielsweise Firmware-Updates und Geräteneustart - sind in der Dokumentation zu [Gerätemanangementanforderungen](https://console.ng.bluemix.net/docs/services/IoT/devices/device_mgmt/requests.html) beschrieben.
4. Ein Gerät kann Aktualisierungen zu seiner Position, zu Diagnoseinformationen und Fehlercodes mithilfe des Gerätemanagementprotokolls kommunizieren.
5. Wenn ein Gerät stillgelegt wird, können Sie es mithilfe des Dashboards oder der REST-API aus {{site.data.keyword.iot_short_notm}} entfernen.

Siehe die Anleitung [Raspberry Pi als verwaltetes Gerät mit IBM Watson IoT Platform verbinden ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-as-managed-device-to-ibm-iot-foundation/){: new_window}.

## Anforderungen des Typs 'Gerät verwalten'
{: #manage_device_request}

Ein Gerät verwendet die Anforderung 'Gerät verwalten', um ein verwaltetes Gerät zu werden. Ein Gerätemanagementagent muss eine Anforderung 'Gerät verwalten' senden, bevor er Anforderungen vom Server empfangen kann. Ein Gerätemanagementagent sendet diesen Anforderungstyp in der Regel bei jedem Start oder Neustart.

### Topic für eine Anforderung des Typs 'Gerät verwalten'

Ein Gerät publiziert eine Anforderung des Typs 'Gerät verwalten' im folgenden Topic:

```
iotdevice-1/mgmt/manage
```

Der Server antwortet auf eine Anforderung des Typs 'Gerät verwalten' im folgenden Topic:

```
iotdm-1/response
```




### Nachrichtenformat für eine Anforderung des Typs 'Gerät verwalten'


In einer Anforderung des Typs 'Gerät verwalten' sind das Feld ``d`` und alle seine untergeordneten Felder optional. Die Werte der Felder ``metadata`` und ``deviceInfo`` ersetzen die entsprechenden Attribute für das sendende Gerät, falls sie gesendet werden.

Das optionale Feld ``lifetime`` gibt den Zeitraum in Sekunden an, in dem das Gerät eine weitere Anforderung des Typs 'Gerät verwalten' senden muss, um nicht als 'ruhend' klassifiziert und ein nicht verwaltetes Gerät zu werden. Wenn das Feld ``lifetime`` ausgelassen oder auf ``0`` gesetzt wird, wird das verwaltete Gerät nicht 'ruhend'. Der für das Feld ``lifetime`` unterstützte Mindestwert lautet ``3600`` Sekunden, das heißt, eine Stunde.

Die optionalen Felder ``supports.deviceActions`` und ``supports.firmwareActions`` geben die Möglichkeiten des Gerätemanagementagenten an. Wenn ``supports.deviceActions`` festgelegt wird, unterstützt der Agent sowohl die Aktion für den Neustart als auch die Aktion für das Zurücksetzen auf die Werkseinstellungen. Für ein Gerät, das nicht zwischen einem Neustart und dem Zurücksetzen auf Werkseinstellungen unterscheidet, ist es akzeptabel, dass für beide Aktionen dasselbe Verhalten verwendet wird. Wenn ``supports.firmwareActions`` festgelegt ist, unterstützt der Agent sowohl die Aktionen für den Firmware-Download als auch für das Firmware-Update.

Das folgende Beispiel zeigt das Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/mgmt/manage
{
    "d": {
        "metadata":{},
        "lifetime": number,
        "supports": {
            "deviceActions": boolean,
            "firmwareActions": boolean
        },
        "deviceInfo": {
            "serialNumber": "string",
            "manufacturer": "string",
            "model": "string",
            "deviceClass": "string",
            "description" :"string",
            "fwVersion": "string",
            "hwVersion": "string",
            "descriptiveLocation": "string"
        }
    },
    "reqId": "string"
}
```

Das folgende Beispiel zeigt das Antwortformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/response
{
    "rc": number,
    "reqId": "string"
}
```


### Antwortcodes für eine Anforderung des Typs 'Gerät verwalten'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|403   |Nicht zulässig (wenn ein Gerät versucht, eine Managementanforderung zu publizieren, in der die Unterstützung für eine ungültige Gruppe von Aktionen angefordert wird).|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.|
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|


## Anforderungen des Typs 'Gerät nicht verwalten'
{: #manage-unmanage}


Ein Gerät verwendet eine Anforderung des Typs 'Gerät nicht verwalten', wenn eine Verwaltung nicht mehr erforderlich ist. Wenn ein Gerät nicht mehr verwaltet wird, sendet {{site.data.keyword.iot_short_notm}} keine neuen Gerätemanagementanforderungen an das Gerät. Nicht verwaltete Geräte können weiterhin Fehlercodes, Protokollnachrichten und Positionsnachrichten publizieren.


### Topic für die Anforderung 'Gerät nicht verwalten'


Ein Gerät publiziert eine Anforderung des Typs 'Gerät nicht verwalten' im folgenden Topic:

```
iotdevice-1/mgmt/unmanage
```

Der Server antwortet auf eine Anforderung des Typs 'Gerät nicht verwalten' im folgenden Topic:

```
iotdm-1/response
```

### Nachrichtenformat für eine Anforderung des Typs 'Gerät nicht verwalten'

Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/mgmt/unmanage
{
    "reqId": "string"
}
```

Antwortformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/response
{
    "rc": number,
    "reqId": "string"
}
```

### Antwortcodes für eine Anforderung des Typs 'Nicht verwaltetes Gerät'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.|
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|


## Anforderung 'Position aktualisieren'
{: #update-location}

Die Metadaten zur Position eines Geräts können in {{site.data.keyword.iot_short_notm}} anhand folgender Möglichkeiten aktualisiert werden:

### Automatische Aktualisierungen der Geräteposition
- Für Geräte, die ihre Position bestimmen können, kann ausgewählt werden, dass der {{site.data.keyword.iot_short_notm}}-Gerätemanagementserver über Positionsänderungen benachrichtigt wird. Das Gerät benachrichtigt {{site.data.keyword.iot_short_notm}} über die Aktualisierung der Position. Das Gerät ruft seine Position beispielsweise von einem GPS-Empfänger ab und sendet eine Gerätemanagementnachricht an die {{site.data.keyword.iot_short_notm}}-Instanz, um seine Position zu aktualisieren. Anhand einer Zeitmarke wird der Zeitpunkt erfasst, an dem die Position vom GPS-Empfänger abgerufen wurde. Die Zeitmarke ist gültig, auch wenn beim Senden der Nachricht zur Aktualisierung der Position eine Verzögerung auftritt. Der Server zeichnet das Datum und die Uhrzeit des Nachrichtenempfangs auf und verwendet diese Informationen, um die Metadaten zur Position zu aktualisieren, wenn keine Zeitmarke verwendet wurde.

### Manuelle Aktualisierungen der Geräteposition mithilfe der REST-API
- Sie können die Metadaten zur Position für ein statisches Gerät manuell festlegen, indem Sie beim Registrieren des Geräts die {{site.data.keyword.iot_short_notm}}-REST-API verwenden. Sie können die Position auch später ändern. Die Einstellung der Zeitmarke ist optional; wird sie jedoch ausgelassen, werden das aktuelle Datum und die Uhrzeit in den Metadaten zur Position des Geräts festgelegt.

### Topic für eine Anforderung des Typs 'Position aktualisieren', die durch ein Gerät ausgelöst wird:


Ein Gerät publiziert eine Anforderung des Typs 'Position aktualisieren' im folgenden Topic:

```
iotdevice-1/device/update/location
```

Der Server antwortet auf eine Anforderung des Typs 'Position aktualisieren' im folgenden Topic:

```
iotdm-1/response
```

### Positionsaktualisierung, die von Benutzern oder Apps ausgelöst wird


Wenn ein Benutzer oder eine Anwendung die Position eines aktiven verwalteten Geräts aktualisiert, empfängt das Gerät eine Aktualisierungsnachricht.




### Topic für eine Anforderung des Typs 'Position aktualisieren', die von Benutzern oder Apps ausgelöst wird


Der Server publiziert eine Anforderung des Typs 'Position aktualisieren' im folgenden Topic:

```
iotdm-1/device/update
```

### Nachrichtenformat für eine Anforderung des Typs 'Position aktualisieren'


Das Feld ``measuredDateTime`` reflektiert das Datum und die Zeit der Positionsmessung.

Bei jeder Aktualisierung der Position werden die für den Breiten- und den Längengrad, die Höhe und die Genauigkeit angegebenen Werte als eine einzelne mehrwertige Aktualisierung betrachtet. Der Breiten- und der Längengrad sind obligatorisch und sie müssen bei jeder Aktualisierung angegeben werden. Der Breitengrad und der Längengrad muss anhand des World Geodetic System 1984 (WGS84) in Dezimalgraden angegeben werden. Höhe und Genauigkeit werden in Meter gemessen und sind optional.

Wenn bei einer Aktualisierung ein optionaler Wert angegeben und bei einer späteren Aktualisierung ausgelassen wird, wird der frühere Wert durch die spätere Aktualisierung gelöscht. Jede Aktualisierung wird als vollständige mehrwertige Aktualisierung betrachtet.


Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/device/update/location
{
    "d": {
        "longitude": number,
        "latitude": number,

        "elevation": number,
        "measuredDateTime": "string in ISO8601 format",
        "updatedDateTime": "string in ISO8601 format",
        "accuracy": number
    },
    "reqId": "string"
}
```

Antwortformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/response
{
    "rc": number,
    "reqId": "string"
}
```

### Antwortcodes für eine Anforderung des Typs 'Position aktualisieren'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.|
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|


### Positionsaktualisierungen, die von Benutzern oder Apps ausgelöst werden


Das folgende Beispiel umreißt das Nutzdatenformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/device/update
{
    "d": {
        "fields": [
            {
                "field": "location",
                "value": {
                    "latitude": number,
                    "longitude": number,
                    "elevation": number,
                    "accuracy": number,
                    "measuredDateTime": "string in ISO8601 format"
                    "updatedDateTime": "string in ISO8601 format",

                }
            }
        ]
    }
}
```

**Hinweis:** Der Parameter ``reqID`` wird nicht verwendet, da das Gerät nicht antworten muss.

## Anforderungen des Typs 'Geräteattribute aktualisieren'
{: #update-attributes}

Mithilfe der REST-API kann {{site.data.keyword.iot_short_notm}} eine Anforderung an ein Gerät senden, um den Wert mindestens eines der folgenden Geräteattribute zu aktualisieren:


|Attribut | Weitere Informationen|
|:---|:---|
|location  | Siehe [Position aktualisieren](index.html#update-location)|
|metadata  | Optional|
|deviceInfo | Optional|
|mgmt.firmware | Siehe [Firmware-Update-Prozess](requests.html#firmware-actions-update)|

### Topic für eine Anforderung des Typs 'Geräteattribute aktualisieren'


Der Server publiziert eine Anforderung des Typs 'Geräteattribute aktualisieren' im folgenden Topic:

```
iotdm-1/device/update
```


### Nachrichtenformat für eine Anforderung des Typs 'Geräteattribute aktualisieren'


Das folgende Beispiel umreißt das Nutzdatenformat für die Anforderung:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/device/update
{
    "d": {
        "fields": [
            {
                "field": "location",
                "value": ""
            }
        ]
    }
}
```


## Anforderungen des Typs 'Fehlercode hinzufügen'
{: #diag-add-error-code}

Geräte können festlegen, den Gerätemanagementserver von {{site.data.keyword.iot_short_notm}} mithilfe des Anforderungstyps 'Fehlercode hinzufügen' über Änderungen ihres Fehlerstatus zu benachrichtigen.

### Topic für eine Anforderung des Typs 'Fehlercode hinzufügen'


Ein Gerät publiziert eine Anforderung des Typs 'Fehlercode hinzufügen' im folgenden Topic:

```
iotdevice-1/add/diag/errorCodes
```

### Nachrichtenformat für eine Anforderung des Typs 'Fehlercode hinzufügen'


Der `errorCode` zugeordnete Wert ist der aktuelle Fehlercode des Geräts und muss der {{site.data.keyword.iot_short_notm}}-Instanz hinzugefügt werden.

Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/add/diag/errorCodes
{
    "d": {
        "errorCode": number
    },
    "reqId": "string"
}
```

Antwortformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/response
{
    "rc": number,
    "reqId": "string"
}
```

### Antwortcodes für eine Anforderung des Typs 'Fehlercode hinzufügen'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|


## Anforderungen des Typs 'Fehlercodes löschen'
{: #diag-clear-error-codes}

Geräte können anfordern, dass {{site.data.keyword.iot_short_notm}} alle Fehlercodes für das Gerät mithilfe des Anforderungstyps 'Fehlercodes löschen' löscht.

### Topic für eine Anforderung des Typs 'Fehlercodes löschen'


Ein Gerät publiziert diese Anforderung im folgenden Topic:

```
iotdevice-1/clear/diag/errorCodes
```

### Nachrichtenformat für eine Anforderung des Typs 'Fehlercodes löschen'


Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/clear/diag/errorCodes
{
    "reqId": "string"
}
```

Antwortformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/response
{
    "rc": 200,
    "reqId": "string"
}
```

### Antwortcodes für eine Anforderung des Typs 'Fehlercodes löschen'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.|
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|


## Anforderungen des Typs 'Protokoll hinzufügen'
{: #diag-add-log}

Für Geräte kann festgelegt werden, ob die {{site.data.keyword.iot_short_notm}}-Gerätemanagementunterstützung über Änderungen benachrichtigt werden soll, indem ein neuer Protokolleintrag hinzugefügt wird. Protokolleinträge enthalten eine Protokollnachricht, eine Zeitmarke, den Schweregrad und optional die mit Base64 codierten binären Diagnosedaten.

### Topic für eine Anforderung des Typs 'Protokoll hinzufügen'
Ein Gerät publiziert diese Anforderung im folgenden Topic:

```
iotdevice-1/add/diag/log
```


### Nachrichtenformat für eine Anforderung des Typs 'Protokoll hinzufügen'

In der folgenden Tabelle wird das Format der Attribute der ausgehenden Nachricht beschrieben:

|Attribut |Beschreibung |
|:---|:---|
|`message`|Gibt eine Diagnosenachricht an, die der {{site.data.keyword.iot_short_notm}}-Instanz hinzugefügt werden muss|
|`timestamp`|Gibt das Datum und die Uhrzeit des Protokolleintrags im Format ISO8601 an |
|`data`|Gibt optionale, mit Base64 codierte Diagnosedaten an|
|`severity`|Gibt den Schweregrad der Nachricht an (0: Informationsnachricht, 1: Warnung, 2: Fehler)|


Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/add/diag/log
{
    "d": {
        "message": "string",
        "timestamp": "string",
        "data": "string",
        "severity": number
    },
    "reqId": "string"
}
```

Antwortformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/response
{
    "rc": number,
    "reqId": "string"
}
```


### Antwortcodes für eine Anforderung des Typs 'Protokoll hinzufügen'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.|
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|

## Anforderungen des Typs 'Protokolle löschen'
{: #diag-clear-logs}

Geräte können anfordern, dass {{site.data.keyword.iot_short_notm}} mithilfe des Anforderungstyps 'Protokolle löschen' alle Protokolleinträge für das Gerät löscht.

### Topic für eine Anforderung des Typs 'Protokolle löschen'


Ein Gerät publiziert die Anforderung 'Protokolle löschen' im folgenden Topic:

```
iotdevice-1/clear/diag/log
```

### Nachrichtenformat für eine Anforderung des Typs 'Protokolle löschen'


Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/clear/diag/log
{
    "reqId": "string"
}
```

Antwortformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdm-1/response
{
    "rc": number,
    "reqId": "string"
}
```

### Antwortcodes für eine Anforderung des Typs 'Protokolle löschen'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.|
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|

## Anforderungen des Typs 'Attributänderungen beobachten'
{: #observations-observe}

{{site.data.keyword.iot_short_notm}} kann eine Anforderung des Typs 'Attributänderungen beobachten' an ein Gerät senden, um mithilfe des Anforderungstyps 'Attributänderungen beobachten' die Änderungen mindestens eines Geräteattributs zu beobachten. Wenn das Gerät die Anforderung empfängt, muss es bei jeder Änderung der Werte für die beobachteten Attribute eine Benachrichtigungsanforderung an {{site.data.keyword.iot_short_notm}} senden.

**Wichtig:** Geräte müssen Operationen zum Beobachten, Benachrichtigen und Abbrechen implementieren, um Anforderungen des Typs [Firmwareaktionen - Update](requests.html#firmware-actions-update) zu unterstützen.

### Topic für eine Anforderung des Typs 'Attributänderungen beobachten'


Der Server publiziert eine Anforderung des Typs 'Attributänderungen beobachten' im folgenden Topic:

```
iotdm-1/observe
```

### Nachrichtenformat für eine Anforderung des Typs 'Attributänderungen beobachten'


Das Array `fields` ist ein Array der Geräteattribute aus dem Gerätemodell. Bei Angabe eines komplexen Feldes wie beispielsweise `mgmt.firmware` wird erwartet, dass seine zugrunde liegenden Felder zum selben Zeitpunkt aktualisiert werden, sodass nur eine einzige Benachrichtigungsnachricht generiert wird.

Der Parameter `message`, der in der Antwort verwendet wird, kann angegeben werden, wenn der Wert des Parameters `rc` nicht `200` lautet. Wenn ein bestimmter Parameterwert nicht abgerufen werden kann, muss der Wert des Parameters `rc` entweder auf `404` gesetzt werden, wenn das Attribut nicht gefunden wurde, oder auf `500`, falls eine andere Ursache vorliegt. Wenn Werte für Parameter nicht gefunden werden können, sollte das Array `fields` Elemente enthalten, bei denen für `field` die Namen der einzelnen Parameter festgelegt sind, die nicht gelesen werden konnten. Der Parameter `value` sollte weggelassen werden. Damit der Parameter für den Antwortcode auf `200` eingestellt werden kann, muss sowohl das Feld `field` als auch `value` angegeben werden, wobei `value` der aktuelle Wert eines Attributs ist, das durch den Wert des Parameters `field` angegeben wird.

Anforderungsformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/observe
{
    "d": {
        "fields": [
            {
                "field": "field_name"
            }
        ]
    },
    "reqId": "string"
}
```

Antwortformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/response
{
    "rc": number,
    "message": "string",
    "d": {
        "fields": [
            {
                "field": "field_name",
                "value": "field_value"
            }
        ]
    },
    "reqId": "string"
}
```


## Anforderungen des Typs 'Attributbeobachtung abbrechen'
{: #observations-cancel}

{{site.data.keyword.iot_short_notm}} kann mithilfe des Anforderungstyps 'Attributbeobachtung abbrechen' eine Anforderung an ein Gerät senden, dass die aktuelle Beobachtung mindestens eines der Geräteattribute abgebrochen werden soll. Der `fields`-Teil der Anforderung ist ein Array der Geräteattributnamen aus dem Gerätemodell, beispielsweise die Parameter `location`, `mgmt.firmware` oder `mgmt.firmware.state`.

Der Parameter `message` kann angegeben werden, wenn der Wert des Parameters `rc` nicht `200` ist.

**Wichtig:** Geräte müssen Operationen zum Beobachten, Benachrichtigen und Abbrechen implementieren, um Anforderungen des Typs [Firmwareaktionen - Update](requests.html#firmware-actions-update) zu unterstützen.

### Topic für eine Anforderung des Typs 'Attributbeobachtung abbrechen'


Der Server publiziert eine Anforderung des Typs 'Attributbeobachtung abbrechen' im folgenden Topic:

```
iotdm-1/cancel
```


### Nachrichtenformat für eine Anforderung des Typs 'Attributbeobachtung abbrechen'


Anforderungsformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/cancel
{
    "d": {
        "fields": [
            {
                "field": "field_name"
            }
        ]
    },
    "reqId": "string"
}
```

Antwortformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/response
{
    "rc": number,
    "message": "string",
    "reqId": "string"
}
```



## Anforderungen des Typs 'Über Attributänderungen benachrichtigen'
{: #observations-notify}

{{site.data.keyword.iot_short_notm}} kann für ein bestimmtes Attribut oder eine Gruppe von Werten eine Beobachtungsanforderung vornehmen, indem der Anforderungstyp 'Über Attributänderungen benachrichtigen' verwendet wird. Wenn der Wert des Attribut bzw. der Attribute geändert wird, muss das Gerät eine Benachrichtigung senden, die den zuletzt aktuellen Wert enthält.

Der Wert des Parameters `field` ist der Name des geänderten Attributs und der Feldwert (`value`) ist der aktuelle Wert des Attributs. Das Attribut kann ein komplexes Feld sein. Wenn als Ergebnis einer einzigen Operation mehrere Werte in einem komplexen Feld aktualisiert werden, wird nur eine einzige Benachrichtigungsnachricht gesendet.

**Wichtig:** Geräte müssen Operationen zum Beobachten, Benachrichtigen und Abbrechen implementieren, um Anforderungen des Typs [Firmwareaktionen - Update](requests.html#firmware-actions-update) zu unterstützen.


### Topic für eine Anforderung des Typs 'Über Attributänderungen benachrichtigen'


Ein Gerät publiziert eine Anforderung des Typs 'Über Attributänderungen benachrichtigen' im folgenden Topic:

```
iotdevice-1/notify
```


### Nachrichtenformat für eine Anforderung des Typs 'Über Attributänderungen benachrichtigen'


Anforderungsformat:

```
Ausgehende Nachricht vom Gerät:

Topic: iotdevice-1/notify
{
    "d": {
        "fields": [
            {
                "field": "field_name",
                "value": "field_value"
            }
        ]
    }
    "reqId": "string"
}
```

Antwortformat:

```
Eingehende Nachricht vom Server:

Topic: iotdm-1/response
{
    "rc": number,
    "reqId": "string"
}
```

### Antwortcodes für eine Anforderung des Typs 'Über Attributänderungen benachrichtigen'

|Antwortcode |Nachricht |
|:---|:---|
|200   |Die Operation war erfolgreich.|
|400   |Die Eingabenachricht stimmt nicht mit dem erwarteten Format überein oder einer der Werte liegt außerhalb des gültigen Bereichs.|
|404   |Das Gerät wurde nicht bei {{site.data.keyword.iot_short_notm}} registriert.|
|409   |Die Ressource konnte aufgrund eines Konflikts nicht aktualisiert werden. (Beispiel: Die Ressource wird von zwei gleichzeitigen Anforderungen aktualisiert). Die Aktualisierung kann später erneut versucht werden.|
|500   |Es ist ein interner Fehler aufgetreten.|
