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


# {{site.data.keyword.iot_short_notm}} Edge auf Geräten installieren (Vorschau)
{: #edge_install_device}

**Wichtig:** Die {{site.data.keyword.iot_full}} Edge-Komponente steht nur im Rahmen eines eingeschränkten Vorschauprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Damit die Maschinen der Vorschau für {{site.data.keyword.iot_short_notm}} Edge von IBM Horizon mit {{site.data.keyword.iot_short_notm}} verwaltet werden können, müssen Sie zunächst die Horizon- und WIoTP-Software auf den Maschinen installieren und dann die Maschine mit Horizon Exchange registrieren. Mit diesen Schritten können Sie Anwendungen aus den Entwicklungsrepositorys der Vorschau in einer temporären Testumgebung verwenden.

## Vor der Installation

### Voraussetzungen für Ubuntu-basierte Edge-Knoten

- Fügen Sie Docker, falls noch nicht installiert, zur Registry hinzu:

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- Fügen Sie das Horizon-Basispaket hinzu (ANAX-Agent):

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Voraussetzungen für Raspberry PI

- Fügen Sie die folgenden Zeilen zu `/etc/apt/sources.list.d/bluehorizon.list` hinzu:
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- Fügen Sie die folgende Zeile zu `/etc/apt/sources.list.d/docker.list` hinzu:

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- Installieren Sie `docker-ce`:

`apt-get update && apt-get install docker-ce`


## Horizon- und WIoTP Horizon-Pakete installieren

`apt-get update && apt-get install -y horizon-wiotp`

## Edge-Knoten registrieren

Führen sie die Agenteneinrichtung aus:

Führen Sie für Produktionsumgebungen Folgendes aus:
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

Es werden die folgenden Schritte ausgeführt:
- Konfigurieren der WIoTP Edge Core IoT-Workload.
- Generieren von Zertifikate des Typs 'edge-mqttbroker'.
- Registrieren des Knotens für WIoT Platform mithilfe des Tool 'hzn' (`hzn --help` für weitere Informationen).

Wenn mehr als eine IP-Adresse im Gerät gefunden wird, werden Sie aufgefordert, eine IP-Adresse auszuwählen, um das interne Zertifikat zu generieren. Diese IP-Adresse wird später verwendet, um die Verbindung zwischen dem Edge-Knoten und einem Gerät zu testen, das Daten sendet.

Stellen Sie sicher, dass während der Ausführung keine Fehlernachrichten auftreten.

Warten Sie einige Sekunden und überprüfen Sie dann, dass die Edge-Komponenten erstellt wurden.

Überprüfen Sie, ob die Edge-Komponentenimages (`docker images`) erstellt wurden (TAG-Versionen können abweichen).

**Hinweis**: Der Prozess zum Herunterladen von Images und zum Starten des Containers kann einige Minuten dauern. Überprüfen Sie die syslog-Datei, um die Protokollierungsdetails anzuzeigen.

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

Prüfen Sie, ob Container initialisiert wurden (`docker ps`).

Beispiel:
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## Geräte auf dem Edge-Knoten simulieren

Sie können mit 'mosquitto-clients' Geräte simulieren, die mit Ihrem Edge-Knoten verbunden sind:

`apt-get install mosquitto-clients`

Führen Sie den folgenden Befehl auf Ihrem Edge-Knoten aus (ersetzen Sie 'org-id', 'gerätetyp', 'geräte-id', 'gerätetoken' und 'ip_des_edge-knotens' mit den entsprechenden Werten).
Um herauszufinden, welche IP übergeben werden muss, überprüfen Sie die Nachrichten, die nach dem Ausführen des Befehls `wiotp_agent_setup` unter dem Abschnitt zum `Generieren interner Edge-Zertifikate` angezeigt werden.

```
mosquitto_pub -p 8883 -i 'd:<org-id>:<gerätetyp>:<geräte-id>' -u 'use-token-auth' -P '<gerätetoken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <ip_des_edge-knotens>
```

Beispiel:

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

Hinweis: Wenn Sie 'mosquitto_pub' außerhalb des Edge-Knotens ausführen, verwenden Sie die Option '-h' des Befehls `<host>` und passen Sie den Pfad '--cafile' an.

### Registrierung eines Edge-Knotens zurücknehmen

Führen Sie die folgenden Schritte aus, um die Registrierung des Edge-Knotens zurückzunehmen:

`hzn unregister`

### 'horizon-wiotp' deinstallieren

Gehen Sie wie folgt vor, um den Horizont-Agenten vollständig von Ihrem Knoten zu entfernen:

`apt-get purge -y horizon horizon-wiotp`

### Fehlerbehebung

So überprüfen Sie die Horizont-Agentenversion:

`dpkg -l | grep horizon`

So überprüfen Sie die Echtzeit-Protokolle des Horizont-Agenten:

`journalctl -af -u horizon.service`

So erfassen Sie Daten für syslog:

`tail -n <num-max-of-lines> /var/log/syslog `

So filtern Sie Protokollnachrichten nach Containern:

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## Weitere Informationen
{: #more_info}

Weitere Informationen zu {{site.data.keyword.iot_short_notm}} Edge finden Sie in den folgenden Abschnitten:
- [{{site.data.keyword.iot_short_notm}} Edge - Übersicht](WIoTP_edge.html#edge_overview)
- [{{site.data.keyword.iot_short_notm}} Edge konfigurieren](WIoTP_edge_config.html#edge_configure)
- [Für {{site.data.keyword.iot_short_notm}} Edge entwickeln](WIoTP_edge_dev.html#edge_dev)
