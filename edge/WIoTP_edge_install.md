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


# Installing {{site.data.keyword.iot_short_notm}} Edge on devices (Preview)
{: #edge_install_device}

**Important:** The {{site.data.keyword.iot_full}} Edge feature is available only as part of a limited preview program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

In order for your {{site.data.keyword.iot_short_notm}} Edge preview machines to be managed by IBM Horizon with {{site.data.keyword.iot_short_notm}}, you must first install the Horizon+WIoTP software on them, then register the machine with the Horizon Exchange. These steps will enable you to use applications from the preview development repositories in a temporary test environment.

## Before you install

### Pre-requisites for Ubuntu-based edge nodes

- Add Docker to the registry if it not alreay installed:

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- Add the base Horizon package (ANAX agent)

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Pre-requisites for for Raspberry PI

- Add the following lines to `/etc/apt/sources.list.d/bluehorizon.list`:
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- Add the following line to `/etc/apt/sources.list.d/docker.list`:

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- Install `docker-ce`:

`apt-get update && apt-get install docker-ce`


## Installing Horizon and WIoTP Horizon Packages

`apt-get update && apt-get install -y horizon-wiotp`

## Registering your Edge node

Run the agent setup:

For production environments, run:
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

It will execute the following steps:
- Configure the WIoTP Edge Core IoT Workload.
- Generate edge-mqttbroker certificates.
- Register the node against the WIoT Platform using the hzn tool (`hzn --help` for more info).

If more than one IP address is found in the device, you will be prompted to choose one to generate the internal certificate. This IP will be used later to test the connection between the edge node and a device sending data.

Make sure there are no error messages during execution.

Wait a few seconds and then verfiy that the Edge components were created.

Check to see if the edge components images were created `docker images` (TAG versions may differ).

**Note**: The process of downloading images and starting container may take a few minutes. Check the syslog file to see logging details.

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

Check if containers were initiated `docker ps`

Example:
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## Simulating devices on your edge node

You can use mosquitto-clients to simulate devices connecting to your edge node:

`apt-get install mosquitto-clients`

Run the following command on your Edge node (replace OrgId, DeviceType, DeviceID, DeviceToken and EdgeNodeIP with the appropriate values).
To find out which IP needs to be passed, check the messages displayed after running the `wiotp_agent_setup` command, underneath the `Generating Edge internal certificates` part.

```mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>```

Example:

```mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15```

Note: If you are running mosquitto_pub outside the edge node use the -h `<host>` option and adjust the  --cafile path.

### Unregistering edge node

To unregister the edge node, run:

`hzn unregister`

### Uninstalling Horizon-wiotp

To completely remove the horizon agent from your node:

`apt-get purge -y horizon horizon-wiotp`

### Troubleshooting

Checking horizon agent version:

`dpkg -l | grep horizon`

Checking horizon agent real time logs:

`journalctl -af -u horizon.service`

Collecting syslog:

`tail -n <num-max-of-lines> /var/log/syslog `

Filtering log messages by containers:

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## Finding more information
{: #more_info}

For more information about {{site.data.keyword.iot_short_notm}} Edge, see the following topics:
- [{{site.data.keyword.iot_short_notm}} Edge overview](WIoTP_edge.html#edge_overview)
- [Configuring {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
- [Developing for {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
