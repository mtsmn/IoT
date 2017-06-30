---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# Configurazione delle politiche di sicurezza
{: #set_up_policies.md}

Quando viene utilizzato un piano di sicurezza avanzato (ASP) per un'organizzazione, un analista della sicurezza può configurare le politiche di sicurezza della connessione e le blacklist o le whitelist. Quando viene utilizzato un piano standard, l'analista può configurare impostazioni che hanno meno opzioni e non può configurare le blacklist o le whitelist.

Per informazioni sull'utilizzo delle API per gestire le politiche, consulta [IBM Watson IoT Platform Risk Management APIs ![Icona link esterno](../../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/riskmgmt.html){: new_window}.

## Configurazione delle politiche di sicurezza della connessione per la sicurezza avanzata
{: #config_connect}

Puoi configurare il livello di sicurezza predefinito che viene applicato a tutti i dispositivi. Puoi quindi aggiungere impostazioni di sicurezza personalizzate per dispositivi specifici.

1. Nella pagina Security **Policies**, fai clic su **Configure** accanto a **Connection Security**.
2. In **Default Connection Security**, seleziona il livello di sicurezza della connessione predefinito dall'elenco a discesa. Il valore che selezioni viene utilizzato per tutti i dispositivi, con l'eccezione dei dispositivi con le impostazioni di connessione personalizzate. Queste politiche influenzano il modo in cui i dispositivi si connettono al server ma non modificano alcuna impostazione sul dispositivo effettivo né inviano alcun messaggio al dispositivo. Puoi selezionare uno dei seguenti livelli di sicurezza come predefinito:
    - TLS facoltativo
    - TLS con autenticazione token
    - TLS con autenticazione certificato client
    - TLS con autenticazione certificato client e token
    - TLS con certificato client o token
3. Se necessario, fai clic su **Add Custom Connection** e seleziona i tipi di dispositivo e i livelli di sicurezza personalizzati.
3. Fai clic su **Refresh Compliance**. In base al livello di sicurezza da te selezionato, la tabella aggiornata mostra il numero di dispositivi interessati e il livello previsto di conformità al livello di sicurezza impostato.
4. Fai clic su **Save**.

**Nota:**
Puoi anche accedere alle impostazioni della sicurezza di connessione dalla pagina **General** in **Settings**. Fai clic su **Open Connection Security Policy**.

## Configurazione delle politiche di connessione per la sicurezza standard
{: #config_connect_standard}

Per le organizzazioni che utilizzano la sicurezza standard, modifica le impostazioni di sicurezza nella pagina **General** in **Settings**. Puoi configurare il livello di sicurezza predefinito che viene applicato a tutti i dispositivi.

1. In **Settings**, seleziona **General**.
2. In **Connection Security**, seleziona il livello di sicurezza della connessione predefinito dall'elenco a discesa. Il valore che selezioni viene utilizzato per tutti i dispositivi. Queste politiche influenzano il modo in cui i dispositivi si connettono al server ma non modificano alcuna impostazione sul dispositivo effettivo né inviano alcun messaggio al dispositivo. Puoi selezionare uno dei seguenti livelli di sicurezza come predefinito:
    - TLS facoltativo
    - TLS con autenticazione token
    - TLS con autenticazione certificato client e token
4. Fai clic su **Save**.

## Configurazione delle blacklist e delle whitelist
{: #config_black_white}

Le organizzazioni che utilizzano la sicurezza avanzata possono limitare l'accesso al server da alcuni dispositivi utilizzando una blacklist o possono utilizzare una whitelist per concedere l'accesso al server a dispositivi specifici. Puoi utilizzare una blacklist o una whitelist ma non puoi utilizzare entrambe.

### Configura una blacklist
{: #config_blacklist}

1. Nella pagina Security **Policies**, nella sezione **Blacklist**, fai clic su **Configure**.
2. Nella pagina **Blacklist**, fai clic su **Add to Blacklist**.
3. Nella finestra **Add to Blacklist**, esegui una delle seguenti azioni:
    - Nella scheda **IP Address/Range**, immetti gli indirizzi IP dei dispositivi che intendi bloccare o gli intervalli di indirizzi IP per più dispositivi consecutivi.
    - Nella scheda **CIDR**, immetti un blocco annotato CIDR (Classless Inter-Domain Routing ).
    - Nella scheda **Country**, immetti o seleziona i paesi da cui desideri bloccare tutti i dispositivi.
4. Nella finestra **Add to Blacklist**, fai clic su **Save**.
5. Rivedi l'elenco dei dispositivi bloccati. Tutti i dispositivi che fanno parte dell'elenco, che è basato sull'indirizzo IP, il CIDR o il paese, non possono connettersi al server {{site.data.keyword.iot_short_notm}}.
6. Fai clic su **Save**.
7. Abilita la blacklist. Se avevi una whitelist abilitata, viene disabilitata.

### Configura una whitelist
{: #config_whitelist}

1. Nella pagina Security **Policies**, fai clic su **Configure** accanto a **Whitelist**.
2. Nella pagina **Whitelist**, fai clic su **Add to Whitelist**.
3. Nella finestra **Add to Whitelist**, esegui una delle seguenti azioni:
    - Nella scheda **IP Address/Range**, immetti gli indirizzi IP dei dispositivi a cui desideri consentire l'accesso o l'intervallo di indirizzi IP per più dispositivi consecutivi.
    - Nella scheda **CIDR**, immetti un blocco annotato CIDR (Classless Inter-Domain Routing ).
    - Nella scheda **Country**, immetti o seleziona i paesi da cui desideri consentire l'accesso per tutti i dispositivi.
4. Nella finestra **Add to Whitelist**, fai clic su **Save**.
5. Rivedi l'elenco dei dispositivi consentiti. Tutti i dispositivi che fanno parte dell'elenco, che è basato sull'indirizzo IP, il CIDR o il paese, possono connettersi al server {{site.data.keyword.iot_short_notm}}.
6. Fai clic su **Save**.
7. Abilita la whitelist. Se avevi una blacklist abilitata, viene disabilitata.
