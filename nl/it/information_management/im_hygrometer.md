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

# Informazioni aggiuntive per la guida passo a passo 2 - configurazione di un'interfaccia logica per un dispositivo di umidità

Utilizza le seguenti informazioni per creare uno scenario in cui i sensori su due dispositivi di umidità pubblicano eventi su IBM Watson™ IoT Platform.  Le letture dai dispositivi di umidità sono associate a una singola lettura di umidità. Un dispositivo si trova nella sala riunioni 1 e l'altro dispositivo si trova nella sala riunioni 2.


## Informazioni su quest'attività

Utilizza la stessa istanza dell'organizzazione {{site.data.keyword.iot_short_notm}} e un token o una chiave API per tale organizzazione che hai utilizzato nella [Guida passo a passo 1](../GA_information_management/ga_im_index_scenario.html). 

Questa configurazione è un prerequisito per l'esercitazione descritta nella documentazione [Guida passo a passo 2](im_index_scenario_thing.html).

In questo scenario, vengono creati un tipo di dispositivo e due istanze del dispositivo.

Le istanze del dispositivo sono denominate *humiditySensor1* e *humiditySensor2*. I dispositivi pubblicano gli eventi di umidità. L'evento di umidità ha il seguente payload di esempio:
```
{
  "hum" : 35
}
```
Gli eventi di umidità vengono pubblicati sull'argomento `iot-2/evt/humevt/fmt/json`. 

**Nota:** l'identificativo dell'evento è *humevt*. Questo identificativo è obbligatorio quando aggiungi un evento di umidità di questi tipi all'interfaccia fisica e quando definisci le associazioni per associare una proprietà associata a un evento in entrata di questo tipo a una proprietà nella tua interfaccia logica. In questo scenario, la proprietà definita nell'interfaccia logica viene denominata **humidity**. Questa interfaccia logica rappresenta lo stato per i dispositivi di questo tipo nella seguente struttura:
```
{
  "humidity" : <current humidity value in Celsius>
}
```

Utilizza il seguente scenario di esempio per configurare il tuo ambiente di interfaccia.

**Nota importante:** devi salvare gli ID che vengono generati nelle risposte curl poiché gli ID sono necessari per completare altre attività.

