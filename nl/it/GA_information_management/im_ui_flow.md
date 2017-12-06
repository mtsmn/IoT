---

copyright:
years: 2017
lastupdated: "2017-08-10"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione alla gestione dei dati utilizzando l'interfaccia web (Beta)
{: #gs_web}

**Importante:** la funzione dell'interfaccia web della gestione dei dati {{site.data.keyword.iot_full}} è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} fornisce strumenti online per aiutarti a normalizzare e trasformare i dati come parte della funzione di gestione dei dati. In aggiunta a [Watson IoT Platform Data Management API ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} che è stato fornito, puoi utilizzare Simple Interface Creator o Advanced Interface Creator per configurare le risorse e iniziare ad associare il tuoi dati del dispositivo. I creatori dell'interfaccia online ti guidano attraverso la procedura nell'interfaccia utente.

Per informazioni sulla funzione di gestione dei dati e i relativi vantaggi, consulta [Introduzione alla gestione dei dati](../GA_information_management/ga_im_device_twin.html#device_twins).

Per informazioni sulle risorse che necessario configurare, consulta [Informazioni sulla gestione dei dati](../GA_information_management/ga_im_definitions.html#definitions_resource).

Con Simple Interface Creator, aggiungi un'interfaccia fisica a una logica viene automaticamente creata e le interfacce vengono associate tra di loro. Con Advanced Interface Creator, hai più controllo sulla configurazione. Aggiungi un'interfaccia fisica e una o più logiche e quindi aggiungi le associazioni tra di esse.

Le interfacce e le associazioni vengono aggiunte nel formato bozza. Dopo averle aggiunte e convalidate, devi attivarle. Per ulteriori informazioni sulle risorse attiva e di bozza, consulta [Creazione, aggiornamento, attivazione e disattivazione delle tue risorse](../GA_information_management/ga_im_definitions.html#draft_active_resources).



## Flusso di elevato livello
{: #interface_flow}

Utilizza la seguente procedura come supporto per la configurazione delle risorse di cui hai bisogno per iniziare a utilizzare la funzione di gestione dei dati.

**Prima di iniziare**
Questa procedura presuppone che tu abbia un tipo di dispositivo con almeno una dispositivo registrato e collegato. Per ulteriori informazioni su questi prerequisiti e per istruzioni su come creare un tipo di dispositivo e registrare un dispositivo, consulta [Configurazione dei dispositivi per l'utilizzo con la funzione di gestione dei dati](im_config_devices.html).

1. Seleziona il tipo di dispositivo di cui desideri normalizzare e trasformare i dati. 
2. Seleziona Simple Interface Creator o Advance Interface Creator:

a. Flusso semplice  
   i. [Crea un'interfaccia fisica di bozza](#create_physical_interface_simple).  
   
b. Flusso avanzato  
   i. [Crea un'interfaccia fisica di bozza](#create_physical_interface_advanced).  
   ii. [Crea una o più interfacce logiche di bozza](#create_logical_interface). (Questo passo viene completato automaticamente quando utilizzi il flusso semplice.)  
   iii. [Associa l'interfaccia fisica alle interfacce logiche](#create_interface_mappings).  
     
     
3. [Imposta le preferenze di notifica](#set_notifications).
4. [Attiva la configurazione](#validate_activate).

*Nota:* puoi anche configurare la funzione di gestione dei dati utilizzando le API. Per informazioni dettagliare sulla configurazione della funzione di gestione dei dati per normalizzare e trasformare i tuoi dati del dispositivo utilizzando le API, consulta [Introduzione alla gestione dei dati]{ga_im_example.html#im_example}. [Manuale passo dopo passo: un esempio dettagliato su come utilizzare i dispositivi tramite un'interfaccia comune](../GA_information_management/ga_im_index_scenario.html#scenario) ti guida nella procedura per creare un'interfaccia logica del tipo di dispositivo per i dispositivi termometro eterogenei.

Per i dettagli sull'API, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Creazione di un'interfaccia fisica di bozza (flusso semplice)
{: #create_physical_interface_simple}

1. Dal menu di navigazione principale, seleziona **Devices**.
2. Seleziona **Device Types** e seleziona il tipo di dispositivo per cui vuoi creare un'interfaccia. Puoi anche [creare un nuovo tipo di dispositivo].
3. Visualizza e aggiorna le informazioni sul tipo di dispositivo, se necessario, e salva tutte le modifiche.
4. Fai clic su **Interface**.
5. Fai clic su **Simple Flow** per aprire Simple Interface Creator.
6. Fai clic su **Create Interface**. 
7. Fai clic su **Add Property** per iniziare ad aggiungere gli eventi e le proprietà all'interfaccia fisica.  
 a. Nella finestra **Add Properties to the Interface**, seleziona gli eventi che desideri aggiungere all'interfaccia. Il sistema è in ascolto per gli eventi attivi per i dispositivi collegati del tipo di dispositivo selezionato. Puoi anche selezionare l'ID del dispositivo dell'ultimo evento in cache o selezionare un altro evento dall'elenco.  
  b. Seleziona le proprietà dall'evento e fai clic su **Add**.  
  c. Aggiungi ulteriori proprietà se necessario.
8. Fai clic su **Done**. L'interfaccia fisica è stata creata. Se desideri aggiungere ulteriori dettagli, puoi aggiornare l'interfaccia selezionando il link **Use Advanced Interface Creator**.


## Creazione di un'interfaccia fisica di bozza (flusso avanzato)
{: #create_physical_interface_advanced}

1. Dal menu di navigazione principale, seleziona **Devices**.
2. Seleziona **Device Types** e seleziona il tipo di dispositivo per cui vuoi creare un'interfaccia. Puoi anche [creare un nuovo tipo di dispositivo].
2. Visualizza le informazioni sul tipo di dispositivo e fai clic su **Interface**.
4. Fai clic su **Advanced Flow** per aprire Advanced Interface Creator.
5. Fai clic su **Create Interface**
7. Nella scheda **Identity** della pagina **Create Physical Interface**, immetti un nome e una descrizione dell'interfaccia fisica.
7. Facoltativo: attiva lo switch Edge Analytics se stai utilizzando dei dispositivi abilitati per Edge con l'interfaccia.
8. Fai clic su **Avanti**.
9. Definisci l'interfaccia fisica aggiungendo i payload e i tipi di evento:
 a. Fai clic su **Create Event Type**. 
 b. Seleziona l'ultimo evento in cache, seleziona un evento dall'elenco o aggiungine uno manualmente.
10. Fai clic su **Done**. L'interfaccia fisica è stata creata nel formato bozza. Puoi procedere con l'aggiunta di una o più interfacce logiche.

## Creazione di un'interfaccia logica di bozza
{: #create_logical_interface}

1. Dopo aver creato l'interfaccia fisica di bozza, fai clic su **Add Logical Interface**.
2. Nella scheda **Identity** della pagina **Create Logical Interface**, immetti un nome e una descrizione dell'interfaccia logica. 
3. Fai clic su **Avanti**.
4. Fai clic su **Add Property** per iniziare a definire le proprietà dell'interfaccia logica.
5. Nella finestra **Create Property** seleziona le proprietà dall'interfaccia fisica che desideri aggiungere all'interfaccia logica e salva le modifiche.
6. Aggiungi ulteriori interfacce logiche se necessario e fai quindi clic su **Next**.
7. {: #set_notifications}Configura le preferenze di notifica per l'interfaccia. Seleziona una delle seguenti opzioni per determinare quando devono essere inviate le notifiche di stato del dispositivo:
 - Nessuna notifica evento - Non viene inviata alcuna notifica. Puoi utilizzare l'API REST per richiamare le informazioni sulla modifica dello stato del dispositivo.
 - Alla modifica dello stato - Riceverai una notifica quando viene modificato lo stato del dispositivo.
 - Per ogni evento - Riceverai una notifica ogni volta in cui la piattaforma elabora un evento per il dispositivo, anche se non provoca un cambiamento di stato.
8. Fai clic su **Done**. Le interfacce sono state create nel formato bozza.
9. {: #validate_activate} Fai clic su **Deploy** per convalidare la configurazione.
10. Fai clic su **Deploy** per attivare la configurazione. Lo stato della configurazione è stato modificato con distribuito. 

