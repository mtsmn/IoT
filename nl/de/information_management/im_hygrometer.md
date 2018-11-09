---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Zusätzliche Informationen für die schrittweise Anleitung 2 - Logische Schnittstelle für ein Feuchtigkeitsgerät konfigurieren

Erstellen Sie mithilfe der folgenden Informationen ein Szenario, in dem Sensoren auf zwei Feuchtigkeitsgeräten Ereignisse an IBM Watson™ IoT Platform publizieren. Messwerte von den Feuchtigkeitsgeräten werden einem einzigen Feuchtigkeitsmesswert zugeordnet. Ein Gerät befindet sich im Besprechungsraum 1, das andere Gerät im Besprechungsraum 2.


## Informationen zu diesem Vorgang

Verwenden Sie dieselbe {{site.data.keyword.iot_short_notm}}-Organisationsinstanz und einen API-Schlüssel oder Token für die Organisation, die Sie in der [schrittweisen Anleitung 1](../GA_information_management/ga_im_index_scenario.html) verwendet haben. 

Diese Konfiguration ist eine Voraussetzung für das Lernprogramm, das in der Dokumentation [Schrittweise Anleitung 2](im_index_scenario_thing.html) beschrieben ist.

In diesem Szenario werden ein Gerätetyp und zwei Geräteinstanzen erstellt.

Die Geräteinstanzen heißen *Feuchtigkeitssensor 1* und *Feuchtigkeitssensor 2*. Die Geräte publizieren Feuchtigkeitsereignisse. Das Feuchtigkeitsereignis weist die folgenden Beispielnutzdaten auf:
```
{
  "hum" : 35
}
```
Feuchtigkeitsereignisse werden für das Topic `iot-2/evt/humevt/fmt/json` publiziert. 

**Hinweis:** Die Ereignis-ID lautet *humevt*. Diese ID ist erforderlich, wenn Sie ein Feuchtigkeitsereignis dieser Typen zur physischen Schnittstelle hinzufügen und Zuordnungen definieren, um eine zugehörige Eigenschaft für ein eingehendes Ereignis dieses Typs einer Eigenschaft in Ihrer logischen Schnittstelle zuzuordnen. Im vorliegenden Szenario trägt die in der logischen Schnittstelle definierte Eigenschaft den Namen **humidity**.Diese logische Schnittstelle stellt den Status für Geräte dieses Typs in der folgenden Struktur dar:
```
{
  "humidity" : <aktueller Feuchtigkeitswert in Celsius>
}
```

Verwenden Sie das folgende Beispielszenario zum Einrichten Ihrer eigenen Schnittstellenumgebung.

**Wichtiger Hinweis:** Sie müssen die IDs speichern, die in den curl-Antworten generiert werden, da die IDs erforderlich sind, um andere Tasks auszuführen.

## Fügen Sie bei Bedarf einen Gerätetyp und ein Gerät hinzu
{: #step14}

In diesem Szenario werden ein Gerätetyp und zwei Geräteinstanzen angenommen. Die Geräteinstanzen *Feuchtigkeitssensor 1* und *Feuchtigkeitssensor 2* sind dem Gerätetyp *Feuchtigkeitssensor* zugeordnet. 

Sie können Gerätetypen und Geräte mithilfe des [{{site.data.keyword.iot_short_notm}}-Dashboards ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link") ](https://internetofthings.ibmcloud.com){: new_window} oder mithilfe von REST-APIs erstellen. Weitere Informationen zur Verwendung des {{site.data.keyword.iot_short_notm}} IoT Platform-Dashboards zum Hinzufügen von Gerätetypen und Geräten finden Sie in der Dokumentation [Einführung zum Datenmanagement mithilfe der Webschnittstelle](../GA_information_management/im_ui_flow.html).

Das folgende Beispiel zeigt, wie ein Gerätetyp mit dem Namen *Feuchtigkeitssensor* mithilfe der REST-API erstellt wird:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "Feuchtigkeitssensor", "description" : "Gerätetyp 'Feuchtigkeitssensor'", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**Wichtiger Hinweis:** Sie können beim Erstellen eines Gerätetyps und eines Geräts Metadaten hinzufügen. Im vorliegenden Szenario werden die folgenden Metadaten zum Gerätetyp *Feuchtigkeitssensor* hinzugefügt:
```
{
    "maxHumidityThreshold": 70
}
```
Diese Metadaten können beim [Erstellen von Regeln](im_rules.html) verwendet werden, die ausgelöst werden, wenn ein Feuchtigkeitsereignis, das dazu führt, dass die Eigenschaft *humidity* des Gerätestatus eine Wert von 70 % überschreitet, von {{site.data.keyword.iot_short_notm}} vom *Feuchtigkeitssensor 1* oder *Feuchtigkeitssensor 2* empfangen wird. 

## Geräte registrieren

Das folgende Beispiel zeigt die Registrierung einer Geräteinstanz mit dem Namen *Feuchtigkeitssensor 1*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "Feuchtigkeitssensor 1", "authToken": "Kennwort"}' \
```
Das folgende Beispiel zeigt die Registrierung einer Geräteinstanz mit dem Namen *Feuchtigkeitssensor 2*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "Feuchtigkeitssensor 2", "authToken": "Kennwort"}' \
```

