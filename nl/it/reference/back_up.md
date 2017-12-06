---

copyright:

years: 2017
lastupdated: "2017-08-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Backup dei dati
{: #back_up}

Utilizza le seguenti informazioni per comprendere la strategia di backup dei dati per {{site.data.keyword.iot_full}}.

## Per quale tipo di dati viene eseguito il backup?

Al momento viene eseguito il backup dei seguenti tipi di dati client come parte della strategia {{site.data.keyword.iot_short_notm}}:

- Informazioni organizzative 
- Informazioni sul dispositivo
- Chiavi API e token
- Informazioni sull'utente
- Tutti i record delle richieste di gestione dei dati, inclusa la cronologia di tutte le richieste avviate - ad esempio lo stato corrente della richiesta
- Definizioni dei bundle della richiesta di gestione del dispositivo personalizzate

## Per quale tipo di dati non viene eseguito il backup? 

Non viene eseguito il backup per i seguenti tipi di dati in {{site.data.keyword.iot_short_notm}}:

- Eventi del dispositivo
- Stato di messaggistica transitorio, ad esempio i dati in elaborazione
- Regole di analisi e configurazione avvisi
- Messaggi MQTT inviati e ricevuti come parte di una richiesta di gestione del dispositivo

## Quanto frequentemente viene eseguito il backup dei dati e dove viene archiviato?

Viene eseguito il backup dei dati ogni 24 ore.

I dati vengono archiviati all'esterno del servizio {{site.data.keyword.iot_short_notm}} principale, fornendo ridondanza geografica e consentendo il ripristino dei servizi nel caso di un incidente significativo. I clienti saranno contattati se il ripristino dei dati fosse richiesto. Tutti i dati sono codificati e sono conformi con i requisiti di protezione dei dati locali.

Le seguenti ubicazioni esterne sono al momento utilizzate per il backup dei dati:

Ubicazione                   | Ubicazione di backup 
------------- | -------------
Bluemix Stati Uniti Sud (Dallas)| Washington
Bluemix Regno Unito (Londra) | Francoforte
Bluemix Germania (Francoforte) | Londra
Bluemix Dedicato| Come richiesto dal cliente al momento dell'ordine di {{site.data.keyword.iot_short_notm}} Dedicato

**Nota:** le ubicazioni potrebbero venire modificate nel futuro per riflettere le leggi sulla privacy dei dati, ad esempio il potenziale impatto del Brexit sulle regole di sovranità dei dati in Europa:
