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


# Instalando o  {{site.data.keyword.iot_short_notm}}  Edge on devices (Preview)
{: #edge_install_device}

**Importante:** o recurso {{site.data.keyword.iot_full}} Edge está disponível apenas como parte de um programa de visualização limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Para que suas máquinas de visualização {{site.data.keyword.iot_short_notm}} Edge sejam gerenciadas pelo IBM Horizon com o {{site.data.keyword.iot_short_notm}}, deve-se primeiro instalar o software Horizon + WIoTP nelas e, em seguida, registrar a máquina com o Horizon Exchange. Estas etapas permitirão que você use aplicativos por meio dos repositórios de desenvolvimento de visualização em um ambiente de teste provisório.

## Antes de você instalar

### Pré-requisitos para nós de extremidade baseados em Ubuntu

- Inclua o Docker no registro se ele ainda não estiver instalado:

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- Incluir o pacote Horizon base (agente ANAX)

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Pré-requisitos para para Raspberry PI

- Inclua as linhas a seguir em `/etc/apt/sources.list.d/bluehorizon.list`:
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- Inclua a linha a seguir em  ` /etc/apt/sources.list.d/docker.list `:

` deb [ arch = armhf ] https://download.docker.com/linux/raspbian esticar borda `

- Instale o  ` docker-ce `:

` apt-get update & & apt-get install docker-ce `


## Instalando o Horizon e o WIoTP Horizon Packages

` apt-get update & & apt-get install -y horizon-wiotp `

## Registrando seu Nó do Edge

Execute a configuração do agente:

Para ambientes de produção, execute:  ` wiotp_agent_setup -- org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

Ele executará as seguintes etapas:
- Configurar o WIoTP Edge Core IoT Workload.
- Gerar certificados edge-mqttbroker.
- Registre o nó com relação ao WIoT Platform usando a ferramenta hzn (`hzn --help` para obter mais informações).

Se mais de um endereço IP for localizado no dispositivo, será solicitado que você escolha um para gerar o certificado interno. Esse IP será usado posteriormente para testar a conexão entre o nó de borda e um dispositivo que está enviando dados.

Certifique-se de que não haja mensagens de erro durante a execução.

Espere alguns segundos e, em seguida, verifique se os componentes do Edge foram criados.

Verifique se as imagens dos componentes de borda foram criadas `docker images` (as versões de TAG podem diferir).

**Nota**: o processo de fazer download de imagens e iniciar o contêiner pode demorar alguns minutos. Verifique o arquivo syslog para ver os detalhes da criação de log.

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

Verifique se os contêineres foram iniciados `docker ps`

Exemplo:
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## Simulando dispositivos em seu nó de extremidade

É possível usar mosquitto-clients para simular dispositivos conectando-se ao seu nó de borda:

` apt-get install mosquitto-clients `

Execute o comando a seguir em seu nó Edge (substitua OrgId, DeviceType, DeviceID, DeviceToken e EdgeNodeIP pelos valores apropriados).
Para descobrir qual IP precisa ser passado, verifique as mensagens exibidas após a execução do comando `wiotp_agent_setup`, abaixo da parte `Generating Edge internal certificates`.

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

Exemplo:

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

Nota: se você estiver executando mosquitto_pub fora do nó de borda, use a opção -h `<host>` e ajuste o caminho --cafile.

### Cancelando o registro do nó de borda

Para cancelar o registro do nó de extremidade, execute:

`hzn unregister`

### Desinstalando o Horizon-wiotp

Para remover completamente o agente de horizonte de seu nó:

`apt-get purge -y horizon horizon-wiotp`

### Resolução de Problemas

Verificando versão do agente do horizonte:

` dpkg -l | horizonte grep `

Verificando logs de tempo real do agente do horizonte:

` journalctl -af -u horizon.service `

Coletando syslog:

` tail -n <num-max-of-lines> /var/log/syslog  `

Filtrando mensagens de log por contêineres:

` tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector' `

` tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker' `

` tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im' `


## Localizando Mais Informações
{: #more_info}

Para obter mais informações sobre o {{site.data.keyword.iot_short_notm}} Edge, consulte os tópicos a seguir:
- [ {{site.data.keyword.iot_short_notm}}  Visão Geral do Edge ](WIoTP_edge.html#edge_overview)
- [ Configurando o  {{site.data.keyword.iot_short_notm}}  Edge ](WIoTP_edge_config.html#edge_configure)
- [ Desenvolvendo para  {{site.data.keyword.iot_short_notm}}  Edge ](WIoTP_edge_dev.html#edge_dev)
