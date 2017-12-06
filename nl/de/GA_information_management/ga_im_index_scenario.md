---

copyright:
years: 2016, 2017
lastupdated: "2017-09-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Schrittweise Anleitung: Detailliertes Beispiel zur Vorgehensweise beim Arbeiten mit Geräten über eine allgemeine Schnittstelle
{: #scenario}

Erstellen Sie mithilfe der folgenden Informationen ein Szenario, in dem Ereignisse von zwei Temperatursensoren an {{site.data.keyword.iot_full}} publiziert werden. Ein Sensor misst die Temperatur in Grad Celsius. Der andere Sensor misst die Temperatur in Grad Fahrenheit. Die gemessenen Werte werden einem einzigen Temperaturmesswert in Grad Celsius zugeordnet. Wenn diese Geräte einen neuen Temperaturmesswert ausgeben, wird der Wert der Eigenschaft geändert, die dem Gerätestatus zugeordnet ist.
{: shortdesc}

## Voraussetzungen

Sie müssen über eine {{site.data.keyword.iot_short_notm}}-Organisationsinstanz und über einen API-Schlüssel oder ein Token für die Organisation verfügen.

## Informationen zu diesem Vorgang

In diesem Szenario werden zwei Geräte konfiguriert.

Ein Gerät trägt den Namen *Temperatursensor 1*. Dieses Gerät publiziert Temperaturereignisse, die in Grad Celsius gemessen werden. Das Temperaturereignis wird im Topic `iot-2/evt/tevt/fmt/json` publiziert und enthält die folgenden Beispielnutzdaten:
```
{
  "t" : 34,5
}
```

**Hinweis:** Die Ereignis-ID lautet *tevt*. Diese ID ist erforderlich, wenn Sie ein Temperaturereignis dieses Typs zur physischen Schnittstelle hinzufügen und Zuordnungen definieren, um eine zugehörige Eigenschaft für ein eingehendes Ereignis dieses Typs einer Eigenschaft in Ihrer logischen Schnittstelle zuzuordnen. Im vorliegenden Szenario trägt die in der logischen Schnittstelle definierte Eigenschaft den Namen **temperature**.

Das andere Gerät trägt den Namen *Temperatursensor 2*. Dieses Gerät publiziert Temperaturereignisse, die in Grad Fahrenheit gemessen werden. Das Temperaturereignis wird im Topic `iot-2/evt/tempevt/fmt/json` publiziert und enthält die folgenden Beispielnutzdaten:
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

![Zuordnung zwischen Temperatursensorgeräten und einer Anwendung unter {{site.data.keyword.iot_short_notm}}.](images/Information)  

Verwenden Sie das folgende Beispielszenario zum Einrichten Ihrer eigenen Schnittstellenumgebung.

## Fügen Sie bei Bedarf einen Gerätetyp und ein Gerät hinzu
{: #step14}

Im vorliegenden Szenario werden zwei Gerätetypen und zwei Geräteinstanzen angenommen. Die Geräteinstanz *Temperatursensor 1* ist dem Gerätetyp *EnvSensor1* zugeordnet. Die Geräteinstanz *Temperatursensor 2* ist dem Gerätetyp *EnvSensor2* zugeordnet.

Informationen über die Verwendung von REST-APIs zum Hinzufügen eines Gerätetyps finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html).

## Schritt 1: Entwurf einer Ereignisschemadatei erstellen
{: #step1}

Erstellen Sie für das vorliegende Szenario zwei Entwürfe einer Ereignisschemadatei, um die Struktur der eingehenden Temperaturereignisse zu definieren.

Das folgende Beispiel zeigt das Erstellen eines Entwurfs einer Schemadatei mit dem Namen *tEventSchema.json*. In dieser Datei wird die Struktur eines eingehenden Ereignisses von einem Temperatursensor definiert, der die Temperatur in Grad Celsius misst:

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor1 tEvent Schema",
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
**Tipp:** Verwenden Sie den Parameter "required", um eine oder mehrere Eigenschaften als erforderlich zu markieren. Erforderliche Eigenschaften müssen in einer Gerätenachricht enthalten sein, damit {{site.data.keyword.iot_short_notm}} den Gerätestatus mit den Gerätedaten aktualisieren kann. Eine Nachricht, die die erforderliche Eigenschaft nicht enthält, wird nicht verarbeitet.   

Der Schemadateiname *tEventSchema* wird verwendet, wenn Sie eine Ereignisschemaressource für Ihren Ereignistyp erstellen.

Das folgende Beispiel zeigt das Erstellen eines Entwurfs einer Schemadatei mit dem Namen *tempEventSchema.json*. In dieser Datei wird die Struktur eines eingehenden Ereignisses von einem Temperatursensor definiert, der die Temperatur in Grad Fahrenheit misst.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor2 tempEvent Schema",
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

## Schritt 2: Entwurf einer Ereignisschemaressource für den Ereignistyp erstellen
{: #step2}

Verwenden Sie die folgende API, um eine Ereignisschemaressource zu erstellen:

```
POST /draft/schemas
```

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung  
------	|	-----  
name	|	Geben Sie einen Namen für das Ereignisschema an, das Sie erstellen.  
schemaFile	|	Pfad zur lokalen JSON-Datei des Ereignisschemas.  



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

## Schritt 3: Entwurf eines Ereignistyps erstellen, der das Ereignisschema referenziert
{: #step3}

Jeder Ereignistyp verwendet zum Verweisen auf das Ereignisschema, das im vorherigen Beispiel erstellt wurde, die in der Antwort auf die POST-Methode zum Erstellen der Ereignisschemaressource zurückgegebene Schema-ID.

Verwenden Sie die folgende API, um einen Entwurf eines Ereignistyps zu erstellen:

```
POST /draft/event/types
```

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung
------	|	-----
name	|	Geben Sie einen Namen für den Ereignistyp an, den Sie erstellen.
schemaId	|	Die für die Ereignisschemaressource erstellte ID.



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

## Schritt 4: Entwurf einer physischen Schnittstelle erstellen
{: #step7}

Verwenden Sie die folgende API, um einen Entwurf einer physischen Schnittstelle zu erstellen:

```
POST /draft/physicalinterfaces
```

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung
------	|	-----
name	|	Geben Sie einen Namen für die physische Schnittstelle an, die Sie erstellen.


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Für das vorliegende Szenario sind zwei physische Schnittstellen erforderlich (eine für jeden Ereignistyp).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen der ersten physischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Env sensor physical interface 1"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "Env sensor physical interface 1",
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
  --data '{"name" : "Env sensor physical interface 2"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

Die in der Antwort zurückgegebene ID der physischen Schnittstelle (*5847d1df6522050001db0e1b*), wird in der URL der POST-Methode verwendet, die aufgerufen wird, um ein Temperaturereignis zur physischen Schnittstelle hinzuzufügen, das in Grad Fahrenheit gemessen wird.   

## Schritt 5: Ereignistyp zum Entwurf einer physischen Schnittstelle hinzufügen
{: #step8}

Verwenden Sie die folgende API, um einen Ereignistyp zu Ihrer physischen Schnittstelle hinzuzufügen:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung
------	|	-----
eventId	|	Geben Sie den Ereignisnamen von den Ereignisnutzdaten Ihres Geräts ein.
eventTypeId	|	Die für den Ereignistyp erstellte ID.



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

## Schritt 6: Entwurf eines Gerätetyps für die Verbindung mit dem Entwurf einer physischen Schnittstelle aktualisieren
{: #step9}

Verwenden Sie die folgende API, um den Entwurf eines Gerätetyps zu aktualisieren:

```
POST /draft/device/types/{typeId}/physicalinterface
```

Dabei steht *typeId* für die ID des Gerätetyps.


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In diesem Szenario wird der Gerätetyp *EnvSensor1* zum Verbinden mit der physischen Schnittstelle *5847d1df6522050001db0e1a* und der Gerätetyp *EnvSensor2* zum Verbinden mit der physischen Schnittstelle *5847d1df6522050001db0e1b* aktualisiert.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Aktualisieren des Gerätetyps *EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/physicalinterface \
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
  "name" : "Env sensor physical interface 1",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Geräte-ID *EnvSensor1* ist erforderlich, wenn Sie Ihre physische Schnittstelle und Ihre logische Schnittstelle hinzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Aktualisieren des Gerätetyps *EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/physicalinterface \
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
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Geräte-ID *EnvSensor2* ist erforderlich, wenn Sie Ihre physische Schnittstelle und Ihre logische Schnittstelle hinzufügen.



## Schritt 7: Schemadatei für Entwurf einer logischen Schnittstelle erstellen
{: #step4}

Das folgende Beispiel zeigt das Erstellen einer Schemadatei für den Entwurf einer logischen Schnittstelle mit dem Namen *envSensor.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Environment Sensor Schema",
    "description" : "Schema to represent a canonical environment sensor device",
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

## Schritt 8: Schemaressource für Entwurf einer logischen Schnittstelle erstellen
{: #step5}

Verwenden Sie die folgende API, um eine Schemaressource für den Entwurf einer logischen Schnittstelle zu erstellen:

```
POST /draft/schemas
```

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung
------	|	-----
name	|	Geben Sie einen Namen für das Schema der logischen Schnittstelle an, das Sie erstellen.
schemaFile	|	Pfad zur lokalen JSON-Schemadatei der logischen Schnittstelle.


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen des Schemas der logischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=temperatureEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/envSensor.json"'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "temperatureEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "envSensor.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
Verwenden Sie die Schema-ID *5846ec826522050001db0e11*, die in der Antwort auf die POST-Methode zurückgegeben wird, um das Schema der logischen Schnittstelle zu der logischen Schnittstelle hinzuzufügen.

## Schritt 9: Entwurf einer logischen Schnittstelle erstellen, die das Schema eines Entwurfs einer logischen Schnittstelle referenziert
{: #step6}

Verwenden Sie die folgende API, um einen Entwurf einer logischen Schnittstelle zu erstellen:

```
POST /draft/logicalinterfaces
```

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung
------	|	-----
name	|	Geben Sie einen Namen für die logische Schnittstelle an, das Sie erstellen.
schemaId	|	Die für die Schemaressource der logischen Schnittstelle erstellte ID. 


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

Verwenden Sie in diesem Szenario die Schema-ID *5846ec826522050001db0e11*, die in der vorherigen Antwort zurückgegeben wurde, um das Schema der logischen Schnittstelle zu der logischen Schnittstelle hinzuzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer logischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "environment sensor interface", "schemaId" : "5846ec826522050001db0e11"}'
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
  "name" : "environment sensor interface",
  "version" : "draft"
}
```
Verwenden Sie in diesem Szenario die ID der logischen Schnittstelle *5846ed076522050001db0e12*, die in der Antwort auf die POST-Methode zurückgegeben wird, um Ihre logische Schnittstelle zu Ihrem Gerätetyp hinzuzufügen. Diese ID wird auch verwendet, um ein eingehendes Geräteereignis einer Eigenschaft zuzuordnen, die von der logischen Schnittstelle definiert wird.

## Schritt 10: Entwurf einer logischen Schnittstelle zu einem Gerätetyp hinzufügen
{: #step10}

Verwenden Sie die folgende API, um einen Entwurf einer logischen Schnittstelle zu einem Gerätetyp hinzuzufügen:

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

Dabei steht *typeId* für den Namen des Gerätetyps. 

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).  
**Hinweis:** In diesem Szenario ist dieselbe logische Schnittstelle *5846ed076522050001db0e12* sowohl *EnvSensor1* als auch *EnvSensor2* zugeordnet.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen der logischen Schnittstelle *5846ed076522050001db0e12*, die auf die ID des Schemas der logischen Schnittstelle *5846ec826522050001db0e11* verweist, zu dem Gerätetyp *EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/logicalinterfaces \
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
  "name" : "environment sensor interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen der logischen Schnittstelle *5846ed076522050001db0e12*, die der ID des logischen Schemas *5846ec826522050001db0e11* zugeordnet ist, zu dem Gerätetyp *EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/logicalinterfaces \
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
  "name" : "environment sensor interface",
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

Folgende Parameter sind erforderlich:  

Parameter	|	Beschreibung
------	|	-----
logicalInterfaceId	|	Die für die logische Schnittstelle erstellte ID. 
propertyMappings	|	Eine gültige JSON-Struktur, die für die logische Schnittstelle definierte Eigenschaften bestimmten Eigenschaften der Ereignisnutzdaten des Geräts zuordnet.	
Optional können Sie den Parameter *notificationStrategy* festlegen.  

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In diesem Szenario werden Zuordnungen für den Gerätetyp *EnvSensor1* definiert, um die Eigenschaft **t** im eingehenden Ereignis *tevt* der Eigenschaft **temperature** in der logischen Schnittstelle zuzuordnen. Außerdem werden Zuordnungen für den Gerätetyp *EnvSensor2* definiert, um die Eigenschaft **temp** im eingehenden Ereignis *tempevt* der Eigenschaft **temperature** in der logischen Schnittstelle zuzuordnen. Die Benachrichtigungseinstellungen werden auf den Wert "on-state-change" gesetzt. 

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen einer Zuordnung zum Gerätetyp *EnvSensor1*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Wichtig:** Stellen Sie für Ereignisnutzdaten für strukturiertes [JSON ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://json-schema.org/){:new_window} sicher, dass der Eintrag "$event.*property*" der JSON-Struktur Ihrer Eigenschaften entspricht. Wenn die Nutzdaten beispielsweise `{"d":{"t":<temp>}}` lauten, verwenden Sie `$event.d.t`.

Geben Sie die ID der logischen Schnittstelle *5846ed076522050001db0e12* an, die in der Antwort auf die POST-Methode zum Erstellen der logischen Schnittstelle und des Gerätetyps *EnvSensor1* zurückgegeben wurde.

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
Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen einer Zuordnung zum Gerätetyp *EnvSensor2*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Geben Sie die ID der logischen Schnittstelle *5846ed076522050001db0e12* an, die in der Antwort auf die POST-Methode zum Erstellen der logischen Schnittstelle und des Gerätetyps *EnvSensor2* zurückgegeben wurde.
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

Überprüfen und aktivieren Sie die Konfiguration, die in Bezug zur Aktualisierung des Gerätestatus für jeden Entwurf eines Gerätetyps steht. Diese Konfiguration enthält die Entwurfsversionen Ihrer Schemas, Ereignistypen, physischen Schnittstellen, logischen Schnittstellen und Zuordnungen.

Verwenden Sie die folgende API, um die Konfiguration Ihres Entwurfs eines Gerätetyps zu überprüfen und zu aktivieren:

```
PATCH /draft/device/types/{typeId}
```

Dabei steht *typeId* für die ID des Gerätetyps.

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Im vorliegenden Szenario muss die Konfiguration für zwei Entwürfe eines Gerätetyps überprüft und aktiviert werden.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Überprüfen und Aktualisieren Ihrer Konfiguration für den Entwurf des Gerätetyps *EnvSensor1*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

Das folgende Beispiel zeigt eine Antwort auf die PATCH-Methode:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor1' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor1"]
  },
 "failures": []
}
```
Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Überprüfen und Aktualisieren Ihrer Konfiguration für den Entwurf des Gerätetyps *EnvSensor2*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
Das folgende Beispiel zeigt eine Antwort auf die PATCH-Methode:

```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor2' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor2"]
  },
 "failures": []
}
```

## Schritt 13: Ein eingehendes Geräteereignis publizieren
{: #step12}

Publizieren Sie ein Temperaturereignis von *Temperatursensor 1* zum Thema `iot-2/evt/tevt/fmt/json` mit den folgenden Beispielnutzdaten:
```
{
  "t" : 34,5
}
```
Publizieren Sie ein Temperaturereignis von *Temperatursensor 2* zum Thema `iot-2/evt/tempevt/fmt/json` mit den folgenden Beispielnutzdaten:
```
{
  "temp" : 72,55
}
```

Informationen zum Publizieren eines eingehenden Ereignisses von einem Gerät finden Sie in [MQTT-Konnektivität für Anwendungen](../applications/mqtt.html#publishing_device_events).


## Schritt 14: Gerätestatus abrufen
{: #step13}

Sie können den Status des Geräts entweder mithilfe von HTTP-REST-APIs oder durch Subskription eines Themas abrufen.

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
logicalInterfaceId	|	Die für die logische Schnittstelle erstellte ID.

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Abrufen des aktuellen Status von *Temperatursensor 1* durch Referenzieren der ID der erstellten logischen Schnittstelle.
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor1/devices/TemperatureSensor1/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

Die ID der logischen Schnittstelle *5846ed076522050001db0e12* wird in der GET-Methode verwendet. Diese ID wird in der Antwort auf die POST-Methode zurückgegeben, die zum Erstellen der logischen Schnittstelle verwendet wurde.
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
Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Abrufen des aktuellen Status von *Temperatursensor 2* durch Referenzieren der ID der erstellten logischen Schnittstelle.
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor2/devices/TemperatureSensor2/state/5846ed076522050001db0e12 \
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
Beachten Sie, dass der Temperaturmesswert in Grad Celsius zurückgegeben wird und nicht in Grad Fahrenheit.

Ihre Anwendung kann diese normalisierten Daten nutzen, ohne über eine Konfiguration zum Erkennen oder Umwandeln der verschiedenen Temperaturmaßeinheiten zu verfügen.


