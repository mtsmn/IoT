---

copyright:
years: 2017, 2018
lastupdated: "2018-08-28"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione alla gestione dei dati
{: #device_twins}

<!--An unprecedented number of devices and sensors exist in the modern world. Connected devices generate vast amounts of digital data at extraordinary speeds. Such volumes of data represent great opportunities but also challenges, in terms of how big data can be processed, analyzed and presented to help to deliver insights and drive transformation.-->

I dispositivi potrebbero fornire output di dati simili, ma possono variare per marca, modello e versione e possono produrre dati in diversi formati. Ad esempio, un dispositivo con un sensore di temperatura in un ufficio potrebbe riportare la temperatura in gradi Fahrenheit o in gradi Celsius. Non è efficiente configurare le applicazioni per poter utilizzare i dati in tutti questi formati; è necessario invece che i dati vengano raccolti, trasformati e normalizzati per creare un singolo modello logico in modo che un'applicazione possa interagire con i diversi dispositivi allo stesso modo. 

Il componente di gestione dei dati di {{site.data.keyword.iot_short_notm}} include una funzione di dispositivo gemello e una funzione di asset gemello. La funzione di dispositivo gemello ti consente di trarre vantaggio dalla raccolta, trasformazione e normalizzazione dei diversi formati dei dati del dispositivo in un singolo modello logico. La funzione di asset gemello ti consente di raggruppare diversi dispositivi per creare un oggetto, che è una struttura di dati basata su asset di valore superiore. Puoi anche raggruppare gli oggetti per creare nuovi oggetti. Un'applicazione può interagire con il modello logico, indipendentemente dal formato dei dati utilizzato dai singoli dispositivi o oggetti. 

Ad esempio, un gruppo di dispositivi di segnalazione della temperatura, dell'umidità e della luce ambientale può essere aggregato in un oggetto "Stanza" per rappresentare il livello di comfort all'interno di uno specifico ufficio. Una serie di oggetti "Stanza" può essere aggregata in un oggetto "Piano" per rappresentare tutti gli uffici su uno specifico livello e una serie di oggetti "Piano" può essere aggregata in un oggetto "Edificio". Utilizzando un'astrazione dell'oggetto, la tua applicazione viene disaccoppiata dalle specifiche di modalità di connessione dei dispositivi, dal formato in cui i dispositivi pubblicano i dati di evento e dalla modalità di combinazione dei dati.
{: shortdesc}

## Dispositivi gemelli

Un dispositivo gemello è una rappresentazione digitale basata su cloud di un dispositivo fisico connesso a {{site.data.keyword.iot_short_notm}}. Un dispositivo gemello rappresenta un modello logico degli eventi pubblicati da un dispositivo. Dopo la definizione e l'istanziamento, il dispositivo gemello fornisce un modo coerente di interagire con un dispositivo in modo analogo a REST, indipendentemente se il dispositivo è online o offline. Le proprietà di un dispositivo, incluse le informazioni sullo stato corrente del dispositivo (stato dispositivo), possono essere richiamate mediante una richiesta HTTP o una sottoscrizione a un argomento IoT.

I dispositivi gemelli possono aiutarti a:
- Fornire agli sviluppatori di applicazioni delle interfacce congruenti per accedere ai dati dei dispositivi controllati dagli eventi in un modo simile a REST.
- Accedere allo stato di un dispositivo.
- Normalizzare i dati dai dispositivi di fabbricazioni o modelli diversi che pubblicano i dati in formati differenti.
- Filtrare i dati non necessari.


Per creare un dispositivo gemello, devi definire le seguenti risorse in {{site.data.keyword.iot_short_notm}}:
- La struttura degli eventi inviati dal tuo dispositivo.  
La struttura di un evento in entrata viene definita nelle risorse interfaccia fisica, tipo di evento e schema dell'evento. 
- Le proprietà che desideri registrare.  
Queste proprietà definiscono la struttura dello stato del dispositivo che può essere utilizzata dalle tue applicazioni. Le proprietà sono definite nelle risorse dell'interfaccia logica e dello schema logico.  
- L'associazione degli eventi dell'interfaccia fisica alle proprietà dell'interfaccia logica.  
Utilizza le risorse di associazioni per associare gli eventi alle proprietà.

Il seguente diagramma mostra due diversi dispositivi di temperatura in posizioni separate. Un dispositivo riporta i dati in gradi Celsius e l'altro dispositivo riporta i dati in gradi Fahrenheit. I dati vengono inviati a {{site.data.keyword.iot_short_notm}} nei formati di temperatura "t" e "temp". {{site.data.keyword.iot_short_notm}} trasforma automaticamente i gradi Fahrenheit in gradi Celsius. I formati di temperatura "t" e "temp" vengono normalizzati nel formato logico "temperature". L'applicazione può interrogare lo stato di entrambi i dispositivi accedendo al valore del parametro "temperature". 

![Panoramica della gestione dei dati in {{site.data.keyword.iot_short_notm}}.](../information_management/images/ga_im_resources_overview.svg "Panoramica della gestione dei dati in {{site.data.keyword.iot_short_notm}}")


## Asset gemelli (oggetti)

Gli asset gemelli ti consentono di portare un passo avanti il concetto di dispositivi gemelli. Un asset gemello consente l'aggregazione di dispositivi in una singola entità chiamata Oggetto. Un oggetto, o asset gemello, è un concetto simile a dispositivo gemello, ma l'asset gemello rappresenta un gruppo di dispositivi come singolo modello logico. Puoi anche aggregare gli oggetti per formare livelli di astrazione più alti. Ad esempio, un oggetto "Stanza" potrebbe aggregare i seguenti dispositivi:

- Un dispositivo con un sensore di temperatura (termometro)
- Un dispositivo con un sensore di umidità (igrometro)

Un oggetto "Piano" potrebbe quindi aggregare più oggetti "Stanza". 

La struttura di un oggetto viene definita utilizzando uno schema JSON. Lo schema fa riferimento alle interfacce logiche dei dispositivi o degli oggetti aggregati. Le proprietà di un oggetto, incluse le informazioni sullo stato corrente dell'oggetto, possono essere richiamate mediante una richiesta HTTP o una sottoscrizione a un argomento IoT.

Gli asset gemelli possono aiutarti a:
 
- Aggregare più dispositivi gemelli o oggetti per definire nuovi oggetti.
- Accedere allo stato di un oggetto.
- Gestire gli asset senza essere esposti alla loro singola strumentazione.
- Filtrare i dati non necessari.
- Normalizzare le interfacce dell'oggetto per disaccoppiare le tue applicazioni dalle complessità di come vengono costruiti specifici oggetti.


Per creare un asset gemello, devi definire le seguenti risorse in {{site.data.keyword.iot_short_notm}}:

- La struttura dell'oggetto.  
La struttura dell'oggetto è definita dallo schema oggetto che specifica i dispositivi o gli oggetti aggregati.
- La struttura dello stato dello oggetto desiderato che è costituito dalle proprietà che vuoi registrare.  
Queste proprietà definiscono la struttura dello stato dell'oggetto che può essere utilizzata dalle tue applicazioni. Le proprietà sono definite nelle risorse dell'interfaccia logica e dello schema logico.  
- Come l'interfaccia dell'oggetto è associata alle proprietà dell'interfaccia logica.  
Utilizza le risorse di associazioni per associare gli eventi alle proprietà.


Il seguente diagramma mostra i sensori di temperatura e umidità su dispositivi diversi che pubblicano i dati evento di temperatura e umidità in {{site.data.keyword.iot_short_notm}}. Due dispositivi gemelli, ognuno dei quali rappresenta un dispositivo fisico, hanno interfacce logiche associate e sono creati in {{site.data.keyword.iot_short_notm}}. I dati pubblicati dal dispositivo di temperatura vengono associati all'interfaccia logica "IThermometer". I dati pubblicati dal dispositivo di umidità vengono associati all'interfaccia logica "IHygrometer". Le interfacce logiche sono aggregate in un tipo di oggetto *Room* con un'interfaccia logica "IRoom". L'interfaccia logica "IRoom" definisce le proprietà di temperatura e umidità e ti consente di creare il tuo proprio modello logico aggregando i dispositivi in un unico oggetto con cui l'applicazione può interagire.  

![Panoramica della gestione dei dati in {{site.data.keyword.iot_short_notm}}.](../information_management/images/Thing)

**Importante:** la funzione degli oggetti di {{site.data.keyword.iot_short_notm}} è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.


Per ulteriori informazioni sulla definizione e configurazione delle risorse e delle informazioni chiave, consulta [Informazioni sulla gestione dei dati](ga_im_definitions.html). 

## Passi successivi

- Crea il tuo proprio dispositivo gemello in {{site.data.keyword.iot_short_notm}}. Per ulteriori informazioni, consulta la documentazione [Introduzione alla gestione dei dati utilizzando l'interfaccia web](im_ui_flow.html). 
- Crea un dispositivo gemello e un asset gemello utilizzando le API REST. Per ulteriori informazioni, consulta la documentazione [Introduzione alla gestione dei dati](../information_management/getting_started_things.html).  
- Crea regole che vengono attivate quando {{site.data.keyword.iot_short_notm}} riceve i dati di evento che corrispondono a una condizione o una serie di condizioni specificata. Per ulteriori informazioni, consulta la documentazione [Regole integrate](../information_management/im_rules.html) (beta).

Per informazioni più dettagliate su ciascuno dei passi indicati nella documentazione *Introduzione alla gestione dei dati*, consulta gli scenari di esempio documentati nei seguenti argomenti: 

- [Guida passo a passo 1: un esempio dettagliato su come utilizzare i dispositivi tramite un'interfaccia comune](ga_im_index_scenario.html#scenario) 
- [Guida passo a passo 2: un esempio dettagliato su come utilizzare gli oggetti tramite un'interfaccia comune](../information_management/im_index_scenario_thing.html#scenario) 


