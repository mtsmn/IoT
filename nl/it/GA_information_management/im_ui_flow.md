---

copyright:
years: 2016, 2018
lastupdated: "2018-05-14"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione alla gestione dei dati utilizzando l'interfaccia web
{: #gs_web}

{{site.data.keyword.iot_full}} fornisce strumenti online per aiutarti a normalizzare e trasformare i dati come parte della funzione di gestione dei dati.

## Panoramica
{: #web_overview}

In aggiunta a [Watson IoT Platform Data Management API ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} che è stato fornito, puoi utilizzare Simple Interface Creator o Advanced Interface Creator per configurare le risorse e iniziare ad associare il tuoi dati del dispositivo. I creatori dell'interfaccia online ti guidano attraverso la procedura nell'interfaccia utente.

Un'interfaccia fisica viene utilizzata per modellare l'interfaccia tra un dispositivo fisico e Watson IoT Platform. I tipi di evento possono essere associati all'interfaccia fisica. Un'interfaccia logica viene utilizzata per definire la vista normalizzata sullo stato del dispositivo in Watson IoT Platform e viene generalmente utilizzata dalle applicazioni. Un'interfaccia logica deve essere associata a uno schema dell'interfaccia logica. Lo stato viene aggiornato in risposta ad eventi del dispositivo in entrata.

Per ulteriori informazioni sulla funzione di gestione dei dati e i relativi vantaggi, consulta [Introduzione alla gestione dei dati](../GA_information_management/ga_im_device_twin.html#device_twins).

Per informazioni sulle risorse che necessario configurare, consulta [Informazioni sulla gestione dei dati](../GA_information_management/ga_im_definitions.html#definitions_resource).

Con Simple Interface Creator, aggiungi un'interfaccia fisica e quindi viene creata automaticamente un'interfaccia logica e le due vengono associate tra loro. Con Advanced Interface Creator, hai più controllo sulla configurazione. Aggiungi un'interfaccia fisica e una o più logiche e quindi aggiungi le associazioni tra di esse.

Le interfacce e le associazioni vengono aggiunte nel formato bozza. Dopo averle aggiunte e convalidate, devi attivarle. Per ulteriori informazioni sulle risorse attiva e di bozza, consulta [Creazione, aggiornamento, attivazione e disattivazione delle tue risorse](../GA_information_management/ga_im_definitions.html#draft_active_resources).

Le librerie di interfacce contengono modelli di interfacce fisiche e logiche che puoi riutilizzare.

## Flusso di elevato livello
{: #interface_flow}

Utilizza la seguente procedura come supporto per la configurazione delle risorse di cui hai bisogno per iniziare a utilizzare la funzione di gestione dei dati.

### Prima di cominciare

Questa procedura presuppone che tu abbia un tipo di dispositivo con almeno un dispositivo registrato e collegato. Per ulteriori informazioni su questi prerequisiti e per istruzioni su come creare un tipo di dispositivo e registrare un dispositivo, consulta [Connessione dispositivi](../iotplatform_task.html#iotplatform_task).

### Passi

1. Seleziona Simple Interface Creator o Advanced Interface Creator:
  - **Flusso di Simple Interface Creator**
    1. [Crea un'interfaccia fisica di bozza](#create_physical_interface_simple).
  - **Flusso di Advanced Interface Creator**
    1. [Crea un'interfaccia fisica di bozza](#create_physical_interface_advanced).
    2. [Crea una o più interfacce logiche di bozza](#create_logical_interface). (Questo passo viene completato automaticamente quando utilizzi il flusso semplice.)
    3. [Associa l'interfaccia fisica alle interfacce logiche](#create_interface_mappings).
2. [Imposta le preferenze di notifica](#set_notifications).
3. [Attiva la configurazione](#validate_activate).

Puoi anche configurare la funzione di gestione dei dati utilizzando le API. Per informazioni dettagliare sulla configurazione della funzione di gestione dei dati per normalizzare e trasformare i tuoi dati del dispositivo utilizzando le API, consulta [Introduzione alla gestione dei dati](ga_im_example.html#im_example). [Guida passo a passo: un esempio dettagliato su come utilizzare i dispositivi tramite un'interfaccia comune](../GA_information_management/ga_im_index_scenario.html#scenario) ti guida nella procedura per creare un'interfaccia logica del tipo di dispositivo per i dispositivi termometro eterogenei. Per i dettagli sull'API, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.
{: tip}


## Creazione di un'interfaccia fisica di bozza (flusso semplice)
{: #create_physical_interface_simple}

1. Dal menu di navigazione principale, fai clic su **Devices**.
2. Fai clic su **Device Types** e seleziona il tipo di dispositivo per il quale vuoi creare un'interfaccia. Puoi anche creare un nuovo tipo di dispositivo. Per ulteriori informazioni, consulta [Connessione dispositivi](../iotplatform_task.html#iotplatform_task).
3. Visualizza le informazioni sul tipo di dispositivo e fai clic su **Interface**.
4. Fai clic su **Simple Flow** per aprire Simple Interface Creator.
5. Fai clic su **Create Interface**.
6. Fai clic su **Add Property** per iniziare ad aggiungere eventi e proprietà all'interfaccia fisica.
   a. Nella finestra **Add Properties to the Interface**, seleziona gli eventi che desideri aggiungere all'interfaccia. Il sistema è in ascolto per gli eventi attivi per i dispositivi collegati del tipo di dispositivo selezionato. Puoi anche selezionare l'ID del dispositivo dell'ultimo evento in cache o selezionare un altro evento dall'elenco.
   b. Seleziona le proprietà dall'evento e fai clic su **Add**.
   c. Aggiungi ulteriori proprietà se necessario.
7. Fai clic su **Done**. L'interfaccia fisica è stata creata. Se vuoi aggiungere ulteriori dettagli, puoi [eseguire l'aggiornamento a un'interfaccia avanzata](#create_physical_interface_advanced) selezionando il link **Use Advanced Interface Creator**.


## Creazione di un'interfaccia fisica di bozza (flusso avanzato)
{: #create_physical_interface_advanced}

1. Dal menu di navigazione principale, fai clic su **Devices**.
2. Fai clic su **Device Types** e seleziona il tipo di dispositivo per il quale vuoi creare un'interfaccia. Puoi anche creare un nuovo tipo di dispositivo. Per ulteriori informazioni, consulta [Connessione dispositivi](../iotplatform_task.html#iotplatform_task).
2. Visualizza le informazioni sul tipo di dispositivo e fai clic su **Interface**.
3. Fai clic su **Advanced Flow** per aprire Advanced Interface Creator.
4. Scegli una delle seguenti opzioni:
 - Per creare una nuova interfaccia fisica, fai clic su **Create Interface**.
 - Per utilizzare un modello di interfaccia dalla libreria, fai clic su **Add From Library**, seleziona l'interfaccia e passa quindi a [Creazione di un'interfaccia logica di bozza](#create_logic_interface).
5. Nella scheda **Identity** della pagina **Create Physical Interface**, immetti un nome e una descrizione dell'interfaccia fisica.
6. Facoltativo: se stai utilizzando dei dispositivi abilitati per Edge con l'interfaccia, attiva lo switch Edge. Per ulteriori informazioni su Edge, consulta [Analisi Edge](../edge_analytics.html#edge_analytics).
7. Fai clic su **Avanti**.
8. Definisci l'interfaccia fisica aggiungendo e modificando i payload e i tipi di evento:
   a. Seleziona un tipo di evento dall'elenco per modificare un tipo di evento esistente o fai clic su **Create Event Type**.
   b. Seleziona l'ultimo evento memorizzato nella cache, seleziona un evento dall'elenco o aggiungi un evento manualmente e quindi fai clic su **Add**.
9. Fai clic su **Done**. L'interfaccia fisica è stata creata nel formato bozza. Puoi procedere con l'aggiunta di una o più interfacce logiche.

## Creazione di interfacce logiche di bozza (flusso avanzato)
{: #create_logical_interface}

1. Dopo aver creato l'interfaccia fisica di bozza, scegli una delle seguenti opzioni:
 - Fai clic su **Add Logical Interface** per creare una nuova interfaccia logica.
 - Fai clic su **Add From Library** per utilizzare un modello di interfaccia dalla libreria di interfacce. Seleziona l'interfaccia e vai al passo 7.
2. Nella scheda **Identity** della pagina **Create Logical Interface**, immetti un nome e una descrizione dell'interfaccia logica.
3. Fai clic su **Avanti**.
4. Fai clic su **Add Property** per iniziare a definire le proprietà dell'interfaccia logica.
5. Nella finestra **Create Property** seleziona le proprietà dall'interfaccia fisica che desideri aggiungere all'interfaccia logica e salva le modifiche.
6. Fai clic su **Add Object**.
7. Aggiungi ulteriori interfacce logiche se necessario e fai quindi clic su **Next**.
8. {: #set_notifications}Configura le preferenze di notifica per l'interfaccia. Seleziona una delle seguenti opzioni per determinare quando devono essere inviate le notifiche di stato del dispositivo:
 - Nessuna notifica evento - Non viene inviata alcuna notifica. Puoi utilizzare l'API REST per richiamare le informazioni sulla modifica dello stato del dispositivo.
 - Alla modifica dello stato - Riceverai una notifica quando viene modificato lo stato del dispositivo.
 - Per ogni evento - Riceverai una notifica ogni volta in cui la piattaforma elabora un evento per il dispositivo, anche se non provoca un cambiamento di stato.
8. Fai clic su **Done**. Le interfacce vengono create e ![Icona stato di bozza](images/draft_icon.png) indica lo stato di bozza. Puoi procedere con la modifica o l'attivazione delle interfacce.

## Attivazione della configurazione
{: #validate_activate}

Dopo aver creato le interfacce fisiche e logiche di bozza, devi attivare la configurazione.

- Fai clic su **Activate** per attivare la configurazione. Lo stato della configurazione è impostato su attivato.
![Distribuzione attiva](images/active_deployment.png) Se apporti modifiche alle interfacce, le interfacce diventano di nuovo delle bozze e devi riattivarle.
