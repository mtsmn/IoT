---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Schrittweise Anleitung 1: Detailliertes Beispiel zur Vorgehensweise beim Arbeiten mit Geräten über eine allgemeine Schnittstelle
{: #scenario}

Erstellen Sie mithilfe der folgenden Informationen ein Szenario, in dem Ereignisse von zwei Temperaturgeräten an {{site.data.keyword.iot_full}} publiziert werden. Ein Gerät misst die Temperatur in Grad Celsius. Das andere Gerät misst die Temperatur in Grad Fahrenheit. Die gemessenen Werte werden einem einzigen Temperaturmesswert in Grad Celsius zugeordnet. Wenn diese Geräte einen neuen Temperaturmesswert ausgeben, wird der Wert der Eigenschaft geändert, die dem Gerätestatus zugeordnet ist.
{: shortdesc}

## Vorbereitende Schritte

Wenn Sie eine [Ressource](ga_im_definitions.html#definitions_resources) erstellen, wird diese Ressource als Entwurfsversion erstellt. Die Entwurfsversion stellt eine Arbeitsdatei der Ressource dar, die Sie über APIs direkt abfragen, aktualisieren und löschen können. Bei Verwendung von REST-APIs wird das Präfix **draft/** verwendet, um Ressourcen zu identifizieren, die sich im Entwurfsstatus befinden. 

Weitere Informationen zu Entwurfsversionen und aktiven Versionen von Ressourcen finden Sie in [Erklärung des Datenmanagements](ga_im_definitions.html).

## Voraussetzungen

Sie müssen über eine {{site.data.keyword.iot_short_notm}} [-Oranisationsinstanz](../iotplatform_overview.html#organizations) und einen API-Schlüssel und ein Authentifizierungstoken für diese Organisation verfügen, um Anforderungen authentifizieren zu können. 

Informationen zum Generieren eines API-Schlüssels finden Sie in der [API](../reference/api.html)-Dokumentation. Weitere Informationen zum Generieren eines API-Tokens finden Sie in der Dokumentation zum [Lernprogramm zur Einführung](../getting-started.html).


## Informationen zu diesem Vorgang

In diesem Szenario werden zwei Geräte erstellt.

Ein Gerät heißt *tSensor*. Dieses Gerät publiziert Temperaturereignisse, die in Grad Celsius gemessen werden. Das Temperaturereignis wird im Topic `iot-2/evt/tevt/fmt/json` publiziert und enthält die folgenden Beispielnutzdaten:
```
{
  "t" : 34,5
}
```

**Hinweis:** Die Ereignis-ID lautet *tevt*. Diese ID ist erforderlich, wenn Sie ein Temperaturereignis dieses Typs zur physischen Schnittstelle hinzufügen und Zuordnungen definieren, um eine zugehörige Eigenschaft für ein eingehendes Ereignis dieses Typs einer Eigenschaft in Ihrer logischen Schnittstelle zuzuordnen. Im vorliegenden Szenario trägt die in der logischen Schnittstelle definierte Eigenschaft den Namen **temperature**.

Das andere Gerät wird als *tempSensor* bezeichnet. Dieses Gerät publiziert Temperaturereignisse, die in Grad Fahrenheit gemessen werden. Das Temperaturereignis wird im Topic `iot-2/evt/tempevt/fmt/json` publiziert und enthält die folgenden Beispielnutzdaten:
```
{
  "temp" : 72,55
}
```

**Hinweis:** Die Ereignis-ID lautet *tempevt*. Diese ID ist erforderlich, wenn Sie ein Temperaturereignis dieses Typs zur physischen Schnittstelle hinzufügen und Zuordnungen definieren, um eine zugehörige Eigenschaft für ein eingehendes Ereignis dieses Typs einer Eigenschaft in Ihrer logischen Schnittstelle zuzuordnen. Im vorliegenden Szenario trägt die in der logischen Schnittstelle definierte Eigenschaft den Namen **temperature**.

Außerdem wird eine logische Schnittstelle konfiguriert. Diese logische Schnittstelle stellt den Status für Geräte dieses Typs in der folgenden Struktur dar:
```
{
  "temperature" : <aktueller Temperaturwert in Celsius>
  }
```
Mithilfe dieser Konfiguration können Sie Ihre Anwendung so konfigurieren, dass der zugeordnete Wert für **temperature** verarbeitet wird, anstatt den zugeordneten Wert für **t** zu verarbeiten und den zugeordneten Wert für **temp** erst nach Umwandlung in Grad Celsius zu verarbeiten.

![Zuordnung zwischen Temperatursensorgeräten und einer Anwendung unter {{site.data.keyword.iot_short_notm}}.](../information_management/images/Information Management Device example.svg "Zuordnung zwischen Temperatursensorgeräten und einer Anwendung unter {{site.data.keyword.iot_short_notm}}")

Verwenden Sie das folgende Beispielszenario zum Einrichten Ihrer eigenen Schnittstellenumgebung.

**Wichtiger Hinweis:** Sie müssen die IDs speichern, die in den curl-Antworten generiert werden, da die IDs erforderlich sind, um andere Tasks auszuführen.
Eine Tabelle mit den Namen, Werten und Kennungen der Ressourceneigenschaften, die in diesem Handbuch verwendet werden, ist in [Zusätzliche Informationen zu den schrittweisen Anleitungen 1 and 2 - Ressourcennamen und IDs](../information_management/im_id_reference.html) zu finden.

## Fügen Sie bei Bedarf einen Gerätetyp und ein Gerät hinzu
{: #step14}

Im vorliegenden Szenario werden zwei Gerätetypen und zwei Geräteinstanzen angenommen. Die Geräteinstanz *tSensor* ist dem Gerätetyp *TSensor* zugeordnet. Die Geräteinstanz *tempSensor* ist dem Gerätetyp *TempSensor* zugeordnet.  

Sie können Gerätetypen und Geräte mithilfe des [{{site.data.keyword.iot_short_notm}}-Dashboards ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://internetofthings.ibmcloud.com){: new_window} oder mithilfe von REST-APIs erstellen. Weitere Informationen zur Verwendung des {{site.data.keyword.iot_short_notm}}-Dashboards zum Hinzufügen von Gerätetypen und Geräten finden Sie in der Dokumentation [Einführung zum Datenmanagement mithilfe der Webschnittstelle](im_ui_flow.html).

Das folgende Beispiel zeigt, wie ein Gerätetyp mit dem Namen *TSensor* mithilfe der REST-API erstellt wird:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TSensor", "description" : "The Celsius sensor device type", "metadata": {"tempThresholdMax": 44,
    "tempThresholdMin": 10}}' \
 ```
 
 Das folgende Beispiel zeigt, wie ein Gerätetyp mit dem Namen *TempSensor* mithilfe der REST-API erstellt wird:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TempSensor", "description" : "The Fahrenheit sensor device type"}' \
 ```

**Hinweis:** Sie können beim Erstellen eines Gerätetyps und eines Geräts Metadaten hinzufügen. Im vorliegenden Szenario werden die folgenden Metadaten zum Gerätetyp *TSensor* hinzugefügt:
```
{
    "tempThresholdMax": 44,
    "tempThresholdMin": 10 
}
```
Diese Metadaten werden bei der Erstellung von [Regeln](../information_management/im_rules.html) verwendet, die ausgelöst werden, wenn ein Temperaturereignis, das dazu führt, dass die Eigenschaft *temperature* des Gerätestatus 44 Grad Celsius überschreitet, von {{site.data.keyword.iot_short_notm}} vom Gerät *tSensor* empfangen wird. 


Anschließend müssen Sie eine Geräteinstanz registrieren, die einem Gerätetyp zugeordnet ist. Das folgende Beispiel zeigt, wie eine Geräteinstanz namens *tSensor*, die dem Gerätetyp *TSensor* zugeordnet ist, mithilfe der REST-API registriert wird:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tSensor", "authToken": "password"}' \
```

Das folgende Beispiel zeigt, wie eine Geräteinstanz namens *tempSensor*, die dem Gerätetyp *TempSensor* zugeordnet ist, mithilfe der REST-API registriert wird:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tempSensor", "authToken": "password"}' \
```

Informationen zur Verwendung von REST-APIs zum Hinzufügen von Gerätetypen und Geräten finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}} HTTP-REST-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

**Hinweis:** Wenn ein Gerät über die HTTP-REST-API von Watson IoT Platform eine HTTP-Anforderung absetzt, sind ein Benutzername und ein Kennwort erforderlich. Das Kennwort ist der Wert des Authentifizierungstokens, der entweder automatisch generiert oder manuell angegeben wird, wenn ein Gerät registriert wird. Wenn Sie einen MQTT-Client verwenden, müssen Sie sich das Authentifizierungstoken Ihres Geräts notieren, da Sie das Token zum Abrufen des Gerätestatus oder des 'Ding'-Status benötigen, indem Sie eine Topic-Zeichenfolge subskribieren.

## Schritt 1: Eine Ereignisschemadatei erstellen
{: #step1}

Erstellen Sie für das vorliegende Szenario zwei Ereignisschemadateien, um die Struktur der eingehenden Temperaturereignisse zu definieren.

Das folgende Beispiel zeigt das Erstellen einer Schemadatei mit dem Namen *tEventSchema.json*. In dieser Datei wird die Struktur eines eingehenden Ereignisses von einem Temperaturgerät definiert, das die Temperatur in Grad Celsius misst:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tEventSchema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
      "description" : "temperature in degrees Celsius",
      "type" : "number",
      "minimum" : -273.15,
      "default" : 0.0
    }
  },
  "required" : ["t"]
}
  ```
**Tipp:** Verwenden Sie den Parameter **required**, um eine oder mehrere Eigenschaften als erforderlich zu markieren. Erforderliche Eigenschaften müssen in einer Gerätenachricht enthalten sein, damit {{site.data.keyword.iot_short_notm}} den Gerätestatus mit den Gerätedaten aktualisieren kann. Eine Nachricht, die die erforderliche Eigenschaft nicht enthält, wird nicht verarbeitet.   

Der Schemadateiname *tEventSchema* wird verwendet, wenn Sie eine Ereignisschemaressource für Ihren Ereignistyp erstellen.

Das folgende Beispiel zeigt das Erstellen einer Schemadatei mit dem Namen *tempEventSchema.json*. In dieser Datei wird die Struktur eines eingehenden Ereignisses von einem Temperaturgerät definiert, das die Temperatur in Grad Fahrenheit misst:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tempEventSchema",
  "description" : "defines the structure of a temperature event in degrees Fahrenheit",
  "properties" : {
    "temp" : {
      "description" : "temperature in degrees Fahrenheit",
      "type" : "number",
      "minimum" : -459.67,
      "default" : 0.0
    }
  },
  "required" : ["temp"]
}
  ```