## Se necessario, aggiungi un tipo di dispositivo e un dispositivo
{: #step14}

In questo scenario, vengono utilizzati un tipo di dispositivo e due istanze del dispositivo. Le istanze del dispositivo *humiditySensor1* e *humiditySensor2* sono associate al tipo di dispositivo *HumiditySensor*. 

Puoi creare tipi di dispositivo e dispositivi utilizzando il [dashboard {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://internetofthings.ibmcloud.com){: new_window} o mediante le API REST. Per ulteriori informazioni sull'utilizzo del dashboard {{site.data.keyword.iot_short_notm}} IoT Platform per aggiungere tipi di dispositivo e dispositivi, consulta la documentazione [Introduzione alla gestione dei dati utilizzando l'interfaccia web](../GA_information_management/im_ui_flow.html).

Il seguente esempio mostra come creare un tipo di dispositivo denominato *HumiditySensor* utilizzando l'API REST:

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**Nota importante:** puoi aggiungere i metadati quando crei un tipo di dispositivo e un dispositivo. In questo scenario, vengono aggiunti i seguenti metadati al tipo di dispositivo *HumiditySensor*:
```
{
    "maxHumidityThreshold": 70
}
```
Questi metadati possono essere utilizzati quando si [creano regole](im_rules.html) che vengono attivate se {{site.data.keyword.iot_short_notm}} riceve da *humiditySensor1* o da *humiditySensor2* un evento di umidità che fa sì che la proprietà *humidity* dello stato del dispositivo superi un valore del 70% . 

## Registra i dispositivi

Il seguente esempio mostra come registrare un'istanza del dispositivo denominata *humiditySensor1*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
Il seguente esempio mostra come registrare un'istanza del dispositivo denominata *humiditySensor2*:
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**Nota:** quando un dispositivo effettua una richiesta HTTP tramite l'API REST HTTP di Watson IoT Platform, sono richiesti un nome utente e una password. La password è il valore del token di autenticazione che viene generato automaticamente o specificato manualmente quando si registra un dispositivo. Se utilizzi un client MQTT, devi prendere nota del token di autenticazione del tuo dispositivo perché ti servirà per richiamare lo stato del dispositivo o dell'oggetto mediante la sottoscrizione a una stringa di argomento IoT.

## Passo 1: crea un file dello schema evento
{: #step1}

Per questo scenario, crea un file schema di evento per definire la struttura degli eventi di umidità in entrata. 

Il seguente esempio mostra come creare un file di schema denominato *humEventSchema.json*. Questo file definisce la struttura di un evento in entrata da un dispositivo di umidità:

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "defines the structure of a humidity event",
    "properties" : {
        "humidity" : {
            "description" : "Percentage humidity",
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
**Suggerimento:** utilizza il parametro **required** per contrassegnare una o più proprietà come obbligatorie. Le proprietà obbligatorie devono essere incluse nel messaggio di dispositivo in modo che {{site.data.keyword.iot_short_notm}} aggiorni lo stato del dispositivo con i dati del dispositivo. Un messaggio che non include la proprietà obbligatoria non viene elaborato.   


## Passo 2: crea una risorsa dello schema evento per il tuo tipo di evento.
{: #step2}

Per creare una risorsa dello schema evento, utilizza la seguente API:

```
POST /draft/schemas
```

Il file di definizione schema viene passato a Watson IoT Platform all'interno di un POST a più parti (multipart/form-data). Il corpo del POST deve contenere almeno due parti:

- Una con il nome di **schemaFile** che contiene il contenuto effettivo del file come corpo della parte.
- Una con il nome di **name** che contiene una stringa che definisce il nome della risorsa dello schema nel corpo della parte.

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

Il seguente esempio mostra come utilizzare cURL per creare la risorsa dello schema di evento *humEventSchema.json*:

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**Nota:** il valore di autorizzazione di esempio `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` è costituito dalle seguenti informazioni:
`{API Key}:{authorization token}`,  che vengono quindi codificate in Base64.

Il seguente esempio mostra una risposta al metodo POST:

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
L'identificativo dello schema *5846cd7c6522050001db0e20* restituito in risposta al metodo POST è richiesto quando aggiungi uno schema evento al tuo tipo di evento.



## Passo 3: crea un tipo di evento che fa riferimento allo schema evento
{: #step3}

Ogni tipo di evento fa riferimento allo schema evento pertinente che è stato creato nel precedente esempio utilizzando l'identificativo dello schema restituito nella risposta al metodo POST utilizzato per creare la risorsa dello schema evento.

Per creare un tipo di evento, utilizza la seguente API:

```
POST /draft/event/types
```
Il tipo di evento bozza deve fare riferimento alla definizione di schema che definisce la struttura dell'evento MQTT in entrata.


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


Il seguente esempio mostra come utilizzare cURL per creare un tipo di evento per un evento di umidità:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

L'identificativo dello schema *5846cd7c6522050001db0e20* è utilizzato per aggiungere lo schema evento al tipo di evento. Questo identificativo era stato restituito in risposta al metodo POST che era stato utilizzato per creare la risorsa dello schema evento *humEventSchema.json*

Il seguente esempio mostra una risposta al metodo POST:

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

L'identificativo del tipo di evento *5846cd7c6522050001db0e21* restituito in risposta al metodo POST viene utilizzato per aggiungere un tipo di evento all'interfaccia fisica.


## Passo 4: crea un'interfaccia fisica
{: #step7}

Per creare un'interfaccia fisica, utilizza la seguente API:

```
POST /draft/physicalinterfaces
```

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

In questo scenario, abbiamo bisogno di un'interfaccia fisica per il tipo di evento che abbiamo creato in precedenza.

Il seguente esempio mostra come utilizzare cURL per creare l'interfaccia fisica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

Il seguente esempio mostra una risposta al metodo POST:

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

L'identificativo dell'interfaccia fisica *5846cd7c6522050001db0e22* restituito nella risposta viene utilizzato nell'URL del metodo POST che viene richiamato per aggiungere un evento di temperatura misurato in gradi Celsius all'interfaccia fisica.


## Passo 5: aggiungi il tipo di evento all'interfaccia fisica
{: #step8}

Per aggiungere un tipo di evento all'interfaccia fisica, utilizza la seguente API:

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

In questo scenario, viene aggiunto il seguente tipo di evento alle interfacce fisiche specificate:
- l'evento di umidità *humevt* viene aggiunto all'interfaccia fisica con l'identificativo *5846cd7c6522050001db0e22* utilizzando l'*eventId* dall'argomento e l'*eventTypeId* dalla creazione della risorsa dello schema evento.

Il seguente esempio mostra come utilizzare cURL per aggiungere l'evento di umidità *humevt* all'interfaccia fisica con l'identificativo *5846cd7c6522050001db0e22* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

Il seguente esempio mostra una risposta al metodo POST:

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## Passo 6: aggiorna il tipo dispositivo per la connessione dell'interfaccia fisica
{: #step9}

Per aggiornare il tipo dispositivo, utilizza la seguente API:

```
POST /draft/device/types/{typeId}/physicalinterface
```

dove *typeId* è l'identificativo del tipo di dispositivo.


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

In questo scenario, il tipo di dispositivo *HumiditySensor* viene aggiornato per il collegamento all'interfaccia fisica *5846cd7c6522050001db0e22*.

Il seguente esempio mostra come utilizzare cURL per aggiornare il tipo di dispositivo *HumiditySensor*:

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

Il seguente esempio mostra una risposta al metodo POST:
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificativo del dispositivo *HumiditySensor* è obbligatorio quando aggiungi la tua interfaccia fisica e la tua interfaccia logica.
  


## Passo 7: crea un file schema dell'interfaccia logica
{: #step4}

Il seguente esempio mostra come creare un file schema dell'interfaccia logica denominato *hygrometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema that defines the canonical interface for a hygrometer",
    "properties" : {
        "humidity" : {
            "description" : "percentage humidity",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## Passo 8: crea una risorsa dello schema dell'interfaccia logica
{: #step5}
Il seguente esempio mostra come utilizzare cURL per creare un'interfaccia logica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
Il seguente esempio mostra una risposta al metodo POST:
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
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometer",
  "version" : "draft"
}
```

## Passo 9: crea un'interfaccia logica che faccia riferimento a uno schema dell'interfaccia logica
{: #step6}

Per creare un'interfaccia logica, utilizza la seguente API: 

```
POST /draft/logicalinterfaces
```

Facoltativamente, puoi specificare un alias significativo per la tua interfaccia logica. È possibile fare riferimento all'alias nella chiamata API o nella sottoscrizione alla stringa di argomento utilizzata per richiamare lo stato di un dispositivo, invece di utilizzare l'identificativo dell'interfaccia logica generato automaticamente.

**Nota:** il nome alias deve avere una lunghezza compresa tra 1 e 36 caratteri e può includere caratteri alfanumerici, trattini, punti, caratteri di sottolineatura. Il nome alias non può essere una stringa esadecimale di 24 caratteri. 

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

In questo scenario, utilizza l'identificativo dello schema *5846cd7c6522050001db0e23* restituito nella risposta precedente per aggiungere lo schema dell'interfaccia logica all'interfaccia logica.

Il seguente esempio mostra come utilizzare cURL per creare un'interfaccia logica:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

Il seguente esempio mostra una risposta al metodo POST:

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
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometermometer",
  "version" : "draft"
}
```
In questo scenario, utilizza l'identificativo dell'interfaccia logica *5846cd7c6522050001db0e24* restituito nella risposta al metodo POST per aggiungere l'interfaccia logica al tuo tipo di dispositivo. Puoi anche utilizzare questo identificativo per associare un evento del dispositivo in entrata a una proprietà definita dall'interfaccia logica. Puoi utilizzare l'alias dell'interfaccia logica *IHygrometer* per richiamare lo stato del dispositivo attraverso le API REST HTTP o sottoscrivendo a una stringa di argomento.


## Passo 10: aggiungi l'interfaccia logica al tipo di dispositivo

Il seguente esempio mostra come utilizzare cURL per aggiungere l'interfaccia logica 5846cd7c6522050001db0e24 che fa riferimento all'identificativo dello schema dell'interfaccia logica 5846cd7c6522050001db0e23 al tipo di dispositivo HumiditySensor:
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
Il seguente esempio mostra una risposta al metodo POST:
```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Hygrometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846cd7c6522050001db0e24",
  "schemaId" : "5846cd7c6522050001db0e23"
}
```

## Passo 11: aggiungi le associazioni al tipo di dispositivo

    
Il seguente esempio mostra come utilizzare cURL per aggiungere un'associazione al tipo di dispositivo *HumiditySensor*:

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

Il seguente esempio mostra una risposta al metodo POST:
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

## Passo 12: attiva la configurazione per il tipo di dispositivo


    
Il seguente esempio mostra come utilizzare cURL per convalidare e attivare la tua configurazione per il tipo di dispositivo *HumiditySensor*:
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
Il seguente esempio mostra una risposta al metodo PATCH:
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
