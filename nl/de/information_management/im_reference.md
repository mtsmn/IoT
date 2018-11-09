---

copyright:
years: 2017, 2018
lastupdated: "2018-03-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Referenzinformationen zum Datenmanagement

Verwenden Sie die folgenden Referenzinformationen, um sich über die Einschränkungen zu informieren, die bei der Verwendung der Datenmanagementkomponente von {{site.data.keyword.iot_full}} gelten. 

## Schemaeinschränkungen

Beim Datenmanagement werden die folgenden Schematypen verwendet:
- Schema für Ereignistypen
- Schema für logische Schnittstellen
- Schema für 'Ding'-Typen

In der folgenden Tabelle sind die Einschränkungen aufgelistet, die für die einzelnen Schematypen gelten:

Schematyp         |        Einschränkung
------------------- | -------------
Alle       | Alle Schemas müssen dem gültigen JSON-Schema entsprechen. 
Alle       | Alle Schemas müssen dem gültigen JSON-Schema entsprechen. 
Alle       | Das Stammverzeichnis aller Schemas muss vom Typ 'object' sein. 
Alle       | Alle Schemas dürfen maximal sieben Verschachtelungsebenen aufweisen.
Logische Schnittstelle        |  Für alle bei Bedarf angegebenen Eigenschaften muss ein Standardwert definiert sein. 
Logische Schnittstelle     | Wenn ein Objekt im Schema definiert ist, muss mindestens eine Eigenschaft für dieses Objekt angegeben sein. 
Logische Schnittstelle | Wenn ein Array im Schema definiert ist, dürfen zugeordnete Elemente nur einen Typ aufweisen, z. B. nur den Typ 'string'. 
'Ding'-Typ        | Es sind nur Eigenschaften der höchsten Ebene zulässig. Über die erste Ebene hinweg ist keine Verschachtelung zulässig. 
'Ding'-Typ        | Die Eigenschaft der höchsten Ebene muss den Typ 'object' aufweisen.
'Ding'-Typ        | Die Eigenschaft der höchsten Ebene muss '$logicalInterfaceRef' und einen Typ referenzieren. Der Wert für '$logicalInterfaceRef' ist die ID oder der Aliasname einer logischen Schnittstelle. Der Typ muss auf 'object' oder 'array' gesetzt sein. 

## Beispiele für gültige und ungültige Schemas

### Beispiel 1: Gültige Definition eines Schemas der logischen Schnittstelle
Im folgenden Beispiel wird ein Schema der logischen Schnittstelle definiert, das den Vorgaben entspricht, die im Abschnitt mit den Schemaeinschränkungen aufgeführt sind:

  - Das Schema entspricht einem gültigen JSON-Format und einem gültigen JSON-Schema.
  - Das Schema hat den Typ 'object'.
  - Das Schema weist weniger als sieben Verschachtelungsebenen auf. 
  - Für das Schema sind zwei Eigenschaften definiert. 
  - Für die erforderlichen Eigenschaften **a** und **b** wurden Standardwerte angegeben.

```
{
 "type": "object",
 "properties":{
    "a":{
       "type":"string"
    },
    "b":{
       "type": "number"
      }
  },
  "default":{"a":"HelloWorld", "b":2},
  "required": ["a", "b"]
}
```


### Beispiel 2: Ungültige Definition eines Schemas der logischen Schnittstelle
Das folgende Beispiel zeigt ein ungültiges Schema der logischen Schnittstelle. Das Schema ist ungültig, weil das Array so definiert ist, dass es Elemente des Typs 'string' oder 'number' enthält. Wenn ein Array im Schema definiert ist, dürfen zugeordnete Elemente nur einen Typ aufweisen, z. B. nur den Typ 'string'.

```
{
 "type": "object",
 "properties":{
    "a":{
      "type":"array",
       "items":{
          "type": ["string", "number"]
       }
    }
  }
}
```
### Beispiel 3: Gültige Definition eines Schemas des 'Ding'-Typs
Im folgenden Beispiel wird ein Schema des 'Ding'-Typs definiert, das den Vorgaben entspricht, die im Abschnitt mit den Schemaeinschränkungen aufgeführt sind:

  - Das Schema entspricht einem gültigen JSON-Format und einem gültigen JSON-Schema.
  - Das Schema hat den Typ 'object'.
  - Das Schema weist weniger als sieben Verschachtelungsebenen auf. 
  - Das Schema definiert nur die Eigenschaften der Ausgangsebene. 
  - Die Eigenschaften der Ausgangsebene verweisen auf '$logicalInterfaceRef' und einen Typ, der entweder auf 'array' oder 'object' gesetzt ist. Der Typ 'array' kann verwendet werden, um eine Reihe von Geräten oder Dingen zusammenzufassen, z. B. eine Reihe von Temperatursensoren. Der Typ 'object' kann verwendet werden, um ein einzelnes Gerät oder Ding zu referenzieren, z. B. einen einzelnen Feuchtigkeitssensor.   
  - Für die erforderlichen Eigenschaften muss kein Standardwert angegeben werden. Diese Einschränkung gilt nur für Schemas der logischen Schnittstelle. In diesem Schematyp kann kein Standardwert angegeben werden. 

```
{
   "type" : "object",
   "properties" : {
       "temperatureSensors": {
           "description": "Zusammengefasste Temperatursensoren",
           "$logicalInterfaceRef": "IThermometer",
           "type" : "array"
       },
       "humiditySensor": {
           "description": "Der Feuchtigkeitssensor",
           "$logicalInterfaceRef": "5846cd7c6522050001db0e24"
            "type" : "object"
       }
   },
   "required" : [
       "temperatureSensors",
       "humiditySensor"
   ]
  }
```