Der Schemadateiname *tempEventSchema* wird verwendet, wenn Sie eine Ereignisschemaressource für Ihren Ereignistyp erstellen.   

## Schritt 2: Ereignisschemaressource für den Ereignistyp erstellen
{: #step2}

Verwenden Sie die folgende API, um eine Ereignisschemaressource zu erstellen:

```
POST /draft/schemas
```

Die Schemadefinitionsdatei wird in einer mehrteiligen POST-Anforderung (multipart/form-data) an Watson IoT Platform übergeben. Der Hauptteil der POST-Anforderung muss aus mindestens zwei Teilen bestehen:

- Ein Teil mit der Bezeichnung **schemaFile**, der den tatsächlichen Inhalt der Datei als Hauptteil des Teils enthält.
- Ein Teil mit der Bezeichnung **name**, der eine Zeichenfolge enthält, die den Namen der Schemaressourcendatei im Hauptteil des Teils definiert.

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen der Ereignisschemaressource *tEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

**Tipp:** Der Beispielberechtigungswert `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` setzt sich aus den folgenden Informationen zusammen:
`{API Key}:{authorization token}` als Base64-Codierung.

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "name" : "tEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "tEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e0d",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Schema-ID *5846cd7c6522050001db0e0d*, die als Antwort auf die POST-Methode zurückgegeben wird, ist erforderlich, wenn Sie ein Ereignisschema zu Ihrem Ereignistyp hinzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen der Ereignisschemaressource *tempEventSchema.json*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "schemaType" : "json-schema",
  "schemaFileName" : "tempEventSchema.json",
  "updated" : "2016-12-06T14:44:51Z",
  "name" : "tempEventSchema",
  "version" : "draft",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "created" : "2016-12-06T14:44:51Z",
  "id" : "5846cee36522050001db0e0e",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e/content"
  },
  "contentType" : "application/octet-stream",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Schema-ID *5846cee36522050001db0e0e*, die als Antwort auf die POST-Methode zurückgegeben wird, ist erforderlich, wenn Sie ein Ereignisschema zu Ihrem Ereignistyp hinzufügen.

