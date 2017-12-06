---

copyright: years: 2016, 2017 lastupdated: "2017-10-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Pacote de gateway da Internet of Things (Beta)
{: #gw_package}

O pacote de gateway da Internet of Things do {{site.data.keyword.iot_full}} permite que um
gateway registrado no {{site.data.keyword.iot_short_notm}} envie eventos para a
plataforma de seus dispositivos que estão no grupo de recursos que está associado ao
gateway.
{:shortdesc}

**Importante:** O pacote de gateway da Internet of Things está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Visão geral

O pacote de gateway da Internet of Things inclui as seguintes entidades:

| Entidade | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | pacote | org, domain, gatewayTypeId, gatewayId, gatewayToken  | Trabalhar com o gateway do {{site.data.keyword.iot_short_notm}} |
| `/watson-iot/iot-gateway/publishEvent` | ação | org, domain, gatewayTypeId, gatewayId, gatewayToken, eventType, typeId, deviceId, payload | Enviar eventos de um gateway registrado em nome de seus dispositivos associados para o {{site.data.keyword.iot_short_notm}}   |

## Criando uma ligação de pacote de gateway da Internet of Things
Para criar uma ligação de pacote de gateway da Internet of Things, deve-se especificar os parâmetros a seguir:

| Parâmetro |  Descrição |
| --- | ---  |
| org | O identificador da organização |
| gatewayTypeId | O identificador de tipo de gateway do gateway registrado |
| gatewayId | O identificador de gateway do gateway registrado |
| gatewayToken | O token de autorização do gateway registrado |


Conclua as etapas a seguir para criar uma ligação de pacote:  
1. [Efetue login no console do Bluemix
![Ícone de link externo](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/).
2. Crie o [Internet of Things
Platform Service](https://console.bluemix.net/docs/services/IoT/index.html) no Bluemix e
[anote
a chave API e o token da API](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications). Essas
informações são necessárias para criar um tipo de gateway e para registrar um gateway.
3. [Crie
um tipo de gateway](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), por exemplo myGWType, em sua organização do
Watson IoT e [registre
uma instância do gateway](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), por exemplo myGWId. Anote o Token de gateway para o gateway registrado.
4. Crie uma ligação de pacote com o pacote de gateway do da Internet of Things usando o exemplo de comando a seguir:
   ```
   wsk package bind /watson/iotgw myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. Verifique se a ligação do pacote existe usando o comando a seguir:  
   ```
   wsk package list
   ```

## Publicando eventos de dispositivo

A ação `/watson/iotgw/publishEvent` publica eventos de um gateway
do {{site.data.keyword.iot_short_notm}} registrado, em nome de seus dispositivos
associados. A tabela a seguir descreve os parâmetros que são usados ao publicar eventos:  

Parâmetro |  Descrição | Exemplo
------------- | ------------- | -------------
org | O identificador da organização à qual o gateway registrado pertence  | `-p org "uguhsp"`
domínio | Opcional. O domínio ao qual o gateway registrado pertence. O padrão aponta para `messaging.internetofthings.ibmcloud.com` | `-p domain "messaging.internetofthings.ibmcloud.com"`
gatewayTypeId | O identificador de tipo de gateway do gateway registrado. | `-p gatewayTypeId "myGatewayType"`
gatewayId | O identificador de gateway do gateway registrado. O identificador deve ser exclusivo dentro de uma organização para um determinado tipo de gateway. | `-p gatewayId "00aabbccde03"`
gatewayToken | O token (senha) que é usado pelo gateway registrado para se conectar ao Watson IoT Platform.  | `-p gatewayToken "ZZZ"`
eventType | O tipo de evento no qual o gateway registrado publica eventos em nome de seus dispositivos associados. | `-p eventType "evt"`
typeId | O tipo do dispositivo que está associado ao gateway registrado. O gateway registrado publica eventos em nome do dispositivo associado. | `-p typeId "myDeviceType"`
deviceId | O identificador do dispositivo que está associado ao gateway registrado. O gateway registrado publica eventos em nome do dispositivo associado. O identificador de dispositivo deve ser exclusivo dentro de uma organização para um determinado tipo de gateway. | `-p deviceId "00aabbccde03_0001"`
payload | A carga útil que o gateway registrado publica em nome do dispositivo. | `-p payload "{'d':{'temp':38}}"`


O exemplo a seguir mostra como publicar eventos do pacote *iotgw*:

Publicar um evento de dispositivo usando a ação *publishEvent* na
ligação de pacote que você criou. Deve-se substituir `/myNamespace/myGateway` pelo nome do seu pacote.

 ```
  wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## Trabalhando com o repositório

Conclua as etapas a seguir para implementar o pacote usando `installCatalog.sh`
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

em que *AUTH* é a chave de autorização, *APIHOST* é o nome do host do OpenWhisk e *WSK_CLI* é o local do binário da CLI do Openwhisk.
