---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configurazione di {{site.data.keyword.iot_short_notm}} Edge (Panoramica)
{: #edge_configure}

**Importante:** la funzione {{site.data.keyword.iot_full}} Edge è disponibile solo come parte di un programma di anteprima limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Prima di poter iniziare a ricevere i dati dai dispositivi collegati al tuo nodo Edge, devi collegare il gateway a {{site.data.keyword.iot_short_notm}}. Il collegamento di un gateway a {{site.data.keyword.iot_short_notm}} implica la creazione di un tipo dispositivo gateway e la registrazione del gateway con {{site.data.keyword.iot_short_notm}}. Puoi quindi utilizzare le informazioni di registrazione per collegare il dispositivo gateway a {{site.data.keyword.iot_short_notm}}.

## Flusso di configurazione di alto livello

1. [Crea un'organizzazione](#edge_org) all'interno dell'ambiente di test di anteprima di {{site.data.keyword.iot_short_notm}} Edge.
2. [Abilita l'anteprima di {{site.data.keyword.iot_short_notm}}](#edge_enable).
3. Configura la tua organizzazione per utilizzare i nodi del gateway Edge con {{site.data.keyword.iot_short_notm}} [creando un tipo di gateway con Edge abilitato](#edge_gw_type), [aggiungendo dispositivi gateway](#edge_gw) e selezionando i servizi Edge da eseguire sui tuoi nodi Edge. Se necessario, puoi [creare servizi personalizzati](WIoTP_edge_dev.html#create_service).
4. [Configura i servizi Edge](#config_service).
5. [Installa Edge sui tuoi dispositivi](#edge_install_device).
6. [Gestisci e monitora i servizi Edge](#monitor_service).

## Crea un'organizzazione
{: #edge_org}

Crea un'organizzazione all'interno dell'ambiente di test di anteprima di {{site.data.keyword.iot_short_notm}} Edge completando la seguente procedura:
1. Registra un account {{site.data.keyword.Bluemix_notm}} e crea un'istanza del servizio {{site.data.keyword.iot_short_notm}} nella tua organizzazione {{site.data.keyword.Bluemix_notm}}. Puoi creare un'istanza {{site.data.keyword.iot_short_notm}} direttamente dalla pagina [{{site.data.keyword.iot_short_notm}} nel catalogo dei servizi IBM Cloud ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}.  
2. Completa le informazioni per eseguire il provisioning dell'organizzazione IBM Cloud. **Importante:** per utilizzare l'anteprima di {{site.data.keyword.iot_short_notm}} Edge devi distribuire la tua organizzazione IBM Cloud nell'ubicazione **Stati Uniti Sud**.
3. Fai clic su **Crea**.
4. Nell'istanza di {{site.data.keyword.iot_short_notm}} che si apre,fai clic su **Launch**.
Viene creata una nuova organizzazione {{site.data.keyword.iot_short_notm}}. L'ID organizzazione viene visualizzato all'inizio dell'URL e nel dashboard sotto il tuo nome utente. A questo punto, puoi abilitare {{site.data.keyword.iot_short_notm}} Edge per l'organizzazione.

## Abilita l'anteprima di {{site.data.keyword.iot_short_notm}} Edge
{: #edge_enable}

L'anteprima di {{site.data.keyword.iot_short_notm}} Edge è disabilitata per impostazione predefinita. Per abilitare {{site.data.keyword.iot_short_notm}} Edge:

1. Dal dashboard {{site.data.keyword.iot_short_notm}} relativo alla tua organizzazione, seleziona **Settings**.
2. Nella sezione **Experimental Features**, attiva le funzioni sperimentali, inclusa l'anteprima di {{site.data.keyword.iot_short_notm}} Edge.
3. Aggiorna il tuo browser per visualizzare la nuova funzione **Edge Services** e il collegamento rapido nel menu di navigazione del dashboard. **Nota:** se l'icona **Edge Services** non viene visualizzata nel menu di navigazione del dashboard, assicurati di aver selezionato **Stati Uniti Sud** per la tua ubicazione.

L'indicatore viene aggiunto all'archiviazione del browser locale.

## Crea un tipo di gateway con le funzionalità Edge abilitate
{: #edge_gw_type}

Ogni gateway collegato a {{site.data.keyword.iot_short_notm}} deve essere associato a un tipo di gateway. I tipi di gateway sono gruppi di dispositivi che condividono caratteristiche comuni.

1. Dal menu di navigazione principale del dashboard {{site.data.keyword.iot_short_notm}}, seleziona **Devices**.
2. Nella scheda **Device Types**, fai clic su **Add Device Type**.
3. Nella scheda **Identity** della pagina **Add Type**, seleziona il tipo **Gateway**, quindi immetti un nome per il tipo di gateway, ad esempio `my_gateway_type`, e una descrizione.
**Importante:** il nome del tipo di gateway non può avere più di 36 caratteri e può contenere solo:
<ul>
 <li>Caratteri alfanumerici (a-z, A-Z, 0-9)</li>
 <li>Trattini (-)</li>
 <li>Caratteri di sottolineatura (&lowbar;)</li>
 <li>Punti (.)</li>
 </ul>
3. Facoltativo: immetti i metadati e gli attributi per il tipo di gateway.
4. Attiva il pulsante **Edge Capabilities**.
5. Seleziona il tipo di architettura per il tipo di gateway. Il tipo di architettura deve corrispondere all'architettura del tuo dispositivo. Ad esempio, un dispositivo Odroid viene eseguito sull'architettura ARM64.
6. Fai clic su **Avanti**.
7. Facoltativo: nella scheda **Device Information**, immetti ulteriori dettagli e metadati relativi al tipo di dispositivo gateway.
8. Seleziona i servizi Edge da aggiungere ai tuoi dispositivi Edge. Il servizio Core IoT viene aggiunto a tutti i dispositivi, ma puoi aggiungere altri servizi dal catalogo completando la seguente procedura:
 1. Fai clic su **Add Services**.
 2. Nella finestra **Add Edge Services**, seleziona la versione appropriata dei servizi che vuoi aggiungere e fai quindi clic su **Done**.
9. Nella finestra **Add Gateway Type**, fai clic su **Done**.
Puoi procedere con [l'aggiunta di dispositivi gateway a questo tipo di gateway](#edge_gw).

## Aggiunta di dispositivi gateway
{: #edge_gw}

1. Dal menu di navigazione principale del dashboard {{site.data.keyword.iot_short_notm}}, seleziona **Devices**.
2. Fai clic su **Add Device**.
3. Nella scheda **Identity**, seleziona il tipo di dispositivo gateway a cui aggiungi i dispositivi e immetti un ID univoco per il dispositivo gateway che stai aggiungendo.
L'ID dispositivo viene utilizzato per identificare il dispositivo gateway nel dashboard {{site.data.keyword.iot_short_notm}} ed è anche un parametro obbligatorio per la connessione del tuo dispositivo gateway a {{site.data.keyword.iot_short_notm}}.
**Importante:** l'ID del tipo dispositivo non deve essere più lungo di 36 caratteri e può contenere solo:
 <ul>
 <li>Caratteri alfanumerici (a-z, A-Z, 0-9)</li>
 <li>Trattini (-)</li>
 <li>Caratteri di sottolineatura (&lowbar;)</li>
 <li>Punti (.)</li>
 </ul>
 **Suggerimento:** per i dispositivi collegati alla rete, l'ID del dispositivo può essere ad esempio l'indirizzo MAC del dispositivo senza i due punti di separazione.
4. Fai clic su **Avanti**.
5. Facoltativo: nella scheda **Device Information**, immetti ulteriori dettagli e metadati relativi al dispositivo gateway.
6. Fai clic su **Avanti**.
7. Nella scheda **Security** della pagina **Add Device**, genera automaticamente un token di autenticazione o fornisci il tuo proprio token di autenticazione per questo dispositivo. Se scegli di creare il tuo proprio token, assicurati che sia compreso tra 8 e 36 caratteri, contenga un mix di caratteri maiuscoli e minuscoli, numeri, un trattino, un carattere di sottolineatura o un punto. Il token non deve contenere sequenze di caratteri ripetuti, parole del dizionario, nomi utente o altre sequenze predefinite.
9. Fai clic su **Avanti**.
10. Fai clic su **Done**.
11. Nella pagina Device Information, copia e salva le seguenti informazioni sul dispositivo:
 - ID organizzazione, come ad esempio `tubo8x`
 - Tipo del dispositivo, come ad esempio `my_gateway_type`
 - ID del dispositivo. **Suggerimento:** per i dispositivi collegati alla rete, questo può essere ad esempio l'indirizzo MAC senza i due punti di separazione.
 - Metodo di autenticazione, come ad esempio `token`
 - Token di autenticazione, ad esempio `PtBVriRqIg4uh)_-Kl`
  Utilizza queste informazioni per collegare il gateway a {{site.data.keyword.iot_short_notm}} e iniziare a ricevere dati dai dispositivi collegati al gateway.

## Configurazione dei servizi Edge
{: #config_service}

Dopo aver creato un nuovo tipo di dispositivo con le funzionalità Edge abilitate, puoi selezionare diversi servizi da installare nel tuo nodo Edge.

1. Nel menu di navigazione principale del dashboard {{site.data.keyword.iot_short_notm}}, seleziona **Devices**.
2. Fai clic su **Device Types**.
3. Seleziona il tipo di dispositivo Edge che vuoi configurare.
4. Nella scheda **Edge Services**, seleziona l'icona della matita per modificare il record.
6. Fai clic su **Add Edge Services** per visualizzare un elenco dei servizi che sono stati caricati per questa organizzazione.
7. Nella finestra **Add Edge Services**, seleziona la versione appropriata dei servizi che vuoi aggiungere e fai quindi clic su **Done**.
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## Gestione e monitoraggio dei servizi Edge
{: #monitor_service}

Una volta installati i nodi Edge, {{site.data.keyword.iot_short_notm}} ti consente di monitorare lo stato dei servizi in esecuzione su nodi Edge.

1.	Nel menu di navigazione principale del dashboard {{site.data.keyword.iot_short_notm}}, seleziona **Devices**.
2.	Seleziona il dispositivo corrispondente al tuo nodo Edge
3.	Fai clic su **Edge Services** per visualizzare lo stato dei servizi installati sul nodo Edge.

## Ricerca di ulteriori informazioni
{: #more_info}

Per informazioni sulla connessione de tuo gateway a {{site.data.keyword.iot_short_notm}}, consulta [Connettività MQTT per i gateway](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt).

Per ulteriori informazioni su {{site.data.keyword.iot_short_notm}}, consulta [Introduzione a Watson IoT Platform](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp).

Per ulteriori informazioni su {{site.data.keyword.iot_short_notm}} Edge, consulta i seguenti argomenti:
- [Panoramica di {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge.html#edge_overview)
- [Installazione di {{site.data.keyword.iot_short_notm}} Edge sui dispositivi edge](WIoTP_edge_install.html#edge_install_device)
- [Sviluppo per {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
