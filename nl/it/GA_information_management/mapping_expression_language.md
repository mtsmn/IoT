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

# Informazioni sul linguaggio dell'espressione di associazione
{: #mapping_expression}

Puoi utilizzare il linguaggio dell'espressione di associazione per modificare e combinare i dati e per formattare i risultati di tutte le query che potresti eseguire sui tuoi dati elaborati. Il linguaggio dell'espressione di associazione è un sottoinsieme di [JSONata ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/index.html){:new_window} e può essere utilizzato quando si definiscono le [associazioni](ga_im_definitions.html#resources) o si creano le [regole](../information_management/im_rules.html). 

JSONata è un linguaggio di trasformazione e query semplice per i dati JSON. È inoltre disponibile uno strumento [JSONata Exerciser ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://try.jsonata.org/){:new_window} che fornisce un modo rapido e appropriato per provare JSONata.

Le seguenti informazioni mostrano le funzioni e gli operatori chiave al momento supportati, insieme ad alcuni esempi su come puoi utilizzarli. 

## Operatori JSONata
{: #operators}

Sono supportati tutti gli [Operatori JSONata ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/operators.html){:new_window}, ad eccezione dei seguenti:

- ~> (concatenamento delle funzioni)
- ^(…) (modalità di ordinamento)
- := (bind di variabili)

**Nota:** 
- Utilizza le parentesi ( ) per raggruppare le espressioni e per modificare la precedenza dell'operatore.
- Utilizza virgolette singole o doppie per racchiudere stringhe e nomi di proprietà, ad esempio $event.object.'ab' 
- Utilizza i backtick per racchiudere i nomi delle proprietà che contengono caratteri speciali, inclusi gli spazi, ad esempio 
```
{"x y":22}.`x y`
```
- Utilizza i backtick per racchiudere i nomi delle proprietà che iniziano con un numero, ad esempio
```
`7emperature`
```

## Funzioni JSONata
{: #functions}
Sono supportate le seguenti funzioni JSONata: 

 - Tutte le funzioni [Stringa ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/string-functions.html){:new_window}
 - Tutte le funzioni [Valore numerico ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/numeric-functions.html){:new_window} 	
 - Tutte le funzioni [Aggregazione numerica ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 
 - Tutte le funzioni [Valore booleano ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/boolean-functions.html){:new_window}  
 - Funzioni [Array $count e $append ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/array-functions.html){:new_window} 	

## Estensione del linguaggio dell'espressione di associazione

Il linguaggio dell'espressione di associazione è stato esteso per l'utilizzo con la funzione di gestione dei dati tramite l'introduzione delle variabili $event, $state e $instance. JSON viene associato a queste variabili prima che venga valutata l'espressione. La seguente tabella fornisce una panoramica di queste variabili, definite per l'utilizzo nelle espressioni:

Variabile                   | Input di esempio JSON     | Esempio come un'espressione    | Utilizzata per ...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Seleziona una proprietà di un evento associato da utilizzare in un'espressione. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Seleziona un proprietà nello stato del dispositivo da utilizzare in un'espressione.
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Seleziona l'attributo del dispositivo o del tipo di dispositivo da utilizzare in un'espressione. 

Il seguente esempio utilizza il seguente oggetto come input per un evento:  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
L'oggetto viene convertito in un array utilizzando la seguente espressione:

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
L'espressione comporta la generazione del seguente output:
```
 [
    35,
    72,
    1024
  ]
```

Puoi invertire questo esempio e convertire un array dall'input a un oggetto. Il seguente esempio utilizza il seguente array come input per un evento:
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
L'array viene convertito in un oggetto utilizzando la seguente espressione:
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
 L'espressione comporta la generazione del seguente output:
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
Puoi anche definire un'espressione che combini l'utilizzo di queste variabili. Nel seguente esempio, viene definita una proprietà *temp_adjustment* nei metadati del dispositivo e viene utilizzata per calibrare la lettura dell'evento. La proprietà viene definita in un'associazione a può essere applicata a molti dispositivi. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


L'operatore punto "." viene utilizzato per l'accesso all'oggetto con una chiave letterale, ad esempio $event.object.hh. L'espressione sul lato sinistro è vincolata a una proprietà specifica, nell'evento ($event), nello stato ($state) o nei metadati ($instance). L'espressione sul lato destro contiene le informazioni a cui potresti voler accedere.

Per ulteriori informazioni sull'operatore punto, consulta la sezione [Operatori ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/operators.html){:new_window} della documentazione di JSONata.


## Guida per i linguaggi

- È supportata tutta la [Selezione di base ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/basic.html){:new_window}.
- È supportata tutta la [Selezione complessa ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/complex.html){:new_window}, ad eccezione dei caratteri jolly.
- Le espressioni condizionali e tra parentesi sono supportate come parte di [Costrutti di programmazione ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/programming.html){:new_window}. Le variabili sono supportate attraverso l'utilizzo delle variabili $instance, $state e $event come parte del linguaggio di associazione esteso, specifico per la gestione dei dati di Watson IoT Platform. Le variabili "$" e "$$" utilizzate in JSONata non sono attualmente supportate.

## Costruzione dell'output
Puoi specificare in che modo i dati elaborati vengono presentati nell'output utilizzando i constructor di array o i constructor di oggetti.

### Constructor di array supportati
Gli array possono essere costruiti racchiudendo un elenco di valori letterali o espressioni separati da virgole in una coppia di parentesi quadre []. Le virgole vengono utilizzate per separare più espressioni all'interno del constructor di array, inclusa la generazione della sequenza, ad esempio, *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* e *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### Constructor di oggetti supportati
{: #constructors}
Puoi costruire oggetti JSON nel tuo output usando una coppia di parentesi graffe {} che contengono valori chiave o coppie separate
da virgole, con ogni chiave e valore separati da due punti, ad esempio, *{key1 : value1, key2:value2}* o *{"hello" : "world"}*. La chiave dell'oggetto deve essere una stringa.  


## Esempio: utilizzo degli array per elaborare e riportare dati di temperatura
Le seguenti sezioni si basano sull'esempio in [Introduzione alla gestione dei dati utilizzando le API REST](ga_im_example.html) per mostrare come potresti utilizzare gli array per mantenere una finestra scorrevole delle letture della temperatura e calcolare la somma o la media corrente della lettura contenuta in quella finestra.

Una finestra scorrevole archivia i dati nell'ordine di arrivo. Anziché conservare tutti i dati inseriti, le finestre scorrevoli possono essere configurate per eliminare i dati in modo incrementale. Quando una finestra scorrevole si riempe, tutte le seguenti inserzioni provocano l'eliminazione degli elementi dei dati più vecchi in tale finestra.

Il seguente esempio mostra una finestra scorrevole configurata con una politica di eliminazione basata sul numero della dimensione di 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

Il seguente esempio mostra come la configurazione del file dello schema dell'interfaccia logica illustrata nel passo 7 della [Guida passo a passo 1](ga_im_index_scenario.html#step4) possa essere modificata aggiungendo un array denominato *tempReadings*. L'array viene utilizzato per mantenere una finestra di almeno 5 valori inviati dai dispositivi per gli ultimi 5 eventi. Se non ci sono valori memorizzati in *tempReadings*, l'array viene impostato su [0] e la successiva lettura ricevuta viene aggiunta all'array che cresce fino a quando non vengono ricevute 5 letture. Dopo 5 letture ricevute, una nuova lettura provoca la rimozione della lettura più vecchia dalla finestra. 

**Note importanti:** devi impostare *tempReadings* come "required" e come un array per impostazione predefinita.

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
La seguente sezione mostra un esempio di come puoi configurare le risorse delle associazioni per calcolare la lettura della temperatura media e la somma di tutte le letture della temperatura basate sulle letture contenute nella finestra scorrevole corrente:


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
**Nota:** l'array *$state.tempReadings* viene ricalcolato prima di essere utilizzato nelle funzioni $average e $sum. Il ricalcolo dell'array è necessario per garantire che l'array contenga i valori correnti quando viene valutata l'espressione *tempAverage* o *tempSum*, perché l'ordine delle espressioni di associazione non può essere controllato.

Il seguente esempio mostra la temperatura media e la temperatura sommata basate su una finestra scorrevole di 5 letture della temperatura:
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

## Gestione delle mancate corrispondenze tra l'espressione di associazione e i dati di input

Un aggiornamento dello stato potrebbe non riuscire se una delle espressioni di associazione contiene un riferimento ai dati di input che non sono specificati nell'evento pubblicato.

Ad esempio, potresti configurare le seguenti espressioni:
```
temperature = $event.t 
humidity = $event.hum
```
dove *t* è una proprietà facoltativa per l'evento.

Se viene ricevuto un evento che contiene solo dati di umidità, ad esempio, `{"humidity":22}`, la valutazione dell'espressione `temperature = $event.t` ha esito negativo perché la proprietà *t* facoltativa non è specificata nell'evento del dispositivo pubblicato.

L'aggiornamento dello stato non riesce. La proprietà dello stato dell'umidità non viene aggiornata e viene pubblicato un messaggio di errore sull'argomento di errore MQTT per il dispositivo:
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
Per evitare errori di aggiornamento dello stato a causa di dati facoltativi non specificati, puoi utilizzare la funzione $exists in combinazione con un condizionale ternario per specificare un valore predefinito per le proprietà facoltative. Il seguente esempio definisce un valore predefinito di *0* per la proprietà *t*:

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

Definendo un valore predefinito per la proprietà facoltativa in questo modo, l'espressione viene valutata correttamente anche quando la proprietà *t* non viene specificata nell'evento del dispositivo pubblicato.

Pertanto, se si riceve l'evento `{"humidity":22}`, l'aggiornamento dello stato viene completato correttamente e lo stato del dispositivo viene impostato su `{"humidity":22, "temperature":0}`.
