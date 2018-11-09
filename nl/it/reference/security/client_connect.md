---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Monitoraggio della connettività client (Beta)
{: #connect_status}

{{site.data.keyword.iot_full}} ti consente di interrogare e monitorare lo stato di connessione dei client. Puoi sapere in tempo reale se un client è connesso e puoi risolvere eventuali problemi di connessione.
{:shortdesc}

Ad esempio, puoi vedere quando un client era attivo per l'ultima volta e perché è stato disconnesso oppure puoi visualizzare tutti i client che non si sono connessi nel giorno precedente in modo da poter risolvere eventuali problemi.

**Importante:** la funzione di monitoraggio della connettività client è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Interrogazione dello stato di connessione del client
{: #query_status}

L'API dello stato della connessione client ti consente di richiamare e interrogare lo stato della connessione per tutti i client che sono connessi a {{site.data.keyword.iot_short_notm}}.

Quando interroghi lo stato di connessione, puoi visualizzare le seguenti informazioni:

 - lo stato di connettività (connesso o disconnesso)
 - l'ora dell'ultima connessione, in formato ISO 8601
 - l'ora dell'ultima disconnessione, in formato ISO 8601
 - l'ultimo evento di attività (ad esempio, la pubblicazione MQTT o la sottoscrizione MQTT)
 - l'ora dell'ultima attività, in formato ISO 8601
 - l'entità che ha causato la disconnessione, se il client è disconnesso (client, server o rete)
 - il codice motivo MQTT per la disconnessione, se il client è disconnesso

**Esempi**

Per richiamare lo stato di connessione client per un singolo client:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

Per richiamare lo stato di connessione di tutti i client:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

Per richiamare tutti i client connessi:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

Per richiamare tutti i client che sono stati attivi negli ultimi due giorni:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**Nota:** potrebbe esserci un breve ritardo tra il momento in cui un client si connette e il tempo in cui lo stato della connessione nell'API riflette lo stato corretto.

Per ulteriori informazioni sull'API, inclusi tutti i filtri di query disponibili, consulta [la documentazione Swagger sull'API dello stato della connessione client ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}.

## Monitoraggio dello stato di connessione del client
{: #monitor_status}

 Tutti i client dispongono di un argomento di monitoraggio che ti consente di effettuare la sottoscrizione per ricevere gli aggiornamenti dello stato di connettività dal vivo per il client. Per ulteriori informazioni, vedi [Sottoscrizione ai messaggi di stato del dispositivo](../../applications/mqtt.html#subscribe_device_commands).

## Risoluzione dei problemi relativi alla connettività client

L'API dei log di connessione ti consente di richiamare un elenco di eventi di log di connessione per un dispositivo per diagnosticare i problemi di connettività. Le voci registrano le connessioni riuscite, i tentativi di connessione non riusciti, le disconnessioni intenzionali e le disconnessioni avviate dal server.

Per ulteriori informazioni, consulta il [documento Swagger sull'API dei log di connessione ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}.
