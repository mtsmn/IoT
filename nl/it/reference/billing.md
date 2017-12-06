---
copyright:
  years: 2016, 2017
lastupdated: "2017-09-06"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Fatturazione

I piani del servizio {{site.data.keyword.iot_full}} a pagamento (piani diversi dal 'Lite'), si basano sul concetto di megabyte scambiati e analizzati nel periodo di un mese.  Questo documento illustra come {{site.data.keyword.iot_short_notm}} misura i dati per creare le informazioni di utilizzo che determinano il costo di utilizzo del servizio.  Le informazioni di utilizzo possono essere utilizzate per approssimare il costo di utilizzo di {{site.data.keyword.iot_short_notm}} in base alla progettazione e al numero di dispositivi, applicazioni e gateway.

Per informazioni sul costo di ogni megabyte di dati scambiati o analizzati, consulta il servizio {{site.data.keyword.iot_short_notm}} nel catalogo Bluemix della regione richiesta.

Puoi utilizzare [Pricing Calculator ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://iot-cost-calculator.ng.bluemix.net/) per aiutarti nel calcolare il costo di un servizio {{site.data.keyword.iot_short_notm}}.

I seguenti tre elementi sono misurati separatamente e fatturati in base all'utilizzo: 
- dati scambiati
- dati analizzati
- dati edge analizzati

## Dati scambiati
Il calcolo dei *dati scambiati* include i dati dell'account scambiati dalle applicazioni, dai dispositivi e dai gateway utilizzando la messaggistica MQTT o HTTP, così come i dati scambiati dalle applicazioni utilizzando l'API HTTP.

### Dati MQTT scambiati
Quando utilizzi MQTT, il contenuto del flusso TCP conta i *dati scambiati*.  Il payload di ogni messaggio MQTT viene incluso nel cumulo dei dati scambiati utilizzando il protocollo MQTT.  Il cumulo della dimensione dei dati del payload si verifica quando viene pubblicato il messaggio, così come quando li messaggio viene distribuito a un sottoscrittore.

Il protocollo MQTT è progettato per essere il più leggero possibile. Sebbene {{site.data.keyword.iot_short_notm}} includa le dimensioni dei pacchetti MQTT quando si accumulano i *dati scambiati*, poiché è un protocollo leggero, questo sovraccarico è lento.  La seguente tabella fornisce un'indicazione della dimensione minima di ogni pacchetto MQTT:

|PACCHETTO MQTT                    |Byte dimensione minima  |Variabili e dimensione minima associata in byte|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |ID client (12 se d:xxxxxx:t:i), l'argomento sarà (0) e il messaggio sarà (0) se impostati, nome utente (14 per i dispositivi - 'use-token-auth') e password (18 se autogenerata)|
|CONNACK                        |4                   |Nessuno|
|PUBLISH                        |21                  |Nome argomento (17 if iot-2/evt/i/fmt/f) e payload (0 è il minimo).  Applicato sia ai pacchetti PUBLISH in entrata che in uscita.|
|PUBACK                         |4                   |Nessuno|
|PUBREC                         |4                   |Nessuno|
|PUBREL                         |4                   |Nessuno|
|PUBCOMP                        |4                   |Nessuno|
|SUBSCRIBE                      |26                  |Può includere multipli dello stesso filtro argomento (19 se iot-2/evt/i/fmt/f) e QoS (1)|
|SUBACK                         |5                   |(1) per ogni filtro argomento in SUBSCRIBE|
|UNSUBSCRIBE                    |23                  |Può includere più filtri argomento (19 se iot-2/evt/i/fmt/f)|
|UNSUBACK                       |4                   |Nessuno|
|PINGREQ                        |2                   |Nessuno|
|PINGRESP                       |2                   |Nessuno|
|DISCONNECT                     |2                   |Nessuno|

Quando utilizzi TLS, viene considerato anche l'handshake sicuro. L'handshake è approssimativamente 8 kilobyte. È quindi economicamente vantaggioso mantenere le connessioni MQTT aperte il più a lungo possibile. Aprire le connessioni comporta solo la sovrascrittura PINGREQ e PINGRESP (4 byte) per intervallo keepalive che è specifico per il client e dipende dal periodo keepalive specificato.  Riaprire una connessione TLS utilizzando una sessione TLS esistente diminuisce il numero di byte scambiati perché i certificati non sono inviati in entrambe le direzioni.  Molti client TLS supportano questa impostazione in modo predefinito, ma la sessione ha una durata limitata.  L'utilizzo dei certificati client aumenta la dimensione dell'handshake TLS, a seconda della dimensione del certificato client. 

I record TLS e le sovrascritture TCP e IP non sono considerati.

**Nota** - Quando utilizzi il dashboard {{site.data.keyword.iot_short_notm}} per visualizzare un dispositivo, viene eseguita una sottoscrizione MQTT in modo che il dashboard possa visualizzare gli eventi live.  Questa sottoscrizione MQTT conta anche la quantità totale dei dati scambiati.

### Dati messaggistica HTTP scambiati
Quando utilizzi la messaggistica HTTP, il payload del messaggio viene incluso nel cumulo dei *dati scambiati*, quando pubblichi gli eventi o ricevi i comandi.

Come con MQTT, la sovrascrittura HTTP viene contata.  La sovrascrittura HTTP è significativamente maggiore della sovrascrittura MQTT. La sovrascrittura HTTP per richiesta è approssimativamente 300 byte. Deve inoltre essere aggiunta la dimensione del payload del messaggio.

Quando utilizzi TLS, viene considerato l'handshake sicuro. L'handshake è approssimativamente 8 kilobyte. È quindi economicamente vantaggioso mantenere le connessioni HTTPS aperte il più a lungo possibile. Aprire le connessioni non comporta ulteriore sovrascrittura in termini di *dati scambiati*.  Riaprire una connessione TLS utilizzando una sessione TLS esistente diminuisce il numero di byte scambiati perché i certificati non sono inviati in entrambe le direzioni.  Molti client TLS supportano questa impostazione in modo predefinito, ma la sessione ha una durata limitata.  L'utilizzo dei certificati client aumenta la dimensione dell'handshake TLS, a seconda della dimensione del certificato client.

I record TLS e le sovrascritture TCP e IP non sono considerati.

### Dati API HTTP scambiati
Quando viene richiamata un'API da un'applicazione, sia i corpi della richiesta che della risposta accumulano i *dati scambiati*.  Le dimensioni possono variare significativamente a seconda dell'API.

A differenza della messaggistica MQTT e HTTP, né il sovraccarico HTTP che TLS vengono considerati per i dati API HTTP scambiati.

**Nota** - Quando utilizzi il dashboard {{site.data.keyword.iot_short_notm}}, le API HTTP vengono utilizzate dal dashboard per elencare le informazioni inclusi i dispositivi, i tipi di dispositivo e i log di connessione del dispositivo.  Queste API HTTP richiamano il conteggio di *dati scambiati*.

## Dati analizzati
Il calcolo dei *dati analizzati* misura i dati dell'evento elaborati dal motore delle regole nella piattaforma.  I dati sono considerati elaborati dal motore delle regole quando gli eventi del dispositivo sono valutati da una o più regole, in base a un dispositivo e un tipo di evento specifici. 

## Dati Edge analizzati 
Il calcolo dei *dati edge analizzati* misura i dati dell'evento elaborati in un dispositivo gateway da EAA (Edge Analytics Agent) {{site.data.keyword.iot_short_notm}}.  I dati sono considerati elaborati dall'agent edge quando gli eventi del dispositivo sono valutati da una o più regole edge, in base a un dispositivo e un tipo di evento specifici. 
