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


# Installation de {{site.data.keyword.iot_short_notm}} Edge sur les terminaux (prévisualisation)
{: #edge_install_device}

**Important :** la fonction {{site.data.keyword.iot_full}} Edge n'est disponible que dans le cadre d'un programme de prévisualisation limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Pour que vos machines de prévisualisation {{site.data.keyword.iot_short_notm}} Edge puissent être gérées par IBM Horizon avec {{site.data.keyword.iot_short_notm}}, vous devez d'abord installer le logiciel Horizon+WIoTP sur ces machines, puis les enregistrer auprès de Horizon Exchange. Ces étapes vous permettront d'utiliser des applications des référentiels de développement de prévisualisation dans un environnement de test temporaire.

## Avant l'installation

### Conditions requises pour les noeuds Edge basés sur Ubuntu

- Ajoutez Docker au registre s'il n'est pas déjà installé :

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- Ajoutez le package de base Horizon (agent ANAX)

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Conditions requises pour Raspberry PI

- Ajoutez les lignes suivantes à `/etc/apt/sources.list.d/bluehorizon.list` :
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- Ajoutez la ligne suivante à `/etc/apt/sources.list.d/docker.list` :

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- Installez `docker-ce` :

`apt-get update && apt-get install docker-ce`


## Installation des packages Horizon et WIoTP Horizon

`apt-get update && apt-get install -y horizon-wiotp`

## Enregistrement de votre noeud Edge

Exécutez la configuration d'agent :

Pour les environnements de production, exécutez :
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

Ceci exécutera les opérations suivantes :
- Configuration de la charge de travail Core IoT de WIoTP Edge.
- Génération des certificats edge-mqttbroker.
- Enregistrement du noeud auprès de la plateforme WIoT à l'aide de l'outil hzn (`hzn --help` pour plus d'informations).

Si plusieurs adresses IP sont trouvées sur le terminal, vous serez invité à en choisir une pour générer le certificat interne. Cette adresse IP sera utilisée ultérieurement pour tester la connexion entre le noeud Edge et un terminal envoyant des données.

Vérifiez qu'il n'y a pas de message d'erreur lors de l'exécution.

Attendez quelques secondes puis vérifiez que les composants Edge ont été créés.

Vérifiez si les composants Edge ont été créés avec `docker images` (les versions TAG peuvent être différentes).

**Remarque **: le processus de téléchargement des images et de démarrage du conteneur peut prendre quelques minutes. Consultez le fichier syslog pour voir les détails de consignation.

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

Vérifiez si les conteneurs ont été initiés avec `docker ps`

Exemple :
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## Simulation des terminaux sur votre noeud Edge

Vous pouvez utiliser mosquitto-clients pour simuler la connexion de terminaux à votre noeud Edge :

`apt-get install mosquitto-clients`

Exécutez les commandes suivantes sur votre noeud Edge (remplacez OrgId, DeviceType, DeviceID, DeviceToken et EdgeNodeIP avec les valeurs appropriées).
Pour savoir quelles adresses IP doivent être transférées, vérifiez les messages qui s'affichent après l'exécution de la commande `wiotp_agent_setup`, en dessous de `Generating Edge internal certificates`.

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

Exemple :

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

Remarque : si vous exécutez mosquitto_pub en dehors du noeud Edge, utilisez l'option -h `<host>` et ajustez le chemin d'accès --cafile.

### Désenregistrement du noeud Edge

Pour désenregistrer le noeud Edge, exécutez :

`hzn unregister`

### Désinstallation de Horizon-wiotp

Pour supprimer complètement l'agent horizon de votre noeud :

`apt-get purge -y horizon horizon-wiotp`

### Traitement des incidents

Vérification de la version de l'agent horizon :

`dpkg -l | grep horizon`

Vérification des journaux en temps réel de l'agent horizon :

`journalctl -af -u horizon.service`

Collecte du syslog :

`tail -n <num-max-of-lines> /var/log/syslog `

Filtrage des messages du journal par conteneur :

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## Recherche d'informations supplémentaires
{: #more_info}

Pour en savoir plus sur {{site.data.keyword.iot_short_notm}} Edge, consultez les rubriques suivantes :
- Aperçu de [{{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge.html#edge_overview)
- [Configuration de {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
- [Développement pour {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
