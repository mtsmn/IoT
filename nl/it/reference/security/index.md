---

copyright:
years: 2016, 2017
lastupdated: "2017-10-04"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} security
{: #sec-index}

Come un servizio ospitato sul cloud {{site.data.keyword.iot_full}} incorpora la sicurezza come aspetto importante della sua architettura.
{: shortdesc}

Il seguente documento delle risposte a domande comuni su come i dati della tua organizzazione vengono protetti, si focalizza su aree specifiche:

* Conformità: standard esterni che impostano i riferimenti per la sicurezza.
* Autenticazione: assicura l'identità degli utenti, dei dispositivi o delle applicazioni che stanno tentando di accedere alle informazioni della tua organizzazione.
* Autorizzazione: il servizio si accerta che utenti, dispositivi e applicazioni abbiano l'autorizzazione per accedere alle informazioni della propria organizzazione.
* Codifica: assicura che i dati siano leggibili solo dalle parti autorizzate e non possano essere intercettati.

## {{site.data.keyword.iot_short_notm}} e {{site.data.keyword.Bluemix_notm}}
{: #iot-bluemix-sec}

{{site.data.keyword.iot_short_notm}} viene eseguito nella piattaforma {{site.data.keyword.Bluemix_notm}} e quindi si basa su {{site.data.keyword.Bluemix_notm}} e {{site.data.keyword.BluSoftlayer_full}} per l'accesso e la connettività. La dipendenza da {{site.data.keyword.Bluemix_notm}} e {{site.data.keyword.BluSoftlayer_notm}} rende la sicurezza e l'affidabilità di {{site.data.keyword.Bluemix_notm}} e {{site.data.keyword.BluSoftlayer_notm}} importante per gli utenti di {{site.data.keyword.iot_short_notm}}

Per ulteriori dettagli sulla sicurezza di {{site.data.keyword.Bluemix_notm}}, consulta [{{site.data.keyword.Bluemix_notm}} platform security](index.html#platform-security). 

## {{site.data.keyword.iot_short_notm}} Conformità di sicurezza
{: #compliance}  
![icona ISO 27K](../../images/icon_iso27k1.png "icona ISO 27K")   
{{site.data.keyword.iot_short_notm}} è certificato con lo standard International Organization for Standardization (ISO) 27001, che definisce le procedure consigliate per i processi di gestione della sicurezza delle informazioni. Lo standard ISO 27001 specifica i requisiti per stabilire, implementare e documentare i sistemi di gestione della sicurezza delle informazioni (ISMS) e i requisiti per applicare dei controlli di sicurezza in base alle esigenze delle singole organizzazioni. L'insieme di standard ISO 27000 incorpora un processo di ridimensionamento del rischio e valutazione delle risorse, con l'obiettivo di salvaguardare la riservatezza, l'integrità e la disponibilità delle informazioni scritte, orali ed elettroniche.

{{site.data.keyword.iot_short_notm}} è controllato da una società di sicurezza di terze parti e rispetta tutti i requisiti per l'ISO 27001: {{site.data.keyword.iot_short_notm}} ISO 27001:2013 Certificate of Registration.


## {{site.data.keyword.iot_short_notm}} Terminologia
{: #terminology}

![immagine](terminology_platform.svg)


## Come rendiamo sicura la gestione delle informazioni di IoT nella tua organizzazione?
{: #secure-org}

Le API REST e la GUI basate sul browser sono antecedute da HTTPS, che è un certificato firmato da DigiCert, in questo modo puoi controllare che ti stai collegando al genuino {{site.data.keyword.iot_short_notm}}. L'accesso della GUI basata sul web viene autenticato dal tuo ID IBM o {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.ssoshort}}. L'utilizzo dell'API REST richiede una chiave API, generata tramite la GUI, puoi utilizzarla per autenticare le chiamate API REST nella tua organizzazione.

![immagine](management_platform.svg)


## Come proteggiamo le credenziali di dispositivi e applicazioni?
{: #secure-credentials}

Quando i dispositivi vengono registrati o le chiavi API generate, il token di autenticazione viene distribuito in modo casuale. Questo significa che le credenziali della tua organizzazione non possono mai essere ripristinate dai nostri sistemi - anche nel caso spiacevole che sia compromesso {{site.data.keyword.iot_short_notm}}.

Le credenziali del dispositivo e le chiavi API possono essere revocate individualmente se sono compromesse.

![immagine](authentication_platform.svg)

## Come assicuriamo che i tuoi dispositivi siano collegati in modo sicuro a {{site.data.keyword.iot_short_notm}}?
{: #secure-device-connection}

I dispositivi si connettono utilizzando un ID client e il token di autenticazione generato quando i dispositivi sono stati aggiunti alla tua piattaforma. MQTT viene utilizzato per consentire l'interoperabilità semplice per molte piattaforme e linguaggi. {{site.data.keyword.iot_short_notm}} supporta la connettività su TLS v1.2.

**Importante:** le nuove organizzazioni sono automaticamente configurate per imporre la connessione dei dispositivi utilizzando la sicurezza TLS per impostazione predefinita, che assicura che i dispositivi possano collegarsi solo utilizzando una canale crittografato e sicuro. Tuttavia, {{site.data.keyword.iot_short_notm}} supporta anche casi in cui le organizzazioni devono abilitare i dispositivi a collegarsi senza TLS. Ad esempio, un organizzazione potrebbe utilizzare i dispositivi che non supportano TLS o dispositivi IoT a bassa potenza che non possono liberare la potenza necessaria per codificare o decodificare le trasmissioni. Il piano dell'organizzazione determina quali impostazioni possono essere utilizzate in questi casi.

Per ulteriori informazioni su come configurare la sicurezza di connessione, consulta [Configurazione delle politiche di sicurezza](set_up_policies.html).

![immagine](connectivity_platform.svg)


Per ulteriori informazioni sui requisiti della suite chipher e TLS, consulta la sezione [TLS requirements](connect_devices_apps_gw.html#tls_requirements)  nella documentazione `Application, device, and gateway connections to Watson IoT Platform`.

Puoi utilizzare i certificati e le politiche di sicurezza per migliorare la sicurezza della connessione del dispositivo. Le politiche di sicurezza possono essere impostate per consentire le connessioni non crittografate, per definire solo le connessioni TLS (transport layer security) e per consentire ai dispositivi di autenticarsi con i certificati lato client e nessun token. È possibile utilizzare le blacklist per specificare i dispositivi a cui non è consentito connettersi o le whitelist che possono essere utilizzate per consentire la connessione a dispositivi specifici. Per ulteriori informazioni sulla sicurezza avanzata, vedi [Gestione della sicurezza e del rischio](RM_security.html).

### Disabilitazione e abilitazione dei dispositivi e dei gateway 
{: #disable-devices}

Puoi utilizzare l'API HTTP **Authorization - Device Management** per disabilitare un dispositivo dalla connessione diretta alla piattaforma o da dietro un gateway. Ad esempio, puoi disconnettere in modo forzato il dispositivo di un utente dannoso o un dispositivo che non si sta comportando correttamente e sta causando problemi come l'utilizzo di dati non desiderati a causa di spam. L'API viene utilizzata per disconnettere il dispositivo dalla connessione corrente e per evitare il ricollegamento del dispositivo alla piattaforma.

Per abilitare o disabilitare un dispositivo, utilizza la seguente API, dove *${clientId}* è il ClientID codificato URL nel formato *d:${orgId}:${typeId}:${deviceId}* per i dispositivi o *g:${orgId}:${typeId}:${deviceId}* per i gateway:

    PUT /api/v0002/authorization/devices/${clientId}
    
Nel corpo della richiesta, utilizza un valore di stato di 0 per disabilitare il dispositivo o di 1 per abilitarlo. Ad esempio, il seguente valore di stato indica che il dispositivo è disabilitato:

    { "status": 0 }

Il codice di risposta per l'esito positivo è 200. 

Quando un gateway pubblica un dispositivo disabilitato, riceve una notifica di errore con un codice di risposta di 180. Per ulteriori informazioni, consulta [Notifiche gateway](../../gateways/mqtt.html#notification). 

Per ulteriori informazioni sull'API consulta [Device Security Beta APIs ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){:new_window} e passa a **Authorization - Device Management**.

## Come preveniamo la perdita dei dati tra i dispositivi IoT?
{: #prevent-leak-devices}

Il servizio utilizza schemi di messaggistica sicuri. Una volta autenticati, i dispositivi sono autorizzati solo alla pubblicazione e alla sottoscrizione in uno spazio argomenti limitato:

* '/iot-2/evt/<event_id>/fmt/<format_string>'
* '/iot-2/cmd/<command_id>/fmt/<format_string>'

Tutti i dispositivi operano con lo stesso spazio argomenti. Le credenziali di autenticazione fornite dal client determinano a quale dispositivo è destinato questo spazio dell'argomento da {{site.data.keyword.iot_short_notm}}.  Ciò impedisce ai dispositivi di poter impersonare un altro dispositivo.

L'unico modo per impersonare un altro dispositivo consiste nell'ottenere le credenziali di sicurezza compromesse per il dispositivo.


![immagine](device_scope_platform.svg)


Le applicazioni possono sottoscrivere e pubblicare su argomenti comandi e eventi per tutti i dispositivi nell'organizzazione. Le applicazioni possono analizzare i dati da molti dispositivi contemporaneamente e possono anche simulare o utilizzare un proxy per i dispositivi in aggiunta alla creazione del lato complementare di un loop di comunicazione full duplex.


## Come preveniamo la perdita dei dati IoT tra le organizzazioni?
{: #prevent-leak-org}

Lo spazio argomenti in cui operano i dispositivi e le applicazioni ha come ambito una singola organizzazione. Quando autenticato, {{site.data.keyword.iot_short_notm}} trasforma la struttura dell'argomento utilizzando un ID dell'organizzazione basato sull'autenticazione client, rendendo impossibile l'accesso ai dati di un'organizzazione da parte di un'altra.

![immagine](org_scope_platform.svg)

# Link correlati
{: #rellinks}
## Link correlati
{: #general}
* [Introduzione a {{site.data.keyword.iot_short_notm}}](https://console.ng.bluemix.net/docs/services/IoT/index.html)
* [{{site.data.keyword.Bluemix_notm}} security ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/docs/security/index.html#security){:new_window}
* [{{site.data.keyword.Bluemix_notm}} platform security ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/docs/security/index.html#platform-security){:new_window}
* [Conformità {{site.data.keyword.Bluemix_notm}} ](https://console.ng.bluemix.net/docs/security/index.html#compliance){:new_window}
* [{{site.data.keyword.BluSoftlayer_notm}} security ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](http://www.softlayer.com/security){:new_window}
* [{{site.data.keyword.BluSoftlayer_notm}} compliance ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](http://www.softlayer.com/compliance){:new_window}
