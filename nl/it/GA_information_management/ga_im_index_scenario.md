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


# Manuale passo dopo passo: un esempio dettagliato su come utilizzare i dispositivi tramite un'interfaccia comune 
{: #scenario}

Utilizza le seguenti informazioni per creare uno scenario nel quale due sensori della temperatura pubblicano gli eventi in {{site.data.keyword.iot_full}}. Un sensore misura la temperatura in gradi Celsius. L'altro sensore misura la temperatura in gradi Fahrenheit. Queste letture vengono associate a una sola lettura della temperatura che è in gradi Celsius. Quando viene pubblicata una nuova lettura della temperatura da questi dispositivi, il valore della proprietà associata con lo stato del dispositivo viene modificato.
{: shortdesc}

## Prerequisiti

Devi disporre di un'istanza dell'organizzazione {{site.data.keyword.iot_short_notm}} e un token o una chiave API per tale organizzazione.

## Informazioni su quest'attività

In questo scenario, vengono configurati due dispositivi.

Un dispositivo viene denominato *TemperatureSensor1*. Questo dispositivo pubblica gli eventi sulla temperatura misurati in gradi Celsius. L'evento sulla temperatura viene pubblicato nell'argomento `iot-2/evt/tevt/fmt/json` e ha il seguente payload di esempio:
```
{
  "t" : 34.5
}
```

**Nota:** l'identificativo dell'evento è *tevt*. Questo identificativo è obbligatorio quando aggiungi un evento sulla temperatura di questo tipo all'interfaccia fisica e quando definisci le associazioni per associare una proprietà associata a un evento in entrata di questo tipo a una proprietà nella tua interfaccia logica. In questo scenario, la proprietà definita nell'interfaccia logica viene denominata **temperature**.

L'altro dispositivo viene denominato *TemperatureSensor2*. Questo dispositivo pubblica gli eventi sulla temperatura misurati in gradi Fahrenheit. L'evento sulla temperatura viene pubblicato nell'argomento `iot-2/evt/tempevt/fmt/json` e ha il seguente payload di esempio:
```
{
  "temp" : 72.55
}
```

**Nota:** l'identificativo dell'evento è *tempevt*. Questo identificativo è obbligatorio quando aggiungi un evento sulla temperatura di questo tipo all'interfaccia fisica e quando definisci le associazioni per associare una proprietà associata a un evento in entrata di questo tipo a una proprietà nella tua interfaccia logica. In questo scenario, la proprietà definita nell'interfaccia logica viene denominata **temperature**.

Viene anche configurata un'interfaccia logica. Questa interfaccia logica rappresenta lo stato per i dispositivi di questo tipo nella seguente struttura: 
```
{
  "temperature" : <current temperature value in Celsius>
  }
```
Questa configurazione indica che puoi configurare la tua applicazione per elaborare il valore associato a **temperature**, piuttosto che configurare la tua applicazione per elaborare il valore associato a **t** e per elaborare il valore associato a **temp** dopo aver convertito questo valore in gradi Celsius.

![Associazione tra i dispositivi del sensore della temperatura e un applicazione su {{site.data.keyword.iot_short_notm}}.](images/Information)  

Utilizza il seguente scenario di esempio per configurare il tuo ambiente di interfaccia.

## Se necessario, aggiungi un tipo dispositivo e un dispositivo.
{: #step14}

In questo scenario, vengono prese in considerazione due tipi di dispositivo e due istanze del dispositivo. L'istanza del dispositivo *TemperatureSensor1* è associata con il tipo dispositivo *EnvSensor1*. L'istanza del dispositivo *TemperatureSensor2* è associata con il tipo dispositivo *EnvSensor2*.

Per informazioni sull'utilizzo delle API REST per aggiungere un tipo dispositivo, consulta la documentazione [API REST HTTP {{site.data.keyword.iot_short_notm}}](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html).

## Passo 1: crea un file dello schema evento di bozza 
{: #step1}

Per questo scenario, crea due file dello schema evento di bozza per definire la struttura di ogni evento sulla temperatura in entrata. 

Il seguente esempio mostra come creare un file dello schema di bozza denominato *tEventSchema.json*. Questo file definisce la struttura di un evento in entrata da un sensore della temperatura che misura la temperatura in gradi Celsius:

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
**Suggerimento:** usa il parametro “required” per contrassegnare una o più proprietà come obbligatorie. Le proprietà obbligatorie devono essere incluse nel messaggio di dispositivo in modo che {{site.data.keyword.iot_short_notm}} aggiorni lo stato del dispositivo con i dati del dispositivo. Un messaggio che non include la proprietà obbligatoria non viene elaborato.   

Il nome del file dello schema *tEventSchema* viene utilizzato quando creai una risorsa dello schema evento per il tuo tipo di evento.

Il seguente esempio mostra come creare un file dello schema di bozza denominato *tempEventSchema.json*. Questo file definisce la struttura di un evento in entrata da un sensore della temperatura che misura la temperatura in gradi Fahrenheit:

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
Il nome del file dello schema *tempEventSchema* viene utilizzato quando creai una risorsa dello schema evento per il tuo tipo di evento.   

## Passo 2: crea una risorsa dello schema evento di bozza per il tuo tipo di evento. 
{: #step2}

Per creare una risorsa dello schema evento, utilizza la seguente API:

```
POST /draft/schemas
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione  
------	|	-----  
name	|	Fornisci un nome per lo schema evento che stai creando.  
schemaFile	|	Percorso al file JSON dello schema evento locale.  



Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

Il seguente esempio mostra come utilizzare cURL per creare la risorsa dello schema evento *tEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

Il seguente esempio mostra una risposta al metodo POST:

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
L'identificativo dello schema *5846cd7c6522050001db0e0d* restituito nella risposta al metodo POST è obbligatorio quando aggiungi uno schema evento al tuo tipo di evento.

Il seguente esempio mostra come utilizzare cURL per creare la risorsa dello schema evento *tempEventSchema.json*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

Il seguente esempio mostra una risposta al metodo POST:

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
L'identificativo dello schema *5846cee36522050001db0e0e* restituito nella risposta al metodo POST è obbligatorio quando aggiungi uno schema evento al tuo tipo di evento.

## Passo 3: crea un tipo di evento di bozza che fa riferimento allo schema evento 
{: #step3}

Ogni tipo di evento fa riferimento allo schema evento pertinente che è stato creato nel precedente esempio utilizzando l'identificativo dello schema restituito nella risposta al metodo POST utilizzato per creare la risorsa dello schema evento.

Per creare un tipo di evento di bozza, utilizza la seguente API: 

```
POST /draft/event/types
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione
------	|	-----
name	|	Fornisci un nome per il tipo di evento che stai creando.
schemaId	|	L'identificativo creato per la risorsa di schema evento.


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


Il seguente esempio mostra come utilizzare cURL per creare un tipo di evento per un evento sulla temperatura misurato in gradi Celsius:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

L'identificativo dello schema *5846cd7c6522050001db0e0d* vine utilizzato per aggiungere lo schema evento al tipo di evento. Questo identificativo era stato restituito nella risposta al metodo POST che era stato utilizzato per creare la risorsa dello schema evento *tEventSchema.json*

Il seguente esempio mostra una risposta al metodo POST:

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

L'identificativo del tipo di evento *5846d0fd6522050001db0e0f* restituito nella risposta al metodo POST viene utilizzato per aggiungere un tipo di evento all'interfaccia fisica.

Il seguente esempio mostra come utilizzare cURL per creare un tipo di evento per un evento sulla temperatura misurato in gradi Fahrenheit:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
L'identificativo dello schema *5846cee36522050001db0e0e* vine utilizzato per aggiungere lo schema evento al tipo di evento. Questo identificativo era stato restituito nella risposta al metodo POST che era stato utilizzato per creare la risorsa dello schema evento *tempEventSchema.json*

Il seguente esempio mostra una risposta al metodo POST:

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
L'identificativo del tipo di evento *5846d2846522050001db0e10* restituito nella risposta al metodo POST viene utilizzato per aggiungere un tipo di evento all'interfaccia fisica.

## Passo 4: crea un'interfaccia fisica di bozza 
{: #step7}

Per creare un'interfaccia fisica di bozza, utilizza la seguente API: 

```
POST /draft/physicalinterfaces
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione
------	|	-----
name	|	Fornisci un nome per l'interfaccia fisica che stai creando.


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

In questo scenario, abbiamo bisogno di due interfacce fisiche - una per ogni tipo di evento.

Il seguente esempio mostra come utilizzare cURL per creare la prima interfaccia fisica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Env sensor physical interface 1"}'
```

Il seguente esempio mostra una risposta al metodo POST:

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

L'identificativo dell'interfaccia fisica *5847d1df6522050001db0e1a* restituito nella risposta viene utilizzato nell'URL del metodo POST richiamato per aggiungere un evento sulla temperatura misurato in gradi Celsius all'interfaccia fisica.

l seguente esempio mostra come utilizzare cURL per creare la seconda interfaccia fisica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Env sensor physical interface 2"}'
```

Il seguente esempio mostra una risposta al metodo POST:

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

L'identificativo dell'interfaccia fisica *5847d1df6522050001db0e1b* restituito nella risposta viene utilizzato nell'URL del metodo POST richiamato per aggiungere un evento sulla temperatura misurato in gradi Fahrenheit all'interfaccia fisica.   

## Passo 5: aggiungi il tipo di evento all'interfaccia fisica di bozza 
{: #step8}

Per aggiungere un tipo di evento all'interfaccia fisica, utilizza la seguente API:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione
------	|	-----
eventId	|	Immetti il nome evento dal payload di evento di dispositivo.
eventTypeId	|	L'identificativo creato per il tipo di evento. 


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

In questo scenario, vengono aggiunti i seguenti tipi di evento alle interfacce fisiche specificate:
- l'evento sulla temperatura Celsius *tevt* viene aggiunto all'interfaccia fisica con l'identificativo *5847d1df6522050001db0e1a* utilizzando il *eventId* dall'argomento e il *eventTypeId* dalla creazione della risorsa dello schema evento.
- l'evento sulla temperatura Fahrenheit *tempevt* viene aggiunto all'interfaccia fisica con l'identificativo *5847d1df6522050001db0e1b* utilizzando il *eventId* dall'argomento e il *eventTypeId* dalla creazione della risorsa dello schema evento.


Il seguente esempio mostra come utilizzare cURL per aggiungere l'evento sulla temperatura *tevt* all'interfaccia fisica con l'identificativo *5847d1df6522050001db0e1a* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

Il seguente esempio mostra una risposta al metodo POST:

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

Il seguente esempio mostra come utilizzare cURL per aggiungere l'evento sulla temperatura *tempevt* all'interfaccia fisica con l'identificativo *5847d1df6522050001db0e1b* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

Il seguente esempio mostra una risposta al metodo POST:

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
}
```

## Passo 6: aggiorna il tipo dispositivo di bozza per la connessione dell'interfaccia fisica di bozza 
{: #step9}

Per aggiornare un tipo dispositivo di bozza, utilizza la seguente API: 

```
POST /draft/device/types/{typeId}/physicalinterface
```

dove *typeId* è l'identificativo del tipo di dispositivo.


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In questo scenario, il tipo dispositivo *EnvSensor1* per collegarsi all'interfaccia fisica *5847d1df6522050001db0e1a* e il tipo dispositivo *EnvSensor2* per collegarsi all'interfaccia fisica *5847d1df6522050001db0e1b*.

Il seguente esempio mostra come utilizzare cURL per aggiornare il tipo dispositivo *EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

Il seguente esempio mostra una risposta al metodo POST:
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
L'identificativo del dispositivo *EnvSensor1* è obbligatorio quando aggiungi la tua interfaccia fisica e la tua interfaccia logica.

Il seguente esempio mostra come utilizzare cURL per aggiornare il tipo dispositivo *EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

Il seguente esempio mostra una risposta al metodo POST:

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
L'identificativo del dispositivo *EnvSensor2* è obbligatorio quando aggiungi la tua interfaccia fisica e la tua interfaccia logica.



## Passo 7: crea un file dello schema dell'interfaccia logica di bozza 
{: #step4}

Il seguente esempio mostra come creare un file dello schema dell'interfaccia logica di bozza denominato *envSensor.json*.

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

## Passo 8: crea una risorsa dello schema dell'interfaccia logica di bozza 
{: #step5}

Per creare una risorsa dello schema dell'interfaccia logica di bozza, utilizza la seguente API: 

```
POST /draft/schemas
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione
------	|	-----
name	|	Fornisci un nome per lo schema dell'interfaccia logica che stai creando. 
schemaFile	|	Percorso al file JSON dello schema dell'interfaccia logica locale. 


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

Il seguente esempio mostra come utilizzare cURL per creare lo schema dell'interfaccia logica: 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=temperatureEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/envSensor.json"'
```

Il seguente esempio mostra una risposta al metodo POST:

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
Utilizza l'identificativo dello schema *5846ec826522050001db0e11* restituito nella risposta al metodo POST per aggiungere lo schema dell'interfaccia logica all'interfaccia logica.

## Passo 9: crea un'interfaccia logica di bozza che faccia riferimento a uno schema dell'interfaccia logica di bozza.
{: #step6}

Per creare un'interfaccia logica di bozza, utilizza la seguente API: 

```
POST /draft/logicalinterfaces
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione
------	|	-----
name	|	Fornisci un nome per l'interfaccia logica che stai creando. 
schemaId	|	L'identificativo creato per la risorsa dello schema dell'interfaccia logica. 


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

In questo scenario, utilizza l'identificativo dello schema *5846ec826522050001db0e11* restituito nella precedente risposta per aggiungere lo schema dell'interfaccia logica all'interfaccia logica.

Il seguente esempio mostra come utilizzare cURL per creare un'interfaccia logica: 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "environment sensor interface", "schemaId" : "5846ec826522050001db0e11"}'
```

Il seguente esempio mostra una risposta al metodo POST:

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
In questo scenario, utilizza l'identificativo dell'interfaccia logica *5846ed076522050001db0e12* restituito nella risposta al metodo POST per aggiungere l'interfaccia logica al tuo tipo dispositivo. Puoi anche utilizzare questo identificativo per associare un evento del dispositivo in entrata a una proprietà definita dall'interfaccia logica.

## Passo 10: aggiungi l'interfaccia logica di bozza a un tipo dispositivo 
{: #step10}

Per aggiungere un'interfaccia logica di bozza a un tipo dispositivo, utilizza la seguente API: 

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

dove *typeId* è il nome del tipo di dispositivo. 

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).  
**Nota:** in questo scenario, la stessa interfaccia logica *5846ed076522050001db0e12* viene associata a *EnvSensor1* e *EnvSensor2*.

Il seguente esempio mostra come utilizzare cURL per aggiungere l'interfaccia logica *5846ed076522050001db0e12* che fa riferimento all'identificativo dello schema dell'interfaccia logica *5846ec826522050001db0e11* al tipo di dispositivo *EnvSensor1*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

Il seguente esempio mostra una risposta al metodo POST:

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

Il seguente esempio mostra come utilizzare cURL per aggiungere l'interfaccia logica *5846ed076522050001db0e12* associata all'identificativo dello schema logico *5846ec826522050001db0e11* al tipo di dispositivo *EnvSensor2*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

Il seguente esempio mostra una risposta al metodo POST:

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

## Passo 11: definisci le associazioni per associare le proprietà nell'evento in entrata alle proprietà nell'interfaccia logica 
{: #step11}  
**Importante:** se stai utilizzando un'applicazione client MQTT e desideri sottoscriverti per ricevere le notifiche sugli aggiornamenti dello stato del dispositivo, devi configurare le impostazioni delle notifiche quando definisci le associazioni. Configurando la tua strategia di notifica in questo modo, puoi riutilizzare un'interfaccia logica con più tipi di dispositivo, ognuno dei quali configurato per ricevere le notifiche sulla modifica dello stato in un modo di tua scelta. La strategia di notifica può essere definita indipendentemente se un client MQTT si sottoscrive alla stringa dell'argomento specificata.  
  
Possono essere configurate le seguenti impostazioni di notifica:  

Parametro	|	Descrizione
------	|	-----
never	|	Non viene inviata alcuna notifica - questa è l'impostazione predefinita
on-every-event	|	Le notifiche sono inviate ad ogni evento, indipendentemente se lo stato del dispositivo è stato modificato.
on-state-change	|	Le notifiche sono inviate solo quando un evento provoca un cambiamento dello stato del dispositivo. 

**Nota:** se selezioni un'impostazione di notifica di *never*, la tua applicazione non riceverà mai una notifica su un cambiamento nello stato del dispositivo. Pertanto, potresti voler scegliere una strategia di notifica di *on-every-event* o *on-state-change*.  

Per associare gli eventi, utilizza la seguente API:

```
POST /draft/device/types/{typeId}/mappings
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione
------	|	-----
logicalInterfaceId	|	L'identificativo creato per l'interfaccia logica.  
propertyMappings	|	Una struttura JSON valida che associa le proprietà definite per l'interfaccia logica alle proprietà del payload di evento di dispositivo. 
Puoi facoltativamente impostare il parametro *notificationStrategy*.  

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In questo scenario, definiamo le associazioni al tipo dispositivo *EnvSensor1* per associare la proprietà **t** nell'evento in entrata *tevt* alla proprietà **temperature** nell'interfaccia logica. Definiamo inoltre le associazioni al tipo dispositivo *EnvSensor2* per associare la proprietà **temp** nell'evento in entrata *tempevt* alla proprietà **temperature** nell'interfaccia logica. Le impostazioni di notifica sono impostate su "on-state-change". 

Il seguente esempio mostra come utilizzare cURL per aggiungere un'associazione al tipo dispositivo *EnvSensor1*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Importante:** per i payload di evento [JSON ![Icona di link esterno](../../../icons/launch-glyph.svg "Icona di link esterno")](http://json-schema.org/){:new_window} strutturati, accertati che la voce $event.*property* corrisponda correttamente alla struttura JSON delle tue proprietà. Ad esempio, se il payload è `{"d":{"t":<temp>}}` utilizza `$event.d.t`

Specifica l'identificativo dell'interfaccia logica dell'applicazione *5846ed076522050001db0e12* restituito nella risposta al metodo POST utilizzato per creare l'interfaccia logica e il tipo di dispositivo *EnvSensor1*.

Il seguente esempio mostra una risposta al metodo POST:

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
Il seguente esempio mostra come utilizzare cURL per aggiungere un'associazione al tipo dispositivo *EnvSensor2*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Specifica l'identificativo dell'interfaccia logica dell'applicazione *5846ed076522050001db0e12* restituito nella risposta al metodo POST utilizzato per creare l'interfaccia logica e il tipo di dispositivo *EnvSensor2*.
Viene applicata una conversione per modificare il valore da una misurazione in gradi Fahrenheit a una in gradi Celsius.


Il seguente esempio mostra una risposta al metodo POST:

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

## Passo 12: convalida e attiva la configurazione
{: #step15}

Convalida e attiva la configurazione correlata all'aggiornamento dello stato del dispositivo per ogni tipo dispositivo di bozza. Questa configurazione include i tuoi schemi di bozza, tipi di evento, interfacce fisiche, interfacce logiche e associazioni. 

Per convalidare la tua configurazione del tipo dispositivo di bozza, utilizza la seguente API: 

```
PATCH /draft/device/types/{typeId}
```

dove *typeId* è l'identificativo del tipo di dispositivo.

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In questo scenario, abbiamo bisogno di convalidare e attivare la configurazione per due tipi di dispositivo di bozza. 

Il seguente esempio mostra come utilizzare cURL per convalidare e attivare la tua configurazione per il tipo dispositivo di bozza *EnvSensor1*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

Il seguente esempio mostra una risposta al metodo PATCH:

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
Il seguente esempio mostra come utilizzare cURL per convalidare e attivare la tua configurazione per il tipo dispositivo di bozza *EnvSensor2*:

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
Il seguente esempio mostra una risposta al metodo PATCH:

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

## Passo 13: pubblica un evento del dispositivo in entrata
{: #step12}

Pubblica un evento sulla temperatura da *TemperatureSensor1* nell'argomento `iot-2/evt/tevt/fmt/json` con il seguente payload di esempio:
```
{
  "t" : 34.5
}
```
Pubblica un evento sulla temperatura da *TemperatureSensor2* nell'argomento `iot-2/evt/tempevt/fmt/json` con il seguente payload di esempio:
```
{
  "temp" : 72.55
}
```

Per informazioni sulla pubblicazione di un evento in entrata da un dispositivo, consulta [Connettività MQTT per le applicazioni](../applications/mqtt.html#publishing_device_events).


## Passo 14: richiama lo stato del dispositivo
{: #step13}

Puoi richiamare lo stato del dispositivo utilizzando le API REST HTTP o sottoscrivendo un argomento.

- Se disponi di un'applicazione client MQTT, puoi sottoscriverti alla seguente stringa dell'argomento:  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- In alternativa, puoi richiamare l'ultimo stato del dispositivo utilizzando la seguente API REST HTTP:  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

I seguenti parametri sono obbligatori:  

Parametro	|	Descrizione
------	|	-----
typeId	|	L'identificativo del tipo di dispositivo.
deviceId	|	L'identificativo del dispositivo. 
logicalInterfaceId	|	L'identificativo creato per l'interfaccia logica. 

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Il seguente esempio mostra come utilizzare cURL per richiamare lo stato corrente di *TemperatureSensor1* facendo riferimento all'identificativo dell'interfaccia logica che è stata creata: 
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor1/devices/TemperatureSensor1/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

L'identificativo dell'interfaccia logica *5846ed076522050001db0e12* viene utilizzato nel metodo GET. Questo identificativo viene restituito nella risposta al metodo POST utilizzato per creare l'interfaccia logica. Il seguente esempio mostra una risposta al metodo GET:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
  }
}
```
Il seguente esempio mostra come utilizzare cURL per richiamare lo stato corrente di *TemperatureSensor2* facendo riferimento all'identificativo dell'interfaccia logica che è stata creata:
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor2/devices/TemperatureSensor2/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

L'identificativo dell'interfaccia logica *5846ed076522050001db0e12* viene utilizzato nel metodo GET. Questo identificativo viene restituito nella risposta al metodo POST utilizzato per creare l'interfaccia logica.
Il seguente esempio mostra una risposta al metodo GET:
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
  }
}
```
Tieni presente che la lettura sulla temperatura che viene restituita è in gradi Celsius e non in Fahrenheit.

La tua applicazione può elaborare questi dati normalizzati senza richiedere la configurazione per comprendere o convertire le scale di temperatura differenti.


