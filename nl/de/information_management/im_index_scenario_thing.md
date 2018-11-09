---

copyright:
years: 2016, 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Schrittweise Anleitung 2: Ausführliches Beispiel zur Arbeit mit Dingen über eine gemeinsame Schnittstelle (Beta)
{: #scenario}

Dieses Szenario baut auf der vorherigen [schrittweisen Anleitung 1: Detailliertes Beispiel zur Vorgehensweise beim Arbeiten mit Geräten über eine allgemeine Schnittstelle](../GA_information_management/ga_im_index_scenario.html) auf.

**Wichtig:** Die {{site.data.keyword.iot_full}}-Funktion für Dinge für das Datenmanagement steht nur als Bestandteil des eingeschränkten Beta-Programms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

In diesem Szenario publizieren Temperatur- und Feuchtigkeitsgeräte Umgebungsdaten, die in zwei Besprechungsräumen erfasst werden: Besprechungsraum 1 und Besprechungsraum 2. Für jeden Besprechungsraum werden die Daten zu Temperatur und Feuchtigkeit separat zu zwei logischen Schnittstellen des Gerätetyps zugeordnet. 

Für Besprechungsraum 1 werden die Daten des Temperaturgeräts, die dem Gerätetyp *TSensor* zugeordnet sind, der logischen *Schnittstelle 'Thermometer'* zugeordnet und die Daten des Feuchtigkeitsgeräts, die dem Gerätetyp *Feuchtigkeitssensor* zugeordnet sind, werden der logischen *Schnittstelle 'Hygrometer'* zugeordnet. 

Für Besprechungsraum 2 werden die Daten des Temperaturgeräts, die dem Gerätetyp 'TempSensor' zugeordnet sind, der logischen *Schnittstelle 'Thermometer'* zugeordnet und die Daten des Feuchtigkeitsgeräts, die dem Gerätetyp *Feuchtigkeitssensor* zugeordnet sind, werden der logischen *Schnittstelle 'Hygrometer'* zugeordnet. 

Es wird dann ein 'Ding'-Typ mit der Bezeichnung *Raumtyp* gemeinsam mit zwei 'Ding'-Instanzen für den Raumtyp erstellt: *Besprechungsraum 1* und *Besprechungsraum 2*.

In diesem Szenario wird eine Zusammensetzung eingerichtet, die die logischen Schnittstellen für Thermometer und Hygrometer enthält. Die richtigen Umgebungsgeräte werden dann den einzelnen Rauminstanzen zugeordnet: Beispielsweise werden *tSensor* und *Feuchtigkeitssensor 1* dem *Besprechungsraum 1* zugeordnet.

## Voraussetzungen
{: #pre_req}

Bevor Sie fortfahren, stellen Sie Folgendes sicher:
- Verwenden Sie dieselbe {{site.data.keyword.iot_full}}-Organisationsinstanz und einen API-Schlüssel oder ein Token für die Organisation, die Sie in der schrittweisen Anleitung 1 verwendet haben. Weitere Informationen zu API-Schlüsseln und Tokens finden Sie im Abschnitt 'Authentifizierung' der Dokumentation zu [HTTP-REST-APIs für Anwendungen](../applications/api.html#authentication).
- Sie verfügen über zwei logische Schnittstellen: eine für ein Temperaturgerät und eine für ein Feuchtigkeitsgerät. Informationen zum Konfigurieren einer logischen Schnittstelle für ein Temperaturgerät finden Sie in der Dokumentation [Schrittweise Anleitung 1: Ausführliches Beispiel zur Arbeit mit Geräten über eine allgemeine Schnittstelle](../GA_information_management/ga_im_index_scenario.html#step4). Informationen zum Konfigurieren einer logischen Schnittstelle für ein Feuchtigkeitsgerät finden Sie in der Dokumentation [Zusätzliche Informationen für die schrittweise Anleitung 2: Konfiguration einer logischen Schnittstelle für ein Feuchtigkeitsgerät](im_hygrometer.html).

## Informationen zu diesem Vorgang
{: #about}

In {{site.data.keyword.iot_short_notm}} kann ein Ding aus einer Reihe von Geräten und Dingen bestehen. Ein 'Ding'-Typ definiert, wie sich 'Ding'-Instanzen zusammensetzen. 

Eine logische Schnittstelle ist einem 'Ding'-Typ zugeordnet. Diese Zuordnung definiert die Struktur des Status, der für eine Instanz des 'Ding'-Typs generiert wird. Mit Zuordnungen wird festgelegt, wie Eigenschaften von den aggregierten Geräten und Dingen zu Eigenschaften für einen 'Ding'-Status zugeordnet werden.

Durch die Verwendung einer logischen Schnittstelle entfällt für eine Anwendung die Notwendigkeit zu verstehen, wie ein Gerät oder ein Ding konfiguriert ist. Sie können beispielsweise die Temperatur eines Raumes mithilfe eines einzelnen Geräts messen oder die Raumtemperatur berechnen, indem Sie den durchschnittlichen Messwert einer Reihe von Geräten nehmen. Die Anwendung benötigt Informationen zum Status eines Raumes oder von Räumen, wobei eine Komponente eine Temperatureigenschaft ist. Dabei ist es egal, wie der von der Anwendung empfangene Temperaturwert berechnet wird.

In diesem Szenario geben zwei Temperaturgeräte und zwei Feuchtigkeitsgeräte Ereignisse an {{site.data.keyword.iot_short_notm}} aus. Ein Temperaturgerät und ein Feuchtigkeitsgerät befinden sich im Besprechungsraum 1 eines Bürogebäudes. Die anderen Temperatur- und Feuchtigkeitsgeräte befinden sich im Besprechungsraum 2. Das folgende Diagramm veranschaulicht die Konfiguration für Besprechungsraum 1:

![Zuordnung zwischen einem Temperatur- und einem Feuchtigkeits-'Ding' und einer Anwendung unter {{site.data.keyword.iot_short_notm}}.](images/Information Management Thing example scenario.svg "Zuordnung zwischen einem Temperatur- und einem Feuchtigkeits-'Ding' und einer Anwendung unter {{site.data.keyword.iot_short_notm}}")

Mit einem 'Ding'-Typ mit der Bezeichnung *Raumtyp* wird angegeben, wie sich Instanzen von Räumen zusammensetzen. Eine logische Schnittstelle ist dem *Raumtyp* zugeordnet und legt fest, dass eingehende Ereignisse einem einzelnen Messwert zugeordnet werden, der sowohl die Temperatur als auch die Feuchtigkeit anzeigt. Dieser einzelne Messwert ist der 'Ding'-Status. Mit Zuordnungen wird festgelegt, wie Eigenschaften von den Temperatur- und Feuchtigkeitsgeräten diesem 'Ding'-Status zugeordnet werden. Wenn diese Geräte einen neuen Messwert ausgeben, wird der Wert der Eigenschaft geändert, die dem 'Ding'-Status zugeordnet ist.

In der folgenden Tabelle werden die vier in diesem Beispiel verwendeten Geräte, das Topic, in dem jedes Gerät publiziert wird, sowie ein Beispiel für die Nutzdaten für die einzelnen Geräte angezeigt.

Gerät/Typ | Ereignis | Ereignisnutzdaten/Eigenschaft
------------- |  ------------- | -------------
*tSensor*/TSensor (Besprechungsraum 1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (Besprechungsraum 2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/Feuchtigkeitssensor (Besprechungsraum 1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/Feuchtigkeitssensor (Besprechungsraum 2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**Hinweis:** Die Ereignis-IDs *tevt*, *tempevt* und *humevt* sind erforderlich, wenn Sie Zuordnungen definieren, um eine Eigenschaft, die einem eingehenden Ereignis dieses Typs zugeordnet ist, als eine Eigenschaft in einer logischen Schnittstelle anzugeben. Im vorliegenden Szenario sind zwei Eigenschaften in der logischen Schnittstelle definiert: *temperature* und *humidity*.

Außerdem wird eine logische Schnittstelle konfiguriert. Die logische Schnittstelle stellt den 'Ding'-Status in der folgenden Struktur dar:
```
{
  "temperature" : <aktueller Temperaturwert in Celsius>
  "humidity" : <aktueller Feuchtigkeitswert>
}
```

Verwenden Sie das folgende Beispielszenario zum Einrichten Ihrer eigenen Schnittstellenumgebung. 

**Hinweis:** Eine Tabelle mit den Namen, Werten und Kennungen der Ressourceneigenschaften, die in diesem Handbuch verwendet werden, ist im Abschnitt mit den [Ressourceneigenschaften und Kennungen dokumentiert, die in der Dokumentation zu den schrittweisen Anleitungen 1 und 2 referenziert werden](im_id_reference.html).

## Fügen Sie bei Bedarf einen Gerätetyp und ein Gerät hinzu  
{: #add_device}  
In diesem Szenario werden drei Gerätetypen und vier Geräteinstanzen verwendet. Die Geräteinstanz *tSensor* ist dem Gerätetyp *TSensor* zugeordnet. Die Geräteinstanz *tempSensor* ist dem Gerätetyp *TempSensor* zugeordnet. Die Geräteinstanzen *Feuchtigkeitssensor 1* und *Feuchtigkeitssensor 2* sind dem Gerätetyp *Feuchtigkeitssensor* zugeordnet.

Sie können Gerätetypen und Geräte mithilfe des [{{site.data.keyword.iot_short_notm}}-Dashboards ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://internetofthings.ibmcloud.com){: new_window} oder mithilfe von REST-APIs erstellen.  

Weitere Informationen zur Verwendung des {{site.data.keyword.iot_short_notm}} IoT Platform-Dashboards zum Hinzufügen von Gerätetypen und Geräten finden Sie in der Dokumentation [Einführung zum Datenmanagement mithilfe der Webschnittstelle](../GA_information_management/im_ui_flow.html).

Informationen zur Verwendung von REST-APIs zum Hinzufügen von Gerätetypen und Geräten finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}} HTTP-REST-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window}.



## Schritt 1: Schemadatei für 'Ding'-Typ erstellen  
{: #crt_composition_file}  
Erstellen Sie eine Schemadatei für den 'Ding'-Typ, die die IDs der logischen Schnittstellen des Geräts für jeden Gerätetyp für Feuchtigkeit und Temperatur referenziert.  

Das folgende Beispiel zeigt das Erstellen einer Schemadatei eines 'Ding'-Typs mit dem Namen *roomTypeSchema*.   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON-Schema, das die Struktur des 'Ding'-Typs 'Raum' angibt.",
   "properties" : {
        "thermometer": {
            "type": "object",
            "description": "Das Thermometergerät",
            "$logicalInterfaceRef": "IThermometer"
        },
        "hygrometer": {
            "type": "object",
            "description": "Das Hygrometergerät",
            "$logicalInterfaceRef": "IHygrometer"
        }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```
**Tipp:** Verwenden Sie den Parameter **required**, um eine oder mehrere Eigenschaften als erforderlich zu markieren. Erforderliche Eigenschaften müssen in einer Gerätenachricht enthalten sein, damit Watson IoT Platform den Gerätestatus mit den Gerätedaten aktualisieren kann. Eine Nachricht, die die erforderliche Eigenschaft nicht enthält, wird nicht verarbeitet.   
**Hinweis:** Die Schema-ID, die beim Erstellen einer Schemadatei für den 'Ding'-Typ generiert wird, muss beim Erstellen des 'Ding'-Typs angegeben werden.  

## Schritt 2: Eine 'Ding'-Schema-Ressource erstellen  
{: #crt_composition_resource}  

Erstellen Sie eine Schemaressource, indem Sie die Schemadatei des 'Ding'-Typs hochladen, die im vorherigen Schritt erstellt wurde.  
Laden Sie die Schemadatei des 'Ding'-Typs mit der folgenden API hoch:  
```
POST /draft/schemas
```  
Die Schemadefinitionsdatei wird in einer mehrteiligen POST-Anforderung (multipart/form-data) an Watson IoT Platform übergeben. Der Hauptteil der POST-Anforderung muss aus mindestens zwei Teilen bestehen:

  - Ein Teil mit der Bezeichnung **schemaFile**, der den tatsächlichen Inhalt der Datei als Hauptteil des Teils enthält.
  - Ein Teil mit der Bezeichnung **name**, der eine Zeichenfolge enthält, die den Namen der Schemaressourcendatei im Hauptteil des Teils definiert.


Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).  

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer Schemaressource für einen 'Ding'-Typ:  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "name" : "roomTypeSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "roomType.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5a72ea48d60180002c4f5e58",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
Die Schema-ID *5a72ea48d60180002c4f5e58*, die als Antwort auf die POST-Methode zurückgegeben wird, ist erforderlich, wenn Sie einen 'Ding'-Typ erstellen.


## Schritt 3: 'Ding'-Typ erstellen  
{: #crt_thing_type}  

'Ding'-Typen werden zum Modellieren von 'Ding'-Instanzen verwendet. In einer Organisation muss ein 'Ding'-Typ erstellt werden, bevor eine 'Ding'-Instanz generiert werden kann. Erstellen Sie für dieses Szenario einen 'Ding'-Typ.  
Das Schema, das mit einem 'Ding'-Typ verbunden ist, definiert den Typ von Objekten, die zu einer Instanz eines Dings zusammengefasst werden. Der 'Ding'-Typ muss die Schemaressource des 'Ding'-Typs referenzieren, die Sie im vorherigen Schritt erstellt haben.  
Erstellen Sie einen 'Ding'-Typ unter Verwendung der folgenden API:
```
POST /draft/thing/types
```
Die folgenden Parameter sind im Hauptteil der POST-Methode erforderlich:  
<table>
<tr><th>Parameter</th><th>Beschreibung</th></tr>
<tr><td>id</td><td>Geben Sie eine ID für den 'Ding'-Typ an, den Sie erstellen.</td></tr>
<tr><td>name</td><td>Geben Sie einen Namen für den 'Ding'-Typ an, den Sie erstellen.</td></tr>
<tr><td>schemaId</td><td>Die für die Zusammenfassungsschemaressource erstellte ID.</td></tr>
</table>

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen eines 'Ding'-Typs namens *Raumtyp*.
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "Raumtyp", "name" : "'Ding'-Typ 'Raum'", "description" : "Ding'-Typ 'Raum'", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

Antwort: 
```
{
 "name": "RoomType",
 "description": "'Ding'-Typ 'Raum'",
 "id": "RoomType",
 "schemaId": "5a72ea48d60180002c4f5e58",
 "metadata": {},
  "refs": {
    "schema": "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58",
    "mappings": "/api/v0002/draft/thing/types/RoomType/mappings",
    "logicalInterfaces": "/api/v0002/draft/thing/types/RoomType/logicalinterfaces"
   },
 "version": "draft",
 "created": "2018-02-01T10:22:43Z",
 "createdBy": "ANOther",
 "updated": "2018-02-01T10:22:43Z",
 "updatedBy": "ANOther"
}
```

## Schritt 4: Schemadatei für logische Schnittstelle erstellen  
{: #crt_ai_schema_file}
In Ihrer logischen Schnittstelle können Sie die Struktur der Daten definieren, die als 'Ding'-Status gespeichert sind. Erstellen Sie für dieses Szenario eine logische Schnittstelle, die die Eigenschaften für Temperatur und Feuchtigkeit definiert. Verbinden Sie die logische Schnittstelle mit dem 'Ding'-Typ *Raumtyp* durch das Referenzieren der ID der logischen Schnittstelle, die beim Erstellen der Ressource der logischen Schnittstelle generiert wird.  
Das folgende Beispiel zeigt das Erstellen einer Schemadatei einer logischen Schnittstelle mit dem Namen *roomSchema*.

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON-Schema, das die kanoische Raumstatusstruktur definiert",
    "properties" : {
        "temperature" : {
            "description" : "Temperatur in Grad Celsius",
           "type" : "number",
           "minimum" : -273.15,
           "default" : 0.0
        },
       "humidity" : {
            "description" : "Prozentsatz Feuchtigkeit",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```  
## Schritt 8: Schemaressource für eine logischen Schnittstelle erstellen  
{: #crt_ai_schema_resource}  
Laden Sie die Schemadatei für die logische Schnittstelle hoch, die Sie im vorherigen Schritt erstellt haben, um eine Schemaressource der logischen Schnittstelle für Ihren 'Ding'-Typ zu erstellen. Verwenden Sie dazu die folgende API:  
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
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "roomSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "room.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5a4b9847d60180002efce645/content"
  },
  "id" : "5a4b9847d60180002efce645"
}
```
Verwenden Sie die Schema-ID *5a4b9847d60180002efce645*, die in der Antwort auf die POST-Methode zurückgegeben wird, um die Schemaressource der logischen Schnittstelle zu der logischen Schnittstelle für Ihren 'Ding'-Typ hinzuzufügen.  


## Schritt 6: Logische Schnittstelle für den 'Ding'-Typ erstellen  
{: #crt_thing_ai}  
Die logische Schnittstelle muss die Schemeressource für die logische Schnittstelle referenzieren, die Sie in dem vorherigen Schritt erstellt haben.  
Verwenden Sie die folgende API, um eine logische Schnittstelle zu erstellen:  
```
POST draft/logicalinterfaces
```  
Sie können optional einen aussagekräftigen Aliasnamen für die logische Schnittstelle angeben. Der Aliasname kann in der API-Aufruf- oder Topic-Zeichenfolgensubskription referenziert werden, die verwendet wird, um den Status eines Dings abzurufen, anstatt die automatisch generierte logische Schnittstellen-ID zu verwenden.

**Hinweis:** Der Aliasname muss zwischen 1 und 36 Zeichen lang sein und kann alphanumerische Zeichen, Bindestriche, Punkte und Unterstreichungszeichen enthalten. Der Aliasname darf keine Hexadezimalzeichenfolge mit 24 Zeichen sein.

Verwenden Sie in diesem Szenario die Schema-ID *5a4b9847d60180002efce645*, die in der vorherigen Antwort zurückgegeben wurde, um das Schema der logischen Schnittstelle zu der logischen Schnittstelle hinzuzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer logischen Schnittstelle mit dem Aliasnamen *IRoom*:
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58"
  },
  "schemaId" : "5a72ea48d60180002c4f5e58",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5a4b9847d60180002efce645",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "IRoom",
  "alias" : "IRoom",
  "version" : "draft"
}
```
Verwenden Sie in diesem Szenario die ID *5a4b9847d60180002efce645* der logischen Schnittstelle, die in der Antwort auf die POST-Methode zurückgegeben wird, um Ihre logische Schnittstelle zu Ihrem Gerätetyp hinzuzufügen. Diese ID wird auch verwendet, um ein eingehendes Geräteereignis einer Eigenschaft zuzuordnen, die von der logischen Schnittstelle definiert wird.

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).  


## Schritt 7: Logische Schnittstelle zu einem 'Ding'-Typ hinzufügen  
{: #add_thing_ai}  
Verwenden Sie die folgende API, um eine logische Schnittstelle zu einem 'Ding'-Typ hinzuzufügen:  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  
In diesem Szenario ist die logische Schnittstelle dem 'Ding'-Typ *Raumtyp* zugeordnet.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen der logischen 'Ding'-Schnittstelle *IRoom* zum 'Ding'-Typ *Raumtyp*:  
```
{   
  "id": "5a4b9847d60180002efce645"
}
```
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id": "5a4b9847d60180002efce645"}'
```

Das folgende Beispiel zeigt eine Antwort auf die POST-Methode:
```
{
 "name": "Room Logical Interface",
 "description": "Dies ist eine logische Schnittstelle des Typs 'Raum'",
 "id": "5a4b9847d60180002efce645",
 "schemaId": "5a4b9817d60180002efce644",
 "refs": {
   "schema": "/api/v0002/draft/schemas/5a4b9817d60180002efce644"
 },
 "version": "draft",
 "created": "2018-01-02T14:33:43Z",
 "createdBy": "ANOther",
 "updated": "2018-01-02T14:33:43Z",
 "updatedBy": "ANOther"
}
```

## Schritt 8: Zuordnungen definieren
{: #define_Thing_type_mappings}

Definieren Sie die Zuordnungen für den 'Ding'-Typ, die beschreiben, wie Eigenschaften vom Status der zusammengefassten Geräte oder der Dinge den Eigenschaften zugeordnet werden, die für die logische Schnittstelle des 'Ding'-Typs definiert sind.

Verwenden Sie die folgende API, um Ereignisse zuzuordnen:  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

Dabei ist *thingtypeId* die ID, die in der Antwort auf die POST-Anforderung zurückgegeben wird, wenn der 'Ding'-Typ erstellt wird. 

Im Hauptteil der POST-Anforderung sind die folgenden Parameter erforderlich:  
<table>
<tr>
<th>	Parameter	</th><th>	Beschreibung	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	Eine gültige JSON-Struktur, die für die logische Schnittstelle definierte Eigenschaften zu Eigenschaften der Ereignisnutzdaten des Geräts zuordnet. </td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	Die ID der logischen Schnittstelle ist im Hauptteil der Nutzdaten erforderlich.	</td>
</tr>
</table>  

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Hinzufügen einer Zuordnung zum 'Ding'-Typ *Raumtyp*:

```
{
  "logicalInterfaceId": "5a4b9847d60180002efce645",
  "notificationStrategy": "on-state-change",
  "propertyMappings": {
       "thermometer": {
         "temperature": "$event.temperature"
       },
       "hygrometer": {
         "humidity": "$event.humidity"
       }
   },
}
```  
Das Gerät des Typs *Thermometer* ist in [roomTypeSchema](#crt_composition_file) definiert. Die Eigenschaft *$event.temperature* ist im Schema der logischen Schnittstelle mit der ID *5846ed076522050001db0e12* und dem Alias *IThermometer* definiert.  
Das Gerät des Typs *Hygrometer* ist in [roomTypeSchema](#crt_composition_file) definiert. Die Eigenschaft *$event.Feuchtigkeit* ist im Schema der logischen Schnittstelle mit der ID *5846cd7c6522050001db0e24* und dem Aliasnamen *IHygrometer* definiert.


## Schritt 9: Konfiguration validieren und aktivieren
{: #activate}

Validieren und aktivieren Sie die Konfiguration, die sich auf die Statusaktualisierung für jeden 'Ding'-Typ bezieht. Diese Konfiguration umfasst Ihre Schemas, logischen Schnittstellen und Zuordnungen. 

Verwenden Sie die folgende API, um die Konfiguration Ihres 'Ding'-Typs zu validieren und zu aktivieren:
```
PATCH /draft/thing/types/{thingTypeId}
```
Dabei steht *thingTypeId* für die ID des 'Ding'-Typs. 

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Prüfen und Aktivieren der Konfiguration, die dem 'Ding'-Typ *Raumtyp* zugeordnet ist:
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

Das folgende Beispiel zeigt eine Antwort auf die PATCH-Methode:
```
{
 "message": "CUDIM0300I: State update configuration for Thing Type 'Room Thing Type' has been successfully submitted for  activation.",
"details": {
   "id": "CUDIM0300I",
   "properties": [
     "Thing Type",
     "Room Thing Type"
   ]
 },
 "failures": []
}
```

## Schritt 10: Instanz eines 'Ding'-Typs erstellen
{: #create_Thing_instances}

Ein 'Ding' ist eine Instanz eines 'Ding'-Typs. Mit einem Ding können Sie eine oder mehrere Instanzen eines Geräts oder Dinge in einem einzelnen Objekt zusammenfassen.

Verwenden Sie die folgende API, um ein Ding zu erstellen:

```
POST /thing/types/{thingTypeId}/things
```
Dabei ist *thingtypeId* die ID, die in der Antwort auf die POST-Anforderung zurückgegeben wird, wenn der 'Ding'-Typ erstellt wird. 

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things).

Im vorliegenden Szenario müssen zwei 'Ding'-Instanzen mit dem 'Ding'-Typ *Raumtyp* erstellt werden. Eine 'Ding'-Instanz trägt die Bezeichnung *Besprechungsraum 1* und die andere 'Ding'-Instanz die Bezeichnung *Besprechungsraum 2*.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer 'Ding'-Instanz namens *Raumtyp*. Die 'Ding'-Instanz *Besprechungsraum 1* ist den Geräteinstanzen *tSensor* und *Feuchtigkeitssensor 1* zugeordnet.

```
thingId = "Besprechungsraum 1"
 meetingroom1AggregatedObjects = {
   "thermometer": {
     "type": "Gerät",
     "typeId": "TSensor",
     "id": "tSensor"
   },
   "hygrometer": {
     "type": "Gerät",
     "typeId": "Feuchtigkeitssensor",
     "id": "Feuchtigkeitssensor 1"
   }
 }
``` 

Die erstellte 'Ding'-ID wird in der URL der POST-Methode verwendet, die aufgerufen wird, um ein Temperaturereignis zur logischen 'Ding'-Schnittstelle hinzuzufügen.

Das folgende Beispiel zeigt die Verwendung von 'cURL' zum Erstellen einer 'Ding'-Instanz namens *Besprechungsraum 2*. *Besprechungsraum 2* ist den Geräteinstanzen *tempSensor* und *Feuchtigkeitssensor 2* zugeordnet.

```
thingId = "Besprechungsraum 2"
   meetingroom2AggregatedObjects = {
    "thermometer": {
      "type": "Gerät",
      "typeId": "TempSensor",
      "id": "tempSensor"
    },
    "hygrometer": {
      "type": "Gerät",
      "typeId": "Feuchtigkeitssensor",
      "id": "Feuchtigkeitssensor 2"
    }
  }

``` 

## Schritt 11: Prüfen, dass zugeordnete Geräteereignisse in der logischen Schnittstelle publiziert werden  
{: #publish_event}  
Publizieren Sie die folgenden Ereignisse für Geräte, die in der 'Ding'-Instanz *Besprechungsraum 1* zusammengefasst werden:  
 - Ein Temperaturereignis von *tSensor* für Topic `iot-2/evt/tevt/fmt/json`  
 - Ein Feuchtigkeitsereignis von *Feuchtigkeitssensor 1* für Topic `iot-2/evt/humevt/fmt/json`  
 
Publizieren Sie die folgenden Ereignisse für Geräte, die in der 'Ding'-Instanz *Besprechungsraum 2* zusammengefasst werden:  
 - Ein Temperaturereignis von *tempSensor* für Topic `iot-2/evt/tempevt/fmt/json`  
 - Ein Feuchtigkeitsereignis von *Feuchtigkeitssensor 2* für Topic `iot-2/evt/humevt/fmt/json`  
 
Informationen zum Publizieren eines eingehenden Ereignisses von einem Gerät finden Sie in [MQTT-Konnektivität für Anwendungen](../applications/mqtt.html#publishing_device_events).  

## Schritt 12: Überprüfen, ob der 'Ding'-Status geändert wird  
{: #verify_Thing_state}  

Sie können den 'Ding'-Status entweder mithilfe von HTTP-REST-APIs oder durch Subskription eines Topics abrufen.

Wenn Sie eine MQTT-Clientanwendung benutzen, dann können Sie eine Subskription für die folgende Topic-Zeichenfolge durchführen:
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

Alternativ hierzu können Sie den aktuellsten 'Ding'-Status auch über die folgende HTTP-REST-API abrufen:

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

Folgende Parameter sind erforderlich:  
<table>
<tr>
<th>Parameter	</th><th>	Beschreibung</th>
</tr>
<tr>
<td>thingTypeId	</td><td>Die ID des 'Ding'-Typs.</td>
</tr>
<tr>
<td>thingId	</td><td>	Die 'Ding'-ID.</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>Die für die logische Schnittstelle erstellte ID.</td>
</tr>
</table>  

Weitere Informationen finden Sie in der Dokumentation zur [{{site.data.keyword.iot_short_notm}}-HTTP-REST-API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  


## Nächste Schritte

Erstellen Sie Regeln, die Sie zum Initiieren einer Aktion verwenden können, wenn ein Ereignis, das von {{site.data.keyword.iot_short_notm}} empfangen wird, eine Änderung im Geräte- oder 'Ding'-Status auslöst. Informationen zum Erstellen von Regeln finden Sie unter [Eingebettete Regeln erstellen (Beta)](im_rules.html).