## Schritt 3: Einen Ereignistyp erstellen, der das Ereignisschema referenziert
{: #step3}

Jeder Ereignistyp verwendet zum Verweisen auf das Ereignisschema, das im vorherigen Beispiel erstellt wurde, die in der Antwort auf die POST-Methode zum Erstellen der Ereignisschemaressource zurückgegebene Schema-ID.

Verwenden Sie die folgende API, um einen Ereignistyp zu erstellen:

```
POST /draft/event/types
```
Der Entwurfsereignistyp muss auf die Schemadefinition verweisen, die die Struktur des eingehenden MQTT-Ereignisses definiert.


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen eines Ereignistyps für ein Temperaturereignis, das in Grad Celsius gemessen wird:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

Die Schema-ID *5846cd7c6522050001db0e0d* wird verwendet, um das Ereignisschema zu dem Ereignistyp hinzuzufügen. Diese ID wurde als Antwort auf die POST-Methode zurückgegeben, die zum Erstellen der Ereignisschemaressource *tEventSchema.json* verwendet wurde.

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e0d",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d"
  },
  "name" : "tEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846d0fd6522050001db0e0f",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

Die Ereignistyp-ID *5846d0fd6522050001db0e0f*, die als Antwort auf die POST-Methode zurückgegeben wird, wird verwendet, um einen Ereignistyp zu der physischen Schnittstelle hinzuzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen eines Ereignistyps für ein Temperaturereignis, das in Grad Fahrenheit gemessen wird:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
Die Schema-ID *5846cee36522050001db0e0e* wird verwendet, um das Ereignisschema zu dem Ereignistyp hinzuzufügen. Diese ID wurde als Antwort auf die POST-Methode zurückgegeben, die zum Erstellen der Ereignisschemaressource *tempEventSchema.json* verwendet wurde.

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "schemaId" : "5846cee36522050001db0e0e",
  "created" : "2016-12-06T15:00:20Z",
  "id" : "5846d2846522050001db0e10",
  "updated" : "2016-12-06T15:00:20Z",
  "name" : "tempEvent",
  "version" : "draft",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e"
  },
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Ereignistyp-ID *5846d2846522050001db0e10*, die als Antwort auf die POST-Methode zurückgegeben wird, wird verwendet, um einen Ereignistyp zu der physischen Schnittstelle hinzuzufügen.

