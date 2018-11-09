---

copyright:

years: 2017, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Quote
Questa sezione fornisce le informazioni sui limiti impostati per i servizi {{site.data.keyword.iot_full}} per tipo di piano. I limiti sono qui forniti come parte della politica di utilizzo corretta per aiutarti a garantire che le prestazioni non vengano influenzate negativamente dall'utilizzo improprio di tutti gli utenti del sistema multitenant. I limiti ti aiutano inoltre ad impedire che il carico di lavoro dell'utente reale venga inavvertitamente identificato come un attacco DoS (Denial of Service).

## Introduzione
{: #metrics_about}

Le quote specificano i limiti superiori configurati per una risorsa. Quando l'utilizzo supera un limite specificato, le richieste utente potrebbero venire ritardate o negate. In circostanze eccezionali, superare i limiti che riguardano la stabilità del sistema potrebbe provocare la sospensione dell'organizzazione {{site.data.keyword.iot_short_notm}} per impedire che vengano interessati altri utenti.

Le seguenti tabelle elencano i limiti per {{site.data.keyword.iot_short_notm}}. Molti dei limiti specificati sono per organizzazione {{site.data.keyword.iot_short_notm}}, a meno che esplicitamente elencati come per dispositivo. Alcuni dei limiti hanno un valore massimo imposto, mentre altri limiti possono essere aumentati su richiesta. Se hai bisogno di limiti superiori a quelli specificati, apri un [ticket ![Icona link esterno](../../../icons/launch-glyph.svg)](https://support.ng.bluemix.net/gethelp/){: new_window}.

## Messaggistica
{: #messaging_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard     | Piano dedicato
------------- | -------------|------------- |
Numero massimo di tipi di dispositivo |50 |1000 |1000
Numero massimo di dispositivi connessi contemporaneamente | 500| 500K. Se desideri collegare più di 50.000 dispositivi, contattaci per discutere dei tuoi piani. | 500K
Numero massimo di dispositivi registrati |500 | 1 milione | 100K per incremento
Numero massimo di applicazioni ['A' - condivise](../applications/mqtt.html#scalable_apps) |1 | 10 sottoscrizioni condivise, ognuna con 50 istanze|10 sottoscrizioni condivise, ognuna con 50 istanze
Numero massimo di applicazioni ['a' - non condivise](../applications/mqtt.html#client_connections) |500 chiavi API | 2000 chiavi API | 2000 chiavi API
Numero massimo di dati al mese | 200 MB | illimitato |illimitato
Connessioni dispositivo|10/sec | 300/sec |300/sec
Invii dispositivo al cloud (MQTT) - Dispositivo | 5 msg/sec o 20 KB/sec | 10 msg/sec o 40 KB/sec |10 msg/sec o 40 KB/sec
Invii dispositivo al cloud (MQTT) - Gateway  | 50 msg/sec o 200 KB/sec | 100 msg/sec o 400 KB/sec| 100 msg/sec o 400 KB/sec
Invii dispositivo al cloud (MQTT) - Applicazione | 10 msg/sec o 40 KB/sec | 20 msg/sec o 80 KB/sec| 20 msg/sec o 80 KB/sec
Invii dispositivo al cloud (MQTT) - per organizzazione | 5000 msg/sec | 6000 msg/sec | 6000 msg/sec
Invii dispositivo al cloud (HTTP) - Dispositivo | 30 msg/min o 2 KB/sec | 1 msg/sec o 4 KB/sec | 1 msg/sec o 4 KB/sec
Invii dispositivo al cloud (HTTP) - Gateway | 5 msg/sec o 20 KB/sec | 10 msg/sec o 40 KB/sec | 10 msg/sec o 40 KB/sec
Invii dispositivo al cloud (HTTP) - Applicazione | 1 msg/sec o 4 KB/sec| 2 msg/sec o 8 KB/sec | 2 msg/sec o 8 KB/sec
Invii dispositivo al cloud (HTTP) - per organizzazione | 500 msg/sec | 600 msg/sec | 600 msg/sec
Invii cloud al dispositivo (MQTT) - Dispositivo  |1 msg/sec |2 msg/sec o 8 KB/sec |2 msg/sec o 8 KB/sec
Invii cloud al dispositivo (MQTT) - Gateway| 10 msg/sec |20 msg/sec o 80 KB/sec  |20 msg/sec o 80KB/sec  
Invii cloud al dispositivo (MQTT) - Applicazione | 10 msg/sec |20 msg/sec o 80 KB/sec |20 msg/sec o 80 KB/sec  
Invii cloud al dispositivo (MQTT) - per organizzazione |1000 msg/sec | 12K msg/sec|12K msg/sec
Invii cloud al dispositivo (HTTP) - frequenza continua per dispositivo | 30 msg/min| 30 msg/min  | 30 msg/min
Invii cloud al dispositivo (HTTP) - per organizzazione |  1000 msg/sec|  1200 msg/sec  |  1200 msg/sec
Numero massimo di messaggi non riconosciuti in entrata - per dispositivo | 10 |10 |10
Numero massimo di messaggi non riconosciuti in entrata - per gateway | 1000 |1000 |1000
Numero massimo di messaggi non riconosciuti in entrata - per connessione applicazione  |2000 | 2000|2000
Numero massimo di messaggi non riconosciuti in uscita - per dispositivo |10  |10 |10
Persistenza dispositivo a cloud | 24 ore|24 ore |24 ore
Persistenza cloud a dispositivo |48 ore (predefinito) Massimo 7 giorni e 50 profondità coda| 48 ore (predefinito) Massimo 7 giorni e 50 profondità coda  |48 ore (predefinito) Massimo 7 giorni e 50 profondità coda
Intervallo tra tentativi massimo per la distribuzione di messaggi QoS 1 | Ritenta ricollegandoti |Ritenta ricollegandoti |Ritenta ricollegandoti
Limite di inattività connessione (intervallo keepalive) | Specificato dal client alla connessione |Specificato dal client alla connessione  |Specificato dal client alla connessione
Durata connessione WebSocket |Specificato dal client alla connessione | Specificato dal client alla connessione  |Specificato dal client alla connessione
Dimensione massima messaggi | 128 KB |128 KB |128 KB
Numero massimo di sottoscrizioni per dispositivo |50 |50 |50
Numero massimo di sottoscrizioni per applicazione | 500 |500 |500
Profondità coda - numero massimo di messaggi in buffer per sottoscrizione per dispositivo | 50 |50 |50
Numero massimo di messaggi in buffer per l'applicazione 'A' |50K durevoli, 50K non durevoli |50K durevoli, 50K non durevoli |50K durevoli, 50K non durevoli


## API
{: #api_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Chiamate API |10/sec |10/sec |10/sec

## Motore della regola
{: #rule_engine_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Numero massimo di regole per organizzazione |100|1000|1000
Azioni per regola|10|10|10

## Gestione del dispositivo
{: #device_mgmt_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Dimensione massima dei log di diagnostica per dispositivo |500K|500K|500K
Versioni cronologiche massime delle sospensioni dei log di diagnostica|2  |2 |2
Tempo massimo di sospensione dei log diagnostica|7 giorni| 7 giorni|7 giorni
Numero massimo di azioni avviate per messaggio in elaborazione |200 |200 |200
Numero massimo di dispositivi per azione |5000 |5000 | 5000

## Ultima cache evento
{: #last_event_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Numero massimo di ID evento per dispositivo |5|5|5
Numero massimo di formati |2|3|3
Scadenza da ultima cache evento |7 giorni |45 giorni | 45 giorni

## Estensioni
{: #extensions_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Numero massimo di a cui puoi associarti |12|12|12

## Gestione delle informazioni
{: #information_management_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Profondità nidificazione massima per payload JSON |5|5|5
Dimensione stringa JSON massima |512|512|512

## Sicurezza
{: #security_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Numero massimo di ruoli personalizzati |20|20|20
Numero massimo di membri utente |1000|1000|1000
Numero massimo di dispositivi in un gruppo di risorse |300|300|300
Numero massimo di gruppi di risorse assegnati a un gateway |10|10|10
Numero massimo di gruppi di risorse a cui un dispositivo può appartenere |10|10|10

## Interfaccia utente
{: #UI_metrics}

Metrica        | Piano lite      | Piani sicurezza avanzata e standard       | Dedicato
------------- | -------------|------------- |
Numero massimo di dashboard |50|50|50
Numero massimo di schede nella tabella |30|30|30
