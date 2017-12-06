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

# Informazioni sul linguaggio dell'espressione di associazione
{: #mapping_expression}

Puoi utilizzare il linguaggio dell'espressione di associazione per modificare e combinare i dati e per formattare i risultati di tutte le query che puoi eseguire sui dati elaborati. Il linguaggio dell'espressione di associazione è un sottoinsieme di [JSONata ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://docs.jsonata.org/index.html){:new_window} e può essere utilizzato nella definizione delle [associazioni](ga_im_definitions.html#definitions_resources). JSONata è un linguaggio di trasformazione e query semplice per i dati JSON.

Le seguenti informazioni mostrano le funzioni e gli operatori chiave al momento supportati, insieme ad alcuni esempi su come puoi utilizzarli. 

## Operatori JSONata supportati
{: #operators}

Il seguente sottoinsieme di operatori JSONata sono supportati: 

Tipo di operatore| Operatori supportati | Note   
------------- | ------------- | -------------
Aritmetico | *+* - / * % | L'operatore % restituisce il resto.
Confronto | < <= > >= != = | L'operatore di uguaglianza è = , ed è presente in JSONata.
Booleano| *in*, *and* | Le costanti booleane sono *true* o *false*.
Ternario condizionale | ? | L'operatore ? valuta una delle due espressioni alternative in base al risultato di una condizione di test. L'operatore ha la seguente *espressione* di modulo  ? *value_if_true* : *value_if_false*
Stringa | & | L'operatore & unisce i valori della stringa degli operandi in una sola stringa risultante.
Generatore di sequenze | .. | Crea un array di numeri interi incrementali, iniziando con il numero nel LHS e finendo con il numero nel RHS, ad esempio, [1..4] -> [1,2,3,4]. Gli operandi devono essere un numero intero. Il generatore di sequenze può essere utilizzato solo in un constructor array [].
Altro | . | L'operatore punto viene utilizzato per l'accesso all'oggetto con una chiave letterale, ad esempio $event.object.hh. *
e:* l'espressione al lato sinistro è limitata a una proprietà specifica, nell'evento ($event) o nello stato ($state) o nei metadati ($instance).

**Note:** 
- Utilizza le parentesi ( ) per raggruppare le espressioni e per modificare la precedenza dell'operatore.
- Utilizza le virgolette singole per racchiudere i nomi della proprietà che contengono degli spazi, ad esempio $event.object.'a b' 

## Estensione del linguaggio dell'espressione di associazione 

Il linguaggio dell'espressione di associazione è stato esteso per l'utilizzo con la funzione di gestione dei dati tramite l'introduzione delle variabili $event, $state e $instance. JSON viene associato a queste variabili prima che venga valutata l'espressione. La seguente tabella fornisce una panoramica di queste variabili, definite per l'utilizzo nelle espressioni:

Variabile                   | Input di esempio JSON     | Esempio come un'espressione  | Utilizzata per ...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Seleziona una proprietà di un evento associato da utilizzare in un'espressione.
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Seleziona un proprietà nello stato del dispositivo da utilizzare in un'espressione.
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Seleziona l'attributo del dispositivo o del tipo di dispositivo da utilizzare in un'espressione.

Puoi anche definire un'espressione che combini l'utilizzo di queste variabili. Nel seguente esempio, viene definita una proprietà *temp_adjustment* nei metadati del dispositivo e viene utilizzata per calibrare la lettura dell'evento. La proprietà viene definita in un'associazione a può essere applicata a molti dispositivi. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## Funzioni JSONata supportate
{: #functions}
Il seguente sottoinsieme di funzioni JSONata sono supportate:  

Tipo di funzione                   |Funzione                   | Descrizione | Esempio
------------- | ------------- | ------------- 
Stringa | $substring(string, start_index[, length]) | Sottostringa stringa, ad esempio *$substring("Hello World", 3, 5) => "lo Wo"*. 
Stringa | $string(arg) | Mette l'argomento in un valore della stringa, ad esempio, *$string(2) => "2"*.
Valore numerico | $number(arg) | Mette l'argomento in un valore numerico se possibile, ad esempio, *$number(2) => 2*.
Valore numerico | $sum(array) | Restituisce la somma aritmetica di un array di numeri, ad esempio, *([1,2,3]) = 6*.
Valore numerico | $average(array) | Restituisce il valore medio di un array di numeri, ad esempio, *([1,2,3]) = 2.0*.
Booleano| $exists(expression) | Restituisce *true* se la proprietà nell'espressione esiste, altrimenti *false*.
Array | $count(array) | Restituisce il numero di elementi nel parametro dell'array, ad esempio, *([1,2,3,4]) = 4*. Se il parametro dell'array non è un array, ma piuttosto un valore di un altro tipo di JSON, il parametro viene considerato come un array singleton contenente tale valore e questa funzione restituisce *1*.
Array | $append(array1, array2) | Restituisce un array contenente i valori in *array1*, seguiti dai valori in *array2*, ad esempio, *([1,2], [3,4]) = [1,2,3,4]*. Se entrambi i parametri non sono un array, viene considerato come un array singleton contenente tale valore, ad esempio *$append("Hello", "World") => ["Hello", "World"]*.

## Array
Utilizza gli array JSON per mettere una raccolta di valori in un ordine specifico. Gli array associano ogni valore nell'array a un indice o posizione. Per posizionare valori individuali in un array, devi specificare l'indice utilizzando le parentesi quadre dopo il nome del campo dell'array. Se le parentesi quadre contengono un numero o un'espressione che viene valutata come un numero, tale numero rappresenta l'indice del valore da selezionare. Un array di numeri può anche essere utilizzato come un indice, ad esempio *["a","b","c"][[1,2]] -> ["b", "c"]*. L'array *[1,2]* viene utilizzato come un indice che identifica quali valori selezionare dall'array *["a","b","c"]*. 

Gli indici hanno un offset uguale a zero, per cui il primo valore in un array *arr* è *arr[0]*, ad esempio, *[1,2,3][0] -> 1*. Se il numero non è un numero intero, viene arrotondato a un numero intero, ad esempio *[1,2,3][0.9] -> [1]*.

Utilizza gli indici negativi per il conteggio dalla fine dell'array, ad esempio, *[1,2,3][-1] -> 3*. 

Se l'indice specificato supera la dimensione dell'array, non viene selezionato nulla.

## Creazione dell'output
Puoi specificare come i dati elaborati sono presentati nell'output utilizzando i constructor dell'array o dell'oggetto.

### Constructor dell'array supportati
Gli array possono essere creati includendo un elenco separato da virgole di valori letterali o espressioni in una coppia di parentesi quadre []. Le virgole sono utilizzate per separare più espressioni nel constructor dell'array, ad esempio, *[1, 3-1] = [1,2]*.

### Constructor dell'oggetto supportati
{: #constructors}
Puoi creare gli oggetti JSON nel tuo output utilizzando una coppia di parentesi graffe {} che contengono i valori chiave o le coppie separate
da virgole, con ogni chiave e valore separati da due punti, ad esempio, *{key1 : value1, key2:value2}* or *{"hello" : "world"}*. La chiave dell'oggetto deve essere una stringa.  


## Esempio: utilizza gli array per elaborare e riportare i dati della temperatura
Le seguenti sezioni create nell'esempio in [Introduzione alla gestione dei dati](ga_im_example.html) mostrano come puoi utilizzare gli array per mantenere una finestra scorrevole di letture della temperatura e calcolare la somma corrente o la media delle letture contenute in tale finestra. 

Una finestra scorrevole archivia i dati nell'ordine di arrivo. Anziché conservare tutti i dati inseriti, le finestre scorrevoli possono essere configurate per eliminare i dati in modo incrementale. Quando una finestra scorrevole si riempe, tutte le seguenti inserzioni provocano l'eliminazione degli elementi dei dati più vecchi in tale finestra.

Il seguente esempio mostra una finestra scorrevole configurata con una politica di eliminazione basata sul numero della dimensione di 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

Il seguente esempio mostra come la configurazione del file dello schema dell'interfaccia logica illustrata nel passo 7 del [Manuale passo dopo passo](ga_im_index_scenario.html#step4), possa essere modificata aggiungendo un array denominato *tempReadings*. L'array viene utilizzato per mantenere una finestra di almeno 5 valori inviati dai dispositivi per gli ultimi 5 eventi. Se non sono presenti valori archiviati in *tempReadings*, l'array viene impostato su [0] e la successiva lettura ricevuta viene accodata all'array che cresce fino a 5 letture ricevute. Dopo 5 letture ricevute, una nuova lettura provoca la rimozione della lettura più vecchia dalla finestra. 

**Note importanti:** devi impostare *tempReadings* come "obbligatorio" e come un array per impostazione predefinita. 

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
**Nota:** l'array *$state.tempReadings* viene ricalcolato prima di essere utilizzato nelle funzioni $average e $sum. Il ricalcolo dell'array è necessario per garantire che l'array contenga i valori correnti quando l'espressione *tempAverage* o *tempSum* viene valutata, perché l'ordine delle espressioni di associazione non può essere controllato.

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

