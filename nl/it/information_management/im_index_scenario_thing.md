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

# Guida passo a passo 2: un esempio dettagliato su come utilizzare gli oggetti tramite un'interfaccia comune (Beta)
{: #scenario}

Questo scenario si basa sulla precedente [Guida passo a passo 1: un esempio dettagliato su come utilizzare i dispositivi tramite un'interfaccia comune](../GA_information_management/ga_im_index_scenario.html).

**Importante:** la funzione degli oggetti di {{site.data.keyword.iot_full}} per la gestione dei dati è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

In questo scenario, i dispositivi di temperatura e umidità pubblicano i dati ambientali raccolti in due sale riunioni - Sala riunioni 1 e Sala riunioni 2. Per ogni sala riunioni, i dati dei dispositivi di temperatura e umidità vengono associati separatamente a due interfacce logiche del tipo di dispositivo. 

Per la Sala riunioni 1, i dati del dispositivo di temperatura associati al tipo di dispositivo *TSensor* vengono associati all'interfaccia logica *Thermometer Interface* e i dati del dispositivo di umidità associati al tipo di dispositivo *HumiditySensor* vengono associati all'interfaccia logica *Hygrometer Interface*. 

Per la Sala riunioni 2, i dati del dispositivo di temperatura associati al tipo di dispositivo TempSensor vengono associati all'interfaccia logica *Thermometer Interface* e i dati del dispositivo di umidità associati al tipo di dispositivo *HumiditySensor* vengono associati all'interfaccia logica *Hygrometer Interface*. 

Viene quindi creato un tipo di oggetto denominato *RoomType* insieme a due istanze di oggetto stanza: *meetingroom1* e *meetingroom2*.

Questo scenario imposta una composizione che include le interfacce logiche di termometro e igrometro e quindi associa il dispositivo ambientale corretto a ciascuna delle istanze di stanza, ad esempio *tSensor* e *humiditySensor1* vengono associati a *meetingroom1*.

## Prerequisiti
{: #pre_req}

Prima di continuare, assicurati di:
- Utilizza la stessa istanza dell'organizzazione {{site.data.keyword.iot_full}} e un token o una chiave API per tale organizzazione che hai utilizzato nella guida passo a passo 1. Per ulteriori informazioni sui token e sulle chiavi API, consulta la sezione Autenticazione nella documentazione [API REST HTTP per le applicazioni](../applications/api.html#authentication).
- Aver configurato due interfacce logiche, una per un dispositivo di temperatura e l'altra per un dispositivo di umidità. Per informazioni sulla configurazione di un'interfaccia logica per un dispositivo di temperatura, consulta la documentazione [Guida passo a passo 1: un esempio dettagliato su come utilizzare i dispositivi tramite un'interfaccia comune](../GA_information_management/ga_im_index_scenario.html#step4). Per informazioni sulla configurazione di un'interfaccia logica per un dispositivo di umidità, consulta [Informazioni aggiuntive per la guida passo a passo 2 - configurazione di un'interfaccia logica per un dispositivo di umidità](im_hygrometer.html).

## Informazioni su quest'attività
{: #about}

In {{site.data.keyword.iot_short_notm}}, un oggetto può essere costituito da una serie di dispositivi e oggetti. Un tipo di oggetto definisce come vengono composte le istanze di un oggetto. 

Un'interfaccia logica è associata a un tipo di oggetto. Questa associazione definisce la struttura dello stato generato per un'istanza del tipo di oggetto. Le associazioni vengono utilizzate per definire in che modo le proprietà dei dispositivi e degli oggetti aggregati vengono associate alle proprietà sullo stato di un oggetto.

L'interfaccia logica viene utilizzata per rimuovere la necessità che l'applicazione comprenda come è configurato un dispositivo o un oggetto. Ad esempio, puoi misurare la temperatura di una stanza utilizzando un singolo dispositivo oppure puoi calcolare la temperatura della stanza prendendo la lettura media di diversi dispositivi. L'applicazione richiede informazioni sullo stato di una o più stanze, uno dei cui componenti è una proprietà di temperatura. Non importa il modo in cui viene calcolato il valore della temperatura ricevuto dall'applicazione.

In questo scenario, due dispositivi di temperatura e due dispositivi di umidità pubblicano eventi in {{site.data.keyword.iot_short_notm}}. Un dispositivo di temperatura e un dispositivo di umidità si trovano nella Sala riunioni 1 di un complesso di uffici. Gli altri dispositivi di temperatura e umidità si trovano nella Sala riunioni 2. Il seguente diagramma illustra la configurazione per la Sala riunioni 1:

![Associazione tra l'oggetto di temperatura e umidità e un'applicazione su {{site.data.keyword.iot_short_notm}}.](images/Information)

Un tipo di oggetto denominato *RoomType* viene usato per definire come sono composte le istanze delle stanze. Un'interfaccia logica è associata a *RoomType* e definisce che gli eventi in entrata siano associati a una singola lettura che mostra sia la temperatura che l'umidità. Questa singola lettura è lo stato dell'oggetto. Le associazione sono utilizzate per definire in che modo le proprietà dei dispositivi di temperatura e umidità sono associate a questo stato dell'oggetto. Quando questi dispositivi pubblicano una nuova lettura, il valore della proprietà associata allo stato dell'oggetto viene modificato.

La seguente tabella mostra i quattro dispositivi utilizzati nel nostro esempio, l'argomento su cui pubblica ogni dispositivo e un payload di esempio per ciascun dispositivo.

Tipo/dispositivo | Evento | Payload evento/Proprietà
------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**Nota:** gli identificativi evento *tevt*, *tempevt* e *humevt* sono obbligatori quando definisci le associazioni per associare una proprietà associata a un evento in entrata di quel tipo a una proprietà nell'interfaccia logica. In questo scenario, nell'interfaccia logica vengono definite due proprietà - *temperature* e *humidity*.

Viene anche configurata un'interfaccia logica. L'interfaccia logica rappresenta lo stato dell'oggetto nella seguente struttura:
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

Utilizza il seguente scenario di esempio per configurare il tuo ambiente di interfaccia. 

**Nota:** una tabella che elenca i nomi, i valori e gli identificativi delle proprietà delle risorse utilizzati in questa guida è documentata in [Proprietà e identificativi delle risorse a cui si fa riferimento nelle guide passo a passo 1 e 2](im_id_reference.html).

## Se necessario, aggiungi un tipo di dispositivo e un dispositivo  
{: #add_device}  
In questo scenario, vengono utilizzati tre tipi di dispositivo e quattro istanze del dispositivo. L'istanza del dispositivo *tSensor* è associata al tipo di dispositivo *TSensor*. L'istanza del dispositivo *tempSensor* è associata al tipo di dispositivo *TempSensor*. Le istanze del dispositivo *humiditySensor1* e *humiditySensor2* sono associate al tipo di dispositivo *HumiditySensor*.

Puoi creare tipi di dispositivo e dispositivi utilizzando il [dashboard {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://internetofthings.ibmcloud.com){: new_window} o mediante le API REST. 

Per ulteriori informazioni sull'utilizzo del dashboard {{site.data.keyword.iot_short_notm}} IoT Platform per aggiungere tipi di dispositivo e dispositivi, consulta la documentazione [Introduzione alla gestione dei dati utilizzando l'interfaccia web](../GA_information_management/im_ui_flow.html).

Per informazioni sull'utilizzo delle API REST per aggiungere tipi di dispositivo e dispositivi, consulta la documentazione [API REST HTTP {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window}.



## Passo 1: crea un file schema del tipo di oggetto  
{: #crt_composition_file}  
Crea un file schema del tipo di oggetto che faccia riferimento agli identificativi dell'interfaccia logica del dispositivo per i tipi di dispositivi di temperatura e umidità.  

Il seguente esempio mostra come creare un file schema del tipo di oggetto denominato *roomTypeSchema*.   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON Schema that defines the structure of the Room Thing Type.",
   "properties" : {
        "thermometer": {
            "type": "object",
            "description": "The thermometer device",
            "$logicalInterfaceRef": "IThermometer"
        },
        "hygrometer": {
            "type": "object",
            "description": "The hygrometer device",
            "$logicalInterfaceRef": "IHygrometer"
        }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```
**Suggerimento:** utilizza il parametro **required** per contrassegnare una o più proprietà come obbligatorie. Le proprietà obbligatorie devono essere incluse nel messaggio di dispositivo perché Watson IoT Platform aggiorni lo stato del dispositivo con i dati del dispositivo. Un messaggio che non include la proprietà obbligatoria non viene elaborato.   
**Nota:** l'identificativo di schema che viene generato quando crei un file schema del tipo di oggetto deve essere specificato quando crei il tuo tipo di oggetto.   

## Passo 2: crea una risorsa dello schema oggetto.  
{: #crt_composition_resource}  

Crea la risorsa dello schema caricando il file schema del tipo di oggetto generato nel passo precedente.  
Carica il file schema del tipo di oggetto utilizzando la seguente API:  
```
POST /draft/schemas
```  
Il file di definizione schema viene passato a Watson IoT Platform all'interno di un POST a più parti (multipart/form-data). Il corpo del POST deve contenere almeno due parti:

  - Una con il nome di **schemaFile** che contiene il contenuto effettivo del file come corpo della parte.
  - Una con il nome di **name** che contiene una stringa che definisce il nome della risorsa dello schema nel corpo della parte.


Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).  

Il seguente esempio mostra come utilizzare cURL per creare la risorsa dello schema del tipo di oggetto:  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
Il seguente esempio mostra una risposta al metodo POST:
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
L'identificativo dello schema *5a72ea48d60180002c4f5e58* restituito in risposta al metodo POST è richiesto quando crei un tipo di oggetto.


## Passo 3: crea un tipo di oggetto  
{: #crt_thing_type}  

I tipi di oggetto sono utilizzati per modellare le istanze dell'oggetto. È necessario creare un tipo di oggetto in un'organizzazione prima di poter creare un'istanza dell'oggetto. Per questo scenario, crea un singolo tipo di oggetto.  
Lo schema associato a un tipo di oggetto definisce il tipo di oggetti che vengono aggregati per creare un'istanza di un oggetto. Il tipo di oggetto deve fare riferimento alla risorsa dello schema del tipo di oggetto che hai creato nel passo precedente.  
Crea un tipo di oggetto utilizzando la seguente API:
```
POST /draft/thing/types
```
I seguenti parametri sono richiesti nel corpo del metodo POST:  
<table>
<tr><th>Parametro</th><th>Descrizione</th></tr>
<tr><td>id</td><td>Fornisci un identificativo per il tipo di oggetto che stai creando.</td></tr>
<tr><td>name</td><td>Fornisci un nome per il tipo di oggetto che stai creando.</td></tr>
<tr><td>schemaId</td><td>L'identificativo creato per la risorsa dello schema di composizione.</td></tr>
</table>

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  

Il seguente esempio mostra come utilizzare cURL per creare un tipo di oggetto denominato *RoomType*.
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

Risposta: 
```
{
 "name": "RoomType",
 "description": "Room Thing Type",
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

## Passo 4: crea un file schema dell'interfaccia logica  
{: #crt_ai_schema_file}
Nella tua interfaccia logica, puoi definire la struttura dei dati che vengono memorizzati come stato dell'oggetto. Per questo scenario, crea un'interfaccia logica che definisce le proprietà di temperatura e umidità. Associa l'interfaccia logica al tipo di oggetto *RoomType* facendo riferimento all'identificativo dell'interfaccia logica che viene generato quando crei la risorsa di interfaccia logica.  
Il seguente esempio mostra come creare un file schema dell'interfaccia logica denominato *roomSchema*.

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON Schema that defines the canonical room state structure",
    "properties" : {
        "temperature" : {
            "description" : "Temperature in degrees celsius",
           "type" : "number",
           "minimum" : -273.15,
           "default" : 0.0
        },
       "humidity" : {
            "description" : "Percentage humidity",
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
## Passo 5: crea una risorsa dello schema dell'interfaccia logica.  
{: #crt_ai_schema_resource}  
Carica il file schema dell'interfaccia logica che hai creato nel passo precedente per creare una risorsa dello schema dell'interfaccia logica per il tuo tipo di oggetto utilizzando la seguente API:  
```
POST /draft/schemas
```  

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).  

Il seguente esempio mostra come utilizzare cURL per creare lo schema dell'interfaccia logica:
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
Il seguente esempio mostra una risposta al metodo POST:
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
Utilizza l'identificativo dello schema *5a4b9847d60180002efce645* restituito nella risposta al metodo POST per aggiungere la risorsa dello schema di interfaccia logica all'interfaccia logica per il tuo tipo di oggetto.  


## Passo 6: crea un'interfaccia logica per il tipo di oggetto.  
{: #crt_thing_ai}  
L'interfaccia logica deve fare riferimento alla risorsa dello schema dell'interfaccia logica che hai creato nel passo precedente.  
Per creare un'interfaccia logica, utilizza la seguente API:   
```
POST draft/logicalinterfaces
```  
Facoltativamente, puoi specificare un alias significativo per la tua interfaccia logica. È possibile fare riferimento all'alias nella chiamata API o nella sottoscrizione alla stringa di argomento utilizzata per richiamare lo stato di un oggetto, invece di utilizzare l'identificativo dell'interfaccia logica generato automaticamente.

**Nota:** il nome alias deve avere una lunghezza compresa tra 1 e 36 caratteri e può includere caratteri alfanumerici, trattini, punti, caratteri di sottolineatura. Il nome alias non può essere una stringa esadecimale di 24 caratteri.

In questo scenario, utilizza l'identificativo dello schema *5a4b9847d60180002efce645* restituito nella risposta precedente per aggiungere lo schema dell'interfaccia logica all'interfaccia logica.

Il seguente esempio mostra come utilizzare cURL per creare un'interfaccia logica con l'alias *IRoom*:
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
Il seguente esempio mostra una risposta al metodo POST:
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
In questo scenario, utilizza l'identificativo dell'interfaccia logica *5a4b9847d60180002efce645* restituito nella risposta al metodo POST per aggiungere l'interfaccia logica al tuo tipo di dispositivo. Puoi anche utilizzare questo identificativo per associare un evento del dispositivo in entrata a una proprietà definita dall'interfaccia logica.

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).  


## Passo 7: aggiungi l'interfaccia logica al tipo di oggetto.  
{: #add_thing_ai}  
Per aggiungere l'interfaccia logica a un tipo di oggetto, utilizza la seguente API:  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  
In questo scenario, l'interfaccia logica è associata al tipo di oggetto *RoomType*.

Il seguente esempio mostra come utilizzare cURL per aggiungere l'interfaccia logica dell'oggetto *IRoom* al tipo di oggetto *RoomType*:  
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

Il seguente esempio mostra una risposta al metodo POST:
```
{
 "name": "Room Logical Interface",
 "description": "This is a Room logical interface",
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

## Passo 8: definisci le associazioni
{: #define_Thing_type_mappings}

Definisci le associazioni per il tipo di oggetto che descrivono come associare le proprietà dallo stato degli oggetti o dei dispositivi aggregati alle proprietà definite sull'interfaccia logica del tipo di oggetto.

Per associare gli eventi, utilizza la seguente API:  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

dove *thingtypeId* è l'identificativo che viene restituito nella risposta alla richiesta POST quando viene creato il tipo di oggetto. 

I seguenti parametri sono richiesti nel corpo della richiesta POST:  
<table>
<tr>
<th>	Parametro	</th><th>	Descrizione	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	Una struttura JSON valida che associa le proprietà definite per l'interfaccia logica alle proprietà del payload di evento del dispositivo.</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	L'identificativo dell'interfaccia logica è richiesto nel corpo del payload.</td>
</tr>
</table>  

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).

Il seguente esempio mostra come utilizzare cURL per aggiungere un'associazione al tipo di oggetto *RoomType*:

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
Il dispositivo *thermometer* è definito in [roomTypeSchema](#crt_composition_file). La proprietà *$event.temperature* è definita nello schema dell'interfaccia logica con identificativo *5846ed076522050001db0e12* e alias *IThermometer*.  
Il dispositivo *hygrometer* è definito in [roomTypeSchema](#crt_composition_file). La proprietà *$event.humidity* è definita nello schema dell'interfaccia logica con identificativo *5846cd7c6522050001db0e24* e alias *IHygrometer*.


## Passo 9: convalida e attiva la configurazione
{: #activate}

Convalida e attiva la configurazione correlata all'aggiornamento dello stato dell'oggetto per ogni tipo di oggetto. Questa configurazione include i tuoi schemi, interfacce logiche e associazioni. 

Per convalidare la tua configurazione del tipo di oggetto, utilizza la seguente API:
```
PATCH /draft/thing/types/{thingTypeId}
```
dove *thingTypeId* è l'identificativo del tipo di oggetto. 

Il seguente esempio mostra come utilizzare cURL per convalidare e attivare la configurazione associata al tipo di oggetto *RoomType*:
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

Il seguente esempio mostra una risposta al metodo PATCH:
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

## Passo 10: crea un'istanza di un tipo di oggetto
{: #create_Thing_instances}

Un oggetto è un'istanza di un tipo di oggetto. Un oggetto ti consente di aggregare insieme una o più istanze di un dispositivo o di un oggetto in un singolo oggetto.

Per creare un oggetto, utilizza la seguente API:

```
POST /thing/types/{thingTypeId}/things
```
dove *thingtypeId* è l'identificativo che viene restituito nella risposta alla richiesta POST quando viene creato il tipo di oggetto. 

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things).

In questo scenario, dobbiamo creare due istanze di oggetto di tipo *RoomType*. Una istanza di oggetto è denominata *meetingroom1* e l'altra istanza di oggetto è denominata *meetingroom2*.

Il seguente esempio mostra come utilizzare cURL per creare un'istanza di oggetto denominata *meetingroom1*. L'istanza di oggetto *meetingroom1* è associata alle istanze di dispositivo *tSensor* e *humiditySensor1*.

```
thingId = "meetingroom1"
 meetingroom1AggregatedObjects = {
   "thermometer": {
     "type": "device",
     "typeId": "TSensor",
     "id": "tSensor"
   },
   "hygrometer": {
     "type": "device",
     "typeId": "HumiditySensor",
     "id": "humiditySensor1"
   }
 }
``` 

L'identificativo dell'oggetto creato viene utilizzato nell'URL del metodo POST che viene richiamato per aggiungere un evento di temperatura all'interfaccia logica dell'oggetto.

Il seguente esempio mostra come utilizzare cURL per creare un'istanza di oggetto denominata *meetingroom2*. *meetingroom2* è associato alle istanze di dispositivo *tempSensor* e *humiditySensor2*.

```
thingId = "meetingroom2"
   meetingroom2AggregatedObjects = {
    "thermometer": {
      "type": "device",
      "typeId": "TempSensor",
      "id": "tempSensor"
    },
    "hygrometer": {
      "type": "device",
      "typeId": "HumiditySensor",
      "id": "humiditySensor2"
    }
  }

``` 

## Passo 11: verifica che gli eventi di dispositivo associati siano pubblicati sull'interfaccia logica  
{: #publish_event}  
Pubblica i seguenti eventi per i dispositivi aggregati nell'oggetto *meetingroom1*:  
 - un evento di temperatura da *tSensor* sull'argomento `iot-2/evt/tevt/fmt/json`  
 - un evento di umidità da *humiditySensor1* sull'argomento `iot-2/evt/humevt/fmt/json`  
 
Pubblica i seguenti eventi per i dispositivi aggregati nell'oggetto *meetingroom2*:  
 - un evento di temperatura da *tempSensor* sull'argomento `iot-2/evt/tempevt/fmt/json`  
 - un evento di umidità da *humiditySensor2* sull'argomento `iot-2/evt/humevt/fmt/json`  
 
Per informazioni sulla pubblicazione di un evento in entrata da un dispositivo, consulta [Connettività MQTT per le applicazioni](../applications/mqtt.html#publishing_device_events).  

## Passo 12: verifica che lo stato dell'oggetto sia cambiato.  
{: #verify_Thing_state}  

Puoi richiamare lo stato dell'oggetto utilizzando le API REST HTTP o sottoscrivendo a un argomento.

Se disponi di un'applicazione client MQTT, puoi sottoscriverti alla seguente stringa di argomento:
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

In alternativa, puoi richiamare l'ultimo stato dell'oggetto utilizzando la seguente API REST HTTP:

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

I seguenti parametri sono obbligatori:  
<table>
<tr>
<th>Parametro	</th><th>	Descrizione</th>
</tr>
<tr>
<td>thingTypeId	</td><td>L'identificativo del tipo di oggetto.</td>
</tr>
<tr>
<td>thingId	</td><td>	L'identificativo dell'oggetto.</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>L'identificativo creato per l'interfaccia logica.</td>
</tr>
</table>  

Per ulteriori dettagli, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  


## Passi successivi

Crea regole che puoi utilizzare per avviare un'azione quando un evento ricevuto da {{site.data.keyword.iot_short_notm}} provoca una modifica dello stato del dispositivo o dell'oggetto. Per informazioni sulla creazione delle regole, consulta [Creazione delle regole integrate (Beta)](im_rules.html).
