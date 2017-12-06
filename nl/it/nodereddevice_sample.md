---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Creazione e collegamento di un simulatore del dispositivo Node-RED
Utilizza Node-RED per creare un simulatore del dispositivo e per inviare i dati del dispositivo simulato alla tua organizzazione {{site.data.keyword.iot_full}}.   
{:shortdesc}

## Panoramica

Node-RED è uno strumento utilizzato per la connessione di dispositivi hardware, API e servizi in linea con nuove e interessanti modalità. Per ulteriori informazioni, consulta il sito web [Node-RED ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://nodered.org/){: new_window}.  

Puoi eseguire la tua istanza Node-RED nel tuo proprio ambiente o utilizzarla come un'applicazione {{site.data.keyword.Bluemix_notm}}. I seguenti passi includono le istruzioni per {{site.data.keyword.Bluemix_notm}}.

Crea e collega il simulatore del dispositivo Node-RED per inviare i messaggi del dispositivo MQTT a {{site.data.keyword.iot_short_notm}}. Nel seguente esempio, il simulatore del dispositivo simula un invio di dati per un contenitore merci a un broker MQTT, ad esempio {{site.data.keyword.iot_short_notm}}.

## Passi

1. Crea il simulatore del dispositivo Node-RED completando la seguente procedura:   
    1. Accedi a {{site.data.keyword.Bluemix_notm}} all'indirizzo: https://console.ng.bluemix.net
    2. Seleziona la scheda **Catalog**.
    3. Individua la sezione dei contenitori tipo del catalogo del servizio e fai clic su **Internet of Things Platform Starter**. **Suggerimento:** fai clic  [qui ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window} per andare direttamente alla pagina starter della piattaforma Internet of Things.
    4. Nella pagina starter della piattaforma Internet of Things, seleziona lo spazio dove desideri distribuire Node-RED, verifica le selezioni per la creazione di un'applicazione e fai clic su **Create** per aggiungere Node-RED alla tua organizzazione Bluemix. Ad esempio:
    <ul>
     <li> Spazio: dev
     <li> Nome applicazione: myDevice
     <li> Nome host: myDevice  
    </ul>  
Lascia le altre opzioni come valori predefiniti. Dopo che l'applicazione è stata distribuita, viene visualizzata la pagina Start coding with Node-RED.
**Nota:** il processo di preparazione può richiedere diversi minuti.  

2. Fai clic sul link Routes per aprire Node-RED o su Visit App URL, ad esempio `http://myDevice.mybluemix.net`  
3. Completa la seguente procedura per **Secure your Node-RED editor**:
    1. Fai clic su **Next**
    2. Aggiungi un nome utente e una password.  
    **Nota:** per la password si devono rispettare le regole di complessità obbligatorie, altrimenti il pulsante **Next** rimane grigio.  
    3. Facoltativo. Controlla **Allow anyone to view the editor, but not make any changes** o **Not recommended: Allow anyone to access the editor and make changes**
    4. Fai clic su **Finish** per completare la configurazione.
4. Fai clic su **Go to your Node-RED flow editor**
5. Immetti il nome utente e la password che hai creato precedentemente.  
Il flusso del simulatore del dispositivo è stato reso disponibile all'editor del flusso. Il flusso simula un **Thermostat** che invia la sua posizione, la temperatura misurata e i dati sull'umidità a {{site.data.keyword.iot_short_notm}}.  
6. Conferma che il dispositivo sia collegato controllando che venga visualizzato un punto con il messaggio di connessione per il nodo **Send to IBM IoT Platform**. Il controllo è inoltre valido per il nodo **IBM IoT App In** per il flusso secondario **Temperature Monitor**.  
7. Invia e ricevi i messaggi del dispositivo MQTT completando la seguente procedura:  
    1. Fai clic sul nodo Inject **Send Data** per attivare i dati che devono essere inviati a {{site.data.keyword.iot_short_notm}}.
       **Nota:** puoi attivare il nodo di debug **Debug output payload** per visualizzare quali dati sono inviati e per controllare il nodo della funzione **Device payload** per visualizzare il codice che crea il payload. 
    2. Dopo aver fatto clic su **Send Data**, i messaggi MQTT vengono inviati a {{site.data.keyword.iot_short_notm}} e sono ricevuti dal nodo **IBM IoT App In**. Il flusso secondario **Temperature Monitor** stabilisce se la temperatura è all'interno dei limiti definiti e visualizza un messaggio nella scheda di debug.
       **Nota:** Fai clic sul nodo di switch **temp thresh** per controllare e modificare i valori di soglia
    3. Controlla la scheda di debug. Viene visualizzato un messaggio, ad esempio **Temperature (17) within safe limits**.
    
Hai ora collegato il tuo dispositivo IoT di esempio a {{site.data.keyword.iot_short_notm}} e puoi visualizzare i dati del dispositivo.