**Hinweis:** Wenn ein Gerät über die HTTP-REST-API von Watson IoT Platform eine HTTP-Anforderung absetzt, sind ein Benutzername und ein Kennwort erforderlich. Das Kennwort ist der Wert des Authentifizierungstokens, der entweder automatisch generiert oder manuell angegeben wird, wenn ein Gerät registriert wird. Wenn Sie einen MQTT-Client verwenden, müssen Sie sich das Authentifizierungstoken Ihres Geräts notieren, da Sie das Token zum Abrufen des Gerätestatus oder des 'Ding'-Status benötigen, indem Sie eine IoT-Topic-Zeichenfolge subskribieren.

## Schritt 1: Eine Ereignisschemadatei erstellen
{: #step1}

Erstellen Sie für dieses Szenario eine Ereignisschemadatei, um die Struktur der eingehenden Feuchtigkeitsereignisse zu definieren.

Das folgende Beispiel zeigt das Erstellen einer Schemadatei mit dem Namen *humEventSchema.json*. In dieser Datei wird die Struktur eines eingehenden Ereignisses von einem Feuchtigkeitsgerät definiert:

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "definiert die Struktur eines Feuchtigkeitsereignisses",
    "properties" : {
        "humidity" : {
            "description" : "Prozentsatz Feuchtigkeit",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
      "humidity"
    ]
}
```
**Tipp:** Verwenden Sie den Parameter **required**, um eine oder mehrere Eigenschaften als erforderlich zu markieren. Erforderliche Eigenschaften müssen in einer Gerätenachricht enthalten sein, damit {{site.data.keyword.iot_short_notm}} den Gerätestatus mit den Gerätedaten aktualisieren kann. Eine Nachricht, die die erforderliche Eigenschaft nicht enthält, wird nicht verarbeitet.   


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

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen der Ereignisschemaressource *humEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**Hinweis:** Der Beispielberechtigungswert `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` setzt sich aus den folgenden Informationen zusammen:
`{API Key}:{authorization token}` als Base64-Codierung.

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "name" : "humEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "humEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e20",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Schema-ID *5846cd7c6522050001db0e20*, die als Antwort auf die POST-Methode zurückgegeben wird, ist erforderlich, wenn Sie ein Ereignisschema zu Ihrem Ereignistyp hinzufügen.



## Schritt 3: Einen Ereignistyp erstellen, der das Ereignisschema referenziert
{: #step3}

Jeder Ereignistyp verwendet zum Verweisen auf das Ereignisschema, das im vorherigen Beispiel erstellt wurde, die in der Antwort auf die POST-Methode zum Erstellen der Ereignisschemaressource zurückgegebene Schema-ID.

Verwenden Sie die folgende API, um einen Ereignistyp zu erstellen:

```
POST /draft/event/types
```
Der Entwurfsereignistyp muss auf die Schemadefinition verweisen, die die Struktur des eingehenden MQTT-Ereignisses definiert.


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen eines Ereignistyps für ein Feuchtigkeitsereignis:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

Die Schema-ID *5846cd7c6522050001db0e20* wird verwendet, um das Ereignisschema zu dem Ereignistyp hinzuzufügen. Diese ID wurde als Antwort auf die POST-Methode zurückgegeben, die zum Erstellen der Ereignisschemaressource *humEventSchema.json* verwendet wurde.

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e20",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20"
  },
  "name" : "humEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e21",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

Die Ereignistyp-ID *5846cd7c6522050001db0e21*, die als Antwort auf die POST-Methode zurückgegeben wird, wird verwendet, um einen Ereignistyp zu der physischen Schnittstelle hinzuzufügen.


## Schritt 4: Physische Schnittstelle erstellen
{: #step7}

Verwenden Sie die folgende API, um eine physische Schnittstelle zu erstellen:

```
POST /draft/physicalinterfaces
```

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

In diesem Szenario benötigen Sie eine physische Schnittstelle für den Ereignistyp, den Sie zuvor erstellt haben.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen der physischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Physische Schnittstelle 'Hygrometer'"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Physische Schnittstelle 'Hygrometer'",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

Die in der Antwort zurückgegebene ID der physischen Schnittstelle (*5846cd7c6522050001db0e22*) wird in der URL der POST-Methode verwendet, die aufgerufen wird, um ein Temperaturereignis zur physischen Schnittstelle hinzuzufügen, das in Grad Celsius gemessen wird.


## Schritt 5: Ereignistyp zur physischen Schnittstelle hinzufügen
{: #step8}

Verwenden Sie die folgende API, um einen Ereignistyp zu Ihrer physischen Schnittstelle hinzuzufügen:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

In diesem Szenario wird der folgende Ereignistyp zu den angegebenen physischen Schnittstellen hinzugefügt:
- Das Feuchtigkeitsereignis (*humevt*) wird zur physischen Schnittstelle mit der ID *5846cd7c6522050001db0e22* unter Verwendung von *eventId* aus dem Topic und von *eventTypeId* aus der Erstellung der Ereignisschemaressource hinzugefügt.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen des Feuchtigkeitsereignisses *humevt* zur physischen Schnittstelle mit der ID *5846cd7c6522050001db0e22*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
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

In diesem Szenario wird der Gerätetyp *Feuchtigkeitssensor* zum Verbinden mit der physischen Schnittstelle *5846cd7c6522050001db0e22* aktualisiert.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Aktualisieren des Gerätetyps *Feuchtigkeitssensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Physische Schnittstelle 'Hygrometer'",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Geräte-ID *Feuchtigkeitssensor* ist erforderlich, wenn Sie Ihre physische Schnittstelle und Ihre logische Schnittstelle hinzufügen.
  


## Schritt 7: Schemadatei für logische Schnittstelle erstellen
{: #step4}

Das folgende Beispiel zeigt das Erstellen einer Schemadatei einer logischen Schnittstelle mit dem Namen *hygrometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema, das die kanonische Schnittstelle für ein Hygrometer definiert",
    "properties" : {
        "humidity" : {
            "description" : "Prozentsatz Feuchtigkeit",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## Schritt 8: Schemaressource für eine logischen Schnittstelle erstellen
{: #step5}
Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer logischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Schnittstelle 'Hygrometer'", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Schnittstelle 'Hygrometer'",
  "alias" : "IHygrometer",
  "version" : "draft"
}
```

## Schritt 9: Eine logische Schnittstelle erstellen, die das Schema einer logischen Schnittstelle referenziert
{: #step6}

Verwenden Sie die folgende API, um eine logische Schnittstelle zu erstellen:

```
POST /draft/logicalinterfaces
```

Sie können optional einen aussagekräftigen Aliasnamen für die logische Schnittstelle angeben. Der Aliasname kann in der API-Aufruf- oder Topic-Zeichenfolgensubskription referenziert werden, die verwendet wird, um den Status eines Geräts abzurufen, anstatt die automatisch generierte logische Schnittstellenkennung zu verwenden.  

**Hinweis:** Der Aliasname muss zwischen 1 und 36 Zeichen lang sein und kann alphanumerische Zeichen, Bindestriche, Punkte und Unterstreichungszeichen enthalten. Der Aliasname darf keine Hexadezimalzeichenfolge mit 24 Zeichen sein. 

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

Verwenden Sie in diesem Szenario die Schema-ID *5846cd7c6522050001db0e23*, die in der vorherigen Antwort zurückgegeben wurde, um das Schema der logischen Schnittstelle zu der logischen Schnittstelle hinzuzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer logischen Schnittstelle:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Schnittstelle 'Hygrometer'", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Schnittstelle 'Hygrometer'",
  "alias" : "IHygrometermometer",
  "version" : "draft"
}
```
Verwenden Sie in diesem Szenario die logische Schnittstellen-ID *5846cd7c6522050001db0e24*, die in der Antwort auf die POST-Methode zurückgegeben wird, um Ihre logische Schnittstelle zu Ihrem Gerätetyp hinzuzufügen. Diese ID wird auch verwendet, um ein eingehendes Geräteereignis einer Eigenschaft zuzuordnen, die von der logischen Schnittstelle definiert wird. Sie können den Aliasnamen *IHygrometer* der logischen Schnittstellen verwenden, um den Status des Geräts entweder mithilfe von HTTP-REST-APIs oder durch Subskribieren einer Topic-Zeichenfolge abzurufen.


## Schritt 10: Logische Schnittstelle zu einem Gerätetyp hinzufügen

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen der logischen Schnittstelle '5846cd7c6522050001db0e24', die auf die ID des Schemas der logischen Schnittstelle '5846cd7c6522050001db0e23' verweist, zu dem Gerätetyp 'Feuchtigkeitssensor':
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Schnittstelle 'Hygrometer'",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846cd7c6522050001db0e24",
  "schemaId" : "5846cd7c6522050001db0e23"
}
```

## Schritt 11: Zuordnungen zum Gerätetyp hinzufügen

    
Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen einer Zuordnung zum Gerätetyp *Feuchtigkeitssensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "logicalInterfaceId": "5846cd7c6522050001db0e24",
  "propertyMappings": {
    "humevt": {
      "humidity": "$event.hum"
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

## Schritt 12: Konfiguration für den Gerätetyp aktivieren


    
Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Prüfen und Aktivieren Ihrer Konfiguration für den Gerätetyp *Feuchtigkeitssensor*:
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
Das folgende Beispiel zeigt eine Antwort auf die PATCH-Methode:
```
{
 "message": "CUDRS0520I: State update configuration for device type 'HumiditySensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["HumiditySensor"]
  },
 "failures": []
}
```
