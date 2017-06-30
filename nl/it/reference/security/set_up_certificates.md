---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-15"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Configurazione dei certificati
{: #set_up_certificates}

I certificati sono utilizzati per l'autenticazione del dispositivo o per sostituire il certificato del server {{site.data.keyword.iot_full}} predefinito per la messaggistica MQTT. A ogni dispositivo che non dispone di certificati firmati validi viene negato l'accesso e non può comunicare con il server.

Per configurare i certificati e l'accesso al server per i dispositivi, l'operatore di sistema registra i certificati dell'autorità di certificazione (CA, Certificate Authority) e i certificati del server di messaggistica nella piattaforma {{site.data.keyword.iot_short_notm}}.

Per informazioni sull'utilizzo delle API per gestire i certificati CA e i certificati dei server di messaggistica, consulta [IBM Watson IoT Platform Authentication and Authorization APIs ![Icona link esterno](../../../../icons/launch-glyph.svg)"Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}.

## Certificati CA
I certificati CA consentono all'organizzazione di riconoscere i certificati client sui dispositivi come attendibili in modo che i dispositivi possano collegarsi al server.

Se aggiungi un certificato CA o sostituisci il certificato del server di messaggistica, tutti i dispositivi devono connettersi utilizzando un client MQTT che supporta SNI (Server Name Indication) in modo che tale server possa utilizzare i CA appropriati per l'autenticazione del dispositivo.

Puoi impostare il livello di sicurezza della connessione configurando la politica di sicurezza della connessione. Per ulteriori informazioni su come eseguire questa operazione, consulta [Configurazione delle politiche di sicurezza](set_up_policies.html).

## Certificati client

I singoli certificati client (dispositivo o gateway) rimangono sui dispositivi e non sono caricati sulla piattaforma. Il certificato di firma CA utilizzato per firmare tutti i certificati dispositivo e gateway è l'unico certificato che carichi sulla piattaforma. Se stai utilizzando i certificati server di messaggistica autofirmati, devi caricare i certificati root e intermedi utilizzati per firmare il certificato client (cert.pem).

Il singolo certificato dispositivo o gateway che firmi con il certificato CA deve avere l'ID dispositivo o l'ID gateway inserito come CN (Common Name) o SubjectAltName nel certificato.

Per i dispositivi, il formato del campo **CN** è `CN=d:tipodispositivo:iddispositivo` e il formato del campo **SubjectAltName** è `SubjectAltName=email:d:*tipodispositivo:iddispositivo*` dove `email:d` è costante e `*tipodispositivo*` è il tipo di dispositivo e `*iddispositivo*` è l'ID del dispositivo nella piattaforma.

Per i gateway, il formato del campo **CN** è `CN=g:Idtipo:Iddispositivo` e il formato del campo **SubjectAltName** è SubjectAltName=email:g:*tipodispositivo:iddispositivo*

Nota: non includere `orgId` nei campi **CN** o **SubjectAltName** per i certificati dispositivo o gateway.  `orgId` deve essere fornito come parte delle informazioni SNI fornite dall'implementazione client quando si stabilisce una connessione al server di messaggistica.

Per ulteriori informazioni sui certificati client, consulta [la ricetta Connect Raspberry Pi to IBM Watson IoT Platform using Client side Certificates ![Icona link esterno](../../../../icons/launch-glyph.svg) "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}

## Certificati del server di messaggistica

I certificati del server di messaggistica accettano il dominio predefinito, internetofthings.ibmcloud.com. È necessario rispettare il seguente formato per il CN certificato o perSubjectAltName:

- `orgId.messaging.internetofthings.ibmcloud.com` (dominio IoTP)

Il seguente esempio mostra un CN valido per il certificato server:

`mtxpd0.messaging.internetofthings.ibmcloud.com`

Per ulteriori informazioni sui certificati del server di messaggistica, consulta [la ricetta Connect Raspberry Pi to IBM Watson IoT Platform using Self-Signed Server Certificate ![Icona link esterno](../../../../icons/launch-glyph.svg) "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}

### Custom domains (Beta)
{: #custom_domains}

**Importante**: La funzione Custom domains per i certificati del server di messaggistica è disponibile solo come parte di un programma Beta limitato. Per abilitare i domini personalizzati, attiva **Experimental Features** nella pagina **Settings**.

Come parte della funzione Beta, i certificati del server di messaggistica accettano i domini personalizzati. È necessario rispettare il seguente formato per il CN certificato o perSubjectAltName:

- `orgId.messaging<dominio personalizzato>`

Il campo **CN** accetta i caratteri jolly per i domini personalizzati, come mostrato nel seguente esempio:

- `CN=*.messaging.fab-iot.com`

**Importante**: per i domini personalizzati, è richiesto un servizio DNS esterno per risolvere il dominio personalizzato al server di messaggistica {{site.data.keyword.iot_full}}. Questo servizio DNS non è fornito dalla piattaforma.

## Registrazione dei certificati dell'autorità di certificazione (CA, Certificate Authority) per l'autenticazione del dispositivo
{: #reg_ca_cert}

1. Accedi a {{site.data.keyword.iot_short_notm}} e passa a **General Settings**.
2. Nella sezione **Security**, in **CA Certificates**, fai clic su **Add Certificate**.
3. Individua e seleziona un file del certificato da caricare o trascina un file nella finestra **Add Certificate**.Il file può avere solo un certificato al suo interno e le date del certificato devono essere valide. Sono accettati solo i certificati nel formato .pem o. der. Puoi visualizzare in anteprima le informazioni del certificato nel file selezionato.
4. Immetti una descrizione del file del certificato.
5. Conferma che il file corretto sia selezionato e fai clic su **Save**. Il certificato selezionato viene elencato nella tabella ed è attivo per impostazione predefinita.

## Registrazione dei certificati del server di messaggistica
{: #reg_msg_cert}

Viene fornito un certificato server predefinito con la piattaforma. Puoi utilizzare il certificato predefinito o caricarne uno dalla tua organizzazione. Se ancora non hai un certificato da utilizzare, puoi creare una richiesta per un nuovo certificato. Dopo aver ricevuto il nuovo certificato, devi firmarlo e ricaricarlo nella piattaforma.

**Nota:** le pagine del dashboard della piattaforma possono eseguire connessioni interne al server di messaggistica per richiamare le informazioni sul dispositivo. Quando si configurano i certificati server autofirmati per un'organizzazione, gli utenti dashboard devono aggiungere il certificato server come un certificato attendibile nei loro browser per evitare problemi di connessione perché i browser, per impostazione predefinita, non riconosceranno il server di messaggistica come un server attendibile.

## Caricamento di un certificato del server di messaggistica dalla tua organizzazione
{: #upload_cert}
1. Nella sezione **Security** di **General Settings**, in **Messaging Server Certificates**, fai clic su **Add Certificate**.
2. Individua e seleziona un file del certificato da caricare o trascina un file nella finestra **Add Certificate**.
3. Individua e seleziona un file della chiave privata da caricare o trascina un file nella finestra **Add Certificate**.
4. Immetti la passphrase della chiave privata se è stata crittografata con una passphrase.
5. Conferma che il file corretto sia selezionato e fai clic su **Save**.

## Attivazione di un certificato del server di messaggistica

Per attivare il certificato predefinito o un altro certificato che è già stato caricato, seleziona il certificato che desideri utilizzare dall'elenco a discesa **Default messaging server certificate** nella tabella in **Messaging Server Certificates**.Il certificato selezionato viene elencato nella tabella come un certificato attivo.

## Richiesta di un nuovo certificato del server di messaggistica
{: #request_cert}

Se desideri utilizzare un nuovo certificato del server di messaggistica, puoi generare una richiesta firma certificato (CSR) per richiedere un nuovo certificato.

1. Nella sezione **Security Management** di **General Settings**, in **Messaging Server Certificates**, fai clic su **Generate CSR**.
2. Immetti i dettagli per richiedere una CSR per il tuo server e fai clic su **Generate**. La CSR viene visualizzata nella tabella.
3. Scarica la richiesta e inviala all'autorità di certificazione (CA, Certificate Authority) per la firma.
4. Una volta ottenuto un certificato, torna alla voce CSR nella tabella e carica il nuovo certificato. Dopo aver caricato il certificato, la CSR nella tabella viene sostituita dal certificato caricato.
