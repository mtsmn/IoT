---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guide introduttive a {{site.data.keyword.iot_short_notm}}
{: #getting-started}

Le guide introduttive sono progettate per guidarti attraverso le basi dello sviluppo di un sistema prototipo IoT end-to-end pronto per la produzione {{site.data.keyword.iot_full}} e sono scritte per gli sviluppatori che non hanno dimestichezza con l'utilizzo di {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

Ogni guida fornisce un processo passo dopo passo che ti conduce velocemente a una soluzione in esecuzione e distribuita che illustra una o più funzioni {{site.data.keyword.iot_short_notm}}.

## Utilizzo delle guide  
{: #using-guides}
In ogni guida, ottieni almeno un esempio originato da GitHub codificato in base alle procedure consigliate {{site.data.keyword.iot_short_notm}}. Questi esempi possono essere ridimensionati, protetti, integrati con i sistemi e possono generalmente adattarsi ai tuoi processi di creazione e sviluppo devOps.

Gli utenti inclini tecnicamente possono espandere una soluzione veloce adattando il codice di esempio ai loro propri ambienti o completando la soluzione veloce con esempi hardware che simulano più attentamente cosa viene riscontrato nell'ambiente di produzione. Le guide forniscono ulteriori link alla documentazione rilevante per l'attività, alle documentazioni API, alle librerie client (SDK), ai video didattici e ad altro materiale formativo.

Puoi seguire le guide in ordine, dove la prima guida fornisce una base dalla quale utilizzare le seguenti guide. Se desideri saltare tra le guide e seguire il tuo proprio percorso nella serie, ogni guida viene fornita con un elenco di requisiti. I requisiti ti illustrano il formato dei dati del dispositivo richiesti da queste guide e puoi configurare il tuo proprio ambiente di conseguenza.
{: tip}

## Elenco delle guide
{: #list-of-guides}  

Sono disponibili le seguenti guide:

| Guida | Descrizione |    
| ----- | ---- |   
| [1. Connessione di un dispositivo nastro trasportatore](getting-started-iot-conveyor.html) | Inizia installando un'organizzazione {{site.data.keyword.iot_short_notm}} e collegando un'applicazione di simulazione nastro trasportatore originata da GitHub o, se preferisci qualcosa di più fisico, il tuo proprio simulatore nastro trasportatore basato su Raspberry Pi. </br> Quindi configura alcune schede dashboard {{site.data.keyword.iot_short_notm}} di base per visualizzare e monitorare i tuoi dati del dispositivo. |   
| [2. Monitoraggio dei tuoi dati del dispositivo](getting-started-iot-monitoring.html) | Collega un'applicazione di monitoraggio Node.js o basata sulla libreria widget per visualizzare i dati del nastro trasportatore in tempo reale.  
| [3. Simulazione di un grande numero di dispositivi](getting-started-iot-large-scale-simulation.html) | Espandi la simulazione del dispositivo di esempio aggiungendo un gran numero di simulatori del dispositivo al tuo ambiente. </br>**Importante** l'applicazione richiede 512 MB di memoria, che è maggiore alla quantità allocata per impostazione predefinita e potrebbe anche superare la quantità disponibile per gli account di prova gratuita, che devono prima essere aggiornati a un account a pagamento a consumo o di sottoscrizione. |   