## Schritt 4: Physische Schnittstelle erstellen
{: #step7}

Verwenden Sie die folgende API, um eine physische Schnittstelle zu erstellen:

```
POST /draft/physicalinterfaces
```

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Für das vorliegende Szenario sind zwei physische Schnittstellen erforderlich (eine für jeden Ereignistyp).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen der ersten physischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TSensor Physical Interface"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

Die in der Antwort zurückgegebene ID der physischen Schnittstelle (*5847d1df6522050001db0e1a*) wird in der URL der POST-Methode verwendet, die aufgerufen wird, um ein Temperaturereignis zur physischen Schnittstelle hinzuzufügen, das in Grad Celsius gemessen wird.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen der zweiten physischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TempSensor Physical Interface"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

Die in der Antwort zurückgegebene ID der physischen Schnittstelle (*5847d1df6522050001db0e1b*), wird in der URL der POST-Methode verwendet, die aufgerufen wird, um ein Temperaturereignis zur physischen Schnittstelle hinzuzufügen, das in Grad Fahrenheit gemessen wird.   

## Schritt 5: Ereignistyp zur physischen Schnittstelle hinzufügen
{: #step8}

Verwenden Sie die folgende API, um einen Ereignistyp zu Ihrer physischen Schnittstelle hinzuzufügen:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Im vorliegenden Szenario werden die folgenden Ereignistypen zu den angegebenen physischen Schnittstellen hinzugefügt:
- Das Temperaturereignis in Grad Celsius (*tevt*) wird zur physischen Schnittstelle mit der ID *5847d1df6522050001db0e1a* unter Verwendung von *eventId* aus dem Topic und von *eventTypeId* aus der Erstellung der Ereignisschemaressource hinzugefügt.
- Das Temperaturereignis in Grad Fahrenheit (*tempevt*) wird zur physischen Schnittstelle mit der ID *5847d1df6522050001db0e1b* unter Verwendung von *eventId* aus dem Topic und von *eventTypeId* aus der Erstellung der Ereignisschemaressource hinzugefügt.


Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen des Temperaturereignisses *tevt* zur physischen Schnittstelle mit der ID *5847d1df6522050001db0e1a*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen des Temperaturereignisses *tempevt* zur physischen Schnittstelle mit der ID *5847d1df6522050001db0e1b*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
}
```

## Schritt 6: Gerätetyp für die Verbindung mit der physischen Schnittstelle aktualisieren
{: #step9}

Verwenden Sie die folgende API, um einen Gerätetyp zu aktualisieren:

```
POST /draft/device/types/{typeId}/physicalinterface
```

Dabei steht *typeId* für die ID des Gerätetyps.


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In diesem Szenario wird der Gerätetyp *TSensor* zum Verbinden mit der physischen Schnittstelle *5847d1df6522050001db0e1b* und der Gerätetyp *TempSensor* zum Verbinden mit der physischen Schnittstelle *5847d1df6522050001db0e1a* aktualisiert.


Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Aktualisieren des Gerätetyps *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Gerätekennung *TSensor* ist erforderlich, wenn Sie Ihre physische Schnittstelle und Ihre logische Schnittstelle hinzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Aktualisieren des Gerätetyps *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Gerätekennung *TempSensor* ist erforderlich, wenn Sie Ihre physische Schnittstelle und Ihre logische Schnittstelle hinzufügen.


## Schritt 7: Schemadatei für logische Schnittstelle erstellen
{: #step4}

Das folgende Beispiel zeigt das Erstellen einer Schemadatei einer logischen Schnittstelle mit dem Namen *envSensor.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "thermometerSchema",
    "description" : "Schema that defines the canonical interface for a thermometer",
    "properties" : {
        "temperature" : {
            "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## Schritt 8: Schemaressource für eine logischen Schnittstelle erstellen
{: #step5}

Verwenden Sie die folgende API, um eine Schemaressource für logische Schnittstellen zu erstellen:

```
POST /draft/schemas
```

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen des Schemas der logischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=thermometerSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/thermometer.json"'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "thermometerSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "thermometer.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
Verwenden Sie die Schema-ID *5846ec826522050001db0e11*, die in der Antwort auf die POST-Methode zurückgegeben wird, um das Schema der logischen Schnittstelle zu der logischen Schnittstelle hinzuzufügen.

## Schritt 9: Eine logische Schnittstelle erstellen, die das Schema einer logischen Schnittstelle referenziert
{: #step6}

Verwenden Sie die folgende API, um eine logische Schnittstelle zu erstellen:

```
POST /draft/logicalinterfaces
```

Sie können optional einen aussagekräftigen Aliasnamen für die logische Schnittstelle angeben. Der Aliasname kann in der API-Aufruf- oder Topic-Zeichenfolgensubskription referenziert werden, die verwendet wird, um den Status eines Geräts abzurufen, anstatt die automatisch generierte logische Schnittstellenkennung zu verwenden.  

**Hinweis:** Der Aliasname muss zwischen 1 und 36 Zeichen lang sein und kann alphanumerische Zeichen, Bindestriche, Punkte und Unterstreichungszeichen enthalten. Der Aliasname darf keine Hexadezimalzeichenfolge mit 24 Zeichen sein. 

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

Verwenden Sie in diesem Szenario die Schema-ID *5846ec826522050001db0e11*, die in der vorherigen Antwort zurückgegeben wurde, um das Schema der logischen Schnittstelle zu der logischen Schnittstelle hinzuzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer logischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Thermometer Interface", "alias" : "IThermometer", "schemaId" : "5846ec826522050001db0e11"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "schemaId" : "5846ec826522050001db0e11",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846ed076522050001db0e12",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Thermometer Interface",
  "alias" : "IThermometer",
  "version" : "draft"
}
```
Verwenden Sie in diesem Szenario die ID der logischen Schnittstelle *5846ed076522050001db0e12*, die in der Antwort auf die POST-Methode zurückgegeben wird, um Ihre logische Schnittstelle zu Ihrem Gerätetyp hinzuzufügen. Diese ID wird auch verwendet, um ein eingehendes Geräteereignis einer Eigenschaft zuzuordnen, die von der logischen Schnittstelle definiert wird. Sie können den Aliasnamen *IThermometer* der logischen Schnittstellen verwenden, um [den Status des Geräts](##step13) entweder mithilfe von HTTP-REST-APIs oder durch Subskribieren einer Topic-Zeichenfolge abzurufen.

## Schritt 10: Logische Schnittstelle zu einem Gerätetyp hinzufügen
{: #step10}

Verwenden Sie die folgende API, um eine logische Schnittstelle zu einem Gerätetyp hinzuzufügen:

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

Dabei steht *typeId* für den Namen des Gerätetyps. 

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).  
**Hinweis:** In diesem Szenario ist dieselbe logische Schnittstell *5846ed076522050001db0e12* sowohl *TSensor* als auch *TempSensor* zugeordnet.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen der logischen Schnittstelle *5846ed076522050001db0e12*, die der ID des logischen Schemas *5846ec826522050001db0e11* zugeordnet ist, zu dem Gerätetyp *TSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen der logischen Schnittstelle *5846ed076522050001db0e12*, die auf die ID des Schemas der logischen Schnittstelle *5846ec826522050001db0e11* verweist, zu dem Gerätetyp *TempSensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

## Schritt 11: Zuordnungen definieren, um Eigenschaften im eingehenden Ereignis bestimmten Eigenschaften in der logischen Schnittstelle zuzuordnen
{: #step11}  
**Wichtig:** Wenn Sie eine MQTT-Clientanwendung benutzen und eine Subskription durchführen wollen, um Benachrichtigungen zu Aktualisierungen des Gerätestatus zu erhalten, dann müssen Sie die Benachrichtigungseinstellungen konfigurieren, wenn Sie Zuordnungen definieren. Durch eine derartige Konfiguration Ihrer Benachrichtigungsstrategie können Sie eine logische Schnittstelle gerätetypübergreifend wiederverwenden, wobei jeder Gerätetyp auf eine von Ihnen ausgewählte Art und Weise zum Empfang von Benachrichtigungen zu Statusänderungen konfiguriert wird. Die Benachrichtigungsstrategie kann unabhängig davon definiert werden, ob ein MQTT-Client eine Subskription für die angegebene Topic-Zeichenfolge durchführt.  
  
Die folgenden Benachrichtigungseinstellungen können konfiguriert werden:  

Parameter	|	Beschreibung
------	|	-----
never	|	Das System sendet keine Benachrichtigungen - Dies ist die Standardeinstellung.
on-every-event	|	Das System sendet unabhängig davon, ob sich der Gerätestatus geändert hat, Benachrichtigungen für jedes Ereignis.
on-state-change	|	Das System sendet Benachrichtigungen nur dann, wenn ein Ereignis zu einer Änderung des Gerätestatus führt.  

**Hinweis:** Wenn Sie die Benachrichtigungseinstellung *never* (Nie) auswählen, dann erhält Ihre Anwendung niemals eine Benachrichtigung zu einer Änderung des Gerätestatus. Aus diesem Grund kann es sinnvoll sein, als Benachrichtigungsstrategie *on-every-event* oder *on-state-change* zu verwenden.  

Verwenden Sie die folgende API, um Ereignisse zuzuordnen:

```
POST /draft/device/types/{typeId}/mappings
```

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In diesem Szenario werden Zuordnungen für den Gerätetyp *TSensor* definiert, um die Eigenschaft **t** im eingehenden Ereignis *tevt* der Eigenschaft **temperature** in der logischen Schnittstelle zuzuordnen. Außerdem werden Zuordnungen für den Gerätetyp *TempSensor* definiert, um die Eigenschaft **temp** im eingehenden Ereignis *tempevt* der Eigenschaft **temperature** in der logischen Schnittstelle zuzuordnen. Die Benachrichtigungseinstellungen werden auf den Wert "on-state-change" gesetzt. 

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen einer Zuordnung zum Gerätetyp *TSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Wichtig:** Stellen Sie für Ereignisnutzdaten für strukturiertes [JSON ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://json-schema.org/){:new_window} sicher, dass der Eintrag "$event.*property*" der JSON-Struktur Ihrer Eigenschaften entspricht. Wenn die Nutzdaten beispielsweise `{"d":{"t":<temp>}}` lauten, verwenden Sie `$event.d.t`.

Geben Sie die ID *5846ed076522050001db0e12* der logischen Schnittstelle an, die in der Antwort auf die POST-Methode zum Erstellen der logischen Schnittstelle und des Gerätetyps *TSensor* zurückgegeben wurde.


Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt" : {
      "temperature" : "$event.t"
              }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```
Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen einer Zuordnung zum Gerätetyp *TempSensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Geben Sie die ID *5846ed076522050001db0e12* der logischen Schnittstelle an, die in der Antwort auf die POST-Methode zum Erstellen der logischen Schnittstelle und des Gerätetyps *TempSensor* zurückgegeben wurde.
Durch eine Umwandlung wird der Messwert von Grad Fahrenheit in Grad Celsius geändert.


Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
     "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## Schritt 12: Konfiguration überprüfen und aktivieren
{: #step15}

Prüfen und aktivieren Sie die Konfiguration, die sich auf die Aktualisierung des Gerätestatus für jeden Gerätetyp bezieht. Diese Konfiguration enthält Ihre Schemas, Ereignistypen, physischen Schnittstellen, logischen Schnittstellen und Zuordnungen.

Verwenden Sie die folgende API, um die Konfiguration Ihres Gerätetyps zu überprüfen und zu aktivieren:

```
PATCH /draft/device/types/{typeId}
```

Dabei steht *typeId* für die ID des Gerätetyps.

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In diesem Szenario müssen Sie die Konfiguration für zwei Gerätetypen überprüfen und aktivieren.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Prüfen und Aktivieren Ihrer Konfiguration für den Gerätetyp *TSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
Das folgende Beispiel zeigt eine Antwort auf die PATCH-Methode:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TSensor"]
  },
 "failures": []
}
```

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Prüfen und Aktivieren Ihrer Konfiguration für den Gerätetyp *TempSensor*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

Das folgende Beispiel zeigt eine Antwort auf die PATCH-Methode:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TempSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TempSensor"]
  },
 "failures": []
}
```

## Schritt 13: Ein eingehendes Geräteereignis publizieren
{: #step12}

Publizieren Sie ein Temperaturereignis von *tSensor* zum Topic `iot-2/evt/tevt/fmt/json` mit den folgenden Beispielnutzdaten:
```
{
  "t" : 34,5
}
```
Publizieren Sie ein Temperaturereignis von *tempSensor* zum Topic `iot-2/evt/tempevt/fmt/json` mit den folgenden Beispielnutzdaten:
```
{
  "temp" : 72,55
}
```

Informationen zum Publizieren eines eingehenden Ereignisses von einem Gerät finden Sie in [MQTT-Konnektivität für Anwendungen](../applications/mqtt.html#publishing_device_events).


## Schritt 14: Gerätestatus abrufen
{: #step13}

Sie können den Status des Geräts entweder mithilfe von HTTP-REST-APIs oder durch Subskribieren einer Topic-Zeichenfolge abrufen. Wenn Sie bei der Erstellung der logischen Schnittstelle einen Aliasnamen angegeben haben, können Sie den Aliasnamen anstelle des Parameters *logicalInterfaceId* verwenden, um den Status des Geräts abzurufen. 

- Wenn Sie eine MQTT-Clientanwendung benutzen, dann können Sie eine Subskription für die folgende Topic-Zeichenfolge durchführen:  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- Alternativ hierzu können Sie den aktuellsten Gerätestatus auch über die folgende HTTP-REST-API abrufen:  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung
------	|	-----
typeId	|	Die ID des Gerätetyps.
deviceId	|	Die Geräte-ID.
logicalInterfaceId oder alias|	Die für die logische Schnittstelle erstellte ID oder der benutzerdefinierte Aliasname. 


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Abrufen des aktuellen Status von *tSensor* durch Referenzieren der ID der erstellten logischen Schnittstelle.
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/tSensor/devices/tSensor/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

Die ID der logischen Schnittstelle *5846ed076522050001db0e12* wird in der GET-Methode verwendet. Diese ID wird in der Antwort auf die POST-Methode zurückgegeben, die zum Erstellen der logischen Schnittstelle verwendet wurde.
Das folgende Beispiel zeigt eine Antwort auf die GET-Methode:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
  }
}
```

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Abrufen des aktuellen Status von *tempSensor* durch Referenzierung des Aliasnamens der erstellten logischen Schnittstelle:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices/tempSensor/state/IThermometer \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

Der Aliasname *IThermometer* der logischen Schnittstelle wird in der GET-Methode verwendet. Diese ID wird in der Antwort auf die POST-Methode zurückgegeben, die zum Erstellen der logischen Schnittstelle verwendet wurde.
Das folgende Beispiel zeigt eine Antwort auf die GET-Methode:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
  }
}
```

Beachten Sie, dass der Temperaturmesswert in Grad Celsius zurückgegeben wird und nicht in Grad Fahrenheit.

Ihre Anwendung kann diese normalisierten Daten nutzen, ohne über eine Konfiguration zum Erkennen oder Umwandeln der verschiedenen Temperaturmaßeinheiten zu verfügen.

## Nächste Schritte

Sie können dieses Szenario als Grundlage für die Verwendung der Funktion für Assetzwillinge des Datenmanagements nutzen, indem Sie einen 'Ding'-Typ und eine 'Ding'-Instanz konfigurieren. Informationen zum Konfigurieren von 'Dingen' finden Sie in [Schrittweise Anleitung 2: etailliertes Beispiel zur Vorgehensweise beim Arbeiten mit Geräten über eine allgemeine Schnittstelle (Beta)](../information_management/im_index_scenario_thing.html).

Sie können auch Regeln erstellen, die Sie zum Auslösen von Aktionen verwenden können, wenn ein Ereignis, das von {{site.data.keyword.iot_short_notm}} empfangen wird, eine Änderung im Geräte- oder 'Ding'-Status auslöst. Informationen zum Erstellen von Regeln finden Sie unter [Eingebettete Regeln erstellen (Beta)](../information_management/im_rules.html).

