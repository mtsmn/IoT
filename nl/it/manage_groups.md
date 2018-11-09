---

copyright:
years: 2018
lastupdated: "2018-06-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Gestione dei gruppi (Beta)
{: #groups_overview}

Gli utenti con il ruolo di amministratore possono utilizzare i gruppi {{site.data.keyword.iot_full}} per concedere ai membri e alle chiavi API l'accesso a dispositivi specifici. Dopo aver creato un gruppo e aggiunto dei dispositivi ad esso, aggiungi i membri e le chiavi API al gruppo e assegna loro i ruoli all'interno del gruppo. La combinazione di ruoli e gruppi determina a quali dispositivi possono accedere gli utenti e le chiavi API e le azioni che possono eseguire sui dispositivi.
{: shortdesc}

Puoi gestire i gruppi utilizzando l'interfaccia utente del dashboard {{site.data.keyword.iot_short_notm}} o le API di controllo dell'accesso {{site.data.keyword.iot_short_notm}}.

Per i dettagli su come gestire i ruoli, consulta [Gestione dei ruoli utente](managing_user_roles.html#managing-user-roles).

**Importante:** la funzione dei gruppi nella IU {{site.data.keyword.iot_short_notm}} è disponibile solo come parte di un programma beta limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Per ulteriori informazioni sul controllo dell'accesso e sui gruppi e istruzioni sull'utilizzo delle API di controllo dell'accesso {{site.data.keyword.iot_short_notm}} per gestire i gruppi, consulta [Panoramica sul controllo dell'accesso al livello della risorsa](reference/rlac_overview.html#RLAC_overview).

## Aggiunta di gruppi

**Nota:** per iniziare ad aggiungere i gruppi, attiva la funzione **Groups** nella pagina **Settings**. 

1. Dal tuo dashboard {{site.data.keyword.iot_short_notm}}, seleziona **Access Management** nella barra di navigazione di sinistra.
2. Seleziona la scheda **Groups** e visualizza l'elenco dei gruppi.
3. Fai clic su **Add Group**.
4. Nella finestra **Add Group**, immetti il nome del gruppo e una descrizione facoltativa.
5. Fai clic su **Avanti**.
6. Fai clic su **Add Devices to Group**.
7. Seleziona i dispositivi che vuoi aggiungere al gruppo e fai clic su **Done**.
8. Fai clic su **Finish** per creare il gruppo.
Puoi modificare il gruppo aggiungendo più dispositivi o rimuovendone e puoi aggiungere altri gruppi.

