---

copyright:
  years: 2018
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Installazione di {{site.data.keyword.iot_short_notm}} Edge sui dispositivi (Anteprima)
{: #edge_install_device}

**Importante:** la funzione {{site.data.keyword.iot_full}} Edge è disponibile solo come parte di un programma di anteprima limitato. Futuri aggiornamenti possono includere modifiche incompatibili con la versione corrente di questa funzione. Provala e [facci sapere cosa ne pensi ![Icona link esterno](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Affinché le tue macchine di anteprima {{site.data.keyword.iot_short_notm}} Edge siano gestite da IBM Horizon con {{site.data.keyword.iot_short_notm}}, devi prima installare il software Horizon+WIoTP e quindi registrare la macchina con Horizon Exchange. Questi passi ti consentiranno di utilizzare le applicazioni dai repository di sviluppo di anteprima in un ambiente di test temporaneo.

## Prima di effettuare l'installazione

### Prerequisiti per i nodi edge basati su Ubuntu

- Aggiungi Docker al registro se non è già installato:

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- Aggiungi il pacchetto Horizon di base (agent ANAX)

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Prerequisiti per Raspberry PI

- Aggiungi le seguenti righe in `/etc/apt/sources.list.d/bluehorizon.list`:
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- Aggiungi le seguenti righe in `/etc/apt/sources.list.d/docker.list`:

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- Installa `docker-ce`:

`apt-get update && apt-get install docker-ce`


## Installazione dei pacchetti Horizon e WIoTP Horizon

`apt-get update && apt-get install -y horizon-wiotp`

## Registrazione del nodo Edge

Esegui la configurazione dell'agent:

Per gli ambienti di produzione, esegui:
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

Verranno eseguiti i seguenti passi:
- Configurazione del carico di lavoro di WIoTP Edge Core IoT.
- Generazione dei certificati edge-mqttbroker.
- Registrazione del nodo in WIoT Platform con lo strumento hzn (`hzn --help` per ulteriori informazioni).

Se nel dispositivo viene rilevato più di un indirizzo IP, ti verrà richiesto di sceglierne uno per generare il certificato interno. Questo IP verrà utilizzato successivamente per testare la connessione tra il nodo edge e un dispositivo che invia dati.

Assicurati che non ci siano messaggi di errore durante l'esecuzione.

Attendi qualche secondo e quindi verifica che i componenti Edge siano stati creati.

Controlla se sono state create le immagini dei componenti edge `docker images` (le versioni TAG possono differire).

**Nota**: il processo di download delle immagini e di avvio del contenitore potrebbe richiedere alcuni minuti. Controlla il file syslog per vedere i dettagli di registrazione.

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

Controlla se i contenitori sono stati avviati `docker ps`

Esempio:
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## Simulazione di dispositivi sul tuo nodo edge

Puoi utilizzare mosquitto-clients per simulare i dispositivi che si connettono al tuo nodo edge:

`apt-get install mosquitto-clients`

Immetti il seguente comando sul tuo nodo Edge (sostituisci OrgId, DeviceType, DeviceID, DeviceToken e EdgeNodeIP con i valori appropriati).
Per scoprire quale IP deve essere passato, controlla i messaggi visualizzati dopo l'esecuzione del comando `wiotp_agent_setup`, sotto la parte `Generating Edge internal certificates`.

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

Esempio:

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

Nota: se stai eseguendo mosquitto_pub al di fuori del nodo edge, utilizza l'opzione -h `<host>` e adatta il percorso  --cafile.

### Annullamento della registrazione del nodo edge

Per annullare la registrazione del nodo edge, esegui:

`hzn unregister`

### Disinstallazione di Horizon-wiotp

Per rimuovere completamente l'agent horizon dal tuo nodo, esegui:

`apt-get purge -y horizon horizon-wiotp`

### Risoluzione dei problemi

Controllo della versione dell'agent horizon:

`dpkg -l | grep horizon`

Controllo dei log in tempo reale dell'agent horizon:

`journalctl -af -u horizon.service`

Raccolta di syslog:

`tail -n <num-max-of-lines> /var/log/syslog `

Filtraggio dei messaggi di log in base ai contenitori:

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## Ricerca di ulteriori informazioni
{: #more_info}

Per ulteriori informazioni su {{site.data.keyword.iot_short_notm}} Edge, consulta i seguenti argomenti:
- [Panoramica di {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge.html#edge_overview)
- [Configurazione di {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
- [Sviluppo per {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
