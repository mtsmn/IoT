---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configurazione delle regole di instradamento di {{site.data.keyword.iot_short_notm}} Edge (Anteprima)
{: #edge_rules}

Le regole di instradamento di {{site.data.keyword.iot_full}} Edge definiscono il modo in cui i messaggi vengono inoltrati all'interno del nodo Edge.

**Importante:** la funzione {{site.data.keyword.iot_full}} Edge è disponibile solo come parte di un programma di anteprima limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Per un nodo Edge puoi definire più regole di instradamento. Le regole vengono valutate per ciascun messaggio che un dispositivo o un'applicazione locale pubblica direttamente sul nodo Edge. I messaggi pubblicati sul nodo Edge dal cloud vengono sempre pubblicati localmente e non sono influenzati dall'instradamento.

## Formato delle regole di instradamento
{: #edge_rules_format}

Una regola di instradamento è definita come oggetto JSON nel seguente formato:

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

Una regola di instradamento viene specificata come array degli oggetti JSON nel file `routing.json` che è memorizzato nella cartella `/etc/wiotp-edge` sul nodo Edge.

Le proprietà di ogni regola di instradamento sono definite come segue:

Proprietà      | Tipo     | Descrizione       
------------- | -----------| -----------
rule_id | numero intero, obbligatorio | Un identificativo per la regola. Le regole di instradamento vengono valutate in ordine crescente in base al rule_id. Non ci deve essere più di una regola con lo stesso rule_id.
matching_filter | stringa, obbligatorio | Utilizzato per valutare se la regola debba essere applicata a un messaggio pubblicato. Deve essere nel formato di una sottoscrizione MQTT. Sono supportati i caratteri jolly MQTT a livello singolo (+) e multilivello (#). Questo filtro viene valutato rispetto all'argomento in cui è pubblicato il messaggio. Questa proprietà è definita nel formato dell'argomento dell'applicazione {{site.data.keyword.iot_short_notm}} e include i campi `/type/deviceType/id/deviceId`. Ad esempio: `"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
Destination | stringa, obbligatorio | Definisce dove deve essere inoltrato il messaggio. Imposta uno dei seguenti valori: `"CLOUD"`, `"IM"` (per inoltrare il messaggio al componente Information Management) o `"LOCAL"` (per inoltrare il messaggio localmente sul nodo Edge). Ad esempio, `"destination" : "CLOUD"`
continue_matching | booleano, facoltativo, predefinito = true | Specifica se ulteriori messaggi devono essere valutati utilizzando questa regola se la regola viene soddisfatta una sola volta. Questa proprietà fornisce un controllo migliore sull'istradamento quando esistono regole di instradamento sovrapposte.
forward_skip_levels | numero intero, facoltativo, predefinito = 0) | Specifica il numero di livelli dell'argomento del messaggio originale che devono essere rimossi quando il messaggio viene ripubblicato. Il valore predefinito è 0, che indica che l'argomento originale viene mantenuto. Ad esempio, se `forward_skip_levels` è impostato su `1` e un messaggio è pubblicato nell'argomento `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json`, il messaggio viene ripubblicato nell'argomento `iot-2/type/T1/id/D1/evt/E1/fmt/json`.
store | booleano, facoltativo, predefinito = true) | Definisce se la funzione store-and-forward (memorizza e inoltra) deve essere applicata ai messaggi instradati da questa regola. Questa proprietà si applica solo quando la proprietà di destinazione è impostata su `"CLOUD"`. Se questa proprietà è impostata su false, i messaggi che corrispondono alla regola non vengono memorizzati quando il nodo Edge viene disconnesso dal cloud.

## Esempio di file routing.json
{: #edge_rules_example}

Nel seguente esempio, sono configurate tre regole:
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

La prima regola instrada tutti gli eventi di tipo `AlarmEvent` direttamente al cloud.

La seconda regola instrada tutti gli eventi pubblicati su un argomento che inizia con `sendToCloud/iot-2` direttamente al cloud. Il messaggio viene inoltrato in un argomento che inizia con `iot-2` e non include il prefisso `sendToCloud/`.

La terza regola è la regola predefinita, che instrada tutti gli altri messaggi al broker MQTT locale. Questi messaggi possono quindi essere utilizzati da qualsiasi utente locale con una sottoscrizione corrispondente. L'installazione del nodo Edge include la regola predefinita per instradare tutto localmente, in `/etc/wiotp-edge/routing.json`.

## Modifica delle regole di instradamento
{: #edge_rules_edit}

Puoi modificare le regole direttamente nel file `/etc/wiotp-edge/routing.json`. Dopo aver modificato il file, salva le modifiche ed esegui quindi `docker ps` per determinare l'ID del contenitore edge-connector. Riavvia il contenitore docker utilizzando tale ID.

## Risoluzione dei problemi relativi alle regole di instradamento
{: #edge_rules_ts}

Se il contenitore edge-connector continua a riavviarsi dopo una modifica in `routing.json`, significa che il file delle regole di instradamento contiene errori di sintassi. Controlla il file di log `/var/wiotp-edge/trace/edgeConnector_trace.log` per visualizzare i messaggi di errore.
