---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Esercitazione introduttiva
{: #getting-started-with-iotp}

In questa esercitazione di introduzione a {{site.data.keyword.iot_full}}, connettiamo un dispositivo IoT a {{site.data.keyword.iot_short_notm}} e configuriamo l'analisi per esplorare i dati in tempo reale.
{:shortdesc}

<div id="prerequisites"></div>

## Prima di cominciare
{: #prereqs}

Ti servirà un [account IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/registration/){: new_window},
una istanza del servizio {{site.data.keyword.iot_short_notm}} e un dispositivo che soddisfa i seguenti requisiti:

*	Il tuo dispositivo deve essere in grado di comunicare utilizzando i protocolli HTTP o MQTT.

* I messaggi del dispositivo devono essere conformi ai requisiti del payload del messaggio di {{site.data.keyword.iot_short_notm}}.

Per ulteriori informazioni, consulta [Sviluppo dei dispositivi su Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Passo 1: registra il tuo dispositivo

Puoi aggiungere i dispositivi uno per volta dal [dashboard {{site.data.keyword.iot_short_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://internetofthings.ibmcloud.com){: new_window} o puoi utilizzare l'API [Watson IoT Platform ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} per aggiungere più dispositivi.

Per aggiungere un dispositivo dal dashboard {{site.data.keyword.iot_short_notm}}:

1. Nella console {{site.data.keyword.Bluemix_notm}}, fai clic su **Avvia** nella pagina dei dettagli del servizio {{site.data.keyword.iot_short_notm}}.

    Si apre la console web {{site.data.keyword.iot_short_notm}} in una nuova scheda del browser al seguente URL:

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Dove *org_id* è l'ID della [tua {{site.data.keyword.iot_short_notm}} organizzazione](iotplatform_overview.html#organizations){: new_window}.

2. Nel dashboard della panoramica, dal pannello del menu, seleziona **Devices** e fai clic su **Add Device**.

3. Crea un tipo di dispositivo per il dispositivo che stai aggiungendo.

    Ogni dispositivo collegato a {{site.data.keyword.iot_short_notm}} deve essere associato a un tipo dispositivo. I tipi di dispositivo sono gruppi di dispositivi che condividono caratteristiche comuni.

    1. Fai clic su **Create device type**.
    2. Immetti un nome dispositivo, come `my_device_type`, e una descrizione per il tipo di dispositivo.
    
        > **Nota:** il nome del tipo di dispositivo non deve avere una lunghezza superiore ai 36 caratteri e può contenere solo caratteri alfanumerici (a-z, A-Z, 0-9) e i seguenti caratteri: `_`, `.` e `-`.

    3. Facoltativo: immetti i metadati e gli attributi per il tipo dispositivo.

        Puoi aggiungere e modificare gli attributi e i metadati successivamente.
        {: tip}

4. Fai clic su **Next** per avviare il processo di aggiunta del tuo dispositivo con il tipo dispositivo.

5. Immetti un ID dispositivo, ad esempio `my_first_device`.

    L'ID dispositivo viene utilizzato per identificare il dispositivo nel dashboard {{site.data.keyword.iot_short_notm}} ed è anche un parametro obbligatorio per la connessione del tuo dispositivo a {{site.data.keyword.iot_short_notm}}.

    > **Nota:** il nome del tipo di dispositivo non deve avere una lunghezza superiore ai 36 caratteri e può contenere solo caratteri alfanumerici (a-z, A-Z, 0-9) e i seguenti caratteri: `_`, `.` e `-`.

    Per i dispositivi connessi alla rete, l'ID dispositivo può essere l'indirizzo MAC del dispositivo senza i caratteri due punti di separazione.

5. Fai clic su **Next** per completare il processo.

6. Fornisci un token di autenticazione oppure accetta un token generato automaticamente. Se scegli di creare un tuo token, assicurati che abbia un nome di lunghezza compresa tra gli 8 e i 36 caratteri e che contenga solo caratteri alfanumerici e i seguenti caratteri: `_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)`, and `-`.

    Il token non deve contenere sequenze di caratteri ripetuti, parole del dizionario, nomi utente o altre sequenze predefinite.

7. Verifica che le informazioni di riepilogo siano corrette e quindi fai clic su **Add** per aggiungere la connessione.

8. Nella pagina delle informazioni del dispositivo, copia e salva i seguenti dettagli:

    * ID organizzazione
    * Tipo di dispositivo
    * ID dispositivo
    * Metodo di autenticazione
    * Token di autenticazione

    Ti serviranno l'ID organizzazione, il tipo di dispositivo, l'ID del dispositivo e il token di configurazione per configurare il tuo dispositivo per la connessione a {{site.data.keyword.iot_short_notm}}.

## Passo 2: connetti il tuo dispositivo a {{site.data.keyword.iot_short_notm}}

1. Configura il tuo dispositivo per la messaggistica MQTT e per l'autenticazione utilizzando l'ID organizzazione, il tipo di dispositivo, l'ID dispositivo e il token di autenticazione.

2. Invio dei messaggi del dispositivo alla tua organizzazione {{site.data.keyword.iot_short_notm}} utilizzando il protocollo MQTT.

    Le seguenti informazioni sono necessarie quando connetti il tuo dispositivo:

    * URL: *org_id*.messaging.internetofthings.ibmcloud.com

      Dove *org_id* è l'ID della tua organizzazione {{site.data.keyword.iot_short_notm}}.

    * Porta:
      * 1883
      * 8883 (crittografata)
      * 443 (socket web)
    * ID dispositivo: d:_id_organizzzione:tipo_dispositivo:id_dispositivo_
    * Nome utente: use-token-auth
    * Password: _Token di autenticazione_
    * Formato argomento evento: iot-2/evt/_id_evento/fmt/stringa_formato_

      Dove _id_evento_ specifica il nome evento visualizzato in {{site.data.keyword.iot_short_notm}} e _stringa_formato_ è il formato dell'evento, come ad esempio JSON.

    * Formato messaggio: JSON

  Per ulteriori informazioni, consulta [Connettività MQTT per i dispositivi](/docs/services/IoT/devices/mqtt.html).

## Passo 3: crea tabelle e schede per tenere traccia dei dati dispositivo

Utilizzando tabelle e schede, puoi visualizzare dei grafici che rappresentano i valori dei dataset da uno o più dispositivi per una rapida panoramica e comprensione dei dati del dispositivo.

1. Nel tuo dashboard {{site.data.keyword.iot_short_notm}}, fai clic su **Create New Board**.

2. Immetti un nome e una descrizione per la tabella.

3. Fai clic su **Next** e quindi su **Create**.

4. Fai clic sulla tabella che hai appena creato e fai quindi clic su **Add New Card**. Seleziona Devices come tipo di scheda. La seguente tabella descrive le opzioni di visualizzazione da cui puoi scegliere.

| Tipo | Dati visualizzati |
|---------------|---------------|
| Visualizzazione generica | Il valore di uno o più dataset. Scegli la dimensione del widget grande per visualizzare fino a tre valori di punto dati in una piccola tabella. |
| Grafico a linee | Uno o più dataset in un grafico a scorrimento in tempo reale. Utilizza il menu delle impostazioni per impostare l'intervallo e la conservazione dei dati, l'aspetto dei grafici e altro. |
| Grafico a barre | I valori dei dataset nelle barre etichettate. Utilizza il menu delle impostazioni per cambiare la direzione della barra, verticale o orizzontale. |
| Grafico ad anello | Due o più dataset in una rappresentazione circolare. |
| Valore | Il valore non elaborato di uno o più dataset. |
| Misuratore | Il valore del dataset visualizzato come un misuratore. Utilizza il menu delle impostazioni per facoltativamente impostare le soglie del misuratore per gli intervalli di dati inferiore, medio e superiore. |
| Proprietà del dispositivo | Proprietà specifiche per uno o più dispositivi. |
| Tutte le proprietà del dispositivo | Tutte le proprietà per uno o più dispositivi. |
| Elenco dispositivi | Un elenco per monitorare più dispositivi. Un elenco può essere utilizzato come un'origine dati per altre schede. |
| Informazioni dispositivo | Informazioni di base per un solo dispositivo. |
| Associazione dispositivo | L'ubicazione dei dispositivi in un elenco di dispositivi. |

{: caption="Tabella 1. Opzioni di visualizzazione" caption-side="top"}

## Passo 4: crea schemi di tipo di dispositivo

A questo punto, dovrai creare uno schema del tipo di dispositivo e associare le proprietà dispositivo per creare quindi le regole attivate in base ai punti di dati dalle tue proprietà dispositivo associate.

1. Nel dashboard {{site.data.keyword.iot_short_notm}}, vai a **Devices > Manage Schemas** e fai clic su **Add Schema**.

2. Seleziona un tipo di dispositivo da associare allo schema del messaggio. Può essere definito un solo schema per un tipo di dispositivo.

3. Fai clic su **Add property** e aggiungi una o più proprietà.

    Puoi selezionare le proprietà dal dispositivo collegato, creare proprietà virtuali per modificare o da combinare a proprietà esistenti o aggiungere le proprietà manualmente.

    Le proprietà disponibili sono definite nel payload dei messaggi inviati da un dispositivo.
    {: tip}

    * Per aggiungere una proprietà manualmente, seleziona la **scheda Manual** e definisci i seguenti dettagli di proprietà:
        * Nome
        * Tipo di dati
        * Proprietà
        * Unità dati (facoltativo)
        * Posizioni decimali (facoltativo)

    * Per creare una proprietà virtuale, seleziona la scheda **Virtual Property** e definisci i seguenti dettagli di proprietà:
        * Nome
        * Tipo di dati
        * Proprietà
        * Calcolo
        * Unità dati (facoltativo)
        * Posizioni decimali

    * Per selezionare le proprietà da un dispositivo connesso, seleziona la scheda **From Connected** e seleziona quindi una o più proprietà da aggiungere allo schema. Le proprietà selezionate vengono aggiunte e la descrizione è impostata sul nome della proprietà.

4. Fai clic su **Finish** per creare le proprietà.

## Passo 5: crea regole e azioni

Le regole sono punti di decisione basati sulle condizioni che mettono in corrispondenza i dati in tempo reale con valori di soglia predefiniti o altri dati di proprietà per attivare un avviso se viene soddisfatta una condizione. In aggiunta all'avviso che viene visualizzato nel dashboard {{site.data.keyword.iot_short_notm}}, puoi aggiungere una o più azioni per eseguire la logica di business quando viene attivata una regola.

1. Nel dashboard {{site.data.keyword.iot_short_notm}}, vai a **Rules** e fai clic su **Create Cloud Rule**.

2. Immetti un nome e una descrizione per la regola, seleziona un tipo di dispositivo a cui si applica la regola e fai clic su **Next**.

3. Per configurare la logica della regola, aggiungi una o più condizioni IF da utilizzare come trigger per la regola.

    Puoi aggiungere condizioni in righe parallele per applicarle come condizioni OR o puoi aggiungere le condizioni in colonne sequenziali per applicarle come condizioni AND. Dai un'occhiata ai seguenti esempi:

      * Una semplice regola potrebbe attivare un avviso se un valore di parametro è più grande di un valore specificato: Condition = `temp_cpu>80`
      * Una regola più complessa potrebbe essere attivata quando viene raggiunta una combinazione di soglie: Condition = `temp_cpu>60 AND cpu_load>90`

    Per attivare una condizione che confronta due proprietà o per attivare due o più condizioni della proprietà combinate sequenzialmente utilizzando AND, includi i punti dei dati di attivazione nello stesso messaggio del dispositivo. Se i dati sono ricevuti in più di un messaggio, la condizione o le condizioni in sequenza non vengono attivate.
    {: tip}

4. Configura i requisiti di attivazione condizionali per la tua regola.

    Puoi utilizzare i requisiti di trigger condizionale per controllare il numero di avvisi attivato per una regola in un periodo di tempo. L'attivazione condizionale agisce su qualsiasi condizione nella regola. Ad esempio, se una regola ha cinque condizioni parallele impostate utilizzando OR, ogni condizione che è true viene conteggiata nel numero di trigger condizionali.

      1. Nell'editor della regola, fai clic sul link **Trigger each time conditions are met** predefinito per aprire la finestra di dialogo per la configurazione dei requisiti della frequenza della serie.
      2. Seleziona e configura il trigger condizionale che desideri utilizzare nella regola.

        * Attiva ogni volta che le condizioni sono soddisfatte
        * Attiva se le condizioni sono soddisfatte N volte in M _Unità di tempo_

5. Crea o seleziona una o più azioni che si verificano se vengono soddisfatte le condizioni della regola.

    Ad esempio, un'azione può consistere nell'inviare una email o di inserire un webhook.

6. Facoltativo: seleziona una priorità di avviso per la regola.

    La priorità viene utilizzata per classificare gli avvisi che vengono visualizzati nella tabella **Boards > Rule-Based Analytics**. La priorità predefinita è bassa.

7. Fai clic su **Save** per salvare senza attivare o su **Activate** per salvare e attivare la tua regola.

    Quando attivi la regola, alla tabella **Rule-Based Analytics** viene aggiunto un avviso quando vengono soddisfatte le condizioni e vengono eseguire tutte le azioni della regola.

## Passi successivi

Estendi le funzioni di analisi dati creando e connettendo le tue proprie applicazioni per utilizzare i dati del dispositivo cronologici e in tempo reale.

  * Consulta le [librerie client](/docs/services/IoT/iot_platform_client_lib.html) per gli strumenti per creare il codice per integrare e connettere i tuoi dispositivi e le tue applicazioni.

  * Esplora le [recipe di Watson IoT Platform ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} per ulteriori esercitazioni sull'integrazione dell'analisi avanzata nei tuoi dispositivi e nelle tue applicazioni connessi.
