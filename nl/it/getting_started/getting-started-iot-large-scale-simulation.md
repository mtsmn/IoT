---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guida 3: Simulazione di un grande numero di dispositivi
Nella prima guida, hai configurato un simulatore del dispositivo di base per simulare manualmente uno o più nastri trasportatore. In questa guida, espandiamo questa simulazione aggiungendo un grande numero di simulatori in esecuzione automatica al tuo ambiente per verificare le analisi e il monitoraggio in un ambiente a più dispositivi più realistico.
{:shortdesc}

**Importante:** l'applicazione richiede 512 MB di memoria, che è superiore a quella assegnata per impostazione predefinita e che supera anche la quantità disponibile per gli account di prova gratuiti, incluso l'account standard e di prova {{site.data.keyword.Bluemix}}. I possessori degli account a pagamento a consumo e di sottoscrizione possono aumentare la memoria assegnata fino a 512 MB. I possessori degli account di prova gratuita devono eseguire l'aggiornamento a un account a pagamento a consumo o di sottoscrizione. Per ulteriori informazioni sui tipi di account {{site.data.keyword.Bluemix_notm}}, consulta [Tipi di account](/docs/pricing/index.html#pricing).

**Nota:** Bluemix è ora IBM Cloud. Per ulteriori dettagli, consulta il [Blog di IBM Cloud ](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno").

## Panoramica e obiettivi
{: #overview}

In questa guida, configurerai un'applicazione {{site.data.keyword.Bluemix_notm}} per simulare più dispositivi nastro trasportatore utilizzando un "backend" [Node-RED ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://nodered.org){: new_window}.

L'applicazione contiene tre flussi che:   
1. Creano più dispositivi.   
2. Simulano la pubblicazione dell'evento simultaneo.
3. Eliminano i dispositivi.   

![Eliminazione e creazione del dispositivo](images/device_creation.png "Eliminazione e creazione del dispositivo")

![Simulazione evento dispositivo](images/device_event_simulation.png "Simulazione evento dispositivo")

Completando le istruzioni presenti in questa guida, effettuerai le seguenti operazioni:
- Utilizzare Cloud Foundry per distribuire un'applicazione simulatore del dispositivo abilitato con il webhook e basata su Node-RED.
- Utilizzare le chiamate API per creare e registrare i dispositivi, pubblicare gli eventi del dispositivo ed eliminare i dispositivi.

**Importante:** la simulazione di un gran numero di dispositivi che contemporaneamente invia i dati del dispositivo a {{site.data.keyword.iot_full}} potrebbe utilizzare un gran quantitativo di dati. 

## Prerequisiti
{: #prereqs}  
Avrai bisogno dei seguenti account e strumenti:

* [Account {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/registration/) con    
 - Più di 512 MB di RAM   
 - Più di 1 GB di quota disco  
 - Due servizi disponibili
* [Cloud Foundry Command Line Interface (cf CLI) ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilizza la CLI cf per distribuire e gestire le tue applicazioni {{site.data.keyword.Bluemix_notm}}.
* Facoltativo: [Git ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://git-scm.com/downloads){: new_window}  
Se scegli di utilizzare Git per scaricare gli esempi di codice devi inoltre avere un account [GitHub.com ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com){: new_window}. Puoi anche scaricare il codice come un file compresso senza un account GitHub.com.

Per tutte le chiamate REST nei seguenti passi, puoi utilizzare il plugin del componente aggiuntivo del client REST o cURL disponibile in Mozilla.  
{: tip}

## Passo 1 - Distribuisci l'applicazione a {{site.data.keyword.Bluemix_notm}}
{: #step1}

Utilizza le seguenti istruzioni per creare e distribuire la tua applicazione manualmente.   

1. Clona il repository GitHub dell'applicazione di esempio *Lesson4*.  
Utilizza il tuo strumento git preferito per clonare il seguente repository:  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
Nella shell Git, utilizza il seguente comando:
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
```
3. Configura l'applicazione per il tuo ambiente modificando il file manifest.yml.  
Cosa devi modificare:
 - Per utilizzare un servizio {{site.data.keyword.iot_short_notm}} esistente, aggiorna tutte le istanze di `lesson4-simulate-iotf-service` per rispecchiare tale nome del servizio. Ad esempio, se stai utilizzando il servizio {{site.data.keyword.iot_short_notm}} dalla guida 1, utilizza `iotp-for-conveyor` per il nome del servizio.    
 - Impostare il nome e l'host del dispositivo.   
Nella sezione delle applicazioni, modifica le voci `name` e `host` con qualcosa di univoco, come ad esempio `YOUR_NAME-lesson4-simulate`.   
**Suggerimento:** l'URL ROUTES che utilizzi per accedere all'applicazione viene creato dalla voce `host`, ad esempio: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plan: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
1. Dalla riga di comando, configura il tuo endpoint API seguendo il comando cf api.   
Sostituisci i valore `API-ENDPOINT` con l'endpoint API della tua regione.
   ```
cf api API-ENDPOINT
   ```
Esempio: `cf api https://api.ng.bluemix.net`  
<table>
<tr>
<th>Regione</th>
<th>Endpoint API</th>
</tr>
<tr>
<td>Stati Uniti Sud</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>Regno Unito</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. Accedi al tuo account {{site.data.keyword.Bluemix_notm}}.
  ```
cf login
  ```
Se richiesto, seleziona l'organizzazione e lo spazio in cui desideri distribuire {{site.data.keyword.iot_short_notm}} e l'applicazione di esempio.
6. Crea i servizi necessari in {{site.data.keyword.Bluemix_notm}}.   
 1. Utilizza il seguente comando per creare il servizio cloudantNoSQLDB Lite:
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. Utilizza il seguente comando per creare il servizio {{site.data.keyword.iot_short_notm}}.
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**Importante:** se stai utilizzando un servizio {{site.data.keyword.iot_short_notm}} esistente e hai aggiornato il file manifest.yml di conseguenza, assicurati di utilizzare il nome del servizio esistente. Ad esempio, dalla guida 1, utilizza `iotp-for-conveyor` invece di `lesson4-simulate-iotf-service` nella chiamata cf.
7. Esegui il comando `cf push` per creare il progetto e trasmettilo alla tua organizzazione.  
La tua applicazione di esempio è stata distribuita a {{site.data.keyword.Bluemix_notm}}.  
Quando la distribuzione è completa, viene visualizzato un messaggio che indica che la tua applicazione è in esecuzione.   
Esempio:  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. Ottieni le credenziali del servizio per la tua applicazione.
 1. Accedi a {{site.data.keyword.Bluemix_notm}} all'indirizzo:  
 [https://bluemix.net ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://bluemix.net){: new_window}.
 2. Seleziona l'account e lo spazio in cui hai distribuito l'applicazione.
 3. Dal menu, seleziona **Apps** e **Dashboard**.
 4. In Cloud Foundry Apps, fai clic sul nome dell'applicazione che hai appena distribuito.
 4. Seleziona **Connections**.
 5. Individua il servizio iotf che stai utilizzando con la tua applicazione e fai clic su **View credentials**.  
 6. Nella pagina delle credenziali del servizio, individua `apiKey` e `apiToken`.   
La chiave API e il token di autenticazione vengono utilizzati come credenziali quando utilizzi l'API dell'applicazione per creare ed eseguire i tuoi dispositivi simulati.

## Passo 2 - Proteggi il tuo flusso Node-RED
{: #step2}

Sebbene questo passo sia facoltativo, si consiglia di proteggere il tuo flusso Node-RED.

1. Proteggi il tuo editor in modo che possano accedervi soltanto gli utenti autorizzati.
2. Per proteggere il flusso Node-RED, passa la dashboard {{site.data.keyword.Bluemix_notm}} e seleziona l'applicazione che hai precedentemente distribuito.
3. Fai clic su **Runtime** e su **Environment variables**.
4. Aggiungi le seguenti variabili di ambiente definite dall'utente e i rispettivi valori:
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. Salva il flusso.

Dopo aver protetto il tuo flusso Node-RED, se pensi di utilizzare i comandi API REST per creare, eliminare o simulare più dispositivi, devi fornire le credenziali password e nome utente Node-RED durante l'autenticazione di base.

## Passo 3 - Crea e collega i dispositivi
{: #step3}

Puoi utilizzare l'interfaccia del flusso Node-RED o l'API REST dell'applicazione per completare le seguenti attività.

### Creazione e collegamento di dispositivi utilizzando Node-RED  
Per registrare più dispositivi:  
1. Accedi a {{site.data.keyword.Bluemix_notm}} all'indirizzo:  
[https://bluemix.net ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://bluemix.net){: new_window}.
2. Seleziona l'account e lo spazio in cui hai distribuito l'applicazione.
3. Dal menu, seleziona **Apps** e **Dashboard**.
4. In Cloud Foundry Apps, fai clic sull'URL **ROUTE** dell'applicazione che hai appena distribuito.  
ROUTE_URL viene creato dalla voce `host` che hai utilizzato nel file manifest.yml: `HOST.mybluemix.net`  
Esempio: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
Viene aperta l'interfaccia Node-RED.
5. Fai clic su **Go to your Node-RED flow editor**.
6. Nell'editor del flusso, seleziona la scheda **Device Type and Instance**.
7. Per registrare cinque dispositivi, fai clic sul nodo Inject etichettato **Register 5 motorController devices**.

### Creazione e collegamento di dispositivi utilizzando l'API REST  
Per registrare più dispositivi:  

1. Effettua una chiamata HTTP POST al seguente URL: `ROUTE_URL/rest/devices`  
Esempio: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - Imposta 'Content-Type' e 'Accept' su 'application/json'  
 - Utilizza il seguente payload JSON:  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
  Dove:  
    - numberDevices è il numero di dispositivi da creare e registrare.
    - Facoltativo: typeId è il tipo di dispositivo con cui saranno registrati i dispositivi. Se typeId non viene fornito, viene utilizzato il valore predefinito "iotp-for-conveyor". **Suggerimento:** puoi immettere qualsiasi nome di tipo di dispositivo, ma le altre guide nella serie si aspettano dispositivi del tipo `iotp-for-conveyor`. Se utilizzi un tipo di dispositivo diverso, devi modificare le impostazioni nelle guide di conseguenza.
    - Facoltativo: authToken è il token di autorizzazione con cui saranno registrati i dispositivi.
    - Facoltativo: se chunkSize non viene fornito, viene impostato sul valore predefinito di 500. 'chunksize' deve essere inferiore e un fattore di numberDevices.
    - deviceName è il modello per deviceID per i dispositivi creati.

## Passo 4 - Simula gli eventi del dispositivo
{: #step4}

Poiché i dispositivi simulati sono registrati con {{site.data.keyword.iot_short_notm}}, puoi ora eseguire il simulatore per iniziare a inviare gli eventi del dispositivo.

### Simulazione degli eventi del dispositivo utilizzando Node-RED  
Per inviare gli eventi del dispositivo:  
1. Nell'editor del flusso Node-RED, seleziona la scheda **Simulate multiple devices**.
7. Per simulare cinque dispositivi, fai clic sul nodo Inject etichettato **Simulate 5 devices**.
 


### Simulazione degli eventi del dispositivo utilizzando l'API REST  
Per inviare gli eventi del dispositivo:

1. Effettua una chiamata HTTP POST al seguente URL: `ROUTE_URL/rest/runtest`  
Esempio: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - Imposta 'Content-Type' e 'Accept' su 'application/json'  
 - Utilizza il seguente payload JSON:   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
Dove:
    - numberDevices è il numero di dispositivi per cui si desidera simulare i dati.
    - numberEvents è il numero di eventi che ogni dispositivo simulato invia.
    - timeInterval è la spaziatura degli eventi in millisecondi.
    - deviceType è il tipo di dispositivo per cui hai creato i dispositivi simulati.
    - deviceName è il modello per deviceID per i dispositivi creati.
 

## Passo 5 - Eliminazione dei dispositivi
{: #deleting}

### Node-RED  
Per eliminare i dispositivi:  
1. Nell'editor del flusso Node-RED, seleziona la scheda **Device Type and Instance**.
2. Per eliminare cinque dispositivi, fai clic sul nodo Inject etichettato **Delete 5 devices**.



### API REST  

1. Effettua una chiamata HTTP POST al seguente URL: `ROUTE_URL/rest/deleteDevices`  
Esempio: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - Imposta 'Content-Type' e 'Accept' su 'application/json'  
 - Utilizza il seguente payload JSON:      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName": "belt"  
}</code></pre>
  


## Operazioni successive
{: @whats_next}  
Passa a un altro argomento di tuo interesse:
- [Guida 2: Monitoraggio dei dati del tuo dispositivo ](getting-started-iot-monitoring.html)  
Ora che hai collegato uno o più dispositivi a iniziato a fare buon uso dei dati del dispositivo, è ora di iniziare a monitorare una raccolta di dispositivi.
- [Ulteriori informazioni su {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Ulteriori informazioni sulle API {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/reference/api.html){:new_window}
