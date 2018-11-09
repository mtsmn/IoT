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

# Informazioni di riferimento sulla gestione dei dati

Utilizza le seguenti informazioni di riferimento per conoscere le limitazioni applicabili quando si utilizza il componente di gestione dati di {{site.data.keyword.iot_full}}. 

## Limiti dello schema

Nella gestione dei dati vengono utilizzati i seguenti tipi di schema:
- Schema del tipo di evento
- Schema dell'interfaccia logica
- Schema del tipo di oggetto

La seguente tabella elenca le limitazioni che si applicano a ciascun tipo di schema:

Tipo di schema       |        Restrizione
------------------- | -------------
Tutti        | Tutti gli schemi devono essere conformi a un JSON valido. 
Tutti        | Tutti gli schemi devono essere conformi a uno schema JSON valido. 
Tutti        | La root di tutti gli schemi deve essere di tipo "oggetto". 
Tutti        | Tutti gli schemi hanno un massimo di sette livelli di nidificazione.
Interfaccia logica        |  Tutte le proprietà specificate come obbligatorie devono avere un valore predefinito specificato. 
Interfaccia logica     | Se nello schema è definito un oggetto, deve essere definita almeno una proprietà per quell'oggetto. 
Interfaccia logica | Se nello schema è definito un array, gli elementi associati devono essere di un solo tipo, ad esempio solo di tipo "stringa".
Tipo di oggetto        | Sono consentite solo le proprietà di livello superiore. Non è consentita alcuna nidificazione oltre il primo livello. 
Tipo di oggetto        | La proprietà di livello superiore deve essere di tipo "oggetto".
Tipo di oggetto        | La proprietà di livello superiore deve fare riferimento a un $logicalInterfaceRef e a un tipo. Il valore di $logicalInterfaceRef è l'identificativo o il nome alias di un'interfaccia logica. Il tipo deve essere impostato su "oggetto" o "array". 

## Esempi di schemi validi e non validi

### Esempio 1 - definizione di uno schema di interfaccia logica valido
Il seguente esempio definisce uno schema di interfaccia logica conforme alle restrizioni descritte nella sezione "Limiti dello schema":

  - Lo schema è conforme a un JSON valido e a uno schema JSON valido.
  - Lo schema è di tipo "oggetto".
  - Lo schema ha un livello di nidificazione inferiore a sette. 
  - Per lo schema sono definite due proprietà. 
  - Le proprietà obbligatorie **a** e **b** hanno i valori predefiniti specificati.

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


### Esempio 2 - definizione di uno schema di interfaccia logica non valido
Il seguente esempio mostra uno schema di interfaccia logica non valido. Lo schema non è valido perché l'array è definito per contenere elementi di tipo "stringa" o di tipo "numero". Se nello schema è definito un array, gli elementi associati devono essere di un solo tipo, ad esempio "stringa".

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
### Esempio 3 - definizione di uno schema del tipo di oggetto valido
Il seguente esempio definisce uno schema del tipo di oggetto conforme alle restrizioni descritte nella sezione "Limiti dello schema":

  - Lo schema è conforme a un JSON valido e a uno schema JSON valido.
  - Lo schema è di tipo "oggetto".
  - Lo schema ha un livello di nidificazione inferiore a sette. 
  - Lo schema definisce solo proprietà di livello superiore. 
  - Le proprietà di livello superiore fanno riferimento a un $logicalInterfaceRef e a un tipo impostato su "array" o su "oggetto". Il tipo "array" può essere utilizzato per aggregare una serie di dispositivi o Oggetti, ad esempio una serie di sensori di temperatura. Il tipo "oggetto" può essere utilizzato per fare riferimento a un singolo dispositivo o oggetto, ad esempio un singolo sensore di umidità.   
  - Le proprietà obbligatorie non richiedono un valore predefinito da specificare. Questa restrizione si applica solo agli schemi dell'interfaccia logica. Non è possibile specificare un valore predefinito per questo tipo di schema. 

```
{
   "type" : "object",
   "properties" : {
       "temperatureSensors": {
           "description": "Aggregated temperature sensors",
           "$logicalInterfaceRef": "IThermometer",
           "type" : "array"
       },
       "humiditySensor": {
           "description": "The humidity sensor device",
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
