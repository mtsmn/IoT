---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione alla gestione dei dati utilizzando le API REST
{: #im_example}

Utilizza la seguente procedura come supporto per la configurazione delle risorse di cui hai bisogno per iniziare a utilizzare le funzioni di dispositivo e asset gemello del componente di gestione dei dati {{site.data.keyword.iot_full}}.

Per i dettagli sull'API, consulta la documentazione [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Flusso di lavoro di elevato livello
{: #workflow}

Utilizza la seguente procedura come supporto per la configurazione delle risorse di cui hai bisogno per iniziare ad associare i tuoi dati del dispositivo o dell'oggetto utilizzando la funzione di dispositivo o asset gemello.

**Suggerimento:** per informazioni più dettagliate su ciascuno dei passi, vedi gli scenari di esempio oppure utilizza i link per andare direttamente a uno specifico passo nello scenario di esempio. La [Guida passo a passo 1](ga_im_index_scenario.html#scenario) descrive i passi per creare un'interfaccia logica del tipo di dispositivo per dispositivi termometro eterogenei e la [Guida passo a passo 2](../information_management/im_index_scenario_thing.html#scenario) sviluppa lo scenario descrivendo come creare un'interfaccia logica che consente alla tua applicazione di utilizzare i dati da due diversi tipi di dispositivi climatici combinati in un oggetto di tipo stanza.

Il processo per la creazione e il consumo di interfacce logiche differisce leggermente a seconda che tu stia creando un'interfaccia logica associata a un tipo di dispositivo o a un tipo di oggetto.

### Prima di cominciare
Per creare un'interfaccia logica associata a un tipo di dispositivo o un tipo di oggetto, devi avere [almeno un dispositivo registrato con {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) e che invia eventi con proprietà di stato.  


### Passi

1. 	Definisci le proprietà di stato in entrata.  
Definisci innanzitutto le proprietà di stato in entrata che vuoi che la tua interfaccia logica renda disponibili per le tue applicazioni.  
A seconda dell'interfaccia logica che stai creando, procedi in uno dei due seguenti modi:
<dl>
<dt>Tipo di dispositivo: crea un'interfaccia fisica.</dt>
<dd>
<ol>
<li>[Crea un file di schema di evento](ga_im_index_scenario.html#step1). Il file dello schema evento è un file .JSON locale che definisce la struttura e il formato di un evento in entrata.
<li>[Crea una risorsa dello schema evento per il tuo tipo evento](ga_im_index_scenario.html#step2). La risorsa dello schema evento è n costrutto programmatico utilizzato da {{site.data.keyword.iot_short_notm}}.
<li>[Crea un tipo evento che fa riferimento allo schema evento](ga_im_index_scenario.html#step3). Il tipo evento viene utilizzato da {{site.data.keyword.iot_short_notm}} per associare una o più risorse dello schema evento a un'interfaccia fisica.
<li>[Crea un'interfaccia fisica](ga_im_index_scenario.html#step7).
<li>[Aggiungi il tipo evento all'interfaccia fisica](ga_im_index_scenario.html#step8).
<li>[Aggiungi la tua interfaccia fisica al tuo tipo dispositivo](ga_im_index_scenario.html#step9).
</ol>
</dd>
<dt>Tipo di oggetto: definisci un tipo di oggetto.</dt>
<dd>
<ol>
<li>[Crea un file schema del tipo di oggetto](../information_management/im_index_scenario_thing.html#crt_composition_file).  
Un file schema del tipo di oggetto è un file .JSON locale che definisce la composizione del tipo di oggetto puntando alle interfacce logiche esistenti.
<li>[Crea la risorsa dello schema del tipo di oggetto](../information_management/im_index_scenario_thing.html#crt_composition_resource).  
Carica il file .JSON locale su {{site.data.keyword.iot_short_notm}}.
<li>[Crea un tipo di oggetto](../information_management/im_index_scenario_thing.html#crt_thing_type). Un tipo di oggetto ha lo stesso scopo di un tipo di dispositivo in quanto rappresenta una classe di oggetti.
</ol>
</dd>
</dl>
4. 	Crea l'interfaccia logica.
 1. 	Crea un file dello schema dell'interfaccia logica per il [tipo di dispositivo](ga_im_index_scenario.html#step4) o il [tipo di oggetto](../information_management/im_index_scenario_thing.html#crt_ai_schema_file).  
Un file dello schema dell'interfaccia logica è un file .JSON locale che definisce lo stato del dispositivo reso disponibile alle tue applicazioni.
 2. 	Crea una risorsa dello schema dell'interfaccia logica per il [tipo di dispositivo](ga_im_index_scenario.html#step5) o il [tipo di oggetto](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource).
 3.	Crea un'interfaccia logica per il [tipo di dispositivo](ga_im_index_scenario.html#step6) o il [tipo di oggetto](../information_management/im_index_scenario_thing.html#crt_thing_ai).
 4.	Aggiungi l'interfaccia logica al [tipo di dispositivo](ga_im_index_scenario.html#step10) o al [tipo di oggetto](../information_management/im_index_scenario_thing.html#add_thing_ai).
5. 	Definisci le associazioni per il [tipo di dispositivo](ga_im_index_scenario.html#step11) o il [tipo di oggetto](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings).   
Le associazioni vengono utilizzate per associare le proprietà in entrata alle proprietà nell'interfaccia logica.  
6. 	Convalida e attiva la configurazione associata al [tipo di dispositivo](ga_im_index_scenario.html#step15) o al [tipo di oggetto](../information_management/im_index_scenario_thing.html#activate) di bozza.
7. 	Richiama lo stato del [dispositivo](ga_im_index_scenario.html#step13) o dell' [oggetto](../information_management/im_index_scenario_thing.html##verify_Thing_state).  
Verifica che le sottoscrizioni mostrino i dati aggiornati del dispositivo oppure che i dati aggiornati del dispositivo vengano restituiti utilizzando una chiamata REST o sottoscrivendo a una stringa di argomento.  



