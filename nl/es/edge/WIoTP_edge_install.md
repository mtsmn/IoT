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


# Instalación de {{site.data.keyword.iot_short_notm}} Edge en dispositivos (vista previa)
{: #edge_install_device}

**Importante:** La característica {{site.data.keyword.iot_full}} Edge solo está disponible como parte de un programa de vista previa limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Para que las máquinas de vista previa de {{site.data.keyword.iot_short_notm}} Edge sean gestionadas por IBM Horizon con {{site.data.keyword.iot_short_notm}}, primero debe instalar el software Horizon+WIoTP en ellas y, a continuación, registrar la máquina con Horizon Exchange. Estos pasos le permitirán utilizar aplicaciones desde los repositorios de desarrollo de vista previa en un entorno de prueba temporal.

## Antes de instalar

### Requisitos previos para nodos de extremo basados en Ubuntu

- Añada Docker al registro si no está ya instalado:

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- Añada el paquete Horizon base (agente ANAX)

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Requisitos previos para Raspberry PI

- Añada las líneas siguientes a `/etc/apt/sources.list.d/bluehorizon.list`:
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- Añada la línea siguiente a `/etc/apt/sources.list.d/docker.list`:

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- Instale `docker-ce`:

`apt-get update && apt-get install docker-ce`


## Instalación de Horizon y WIoTP Horizon Packages

`apt-get update && apt-get install -y horizon-wiotp`

## Registro del nodo Edge

Ejecute la configuración del agente:

Para entornos de producción, ejecute:
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

Se ejecutarán los pasos siguientes:
- Configurar la carga de trabajo de WIoTP Edge Core IoT.
- Generar certificados edge-mqttbroker.
- Registrar el nodo en la plataforma WIoT utilizando la herramienta hzn (`hzn --help` para obtener más información).

Si se encuentra más de una dirección IP en el dispositivo, se le solicitará que elija una para generar el certificado interno. Esta IP se utilizará posteriormente para probar la conexión entre el nodo Edge y un dispositivo que envía datos.

Asegúrese de que no haya mensajes de error durante la ejecución.

Espere unos segundos y luego verifique que se han creado los componentes Edge.

Compruebe si se han creado las imágenes de componentes Edge `imágenes docker` (las versiones de TAG pueden diferir).

**Nota**: El proceso de descarga de imágenes y el inicio del contenedor puede tardar unos minutos. Compruebe el archivo syslog para ver los detalles de registro.

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

Compruebe si se han iniciado los contenedores `docker ps`

Ejemplo:
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## Simulación de dispositivos en el nodo edge

Puede utilizar los clientes de Mosquitto para simular dispositivos que se conectan al nodo edge:

`apt-get install mosquitto-clients`

Ejecute el mandato siguiente en el nodo Edge (sustituya OrgId, DeviceType, DeviceID, DeviceToken y EdgeNodeIP por los valores adecuados).
Para averiguar qué IP debe pasarse, compruebe los mensajes que se visualizan después de ejecutar el mandato `wiotp_agent_setup`, debajo de la parte `Generación de certificados internos de Edge`.

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

Ejemplo:

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

Nota: Si está ejecutando mosquitto_pub fuera del nodo edge, utilice la opción -h `<host>` y ajuste la vía de acceso --cafile.

### Eliminación de registro del nodo edge

Para eliminar el registro del nodo edge, ejecute:

`hzn unregister`

### Desinstalación de Horizon-wiotp

Para eliminar completamente el agente de Horizon del nodo:

`apt-get purge -y horizon horizon-wiotp`

### Resolución de problemas

Comprobación de la versión del agente de Horizon:

`dpkg -l | grep horizon`

Comprobación de los registros en tiempo real del agente de Horizon:

`journalctl -af -u horizon.service`

Recopilación de syslog:

`tail -n <num-max-of-lines> /var/log/syslog `

Filtrado de mensajes de registro por contenedores:

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## Búsqueda de información adicional
{: #more_info}

Para obtener más información sobre {{site.data.keyword.iot_short_notm}} Edge, consulte los temas siguientes:
- [Visión general de {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge.html#edge_overview)
- [Configuración de {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
- [Desarrollo para {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
