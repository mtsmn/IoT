---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Panoramica di {{site.data.keyword.iot_short_notm}} Edge (Anteprima)
{: #edge_overview}

**Importante:** la funzione {{site.data.keyword.iot_full}} Edge è disponibile solo come parte di un programma di anteprima limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} Edge ti consente di estendere le funzionalità di {{site.data.keyword.iot_short_notm}} sui dispositivi Edge. I dispositivi Edge risiedono al margine di una rete all'interno di un'organizzazione. Gli esempi includono sensori e controllori industriali in un'ubicazione fisica, ad esempio una fabbrica. Con {{site.data.keyword.iot_short_notm}} Edge, i dati possono essere elaborati all'interno dei dispositivi prima di essere inviati al cloud.

Per utilizzare {{site.data.keyword.iot_short_notm}} Edge, crea i nodi Edge e configurali con i servizi che devono essere eseguiti al loro interno. WIoTP Edge distribuisce quindi tali servizi ai nodi. Con i servizi Edge in esecuzione, i nodi Edge possono inviare messaggi dai dispositivi con il gateway Edge. Il gateway elabora i messaggi, trasforma i dati e quindi può inviare i dati al cloud, a seconda di come sono configurate le interfacce.

L'anteprima di {{site.data.keyword.iot_short_notm}} Edge fornisce il servizio predefinito Core IoT, insieme alla possibilità di creare servizi personalizzati, come un servizio per prevedere errori in un robot del piano di produzione e inviare notifiche di errore. Un esempio di dispositivo edge è un sensore che monitora l'utilizzo della CPU e invia un avviso quando l'utilizzo supera una determinata percentuale.

## Componenti di {{site.data.keyword.iot_short_notm}} Edge

**Nodo Edge**
Un nodo Edge è costituito da un gateway e da un dispositivo che risiede al margine della rete.

**Dispositivo Edge**
Un dispositivo, ad esempio un sensore, che risiede al margine di una rete ed esegue servizi che elaborano i dati prima che vengano inviati al cloud. Un esempio di dispositivo edge è un sensore che monitora l'utilizzo della CPU e invia un avviso quando l'utilizzo supera una determinata percentuale.

**Gateway Edge**
I gateway sono dispositivi specializzati dotati delle funzionalità combinate di un'applicazione e di un dispositivo, che consente loro di fungere da punti di accesso per altri dispositivi. Quando Edge è abilitato, i gateway Edge consentono ai dispositivi che non possono collegarsi direttamente a Internet ad accedere al servizio {{site.data.keyword.iot_short_notm}} collegandosi prima al dispositivo gateway su Edge.

**Tipo di gateway Edge**
Un tipo di gateway raggruppa dispositivi gateway che condividono attributi, come numero di modello o ubicazione. Quando le funzionalità Edge sono abilitate e viene aggiunto un dispositivo gateway edge a {{site.data.keyword.iot_short_notm}}, gli attributi del tipo di gateway Edge vengono utilizzati come modello per il nuovo dispositivo gateway Edge.

**Servizi Edge**
I servizi sono funzionalità che vengono aggiunte a un gateway Edge {{site.data.keyword.iot_short_notm}} ed eseguono processi come la manipolazione dei dati. Puoi creare le applicazioni che utilizzano i servizi.

**Catalogo dei servizi Edge**
Il servizio Core IoT predefinito è incluso nel catalogo e viene aggiunto a tutti i nodi Edge. Puoi aggiungere servizi personalizzati in base alle tue esigenze aziendali. Il catalogo dei servizi Edge contiene tutti i servizi personalizzati disponibili.

**Repository file server**
Il Repository file server contiene immagini e contenitori docker. Tutte le funzionalità o i servizi di elaborazione Edge funzionano all'interno dei contenitori docker. Puoi selezionare le immagini docker da eseguire all'interno dei dispositivi.

## Ricerca di ulteriori informazioni
{: #more_info}

Per ulteriori informazioni su configurazione, installazione e sviluppo per {{site.data.keyword.iot_short_notm}} Edge, consulta i seguenti argomenti:
 - [Configurazione di {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
 - [Installazione di {{site.data.keyword.iot_short_notm}} Edge sui dispositivi edge](WIoTP_edge_install.html#edge_install_device)
 - [Sviluppo per {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
