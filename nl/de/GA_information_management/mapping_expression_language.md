---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-09"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Erklärung zur Zuordnungsausdruckssprache
{: #mapping_expression}

Sie können die Zuordnungsausdruckssprache verwenden, um Daten zu bearbeiten und zu kombinieren und um die Ergebnisse von Abfragen zu formatieren, die für die von Ihnen verarbeiteten Daten ausgeführt werden können. Die Zuordnungsausdruckssprache stellt eine Untergruppe von [JSONata ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.jsonata.org/index.html){:new_window} dar und kann beim Definieren der [Zuordnungen](ga_im_definitions.html#definitions_resources) verwendet werden. JSONata stellt eine Lightweight-Abfrage- und -Transformationssprache für JSON-Daten dar.

In den folgenden Informationen werden die Schlüsseloperatoren und Funktionen dargestellt, die momentan unterstützt werden. Außerdem werden einige Beispiele zu deren Verwendung aufgeführt. 

## Unterstützte JSONata-Operatoren
{: #operators}

Die folgende Untergruppe der JSONata-Operatoren wird unterstützt: 

Typ des Operators                  | Unterstützte Operatoren | Hinweise   
------------- | ------------- | -------------
Arithmetische Operatoren | *+* - / * % | Mit dem Operator % werden die verbleibenden Daten zurückgegeben.
Vergleichsoperatoren| < <= > >= != = | Der Operator für Gleichheit ist = , wie in JSONata.
Boolesche Operatoren | *in*, *and* | Die booleschen Konstanten sind *true* und *false*.
Bedingte ternäre Operatoren | ? | Der Operator '?' wertet einen von zwei alternativen Ausdrücken auf Basis des Ergebnisses eine Testbedingung aus. Der Operator hat das Format *expression*  ? *wert_wenn_true* : *wert_wenn_false*.
Zeichenfolge | & | Der Operator & verknüpft die Zeichenfolgewerte der Operanden zu einer einzigen Ergebniszeichenfolge.
Sequenzgenerator | .. | Erstellt einen Array ansteigender Ganzzahlen. Dabei wird mit der Zahl auf dem LHS begonnen und mit der Zahl auf dem RHS geendet. Beispiel: [1..4] -> [1,2,3,4]. Die Operanden müssen mit einer Ganzzahl ausgewertet werden. Der Sequenzgenerator kann nur in einem Array-Konstruktor [] verwendet werden.
Andere | . | Der Punktoperator wird für den Objektzugriff mit einem Literalschlüssel verwendet. Beispiel: $event.object.hh. *
e:* Der Ausdruck auf der linken Seite ist auf eine bestimmte Eigenschaft beschränkt, entweder im Ereignis ($event), im Status ($state) oder in den Metadaten ($instance).

**Hinweise:** 
- Verwenden Sie runde Klammern ( ) zur Gruppierung von Ausdrücken und zum Ändern der Operatorpriorität.
- Verwenden Sie einfache Anführungszeichen, um Eigenschaftsnamen einzuschließen, die Leerzeichen enthalten. Beispiel: $event.object.'a b' 

## Zuordnungsausdruckssprache erweitern

Die Zuordnungsausdruckssprache wurde durch die Einführung der Variablen '$event', '$state' und '$instance' ergänzt und kann nun zusammen mit der Funktion für das Datenmanagement verwendet werden. JSON wird an diese Variablen gebunden, bevor der Ausdruck ausgewertet wird. In der folgenden Tabelle erhalten Sie einen Überblick über diese Variablen, die zur Verwendung in Ausdrücken definiert sind.

Variable                   | Beispieleingabe - JSON | Beispiel als Ausdruck       | Verwendungszweck   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Wählen Sie eine Eigenschaft eines eingehenden Ereignisses zur Verwendung in einem Ausdruck aus. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Wählen Sie eine Eigenschaft zum Gerätestatus zur Verwendung in einem Ausdruck aus. 
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Wählen Sie das Attribut für das Gerät oder den Gerätetyp zur Verwendung in einem Ausdruck aus. 

Sie können auch einen Ausdruck definieren, in dem diese Variablen gemeinsam benutzt werden. Im folgenden Beispiel ist eine Eigenschaft *temp_adjustment* in den Gerätemetadaten definiert. Sie wird dann zum Kalibrieren der Ereignismesswerte verwendet. Die Eigenschaft ist in einer Zuordnung definiert, kann jedoch auf viele Geräte angewendet werden.  

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## Unterstützte JSONata-Funktionen
{: #functions}
Die folgende Untergruppe der JSONata-Funktionen wird unterstützt: 

Typ der Funktion                   |Funktion                   | Beschreibung | Beispiel
------------- | ------------- | ------------- 
Zeichenfolge | $substring(string, start_index[, length]) | Eine Teilzeichenfolge der Zeichenfolge. Beispiel: *$substring("Hello World", 3, 5) => "lo Wo"*. 
Zeichenfolge | $string(arg) | Setzt das Argument in einen Zeichenfolgewert um. Beispiel: *$string(2) => "2"*.
Numerisch | $number(arg) | Setzt das Argument in einen numerischen Wert um. Beispiel: *$number(2) => 2*.
Numerisch | $sum(array) | Gibt die arithmetische Summe eines Arrays mit Zahlen zurück. Beispiel : *([1,2,3]) = 6*.
Numerisch | $average(array) | Gibt den Mittelwert eines Arrays mit Zahlen zurück. Beispiel : *([1,2,3]) = 2.0*.
Boolesch| $exists(expression) | Gibt *true* zurück, wenn die Eigenschaft im Ausdruck vorhanden ist. Andernfalls wird *false* zurückgegeben.
Array | $count(array) | Gibt die Anzahl der Elemente im Parameter 'array' zurück. Beispiel: *([1,2,3,4]) = 4*. Wenn es sich beim Parameter 'array' nicht um einen Array handelt, sondern um einen Wert mit einem anderen JSON-Typ, dann wird der Parameter als Singleton-Array behandelt, der diesen Wert enthält. Diese Funktion gibt dann *1* zurück.
Array | $append(array1, array2) | Gibt einen Array zurück, der die Werte in *array1* gefolgt von den Werten in *array2* enthält. Beispiel: *([1,2], [3,4]) = [1,2,3,4]*. Wenn keiner der Parameter ein Array ist, wird er als Singleton-Array behandelt, der diesen Wert enthält. Beispiel: *$append("Hello", "World") => ["Hello", "World"]*.

## Arrays
Verwenden Sie JSON-Arrays, um eine Wertesammlung in eine bestimmte Reihenfolge zu bringen. Arrays ordnen jeden Wert im Array einem Index oder einer Position zu. Um die einzelnen Werte in einem Array zu adressieren, müssen Sie den Index angeben. Verwenden Sie hierzu eckige Klammern nach dem Feldnamen des Arrays. Wenn die eckigen Klammern eine Zahl enthalten oder aber einen Ausdruck, der als Zahl ausgewertet wird, dann stellt die Zahl den Index des auszuwählenden Werts dar. Ein Array mit Zahlen kann ebenfalls als Index verwendet werden. Beispiel: *["a","b","c"][[1,2]] -> ["b", "c"]*. Der Array *[1,2]* wird als Index verwendet, der angibt, welche Werte aus dem Array *["a","b","c"]* ausgewählt werden sollen. 

Indizes weisen ein Nulloffset auf, sodass der erste Wert im Array *arr* *arr[0]* lautet. Beispiel: *[1,2,3][0] -> 1*. Wenn es sich bei der Zahl nicht um eine Ganzzahl handelt, wird sie auf eine Ganzzahl abgerundet. Beispiel: *[1,2,3][0.9] -> [1]*.

Verwenden Sie negative Indizes, um vom Ende eines Arrays zu zählen. Beispiel: *[1,2,3][-1] -> 3*. 

Wenn der angegebene Index die Größe des Arrays überschreitet, wird keine Auswahl getroffen.

## Ausgaben erstellen
Sie können angeben, wie verarbeitete Daten in der Ausgabe dargestellt werden sollen. Verwenden Sie hierzu die Array-Konstruktoren oder Objektkonstruktoren.

### Unterstützte Array-Konstruktoren
Arrays können konstruiert werden, indem eine durch Kommas getrennte Liste mit Literalen oder Ausdrücken in eckige Klammern ([]) eingeschlossen wird. Kommas werden verwendet, um mehrere Ausdrücke innerhalb des Array-Konstruktors zu trennen. Beispiel: *[1, 3-1] = [1,2]*.

### Unterstützte Objektkonstruktoren
{: #constructors}
Sie können JSON-Objekte in Ihrer Ausgabe erstellen, indem Sie ein Paar geschweifter Klammern ({}) verwenden, die Schlüsselwerte oder durch Kommas getrennte Wertepaare enthalten. Dabei muss jeder Schlüssel und jeder Wert durch einen Doppelpunkt vom jeweils anderen Wert getrennt werden. Beispiel: *{key1 : value1, key2:value2}* oder *{"hello" : "world"}*. Der Objektschlüssel muss eine Zeichenfolge sein.  


## Beispiel: Arrays zur Verarbeitung und Meldung von Temperaturdaten verwenden
Die folgenden Abschnitte basieren auf dem Beispiel in [Einführung zum Datenmanagement](ga_im_example.html). Hier wird erläutert, wie Sie Arrays verwenden können, um ein gleitendes Fenster mit Temperaturmesswerten zu verwalten, und wie Sie die aktuelle Summe oder den aktuellen Durchschnitt der Messwerte in diesem Fenster berechnen können. 

Ein gleitendes Fenster dient zur Speicherung von Daten in der Reihenfolge ihres Eingangs. Anstatt alle jemals eingefügten Daten beizubehalten, können gleitende Fenster so konfiguriert werden, dass Daten in inkrementeller Vorgehensweise entfernt werden. Wenn ein gleitendes Fenster sich der maximalen Füllung nähert, dann führt jede weitere Einfügung zur Entfernung des jeweils ältesten Datenelements im Fenster.

Im folgenden Beispiel wird ein gleitendes Fenster gezeigt, das mit einer zählerbasierten Bereinigungsrichtlinie der Größe 5 konfiguriert wurde:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

Im folgenden Beispiel wird gezeigt, wie die Konfiguration der Schemadatei der logischen Schnittstelle, die in Schritt 7 der [schrittweisen Anleitung](ga_im_index_scenario.html#step4) gezeigt wird, geändert werden und wie dabei ein Array mit dem Namen *tempReadings* hinzugefügt werden kann. Der Array wird zur Verwaltung eines Fensters der letzten 5 Werte benutzt, die von Geräten für die letzten 5 Ereignisse gesendet wurden. Wenn in *tempReadings* keine Werte gespeichert sind, dann wird der Array auf [0] gesetzt und der nächste empfangene Messwert wird an den Array angefügt, der weiter wächst, bis 5 Messwerte empfangen wurden. Nach Empfang von 5 Messwerten führt ein weiterer neuer Messwert zur Entfernung des ältesten Messwerts aus dem Fenster. 

**Wichtige Hinweise:** Sie müssen für *tempReadings* die Einstellung "required" festlegen und als Standard einen Array definieren. 

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
**Hinweis:** Der Array *$state.tempReadings* wird erneut berechnet, bevor er in den Funktionen '$average' und '$sum' verwendet wird. Die erneute Berechnung des Arrays ist erforderlich, um sicherzustellen, dass der Array die aktuellen Werte enthält, wenn der Ausdruck *tempAverage* oder *tempSum* ausgewertet wird, weil die Reihenfolge der Zuordnungsausdrücke nicht gesteuert werden kann.

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

