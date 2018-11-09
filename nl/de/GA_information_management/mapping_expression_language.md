---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Erklärung zur Zuordnungsausdruckssprache
{: #mapping_expression}

Sie können die Zuordnungsausdruckssprache verwenden, um Daten zu bearbeiten und zu kombinieren und um die Ergebnisse von Abfragen zu formatieren, die für die von Ihnen verarbeiteten Daten ausgeführt werden können. Die Zuordnungsausdruckssprache stellt eine Untergruppe von [JSONata ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/index.html){:new_window} dar und kann beim Definieren der [Zuordnungen](ga_im_definitions.html#resources) oder beim Erstellen von [Regeln](../information_management/im_rules.html) verwendet werden. 

JSONata stellt eine Lightweight-Abfrage- und -Transformationssprache für JSON-Daten dar. Das Tool [JSONata Exerciser ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://try.jsonata.org/){:new_window} ist ebenfalls verfügbar und bietet eine schnelle und komfortable Möglichkeit, JSONata auszuprobieren.

In den folgenden Informationen werden die Schlüsseloperatoren und Funktionen dargestellt, die momentan unterstützt werden. Außerdem werden einige Beispiele zu deren Verwendung aufgeführt. 

## JSONata-Operatoren
{: #operators}

Mit Ausnahme der folgenden Operatoren werden alle [JSONata-Operatoren ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/operators.html){:new_window} unterstützt:

- ~> (Funktionskette)
- ^(…) (order-by)
- := (Variablenbindung)

**Hinweis:** 
- Verwenden Sie runde Klammern ( ) zur Gruppierung von Ausdrücken und zum Ändern der Operatorpriorität.
- Verwenden Sie einfache oder doppelte Anführungszeichen zum Einschließen von Zeichenfolgen und Eigenschaftsnamen. Beispiel: $event.object.'ab'. 
- Verwenden Sie Backticks (`), um Eigenschaftsnamen, die Sonderzeichen, einschließlich Leerzeichen, enthalten, zu verwenden. Beispiel: 
```
{"x y":22}.`x y`
```
- Verwenden Sie Backticks, um Eigenschaftsnamen zu umgeben, die mit einer Zahl beginnen. Beispiel:
```
`7emperature`
```

## JSONata-Funktionen
{: #functions}
Die folgenden JSONata-Funktionen werden unterstützt: 

 - Alle [Zeichenfolgenfunktionen ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/string-functions.html){:new_window}
 - Alle [numerischen Funktionen ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/numeric-functions.html){:new_window} 	
 - Alle [numerischen Aggregationsfunktionen ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 
 - Alle [booleschen Funktionen ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/boolean-functions.html){:new_window}  
 - [Array-Funktionen des Typ '$count' und '$append' ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/array-functions.html){:new_window} 	

## Zuordnungsausdruckssprache erweitern

Die Zuordnungsausdruckssprache wurde durch die Einführung der Variablen '$event', '$state' und '$instance' ergänzt und kann nun zusammen mit der Funktion für das Datenmanagement verwendet werden. JSON wird an diese Variablen gebunden, bevor der Ausdruck ausgewertet wird. In der folgenden Tabelle erhalten Sie einen Überblick über diese Variablen, die zur Verwendung in Ausdrücken definiert sind.

Variable                   | Beispieleingabe - JSON     | Beispiel als Ausdruck    | Verwendungszweck   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Wählen Sie eine Eigenschaft eines eingehenden Ereignisses zur Verwendung in einem Ausdruck aus. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Wählen Sie eine Eigenschaft zum Gerätestatus zur Verwendung in einem Ausdruck aus.
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Wählen Sie das Attribut für das Gerät oder den Gerätetyp zur Verwendung in einem Ausdruck aus. 

Im folgenden Beispiel wird das folgende Objekt als Eingabe für ein Ereignis verwendet:  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
Das Objekt wird mithilfe des folgenden Ausdrucks in ein Array konvertiert:

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
Der Ausdruck führt zur folgenden Ausgabe:
```
 [
    35,
    72,
    1024
  ]
```

Sie können dieses Beispiel umkehren und ein Array von der Eingabe in ein Objekt konvertieren. Im folgenden Beispiel wird das folgende Array als Eingabe für ein Ereignis verwendet:
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
Das Array wird mithilfe des folgenden Ausdrucks in ein Objekt konvertiert:
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
 Der Ausdruck führt zur folgenden Ausgabe:
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
Sie können auch einen Ausdruck definieren, in dem diese Variablen gemeinsam benutzt werden. Im folgenden Beispiel ist eine Eigenschaft *temp_adjustment* in den Gerätemetadaten definiert. Sie wird dann zum Kalibrieren der Ereignismesswerte verwendet. Die Eigenschaft ist in einer Zuordnung definiert, kann jedoch auf viele Geräte angewendet werden. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


Der Punktoperator "." wird für den Objektzugriff mit einem Literalschlüssel verwendet. Beispiel: $event.object.hh. Der Ausdruck auf der linken Seite ist auf eine bestimmte Eigenschaft beschränkt, entweder im Ereignis ($event) oder im Status ($state) oder in den Metadaten ($instance). Der Ausdruck auf der rechten Seite enthält die Informationen, auf die Sie zugreifen möchten.

Weitere Informationen zum Punktoperator finden Sie im Abschnitt zu den [Operatoren ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/operators.html){:new_window} in der JSONata-Dokumentation.


## Sprachleitfaden

- Alle Elemente der [Basisauswahl (Basic Selection) ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/basic.html){:new_window} werden unterstützt.
- Alle Elemente der [komplexen Auswahl (Complex Selection) ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/complex.html){:new_window} werden unterstützt (außer Platzhalter).
- Bedingte Ausdrücke und Klammerausdrücke werden als Teil von [Programmierkonstrukten ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/programming.html){:new_window} unterstützt. Variablen werden durch die Verwendung der Variablen '$instance', '$state' und '$event' als Teil der erweiterten Zuordnungssprache unterstützt, die für das Datenmanagement von Watson IoT Platform spezifisch ist. Die Variablen "$" und "$$", die in JSONata verwendet werden, werden derzeit nicht unterstützt.

## Ausgabe erstellen
Sie können angeben, wie verarbeitete Daten in der Ausgabe dargestellt werden sollen. Verwenden Sie hierzu die Array-Konstruktoren oder Objektkonstruktoren.

### Unterstützte Array-Konstruktoren
Arrays können konstruiert werden, indem eine durch Kommas getrennte Liste mit Literalen oder Ausdrücken in eckige Klammern ([]) eingeschlossen wird. Kommas werden verwendet, um mehrere Ausdrücke innerhalb des Array-Konstruktors, einschließlich Sequenzgenerierung, zu trennen. Beispiel: *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* und *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### Unterstützte Objektkonstruktoren
{: #constructors}
Sie können JSON-Objekte in Ihrer Ausgabe erstellen, indem Sie ein Paar geschweifter Klammern ({}) verwenden, die Schlüsselwerte oder durch Kommas getrennte
Wertepaare enthalten. Dabei muss jeder Schlüssel und jeder Wert durch einen Doppelpunkt vom jeweils anderen Wert getrennt werden. Beispiel: *{key1 : value1, key2:value2}* or *{"hello" : "world"}*. Der Objektschlüssel muss eine Zeichenfolge sein.  


## Beispiel: Temperaturdaten mithilfe von Arrays verarbeiten und melden
Die folgenden Abschnitte basieren auf dem Beispiel in [Einführung zum Datenmanagement mithilfe von REST-APIs](ga_im_example.html). Hier wird erläutert, wie Sie Arrays verwenden können, um ein gleitendes Fenster mit Temperaturmesswerten zu verwalten, und wie Sie die aktuelle Summe oder den aktuellen Durchschnitt der Messwerte in diesem Fenster berechnen können.

Ein gleitendes Fenster dient zur Speicherung von Daten in der Reihenfolge ihres Eingangs. Anstatt alle jemals eingefügten Daten beizubehalten, können gleitende Fenster so konfiguriert werden, dass Daten in inkrementeller Vorgehensweise entfernt werden. Wenn ein gleitendes Fenster sich der maximalen Füllung nähert, dann führt jede weitere Einfügung zur Entfernung des jeweils ältesten Datenelements im Fenster.

Im folgenden Beispiel wird ein gleitendes Fenster gezeigt, das mit einer zählerbasierten Bereinigungsrichtlinie der Größe 5 konfiguriert wurde:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

Im folgenden Beispiel wird gezeigt, wie die Konfiguration der Schemadatei der logischen Schnittstelle, die in Schritt 7 der [schrittweisen Anleitung 1](ga_im_index_scenario.html#step4) gezeigt wird, geändert werden und wie dabei ein Array mit dem Namen *tempReadings* hinzugefügt werden kann. Der Array wird zur Verwaltung eines Fensters der letzten 5 Werte benutzt, die von Geräten für die letzten 5 Ereignisse gesendet wurden. Wenn in *tempReadings* keine Werte gespeichert sind, dann wird der Array auf [0] gesetzt und der nächste empfangene Messwert wird an den Array angefügt, der weiter wächst, bis 5 Messwerte empfangen wurden. Nach Empfang von 5 Messwerten führt ein weiterer neuer Messwert zur Entfernung des ältesten Messwerts aus dem Fenster. 

**Wichtige Hinweise:** Sie müssen für *tempReadings* die Einstellung 'required' festlegen und als Standard ein Array definieren.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature" : {
          "description" : "latest temperature reading in degrees Celsius",
          "type" : "number",
          "minimum" : -273.15,
          "default" : 0.0
      },
      "tempAverage": {
          "type": "number"
      },
      "tempReadings": {
          "default": [],
          "items": {
              "type": "number"
          },
         "type": "array"
      },
      "tempSum": {
          "type": "number"
      }
  },
   "required" : [
      "tempReadings"
  ],
  "type": "object"
}
```
Im folgenden Abschnitt wird ein Beispiel zur Vorgehensweise bei der Konfiguration der Zuordnungsressource zur Berechnung der durchschnittlichen Temperaturmesswerte gezeigt. Außerdem wird die Summe aller Temperaturmesswerte auf Basis der Messwerte dargestellt, die im aktuellen gleitenden Fenster enthalten sind:


```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt" : {
               "tempAverage": "$average($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))",
               "tempReadings": "$count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t)",
               "tempSum": "$sum($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))"
           }
       },
       "updated": "2017-10-13T10:05:40Z",
       "updatedBy": "a-8x7nmj-9iqt56kfil",
       "version": "active"
   }
]
```
**Hinweis:** Das Array *$state.tempReadings* wird erneut berechnet, bevor es in den Funktionen '$average' und '$sum' verwendet wird. Die Neuberechnung des Arrays ist erforderlich, um sicherzustellen, dass das Array die aktuellen Werte enthält, wenn der Ausdruck *tempAverage* oder *tempSum* ausgewertet wird, weil die Reihenfolge der Zuordnungsausdrücke nicht gesteuert werden kann.

Das folgende Beispiel zeigt die durchschnittliche Temperatur und die summierte Temperatur auf Basis eines gleitenden Fensters von 5 Temperaturmesswerten:
```
{
    "state": {
        "tempAverage": 18557.6,
        "tempReadings": [
            17591,
            10262,
            25621,
            16676,
            22638
        ],
        "tempSum": 92788
    },
    "timestamp": "2017-10-13T11:07:20Z",
    "updated": "2017-10-13T11:05:40Z"
}
```

## Handhabung von Diskrepanzen zwischen Zuordnungsausdrücken und Eingabedaten

Eine Statusaktualisierung kann fehlschlagen, wenn einer der Zuordnungsausdrücke einen Verweis auf Eingabedaten enthält, die in dem publizierten Ereignis nicht angegeben sind.

Sie können zum Beispiel die folgenden Ausdrücke konfigurieren:
```
temperature = $event.t 
humidity = $event.hum
```
Dabei ist *t* eine optionale Eigenschaft für das Ereignis.

Wenn ein Ereignis, das nur Feuchtigkeitsdaten enthält, empfangen wird, z. B. `{"humidity":22}`, kann der Ausdruck `temperature = $event.t` nicht ausgewertet werden, weil die optionale Eigenschaft *t* nicht im publizierten Geräteereignis angegeben ist.

Die Statusaktualisierung schlägt fehl. Die Eigenschaft für den Feuchtigkeitsstatus wird nicht aktualisiert und im MQTT-Fehlertopic für das Gerät wird eine Fehlernachricht publiziert:
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
Um zu verhindern, dass Statusaktualisierungsfehler aufgrund von nicht angegebenen optionalen Daten auftreten, können Sie die Funktion '$exists' in Kombination mit einem ternären Zustand verwenden, um einen Standardwert für optionale Eigenschaften anzugeben. Im folgenden Beispiel wird der Standardwert *0* für die Eigenschaft *t* definiert:

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

Durch die Definition eines Standardwerts für die optionale Eigenschaft auf diese Weise wird der Ausdruck erfolgreich ausgewertet, auch wenn die Eigenschaft *t* im publizierten Geräteereignis nicht angegeben ist.

Wenn das Ereignis `{"humidity":22}` empfangen wird, wird die Statusaktualisierung daher erfolgreich abgeschlossen und der Gerätestatus wird auf `{"humidity":22, "temperature":0}` festgelegt.
