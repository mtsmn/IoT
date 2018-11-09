---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurazione del controllo dell'accesso al livello della risorsa
{: #configure_RLAC}

Il controllo dell'accesso al livello della risorsa ti permette di controllare l'accesso della chiave API e dell'utente per gestire i dispositivi. Utilizza i gruppi di risorse per definire quali dispositivi in un'organizzazione ogni utente o chiave API può gestire. Gli utenti e le chiavi API possono essere assegnati a una coppia "ruolo per gruppi", che definisce che possono eseguire solo le operazioni consentite al ruolo specificato nei dispositivi presenti nei gruppi specificati. Per ulteriori informazioni sul controllo dell'accesso al livello della risorsa, consulta [Panoramica sul controllo dell'accesso al livello della risorsa](rlac_overview.html) e [{{site.data.keyword.iot_short_notm}} Access control API documentation ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Configurazione del controllo dell'accesso al livello della risorsa - Flusso del processo
{: #RLAC_process}

Il seguente flusso del processo è generalmente utilizzato per abilitare e utilizzare il controllo dell'accesso al livello della risorsa:
1. [Crea un'organizzazione](../iotplatform_overview.html#organizations).
2. [Crea utenti](../add_users.html#adding-new-users) e [Chiavi API](../platform_authorization.html#api-key).
3. [Crea gruppi di risorse](rlac.html#create_delete_group).
4. [Assegna associazioni ruolo per gruppi per gli utenti e le chiavi API](rlac.html#assign_roletogroup).
5. [Aggiungi i dispositivi ai gruppi di risorse](rlac.html#add_device).
6. [Abilita il controllo dell'accesso al livello della risorsa](rlac.html#RLAC_enable).

Il controllo dell'accesso al livello della risorsa viene imposto in molte API correlate al dispositivo. Per un elenco di API interessate, consulta [API in cui viene imposto il controllo dell'accesso al livello della risorsa](rlac_overview.html#RLAC_enforced_APIs).

## Creazione ed eliminazione dei gruppi di risorse
{: #create_delete_group}

I gruppi di risorse possono essere creati ed eliminati indipendentemente dalla connessione ai gateway.

Per creare un gruppo di risorse e restituire i dettagli del gruppo, utilizza la seguente API:

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

Quando elimini un gruppo di risorse, i dispositivi presenti nel gruppo vengono eliminati dal gruppo, ma i dispositivi stessi non sono altrimenti interessati. Per eliminare un gruppo di risorse, utilizza la seguente API:

    DELETE /api/v0002/groups/{groupUid}


## Assegnazione delle coppie "ruolo per gruppi" agli utenti e alle chiavi API
{: #assign_roletogroup}

L'assegnazione di una coppia "ruolo per gruppi" a un utente o a una chiave API è obbligatoria per limitare l'utente o la chiave API a un gruppo di dispositivi. Gli utenti e le chiavi API senza un gruppo di risorse possono gestire tutti i dispositivi nelle loro organizzazioni senza limitazioni. Una chiave API può disporre solo di un ruolo se viene utilizzata una coppia "ruoli per gruppi". Il ruolo specificato in "ruoli per gruppi" deve corrispondere al ruolo specificato in "ruoli".

Per assegnare una coppia "ruolo per gruppi" a un utente, utilizza la seguente API:

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



Per assegnare una coppia "ruolo per gruppi" a una chiave API, utilizza la seguente API:

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## Aggiunta dei dispositivi e loro rimozione dai gruppi di risorse
{: #add_device}

Prima che un utente o una chiave API con una coppia "ruolo per gruppi" possa gestire un dispositivo, il dispositivo deve essere membro del gruppo di risorse assegnato all'utente o alla chiave API. Quando aggiungi i dispositivi a un gruppo di risorse, il gruppo a cui stai aggiungendo i dispositivi deve essere specificato nel percorso della richiesta e i dispositivi da aggiungere devono essere specificati nel corpo della richiesta. Per aggiungere più dispositivi a un gruppo di risorse contemporaneamente, utilizza la seguente API:

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Quando rimuovi i dispositivi da un gruppo di risorse, i dispositivi specificati nel corpo della richiesta vengono rimossi dal gruppo specificato nel percorso della richiesta. Per rimuovere più dispositivi da un gruppo di risorse, utilizza la seguente API:

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Per ulteriori informazioni sullo schema della richiesta e sulla risposta, consulta [{{site.data.keyword.iot_short_notm}} Access control API documentation ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Abilitazione del controllo dell'accesso al livello della risorsa
{: #RLAC_enable}

L'abilitazione del controllo dell'accesso al livello della risorsa per un'organizzazione abiliterà gli utenti e le chiavi API ad essere limitati ai rispettivi gruppi di risorse assegnati. Per utilizzare il controllo dell'accesso al livello della risorsa, devi abilitare un indicatore della configurazione al livello dell'organizzazione utilizzando la seguente API:

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Ricerca dei gruppi di risorse
{: #find_group}

I gruppi di risorse possono avere associate tag di ricerca. La ricerca delle tag può essere utilizzata per richiamare i dettagli di un gruppo di risorse utilizzando la seguente API:

    GET /api/v0002/groups

Questa API restituisce i gruppi di risorse associati alla tag di ricerca utilizzata. Se non viene specificata alcuna tag di ricerca, vengono restituiti tutti i gruppi di risorse. 

Per trovare l'ID univoco del gruppo di risorse assegnato a un utente, utilizza la seguente API:

    GET /api/v0002/authorization/users/{userUid}

Per trovare l'ID univoco del gruppo di risorse assegnato a una chiave API, utilizza la seguente API:

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## Esecuzione della query dei gruppi di risorse
{: #query_group}

Puoi eseguire la query dei gruppi di risorse utilizzando vari parametri per restituire le proprietà complete di tutti i dispositivi nel gruppo, gli identificativi univoci di tutti i dispositivi nel gruppo o le proprietà del gruppo di risorse.

Per restituire le proprietà complete di tutti i dispositivi nel gruppo di risorse specificato, utilizza la seguente API:

    GET /api/v0002/bulk/devices/{groupUid}

Per restituire solo gli identificativi univoci dei membri del gruppo di risorse, utilizza la la seguente API:

    GET /api/v0002/bulk/devices/{groupUid}/ids

Per restituire le proprietà del gruppo di risorse, inclusi il nome, la descrizione, le tag di ricerca e l'identificativo univoco specificati nel percorso, utilizza la seguente API:

    GET /api/v0002/groups/{groupUid}

Questa API non restituisce l'elenco dei membri del gruppo di risorse.

## Aggiornamento delle proprietà del gruppo
{: #update_group}

Per aggiornare le proprietà di un gruppo, utilizza la seguente API:

    PUT /api/v0002/groups/{groupId}
