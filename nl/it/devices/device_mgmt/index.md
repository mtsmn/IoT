---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-08"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Protocollo di gestione dispositivo
{: #index}

## Introduzione
{: #introduction}

{{site.data.keyword.iot_full}} riconosce i dispositivi e i gateway come le due classi di dispositivo. La classe di dispositivo viene identificata utilizzando il campo "classId". I dispositivi nella classe di dispositivo possono essere dispositivi gestiti o dispositivi non gestiti. 

**Dispositivi gestiti** sono definiti come dispositivi che contengono un agent di gestione del dispositivo. Un agent di gestione del dispositivo è un insieme di logica che consente al dispositivo di interagire con il servizio di gestione del dispositivo {{site.data.keyword.iot_short_notm}} utilizzando il protocollo di gestione del dispositivo. I dispositivi gestiti possono eseguire operazioni di gestione del dispositivo inclusi gli aggiornamenti dell'ubicazione, gli scaricamenti e gli aggiornamenti del firmware, i riavvi e le reimpostazioni dei valori predefiniti.

Il protocollo di gestione del dispositivo definisce un insieme di operazioni supportate. Un agent di gestione del dispositivo può supportare un sottoinsieme di operazioni, ma deve effettuare la richiesta di gestione del dispositivo. Un dispositivo che supporta le operazioni di azione firmware deve supportare anche l'osservazione.

Il protocollo di gestione del dispositivo è creato con il protocollo di messaggistica MQTT. Per ulteriori informazioni su come il protocollo di gestione del dispositivo interagisce con MQTT, consulta [Connettività MQTT per i dispositivi](../mqtt.html).


### Il ciclo di vita di gestione del dispositivo

1. Un dispositivo e il suo tipo dispositivo associato vengono creati in {{site.data.keyword.iot_short_notm}} utilizzando il dashboard o l'API REST.
2. Un dispositivo si collega a {{site.data.keyword.iot_short_notm}} ed effettua una richiesta di gestione del dispositivo per diventare un dispositivo gestito.
3. Puoi visualizzare e modificare i metadati per un dispositivo utilizzando l'API REST {{site.data.keyword.iot_short_notm}}. Queste operazioni API, ad esempio l'aggiornamento del firmware e il riavvio del dispositivo, sono descritte nella documentazione [Richieste di gestione del dispositivo](https://console.ng.bluemix.net/docs/services/IoT/devices/device_mgmt/requests.html).
4. Un dispositivo può comunicare gli aggiornamenti sulla sua ubicazione, le informazioni di diagnostica e i codici di errore utilizzando il protocollo di gestione del dispositivo.
5. Quando un dispositivo viene ritirato, puoi rimuoverlo da {{site.data.keyword.iot_short_notm}} utilizzando il dashboard o l'API REST.

Fai riferimento alla ricetta [Connect Raspberry Pi as Managed Device to IBM Watson IoT Platform ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-as-managed-device-to-ibm-iot-foundation/){: new_window}.

## Richieste di gestione del dispositivo
{: #manage_device_request}

Un dispositivo utilizza la richiesta di gestione del dispositivo per diventare un dispositivo gestito. Un agent di gestione del dispositivo deve inviare una richiesta di gestione dispositivo prima che possa ricevere le richieste dal server. Un agent di gestione del dispositivo normalmente invia questo tipo di richiesta se viene avviato o riavviato.

### Argomento per una richiesta di gestione del dispositivo

Un dispositivo pubblica una richiesta di gestione del dispositivo nel seguente argomento:

```
iotdevice-1/mgmt/manage
```

Il server risponde alla richiesta di gestione del dispositivo nel seguente argomento:

```
iotdm-1/response
```




### Il formato del messaggio per una richiesta di gestione del dispositivo


In una richiesta di gestione del dispositivo, il campo ``d`` e tutti i campi secondari relativi sono facoltativi. I valori del campo ``metadata`` e ``deviceInfo`` sostituiscono gli attributi corrispondenti per l'invio del dispositivo se sono stati inviati.

Il campo ``lifetime`` facoltativo specifica l'arco di tempo in secondi in cui il dispositivo deve inviare un'altra richiesta di gestione del dispositivo per evitare di essere classificato come inattivo e diventare un dispositivo non gestito. Se il campo ``lifetime`` viene omesso o impostato su ``0``, il dispositivo gestito non diventa inattivo. L'impostazione supportata minima per il campo ``lifetime`` è ``3600`` secondi, che corrisponde a 1 ora.

I campi facoltativi ``supports.deviceActions`` e ``supports.firmwareActions`` indicano le funzionalità dell'agent di gestione del dispositivo. Se ``supports.deviceActions`` è impostato, l'agent supporta le azioni di reimpostazione dei valori predefiniti e di riavvio. Per un dispositivo che non distingue un riavvio da una reimpostazione dei valori predefiniti, è accettabile utilizzare lo stesso comportamento per entrambe le azioni. Se ``supports.firmwareActions`` è impostato, l'agent supporta le azioni di aggiornamento e scaricamento firmware.

Il seguente esempio mostra il formato della richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/mgmt/manage
	{
    "d": {
        "metadata":{},
        "lifetime": number,
        "supports": {
            "deviceActions": boolean,
            "firmwareActions": boolean
        },
        "deviceInfo": {
            "serialNumber": "string",
            "manufacturer": "string",
            "model": "string",
            "deviceClass": "string",
            "description" :"string",
            "fwVersion": "string",
            "hwVersion": "string",
            "descriptiveLocation": "string"
        }
    },
    "reqId": "string"
}
```

Il seguente esempio mostra il formato della risposta:

```
Incoming message from the server:

Topic: iotdm-1/response
	{
    "rc": number,
    "reqId": "string"
}
```


### Codici di risposta per una richiesta di gestione del dispositivo

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|403   |Non consentito (se un dispositivo tenta di pubblicare una richiesta di gestione che richiede il supporto per una serie di azioni non valide)|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.|
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|


## Richieste di annullamento della gestione del dispositivo
{: #manage-unmanage}


Un dispositivo utilizza una richiesta di annullamento della gestione del dispositivo quando non è più necessario che sia un dispositivo gestito. Quando un dispositivo divine non gestito, {{site.data.keyword.iot_short_notm}} non invia più nuove richieste di gestione del dispositivo al dispositivo. I dispositivi non gestiti possono continuare a pubblicare codici di errore, messaggi di log e messaggi di ubicazione.

### Argomento per una richiesta di annullamento della gestione del dispositivo


Un dispositivo pubblica una richiesta di annullamento della gestione del dispositivo nel seguente argomento:

```
iotdevice-1/mgmt/unmanage
```

Il server risponde alla richiesta di annullamento della gestione del dispositivo nel seguente argomento:

```
iotdm-1/response
```

### Il formato del messaggio per una richiesta di annullamento della gestione del dispositivo

Formato richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/mgmt/unmanage
{
    "reqId": "string"
}
```

Formato risposta:

```
Incoming message from the server:

Topic: iotdm-1/response
	{
    "rc": number,
    "reqId": "string"
}
```

### Codici di risposta per una richiesta di annullamento della gestione del dispositivo

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.|
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|


## Richieste di aggiornamento dell'ubicazione
{: #update-location}

I metadati di ubicazione di un dispositivo possono essere aggiornati in {{site.data.keyword.iot_short_notm}} nei seguenti modi:

### Aggiornamenti dell'ubicazione del dispositivo automatici
- I dispositivi che determinano la loro ubicazione possono scegliere di inviare notifiche al server di gestione del dispositivo {{site.data.keyword.iot_short_notm}} sulle modifiche dell'ubicazione. Il dispositivo invia notifiche a {{site.data.keyword.iot_short_notm}} sull'aggiornamento dell'ubicazione. Il dispositivo richiama la sua ubicazione, ad esempio da un ricevitore GPS, e invia un messaggio di gestione del dispositivo all'istanza {{site.data.keyword.iot_short_notm}} per aggiornare la sua ubicazione. La data/ora acquisisce l'ora in cui è stata richiamata l'ubicazione dal ricevitore GPS. La data/ora è valida anche se è presente un ritardo nell'invio del messaggio di aggiornamento dell'ubicazione. Se non è stato utilizzato un indicatore di data/ora, il server registra la data e l'ora della ricezione del messaggio e utilizza tali informazioni per aggiornare i metadati di ubicazione.

### Aggiornamenti dell'ubciazione del dispositivo manuali utilizzando l'API REST
- Puoi impostare i metadati dell'ubicazione manualmente per un dispositivo statico utilizzando l'API REST {{site.data.keyword.iot_short_notm}} quando viene registrato il dispositivo. Puoi anche modificare l'ubicazione successivamente. L'impostazione di data/ora è facoltativa, ma quando omessa, vengono impostate la data e l'ora correnti nei metadati del dispositivo.

### Argomento per una richiesta di aggiornamento dell'ubicazione attivata dal dispositivo:


Un dispositivo pubblica una richiesta di aggiornamento dell'ubicazione nel seguente argomento:

```
iotdevice-1/device/update/location
```

Il server risponde alla richiesta di aggiornamento dell'ubicazione nel seguente argomento:

```
iotdm-1/response
```

### Aggiornamento dell'ubicazione attivato dagli utenti o dalle applicazioni


Quando un utente o un'applicazione aggiorna l'ubicazione di un dispositivo gestito attivo, il dispositivo riceve un messaggio di aggiornamento.




### Argomento per una richiesta di aggiornamento dell'ubicazione attivata dagli utenti o dalle applicazioni


Il server pubblica una richiesta di aggiornamento dell'ubicazione nel seguente argomento:

```
iotdm-1/device/update
```

### Il formato del messaggio per una richiesta di aggiornamento dell'ubicazione


Il campo ``measuredDateTime`` è la data e l'ora della misurazione dell'ubicazione.

Ogni volta che l'ubicazione viene aggiornata, i valori forniti per latitudine, longitudine, elevazione e precisione sono considerati un singolo aggiornamento a più valori. La latitudine e la longitudine sono obbligatorie e devono essere entrambe fornite con ogni aggiornamento. La latitudine e la longitudine devono essere specificate in gradi decimali utilizzando il World Geodetic System 1984 (WGS84). L'elevazione e la precisione sono misurate in metri e sono facoltative.

Se un valore facoltativo viene fornito su un aggiornamento e poi omesso in un aggiornamento successivo, il valore precedente viene cancellato dall'aggiornamento successivo. Ogni aggiornamento viene considerato un insieme completo con più valori.


Formato richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/device/update/location
{
    "d": {
        "longitude": number,
        "latitude": number,

        "elevation": number,
        "measuredDateTime": "string in ISO8601 format",
        "updatedDateTime": "string in ISO8601 format",
        "accuracy": number
    },
    "reqId": "string"
}
```

Formato risposta:

```
Incoming message from the server:

Topic: iotdm-1/response
	{
    "rc": number,
    "reqId": "string"
}
```

### Codici di risposta per una richiesta di aggiornamento dell'ubicazione

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.|
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|


### Aggiornamenti dell'ubicazione attivati dagli utenti o dalle applicazioni


Il seguente esempio illustra il formato del payload:

```
Incoming message from the server:

Topic: iotdm-1/device/update
{
    "d": {
        "fields": [
				{
                "field": "location",
                "value": {
                    "latitude": number,
                    "longitude": number,
                    "elevation": number,
                    "accuracy": number,
                    "measuredDateTime": "string in ISO8601 format"
                    "updatedDateTime": "string in ISO8601 format",

                }
            }
        ]
    }
}
```

**Nota:** il parametro ``reqID`` non viene utilizzato perché il dispositivo non è richiesto nella risposta.

## Richieste di aggiornamento dell'attributo del dispositivo
{: #update-attributes}

Utilizzando l'API REST, {{site.data.keyword.iot_short_notm}} può inviare una richiesta a un dispositivo di aggiornare il valore di uno o più dei seguenti attributi del dispositivo:


|Attributo | Ulteriori informazioni|
|:---|:---|
|location  | Consulta [Aggiornamento ubicazione](index.html#update-location)|
|metadata  | Facoltativo|
|deviceInfo | Facoltativo|
|mgmt.firmware | Consulta [Processo di aggiornamento firmware](requests.html#firmware-actions-update)|

### Argomento per una richiesta di aggiornamento degli attributi del dispositivo


Il server pubblica una richiesta di aggiornamento degli attributi del dispositivo nel seguente argomento:

```
iotdm-1/device/update
```


### Il formato del messaggio per una richiesta di aggiornamento degli attributi del dispositivo


Il seguente esempio illustra il formato del payload per la richiesta:

```
Incoming message from the server:

Topic: iotdm-1/device/update
{
    "d": {
        "fields": [
				{
                "field": "location",
                "value": ""
            }
        ]
    }
}
```


## Richieste di aggiunta dei codici di errore
{: #diag-add-error-code}

I dispositivi possono scegliere di inviare notifiche al server di gestione del dispositivo {{site.data.keyword.iot_short_notm}} sulle modifiche dei loro stati di errore utilizzando il tipo di richiesta di aggiunta dei codice di errore.

### Argomento per una richiesta di aggiunta dei codici di errore


Un dispositivo pubblica una richiesta di aggiunta dei codici di errore nel seguente argomento:

```
iotdevice-1/add/diag/errorCodes
```

### Il formato del messaggio per una richiesta di aggiunta dei codici di errore


Il valore associato a `errorCode` è il codice di errore del dispositivo corrente e deve essere aggiunto a {{site.data.keyword.iot_short_notm}}.

Formato richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/add/diag/errorCodes
{
    "d": {
        "errorCode": number
    },
    "reqId": "string"
}
```

Formato risposta:

```
Incoming message from the server:

Topic: iotdm-1/response
	{
    "rc": number,
    "reqId": "string"
}
```

### Codici di risposta per una richiesta di aggiunta dei codici di errore

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|


## Richieste di eliminazione dei codici di errore
{: #diag-clear-error-codes}

I dispositivi possono richiedere che {{site.data.keyword.iot_short_notm}} elimini tutti i codici di errore per il dispositivo utilizzando il tipo di richiesta di eliminazione dei codici di errore

### Argomento per una richiesta di eliminazione dei codici di errore


Un dispositivo pubblica questa richiesta nel seguente argomento:

```
iotdevice-1/clear/diag/errorCodes
```

### Il formato del messaggio per una richiesta di eliminazione dei codici di errore


Formato richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/clear/diag/errorCodes
{
    "reqId": "string"
}
```

Formato risposta:

```
Incoming message from the server:

Topic: iotdm-1/response
	{
    "rc": 200,
    "reqId": "string"
}
```

### Codici di risposta per una richiesta di eliminazione dei codici di errore

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.|
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|


## Richieste di aggiunta dei log
{: #diag-add-log}

I dispositivi possono scegliere se inviare notifiche al supporto di gestione del dispositivo {{site.data.keyword.iot_short_notm}} sulle modifiche aggiungendo una nuova voce di log. Le voci di log includono un messaggio di log, la data/ora, la severità e i dati diagnostici codificati base64 facoltativi.

### Argomento per una richiesta di aggiunta dei log
Un dispositivo pubblica questa richiesta nel seguente argomento:

```
iotdevice-1/add/diag/log
```


### Il formato del messaggio per una richiesta di aggiunta dei log

La seguente tabella descrive il formato degli attributi del messaggio in uscita:

|Attributo |Descrizione |
|:---|:---|
|`message`|Specifica un messaggio di diagnostica che deve essere aggiunto a {{site.data.keyword.iot_short_notm}}|
|`timestamp`|Specifica la data e l'ora della voce di log nel formato ISO8601 |
|`data`|Specifica i dati di diagnostica codificati base64 facoltativi|
|`severity`|Specifica la severità del messaggio (0: informativo, 1: avvertenza, 2: errore)|


Formato richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/add/diag/log
{
    "d": {
        "message": "string",
        "timestamp": "string",
        "data": "string",
        "severity": number
    },
    "reqId": "string"
}
```

Formato risposta:

```
Incoming message from the server:

Topic: iotdm-1/response
	{
    "rc": number,
    "reqId": "string"
}
```


### Codici di risposta per una richiesta di aggiunta dei log

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.|
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|

## Richieste di eliminazione dei log
{: #diag-clear-logs}

I dispositivi possono richiedere che {{site.data.keyword.iot_short_notm}} elimini tutte le voci di log utilizzando il tipo di richiesta di eliminazione dei log.

### Argomento per una richiesta di eliminazione dei log


Un dispositivo pubblica una richiesta di eliminazione dei log nel seguente argomento:

```
iotdevice-1/clear/diag/log
```

### Il formato del messaggio per una richiesta di eliminazione dei log


Formato richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/clear/diag/log
{
    "reqId": "string"
}
```

Formato risposta:

```
Incoming message from the device:

Topic: iotdm-1/response
	{
    "rc": number,
    "reqId": "string"
}
```

### Codici di risposta per una richiesta di eliminazione dei log

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.|
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|

## Richieste di osservazione delle modifiche dell'attributo
{: #observations-observe}

{{site.data.keyword.iot_short_notm}} può inviare una richiesta di osservazione delle modifiche dell'attributo a un dispositivo per osservare le modifiche ad uno o più attributi utilizzando il tipo di richiesta di osservazione delle modifiche dell'attributo. Quando il dispositivo riceve la richiesta, deve inviare una richiesta di notifica a {{site.data.keyword.iot_short_notm}} ogni volta che i valori degli attributi osservati vengono modificati.

**Importante:** i dispositivi devono implementare, osservare, inviare notifiche e annullare le operazioni per supportare i tipi di richiesta [Azioni firmware - Aggiornamento](requests.html#firmware-actions-update).

### Argomento per una richiesta di osservazione delle modifiche dell'attributo


Il server pubblica una richiesta di osservazione delle modifiche dell'attributo nel seguente argomento:

```
iotdm-1/observe
```

### Il formato del messaggio per una richiesta di osservazione delle modifiche dell'attributo


L'array `fields` è un array degli attributi del dispositivo dal modello del dispositivo. Se viene specificato un campo complesso, come ad esempio `mgmt.firmware`, è previsto che i relativi campi sottostanti siano aggiornati nello stesso momento in modo che sia generato un solo messaggio di notifica.

Il parametro `message` utilizzato nella risposta può essere specificato nella risposta se il valore del parametro `rc` non è `200`. Se non è possibile richiamare alcun valore di parametro specificato, il valore del parametro `rc` deve essere impostato su `404` se l'attributo non viene trovato o su `500` per qualsiasi altro motivo. Quando non possibile trovare i valori dei parametri, l'array `fields` deve contenere gli elementi con `field` impostato sul nome di ogni parametro che non può essere letto. Il parametro `value` deve essere omesso. Per il parametro del codice di risposta da impostare su `200`, devono essere specificati `field` e `value`, dove `value` è il valore corrente di un attributo identificato dal valore del parametro `field`.

Formato richiesta:

```
Incoming message from the server:

Topic: iotdm-1/observe
{
    "d": {
        "fields": [
				{
                "field": "field_name"
            }
        ]
    },
    "reqId": "string"
}
```

Formato risposta:

```
Outgoing message from the device:

Topic: iotdevice-1/response
	{
    "rc": number,
    "message": "string",
    "d": {
        "fields": [
				{
                "field": "field_name",
                "value": "field_value"
            }
        ]
    },
    "reqId": "string"
}
```


## Richieste di annullamento dell'osservazione dell'attributo
{: #observations-cancel}

{{site.data.keyword.iot_short_notm}} può inviare una richiesta a un dispositivo per annullare l'osservazione corrente di uno o più attributi del dispositivo utilizzando il tipo di richiesta di annullamento dell'osservazione dell'attributo. La parte `fields` della richiesta è un array dei nomi attributo del dispositivo dal modello del dispositivo, ad esempio i parametri `location`, `mgmt.firmware` o `mgmt.firmware.state`.

È possibile specificare il parametro `message` se il valore del parametro `rc` non è `200`.

**Importante:** i dispositivi devono implementare, osservare, inviare notifiche e annullare le operazioni per supportare i tipi di richiesta [Azioni firmware - Aggiornamento](requests.html#firmware-actions-update).

### Argomento per una richiesta di annullamento dell'osservazione dell'attributo


Il server pubblica una richiesta di annullamento dell'osservazione dell'attributo nel seguente argomento:

```
iotdm-1/cancel
```


### Il formato del messaggio per una richiesta di annullamento dell'osservazione dell'attributo


Formato richiesta:

```
Incoming message from the server:

Topic: iotdm-1/cancel
{
    "d": {
        "fields": [
				{
                "field": "field_name"
            }
        ]
    },
    "reqId": "string"
}
```

Formato risposta:

```
Outgoing message from the device:

Topic: iotdevice-1/response
	{
    "rc": number,
    "message": "string",
    "reqId": "string"
}
```



## Richieste di notifica delle modifiche dell'attributo
{: #observations-notify}

{{site.data.keyword.iot_short_notm}} può effettuare una richiesta di osservazione per un attributo specifico o impostare un insieme di valori utilizzando il tipo di richiesta di notifica delle modifiche dell'attributo. Quando il valore dell'attributo o degli attributi viene modificato, il dispositivo deve inviare una notifica che contiene il valore più recente.

Il valore del parametro `field` è il nome dell'attributo che è stato modificato e il `value` è il valore corrente dell'attributo. L'attributo può essere un campo complesso. Se vengono aggiornati più valori in un campo complesso come risultato di una sola operazione, viene inviato solo un messaggio di notifica.

**Importante:** i dispositivi devono implementare, osservare, inviare notifiche e annullare le operazioni per supportare i tipi di richiesta [Azioni firmware - Aggiornamento](requests.html#firmware-actions-update).


### Argomento per una richiesta di notifica delle modifiche dell'attributo


Un dispositivo pubblica una richiesta di notifica delle modifiche dell'attributo nel seguente argomento:

```
iotdevice-1/notify
```


### Il formato del messaggio per una richiesta di notifica delle modifiche dell'attributo


Formato richiesta:

```
Outgoing message from the device:

Topic: iotdevice-1/notify
{
    "d": {
        "fields": [
				{
                "field": "field_name",
                "value": "field_value"
            }
        ]
    }
    "reqId": "string"
}
```

Formato risposta:

```
Incoming message from the server:

Topic: iotdm-1/response
	{
    "rc": number,
    "reqId": "string"
}
```

### Codici di risposta per una richiesta di notifica delle modifiche dell'attributo

|Codice di risposta |Messaggio |
|:---|:---|
|200   |L'operazione è riuscita.|
|400   |Il messaggio di input non corrisponde al formato previsto o uno dei valori è fuori dall'intervallo valido.|
|404   |Il dispositivo non è stato registrato con {{site.data.keyword.iot_short_notm}}.|
|409   |La risorsa non può essere aggiornata a causa di un conflitto (ad esempio, la risorsa viene aggiornata da due richieste simultanee). L'aggiornamento può essere ritentato successivamente.|
|500   |Si è verificato un errore interno|
