---

copyright:
  years: 2017
lastupdated: "2017-09-18"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Analisi dei dati utilizzando Data Science Experience
{: #DSX_integration}

Puoi utilizzare {{site.data.keyword.iot_full}} con Data Science Experience (DSX) per visualizzare e avere informazioni sui dati inviati dai dispositivi collegati alla piattaforma.
{: shortdesc}

## Panoramica e obiettivi

Questa sezione ti guida, passo dopo passo, nel processo di visualizzazione dei dati di evento del dispositivo {{site.data.keyword.iot_short_notm}} utilizzando DSX come strumento di analisi.

DSX fornisce un ambiente interattivo, collaborativo, basato sul cloud in cui puoi utilizzare più strumenti per attivare le rispettive informazioni approfondite. Utilizza Jupyter Notebook, disponibile in IBM DSX, per caricare i dati {{site.data.keyword.iot_short_notm}} cronologici e utilizzarli per creare visualizzazioni come ausilio nell'analisi dei dati e nell'identificazione di anomalie. Puoi ricavare il valori di soglia dai dati cronologici e utilizzarli per creare regole cloud in {{site.data.keyword.iot_short_notm}}. Le regole cloud avvisano gli utenti quando un dispositivo IoT associato a una regola pubblica una lettura al di fuori dei limiti di soglia configurati.

I dati del dispositivo inviati a {{site.data.keyword.iot_short_notm}} possono essere raccolti e archiviati in {{site.data.keyword.Bluemix}} utilizzando il servizio {{site.data.keyword.cloudantfull}} NoSQL DB. Per raccogliere i dati, devi prima collegare {{site.data.keyword.iot_short_notm}} al servizio {{site.data.keyword.cloudant_short_notm}}. I dati del dispositivo sono archiviati nei database {{site.data.keyword.cloudant_short_notm}} giornalmente, settimanalmente o mensilmente a seconda dell'intervallo bucket configurato.


![Panoramica sull'utilizzo di DSX per analizzare i dati](images/DSX_overview.png)

Come parte di questa guida imparerai:

 - Come configurare l'archivio dei dati della piattaforma in modo che venga utilizzato Cloudant NoSQL DB come servizio storico.
 - Come utilizzare il simulatore Weather Sensors per generare i dati che vengono utilizzati dalla piattaforma.
 - Come esportare i dati e quindi importarli in DSX per analizzarli.
 - Come visualizzare i dati IoT, individuare le anomalie e configurare gli avvisi.


## Prerequisiti

Per completare questa procedura devi disporre dell'accesso a [{{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} con [Cloudant NoSQL DB ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window} e l'accesso a [DSX Account ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://datascience.ibm.com/docs/content/getting-started/get-started.html){: new_window}.


## Passo 1. Configura il simulatore
{:#DSX_sensor_data}

Per poter condurre un'analisi significativa, devi avere dati significativi. Puoi simulare i dati del sensore reali per imparare come i dati del dispositivo Watson IoT Platform possono essere analizzati utilizzando DSX. Questo passo fornisce le istruzioni per:
 - [Configurare il simulatore con un'**istanza esistente di {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Configurare il simulatore con una **nuova istanza di {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)


### Configura il simulatore Weather Sensors con un'istanza esistente di {{site.data.keyword.iot_short_notm}}
{: #sim_existing_platform}

Per simulare i dati del sensore reali nelle tue organizzazioni utilizzando il simulatore Weather Sensors, devi prima configurarlo. Questi passi presuppongono che tu abbia già un'istanza di {{site.data.keyword.iot_short_notm}} attiva e in esecuzione.

1. [Genera la chiave api e il token necessari ad eseguire il simulatore. ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Distribuisci l'applicazione web del simulatore Weather Sensors ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} e segui le istruzioni dettagliate.

   Per ulteriori informazioni su Weather Sensors, consulta [the Weather Sensors simulator guide ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Procedi al [Passo 2. Configura il connettore al database](#DSX_config_db).


### Configura il simulatore Weather Sensors con una nuova istanza di {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Per simulare i dati del sensore reali nelle tue organizzazioni utilizzando il simulatore Weather Sensors, devi prima configurarlo. Questi passi includono le istruzioni per la creazione di un'istanza {{site.data.keyword.iot_short_notm}} insieme al simulatore.

1. [Distribuisci l'applicazione web del simulatore Weather Sensors con un'istanza di {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} e segui le istruzioni dettagliate.

   Per ulteriori informazioni su Weather Sensors, consulta [the Weather Sensors simulator guide ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Attendi che la distribuzione venga completata e passa al dashboard Bluemix.
3. Avvia il servizio {{site.data.keyword.iot_short_notm}} "wiotp-for-weather-sensors-simulator" che è stato creato dal processo di distribuzione.
4. Procedi al [Passo 2. Configura il connettore al database](#DSX_config_db).


## Passo 2. Configura il connettore al database 
{: #DSX_config_db}

I dati del dispositivo possono essere archiviati nei database Cloudant giornalmente, settimanalmente o mensilmente, a seconda dell'opzione di intervallo di bucket selezionata. I dati raccolti da tutti i dispositivi nello stesso intervallo di bucket (giorno, settimana o mese) vengono archiviati nello stesso database.

Indipendentemente dalla configurazione dell'intervallo di bucket, vengono automaticamente creati tre database dal connettore. Un database viene creato per l'intervallo bucket corrente, uno per l'intervallo in entrata e un database per la configurazione. Quando viene raggiunto il termine di un intervallo, i dati del dispositivo vengono archiviati nel database bucket per il nuovo intervallo e viene creato un nuovo database per il bucekt successivo. 

Per utilizzare {{site.data.keyword.cloudant_short_notm}} con DSX, devi configurare l'archivio dei dati della piattaforma in modo che venga utilizzato Cloudant NoSQL DB come servizio storico.

1. Nel dashboard {{site.data.keyword.cloudant_short_notm}}, fai clic su **Extensions** nella barra di navigazione.
2. In **Historical Data Storage**, fai clic su **Setup**. La sezione **Configure Historical Data Storage** elenca tutti i servizi Cloudant NoSQL DB disponibili nello stesso spazio Bluemix come {{site.data.keyword.cloudant_short_notm}}.
3. Seleziona il servizio Cloudant NoSQL DB che desideri collegare.
4. Specifica le seguenti opzioni di configurazione Cloudant NoSQL DB:
  - Intervallo bucket = Day
  - Fuso orario zone = UTC
  - Nome database = default
5. Fai clic su **Done** e conferma l'autorizzazione alla connessione al servizio Cloudant. Assicurati di aver abilitato i popup nel tuo browser per poter avere accesso alla finestra di conferma. Dopo aver correttamente configurato Cloudant NoSQL DB, lo stato di Spazio di archiviazione dei dati cronologici viene modificato in Configurato e i dati del dispositivo vengono archiviati in {{site.data.keyword.cloudant_short_notm}} NoSQL DB.
6. Procedi al [Passo 3. Esegui il simulatore](#run_simulator).


## Passo 3. Esegui il simulatore 
{: #run_simulator}

Il simulatore pubblica i dati dei sensori meteo reali, da 17 stazioni meteo ubicate nell'area Haifa, nella tua organizzazione {{site.data.keyword.iot_short_notm}}.

1. Passa al simulatore.
2. Se hai distribuito il simulatore con un'istanza {{site.data.keyword.iot_short_notm}} associata, procedi al passo 3. Se hai distribuito una versione autonoma del simulatore, immetti i seguenti dettagli:
   - Organizzazione Watson IoT Platform
   - Chiave API
   - token di autenticazione

3. Fai clic su **Run Simulator**. Potrebbero essere necessari alcuni minuti per la generazione.
4. Passa a Watson IoT Platform mentre il simulatore è in esecuzione e verifica che i dispositivi vengano creati e che gli eventi vengano inviati a questi dispositivi.
5. Procedi al [Passo 4. Configura DSX e visualizza i dati](#DSX_visualize_data).


## Passo 4. Configura DSX e visualizza i dati 
{: #DSX_visualize_data}

Per configurare DSX e avviare la visualizzazione dei dati:

1. [Configura un notebook Jupyter pre-configurato](#setup_jupyter_notebook) per ottenere le informazioni approfondite sui tuoi dati e per individuare le anomalie.
2. [Esegui l'analisi.](#run_analysis)
3. [Configura gli avvisi per le anomalie nel sensore](#config_alerts).


### 1. Configura un notebook Jupyter pre-configurato
{: #setup_jupyter_notebook}

Il notebook Jupyter è un'applicazione web che consente di creare e condividere i documenti che contengono il codice eseguibile, le formule matematiche, i grafici/visualizzazione (matplotlib) e il testo esplicativo.

Per configurare un notebook Jupyter pre-configurato per ottenere le informazioni approfondite e per individuare le anomalie:
1. Utilizza un browser supportato per accedere a [DSX ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://datascience.ibm.com/){: new_window} con il tuo ID Bluemix.
2. Fai clic su "+" e seleziona **Create project** per creare un nuovo progetto. I progetti creano spazi per raccogliere e condividere i notebook, il collegamento alle origini dati, creare pipeline e aggiungere dataset.
3. Specifica il nome del progetto e fai clic su **Create**. Durante la configurazione dell'account DSX, il servizio Spark e l'istanza Object Storage vengono creati automaticamente. In alternativa, puoi crearli utilizzando l'interfaccia Bluemix e quindi associarli al tuo progetto DSX in una fase successiva.
4. Importa le tue credenziali Cloudant selezionando il tuo progetto nel menu **DSX** e facendo clic su ![icona Trova e aggiungi dati](images/find_add_data_icon.png).
5. Fai clic sulla scheda **Connections**.
6. Fai clic su **Create Connection** per importare le tue credenziali Cloudant e renderle disponibili in qualsiasi notebook del tuo progetto.
7. Completa la schermata **New Connections** immettendo le seguenti informazioni:
   - Nome connessione
   - Imposta **Service Category** su `Data Service`.
   - Seleziona il tuo servizio Cloudant dal menu a discesa **Target service instance**.
   - Seleziona il database Cloudant corrispondete alla data corrente.
8. Fai clic su **Crea**.
9. Fai clic su **add notebooks** per creare un nuovo notebook Jupyter.
10. Seleziona **From URL** per caricare un notebook esistente, quindi specifica un nome descrittivo per il notebook e immetti il seguente URL per aprire il notebook di esempio:
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```
11. Fai clic su **Create Notebook**. Controlla che il notebook sia stato creato con metadati e codice.
12. Seleziona la cella che inizia con '!pip install --upgrade pixiedust, ,' e fai quindi clic su **Play** per eseguire il codice.
13. Quando l'installazione è stata completata, riavvia il kernel Spark facendo clic sull'icona **Restart Kernel**.
14. Attendi circa 10 secondi per il riavvio del kernel e fai clic sulla cella con il commento per selezionarla.
15. Importa le tue credenziali Cloudant in tale cella completando la seguente procedura:

    1. Fai clic su ![icona Trova e aggiungi i dati](images/find_add_data_icon.png).
    2. Seleziona la scheda **Connections**.
    3. Fai clic su **Insert to code**.
Viene creato un dizionario denominato credentials_1" con le tue credenziali Cloudant. Se il nome non viene specificato come "credentials_1", ridenomina la directory in "credentials_1" perché questo è il nome necessario all'esecuzione del codice del notebook.
16. Nella cella con il nome del database (dbname) immetti il nome del database Cloudant che è l'origine dati, ad esempio, `iotp_yourWIoTPorgId_default_Year-month-day`.
17. Salva il notebook. Il notebook è pronto per essere eseguito.


### 2. Esegui le analisi
{: #run_analysis}

1.	Seleziona la cella che contiene le tue credenziali Cloudant.
2.	Fai clic su **Play** per eseguire il codice della cella.
3.	Controlla i risultati dell'esecuzione e analizza il codice python utilizzato in ogni cella.
4.	Ripeti i passi 2 e 3 per ogni cella. Per le celle con **User input required** specificato, devi fornire nuovi valori di input alla variabile definita nella cella di codice successiva prima di eseguirla.

**Nota:** alcune cella eseguono in background lavori Spark e potrebbero impiegare più tempo per l'esecuzione. Quando l'esecuzione del codice all'interno di una cella viene completata, l'asterisco `*` diventa un numero, ad esempio, In `[*]` diventa In `[1]`. Dopo il completamento di questi passi, puoi creare regole cloud in {{site.data.keyword.iot_short_notm}} per generare automaticamente gli avvisi quando vengono individuate le anomalie.


### 3. Configura gli avvisi per le anomalie nel sensore 
{: #config_alerts}


Puoi creare regole cloud in {{site.data.keyword.iot_short_notm}}. Queste regole possono generare avvisi se vengono individuate anomalie durante la pubblicazione degli eventi che superano i valori di soglia che hai rilevato nel notebook.

In questo esempio, utilizziamo Nitrogen Dioxide (NO2) e le soglie superiore/inferiore per un dispositivo specifico. Stiamo creando un'azione email, in modo che venga inviata un'email a un indirizzo email specificato se il valore supera i valori di soglia che abbiamo impostato.

Per creare le regole cloud:

1. Crea uno schema:
    1. Nella scheda **Devices** del tuo dashboard {{site.data.keyword.iot_short_notm}}, seleziona la scheda **Manage Schemas**.
    2. Fai clic su **Add Schema**.
    3. Seleziona il DeviceType WS per il quale viene creato lo schema e fai clic su **Next**.
    4. Fa clic su **Add a property** per aggiungere il punto dati dai dispositivi collegati.
    5. Dalla scheda **Manual**, imposta il campo del tipo di dati su `Float` e il campo della proprietà su `NO2`.
    6. Fai clic su **OK**.
    7. Fai clic su **Finish**.

2. Crea un'azione:
    1. Seleziona la scheda **Rules** nel dashboard {{site.data.keyword.iot_short_notm}}.
    2. Seleziona la scheda **Actions**.
    3. Fai clic su **+Create an Action**.
    4. Nella schermata **Create New Action**, immetti un nome e seleziona "Send email" come tipo di azione.
    5. Fai clic su **Next**.
    6. Nella schermata **Edit Action**, attiva **Include Data**.
    7. Fai clic su **Finish**.
    8. Dalla scheda **Rules**, seleziona la scheda **Browse**.
    9. Fai clic su **+Create Cloud Rule**.
    10. Nella schermata **Add New Cloud Rule**, immetti un nome per la tua regola e seleziona il tuo nome dello schema nel campo **Applies to**. In questo esempio, il nome dello schema è "WS".
    11. Fai clic su **Next**.

3. Configura la condizione - I valori di soglia che utilizzi in questi passi si trovano nell'ultima parte di codice eseguito nel notebook, accanto al grafico NO2:
    1. Fai clic su New Condition e imposta la prima condizione:
    - Nel campo **Property** immetti `Nitrogen Dioxide`.
    - Nel campo **Operator** seleziona l'icona maggiore di `>`.
    - Nel campo **Value** immetti il valore di soglia maggiore.
    - Fai clic su **OK**.
    2. Seleziona OR e aggiungi la seconda condizione:
    - Nel campo **Property** immetti `Nitrogen Dioxide`.
    - Nel campo **Operator** seleziona l'icona minore di `<`.
    - Nel campo **Value** immetti il valore di soglia inferiore.
    - Fai clic su **OK**. Le condizioni per attivare la regola sono ora configurate.
4. Imposta l'azione su "Send email" e fai clic su **OK** per attivare la regola. Viene generato un avviso email se il valore della lettura Nitrogen Dioxide pubblicato da un dispositivo supera uno dei valori di soglia. Puoi eseguire il simulatore per visualizzare le email di avviso.


## Operazioni successive

Per ulteriori informazioni su DSX, consulta le seguenti risorse: 

 - [Regole cloud WIoTP ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window}
 - [DSX Community Notebooks and Tutorials ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} seguendo i link per ulteriori informazioni sui notebook Jupyter.
 - [Analytics recipes in the Watson IoT Platform cookbook ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
