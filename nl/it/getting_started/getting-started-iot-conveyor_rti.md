---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guida 1: Connessione di un dispositivo nastro trasportatore  
Crea un nastro trasportatore di base con un dispositivo IoT che invia i dati del monitoraggio a {{site.data.keyword.iot_full}} in {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Panoramica e obiettivi
{: #overview}  
Questa guida è la prima di una serie che ti guida, passo dopo passo, attraverso il processo di collegamento dei dispositivi a {{site.data.keyword.iot_short_notm}}, il monitoraggio e l'utilizzo dei dati del dispositivo e altro ancora. Ogni guida si basa sulla guida precedente e il codice di esempio in ogni guida viene utilizzato come l'input della guida successiva. Puoi anche utilizzare queste guide in modo autonomo modificando il codice di esempio per soddisfare i tuoi scopi.

Nella prima guida, configuriamo un nastro trasportatore collegato e lo utilizziamo per inviare i dati IoT a {{site.data.keyword.iot_short_notm}}.
A seconda del tuo livello di competenza, puoi seguire uno o entrambi i seguenti percorsi per configurare il tuo nastro trasportatore:
- Percorso A  
Questo percorso ti permette di iniziare velocemente l'installazione di un'applicazione simulatore del nastro trasportatore in {{site.data.keyword.Bluemix_notm}}. L'applicazione si registra automaticamente a un dispositivo con {{site.data.keyword.iot_short_notm}} e invia automaticamente i dati formattati correttamente alla piattaforma. Le istruzioni per questo percorso sono in [Passo 2A - Utilizza l'applicazione di esempio simulatore da GitHub](#deploy_app).  
- Percorso B  
Questo percorso è tecnicamente più impegnativo e richiede ulteriore hardware, competenze di programmazione Python e la registrazione manuale del tuo dispositivo con {{site.data.keyword.iot_short_notm}}. Le istruzioni per questo percorso sono in [Passo 2B - Crea un nastro trasportatore fisico con un Raspberry Pi e un motore elettrico](#raspberry).

Come parte di questa guida eseguirai queste azioni:
- Creare e distribuire un'organizzazione {{site.data.keyword.iot_short_notm}} utilizzando la CLI Cloud Foundry.
- Creare e distribuire un dispositivo nastro trasportatore di esempio.
- Collegare il dispositivo nastro trasportatore simulato a {{site.data.keyword.iot_short_notm}}.
- Monitorare e visualizzare i dati del dispositivo utilizzando i dashboard {{site.data.keyword.iot_short_notm}}.

Per iniziare con {{site.data.keyword.iot_short_notm}} utilizzando un dispositivo IoT differente, consulta [Esercitazione introduttiva](/docs/services/IoT/getting-started.html).
{: tip}

## Prerequisiti
{: #prereqs}

Avrai bisogno dei seguenti account e strumenti:
* [Account {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/registration/){: new_window}
* [Cloud Foundry Command Line Interface (cf CLI) ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilizza la CLI cf per distribuire le applicazioni e i servizi a {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni, consulta [Cloud Foundry CLI documentation![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.cloudfoundry.org/cf-cli/){: new_window}
* Facoltativo: [Git ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://git-scm.com/downloads){: new_window}  
Se scegli di utilizzare Git per scaricare gli esempi di codice devi inoltre avere un account [GitHub.com ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com){: new_window}. Puoi anche scaricare il codice come un file compresso senza un account GitHub.com.
* Facoltativo: un cellulare su cui eseguire l'applicazione web di esempio *Conveyor belt* per inviare i dati dell'accelerometro.

## Passo 1 - Distribuisci {{site.data.keyword.iot_short_notm}}  
{: #deploy_watson_iot_platform_service}
{{site.data.keyword.iot_short_notm}} fornisce l'accesso all'applicazione potente per i dati e i dispositivi IoT per aiutarti a comporre rapidamente le applicazioni di analisi, i dashboard di visualizzazione e le applicazioni IoT mobili.
Le seguenti istruzioni distribuiranno un'istanza del servizio {{site.data.keyword.iot_short_notm}} con il nome `iotp-for-conveyor` nel tuo ambiente {{site.data.keyword.Bluemix_notm}}. Se hai già un'istanza del servizio in esecuzione, puoi utilizzarla con la guida e saltare questo primo passo. Assicurati semplicemente di utilizzare il nome del servizio e lo spazio {{site.data.keyword.Bluemix_notm}} corretti quando procedi nelle guide.
{: tip}
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
3. Distribuisci il servizio {{site.data.keyword.iot_short_notm}} a {{site.data.keyword.Bluemix_notm}}.  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
  ```
Per YOUR_IOT_PLATFORM_NAME, utilizza *iotp-for-conveyor*.  
Esempio: `cf create-service iotf-service iotf-service-free iotp-for-conveyor`
3. Crea il tuo dispositivo nastro trasportatore di esempio.
 - Percorso A: [Passo 2A - Utilizza l'applicazione simulatore di esempio da GitHub](#deploy_app).
 - Percorso B: [Passo 2B - Crea un nastro trasportatore fisico con Raspberry Pi e un motore elettrico](#raspberry).

## Passo 2A - Distribuisci l'applicazione web del nastro trasportatore di esempio
{: #deploy_app}

L'applicazione di esempio ti consente di simulare un nastro trasportatore industriale collegato a {{site.data.keyword.Bluemix_notm}}. Puoi avviare e arrestare il nastro e modificarne la velocità. Ogni modifica al nastro viene inviata a {{site.data.keyword.Bluemix_notm}} nel formato di un messaggio MQTT che viene visualizzato nell'applicazione. Puoi monitorare il comportamento del nastro utilizzando le schede del dashboard predefinite.
L'applicazione di esempio è stata creata utilizzando le librerie client Node.js all'indirizzo: [https://github.com/ibm-watson-iot/iot-nodejs ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![Applicazione nastro trasportatore](images/app_conveyor_belt.png "Applicazione nastro trasportatore")

1. Utilizza il tuo strumento git preferito per clonare il seguente repository:
https://github.com/ibm-watson-iot/guide-conveyor-simulator
Nella shell Git, utilizza il seguente comando:
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. Nella riga di comando, modifica la directory con quella in cui è ubicata l'applicazione di esempio.
  ```bash
cd guide-conveyor-simulator
  ```
3. Dalla directory *guide-conveyor-simulator*, passa la tua applicazione a {{site.data.keyword.Bluemix_notm}} e forniscile un nuovo nome sostituendo *YOUR_APP_NAME* nel comando cf push. Utilizza l'opzione `--no-start` perché avvierai l'applicazione nel prossimo passo dopo averla associata a {{site.data.keyword.iot_short_notm}}.
**Nota:** la distribuzione della tua applicazione potrebbe richiedere alcuni minuti.
   ```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. Nella directory *guide-conveyor-simulator*, associa la tua applicazione alla tua istanza di {{site.data.keyword.iot_short_notm}} utilizzando i nomi forniti per ognuna di esse.
  ```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
Per ulteriori informazioni sull'associazione delle applicazioni, consulta [Connessione alle applicazioni](/docs/services/IoT/platform_authorization.html#bluemix-binding).
2. Avvia la tua applicazione perché l'associazione abbia effetto.
  ```bash
cf start YOUR_APP_NAME
  ```
In questa fase, la tua applicazione di esempio è stata distribuita a {{site.data.keyword.Bluemix_notm}}.
Quando la distribuzione è completa, viene visualizzato un messaggio che indica che la tua applicazione è in esecuzione.   
Esempio:
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
start command:     ./vendor/initial_startup.rb

     state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
Per visualizzare lo stato della distribuzione dell'applicazione e l'URL dell'applicazione, puoi eseguire il seguente comando:
  ```bash
cf apps
  ```
Risolvi gli errori nel processo di distribuzione utilizzando il comando `cf logs YOUR_APP_NAME --recent`.
{: tip}
1. In un browser, accedi all'applicazione.  
Apri il seguente URL: `https://YOUR_APP_NAME.mybluemix.net`    
Esempio: `https://conveyorbelt.mybluemix.net/`.
2. Immetti un token e un ID del dispositivo per il tuo dispositivo.  
L'applicazione di esempio registra automaticamente un dispositivo del tipo `iot-conveyor-belt` con il token e l'ID del dispositivo che hai fornito. Per ulteriori informazioni sulla registrazione dei dispositivi, consulta [Connessione dispositivi](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
4. Continua con [Passo 3 - Visualizza i dati non elaborati in {{site.data.keyword.iot_short_notm}}](#see_live_data).

## Passo 2B - Crea un nastro trasportatore con tecnologia Raspberry
{: #raspberry}

La soluzione Raspberry Pi è stata creata dalle librerie client Python all'indirizzo: [https://github.com/ibm-watson-iot/iot-python ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/iot-python){: new_window}

### Diagramma schematico del circuito

![Diagramma circuito](images/raspi_circuit.png)

### Connessioni obbligatorie

Le seguenti connessioni tra Raspberry Pi e L298N Dual H Bridge Motor Driver Board sono obbligatorie. Collega:

- Pin 2 of the Raspberry Pi to +5v on the L298N board
- Pin 6 of the Raspberry Pi to GND on the L298N board
- Pin 13 of the Raspberry Pi to the IN1 of the L298N board
- Pin 15 of the Raspberry Pi to the IN2 of the L298N board
- +9v of the battery to +12v on the L298N board
- -9v of the battery to GND on the L298N board
- The +ve node of the motor to OUT1 on the L298N board
- The -ve node of the motor to OUT2 on the L298N board

### Requisiti hardware

1. [Raspberry Pi(2/3) ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://www.raspberrypi.org/){: new_window} con Raspbian Jessie (8.x).
2. [L298N Dual H Bridge Motor Driver Board ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. 9V DC motor
4. 9V Battery

### Requisiti software Raspberry Pi
- Python 2.7.9 e successivo, installato con Raspbian Jessie (8.x).
- [Libreria Watson IoT Python ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- Facoltativo: Git.  
Se non hai git installato sul tuo Raspberry Pi, puoi installarlo utilizzando il seguente comando:  
```bash
$ sudo apt-get install git
```

### Passi dettagliati

1. Apri il terminale o esegui SSH al tuo Raspberry Pi.
2. Utilizza il tuo strumento git preferito per clonare il seguente repository nel tuo Raspberry Pi:
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
Nella Shell Git, utilizza il seguente comando:
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. Registra il dispositivo con {{site.data.keyword.iot_short_notm}}.  
Per ulteriori informazioni sulla registrazione dei dispositivi, consulta [Connessione dispositivi](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
	 1. Nella console {{site.data.keyword.Bluemix_notm}}, fai clic su **Launch** nella pagina dei dettagli del servizio {{site.data.keyword.iot_short_notm}}.
	     Si apre la console web {{site.data.keyword.iot_short_notm}} in una nuova scheda del browser al seguente URL:
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     Dove ORG_ID è l'ID univoco di sei caratteri della [tua organizzazione {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}.
	 2. Nel dashboard della panoramica, dal pannello del menu, seleziona **Devices** e fai clic su **Add Device**.
	 3. Crea un tipo di dispositivo per il dispositivo che stai aggiungendo.
	     1. Fai clic su **Create device type**.
	     2. Immetti il nome del tipo di dispositivo `iotp-for-conveyor` e una descrizione per il tipo di dispositivo.  
	**Suggerimento:** puoi immettere qualsiasi nome per il tipo di dispositivo, ma le altre guide della serie prevedono dispositivi che utilizzano il tipo di dispositivo `iotp-for-conveyor`. Se utilizzi un tipo di dispositivo diverso, devi modificare le impostazioni nelle altre guide di conseguenza.
	     3. Facoltativo: immetti i metadati e gli attributi per il tipo dispositivo.
	 4. Fai clic su **Next** per avviare il processo di aggiunta del tuo dispositivo con il tipo dispositivo.
	 5. Immetti un ID dispositivo, ad esempio `conveyor_belt`.
	 5. Fai clic su **Next** per completare il processo.
	 6. Fornisci un token di autenticazione o accetta un token generato automaticamente.
	 7. Verifica che le informazioni di riepilogo siano corrette e quindi fai clic su **Add** per aggiungere la connessione.
	 8. Nella pagina delle informazioni del dispositivo, copia e salva i seguenti dettagli:
	     * ID organizzazione
	     * Tipo di dispositivo
	     * ID dispositivo
	     * Metodo di autenticazione
	     * Token di autenticazione
	     Ti serviranno i valori per l'ID organizzazione, il Tipo di dispositivo, l'ID dispositivo e il Token di autenticazione per configurare il tuo dispositivo per la connessione a {{site.data.keyword.iot_short_notm}}.
4. Passa alla root *guide-conveyor-rasp-pi* del repository clonato.
5. Rendi eseguibile lo script della shell utilizzando il comando `sudo chmod +x setup.sh`
6. Esegui il file *setup.sh* e immetti i dettagli che hai copiato dalla pagina delle informazioni del dispositivo quando richiesto.
```bash  
./setup.sh
```  

7. Esegui il programma deviceClient.  
Quando esegui il programma, il tuo Raspberry Pi avvia il motore e sarà eseguito per un massimo di 2 minuti in base alle impostazioni del parametro.
```bash
python deviceClient.py -t 2
```

Mentre il motore è in esecuzione, il programma pubblica gli eventi di tipo `sensorData` che hanno la seguente struttura del payload di esempio:  
```
{
"d": {
  "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. Prosegui con il [Passo 3 - Visualizza i dati non elaborati in {{site.data.keyword.iot_short_notm}}](#see_live_data).

## Passo 3 - Visualizza i dati non elaborati in {{site.data.keyword.iot_short_notm}}
{: #see_live_data}
1. Verifica che il dispositivo sia registrato con {{site.data.keyword.iot_short_notm}}.
 1. Accedi al tuo dashboard {{site.data.keyword.Bluemix_notm}} all'indirizzo: https://bluemix.net
 2. Dal [tuo elenco di servizi ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://bluemix.net/dashboard/services){: new_window}, fai clic sul servizio {{site.data.keyword.iot_short_notm}} *iotp-for-conveyor*.
 3. Fai clic su *Launch* per aprire il dashboard {{site.data.keyword.iot_short_notm}} in una nuova scheda del browser.  
Puoi inserire tra i preferiti l'URL per accedervi facilmente in seguito.   
Esempio: `https://*iot-org-id*.internetofthings.ibmcloud.com`.
 4. Dal menu, seleziona **Devices** e verifica che il tuo nuovo dispositivo venga visualizzato.
2. Visualizza dati non elaborati
 1. Dal menu, seleziona **Boards**.
 3. Seleziona la tabella **Device Centric Analytics**.
 4. Individua la scheda **Devices I Care About** e seleziona il tuo dispositivo.  
Il nome del dispositivo viene visualizzato nella scheda Device Properties.
4. Invia i dati del sensore alla piattaforma.   
Il dispositivo invia i dati a {{site.data.keyword.iot_short_notm}} quando vengono modificate le letture del sensore. Puoi simulare questo invio di dati arrestando, avviando o modificando la velocità del nastro trasportatore.   
**Percorso A:** Se stai accedendo all'applicazione da un browser di un dispositivo mobile, prova ad agitare il tuo smartphone per attivare i dati dell'accelerometro per il nastro trasportatore.
{: tip}
3. Verifica che i punti dei dati del dispositivo aggiornato che corrispondono al messaggio pubblicato vengano visualizzati nella scheda delle proprietà del dispositivo.  
Esempio messaggio A:
  ```
{
	"d": {
		"id": "belt1",
		"ts": 1494341288662,
		"ay": "0.00",
		"running": true,
		"rpm": "0.6"
	}
}
  ```
Esempio messaggio B:
  ```
{
	"d": {
    "elapsed": 1,
    "running": true,
    "temperature": 35.00,
    "ay": "0.00",
    "rpm": "0.6"
  }
}
  ```


## Passo 4 - Visualizza i dati live in {{site.data.keyword.iot_short_notm}}
{: #add_card}
Per creare una scheda del dashboard per visualizzare i dati live del nastro trasportatore:
1. Nella stessa tabella Device Centric Analytics, fai clic su **Add New Card** e seleziona quindi **Line Chart**.
2. Per i dati di origine della scheda, fai clic su **Cards**.   
Viene visualizzato un elenco di nomi scheda.
3. Seleziona **Devices I Care About** e fai clic su **Next**.
4. Fai clic su **Connect new data set** e immetti i seguenti valori per i parametri del dataset:
  - Evento: sensorData
  - Proprietà: d.rpm
  - Nome: Belt RPM
  - Tipo: Float
  - Unità: rpm
5. Fai clic su **Avanti**.
6. Nella pagina di anteprima della scheda, seleziona **L** e fai clic su **Next**.
7. Nella pagina delle informazioni sulla scheda, modifica il nome del titolo con `Belt data` e fai clic su **Submit**.
8. Modifica la velocità del tuo nastro per visualizzare i dati live nella tua nuova scheda.
9. Facoltativo: aggiungi un secondo dataset per aggiungere i dati di accelerazione per il nastro.  
Se utilizzi il tuo telefono per collegarti all'applicazione di esempio, puoi agitarlo per inviare i dati di accelerazione del nastro.
 1. Fai clic sull'icona di menu sulla tua scheda e seleziona l'opzione per modificare la scheda.
 2. Per i dati di origine della scheda, seleziona **Cards**.   
 3. Seleziona **Devices I Care About** e fai clic su **Next**.
 4. Fai clic su **Connect new data set** e immetti i seguenti valori:
    - Evento: sensorData
    - Proprietà: d.ay
    - Nome: Accel. Y
    - Tipo: Float
    - Unità: gs
 5. Fai clic su **Avanti**.
 6. Solo percorso A: Agita il tuo telefono per visualizzare i dati dell'accelerometro live nella tua nuova scheda.
Per ulteriori informazioni sulla creazione di tabelle e schede, consulta [Visualizzazione dei dati in tempo reale utilizzando le tabelle e le schede](/docs/services/IoT/data_visualization.html#boards_and_cards).

## Operazioni successive
{: @whats_next}  
Continua con la prossima guida o passa a un altro argomento di tuo interesse:
- Percorso A: Modifica l'applicazione del nastro trasportatore per soddisfare i tuoi bisogni.  
Per dettagli tecnici, consulta:
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Librerie client Node.js ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **Nota:** Bluemix è ora IBM Cloud. Per ulteriori dettagli, consulta il [Blog di IBM Cloud ](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno").
 
- Percorso B: Modifica la configurazione Raspberry Pi per soddisfare i tuoi bisogni.  
Per dettagli tecnici, consulta:
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}
- [Guida 2: Utilizzo delle azioni e delle regole in tempo reale di base](getting-started-iot-rules.html)  
Ora che hai correttamente configurato il tuo nastro trasportatore, lo hai collegato a {{site.data.keyword.iot_short_notm}} e inviato alcuni dati, è ora di rendere tali dati utili utilizzando le regole e le azioni.

**Nota:** Bluemix è ora IBM Cloud. Per ulteriori dettagli, consulta il [Blog di IBM Cloud ](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno").

- [Guida 3: Monitoraggio dei tuoi dati del dispositivo](getting-started-iot-monitoring.html)  
Ora che hai collegato uno o più dispositivi a iniziato a fare buon uso dei dati del dispositivo, è ora di iniziare a monitorare una raccolta di dispositivi.
- [Guida 4: Simulazione di un grande numero di dispositivi](getting-started-iot-large-scale-simulation.html)  
L'applicazione di esempio nastro trasportatore nel percorso A ti consente di simulare manualmente uno o alcuni dispositivi nastro trasportatore. Questa guida ti permette di configurare un ambiente simulato con un gran numero di dispositivi.
- [Ulteriori informazioni su {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html)
- [Ulteriori informazioni sulle API {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/reference/api.html)
