---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Controle de acesso do gateway
{: #gateway-access-control}

Os dispositivos de gateway são uma classe especializada de dispositivo e podem agir
em nome de outros dispositivos. Os grupos de recursos de gateway definem em nome de
quais dispositivos dentro de uma organização cada gateway pode agir. As funções de gateway determinam a capacidade do gateway para registrar dispositivos para o Watson IoT Platform.
{: #shortdesc}

Você usa grupos de recursos para ativar os gateways para agir em nome de um subconjunto de dispositivos de uma organização. Os gateways podem publicar ou assinar mensagens somente para eles mesmos e em nome de dispositivos que estão em seu grupo de recursos. 

Quando um novo gateway é criado, um grupo de recursos padrão também é criado e designado ao gateway. O grupo de recursos padrão não pode ser removido do gateway. Os gateways existentes sem quaisquer grupos de recursos podem continuar a agir em nome de todos os dispositivos na organização.

Para obter informações sobre como publicar eventos de dispositivos de gateway usando APIs, veja [APIs HTTP para dispositivos de gateway](gw_api.html).

## Designando uma função a um gateway
{: #gw_roles}

Para designar um gateway a um grupo de recursos, deve-se primeiro designar o gateway a uma função. Os gateways recém-criados são designados à função *Gateway privilegiado* por padrão e são designados a um grupo de recursos padrão. Os gateways com a função privilegiada podem registrar automaticamente os dispositivos, que são incluídos no grupo de recursos padrão.

Gateways com a função *Gateway padrão* não podem registrar automaticamente dispositivos. 

Quando um gateway é designado a um grupo de recursos, o gateway pode agir somente em nome dos dispositivos nesse grupo de recursos e por si mesmo, mesmo se sua função for mudada.

Para obter mais informações sobre funções de gateway, veja [Funções de gateway](../roles_index.html#gateway_roles).

Para designar uma função a um gateway, use a API a seguir, em que
*${clientID}* é o identificador de cliente codificado pela URL no formato
*d:${orgId}:${typeId}:${deviceId}* pra dispositivos ou
*g:${orgId}:${typeId}:${deviceId}* para gateways:

```
PUT /authorization/devices/${clientID}/roles

Corpo da solicitação:
{
    "roles": [ {
            "roleId": "PD_STANDARD_GW_DEVICE", "roleStatus": 1 }
    ]
}

Resposta da solicitação: 200
{
    "roles": [ {
            "roleId": "PD_STANDARD_GW_DEVICE", "roleStatus": 1 }
    ],
    "rolesToGroups": {
        "PD_STANDARD_GW_DEVICE": ["gw_def_res_grp:abcdef:gatewayTypeId:gatewayDeviceId"]
    }
}
```

Para obter detalhes sobre o esquema de solicitação, veja a [{{site.data.keyword.iot_full}} documentação da API de gateway limitado ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_authorization_devices_deviceId_roles){: new_window}.

## Incluindo dispositivos e removendo dispositivos de um grupo de recursos
{: #devices_groups}

Antes que um gateway com a função *Gateway padrão* possa agir em nome de um dispositivo, o dispositivo deve ser um membro do grupo de recursos designado ao gateway. Para incluir vários dispositivos em um grupo de recursos ao mesmo tempo, use a API a seguir:

```
 PUT /bulk/devices/{groupId}/add
```

O grupo no qual incluir dispositivos deve ser especificado no caminho da solicitação e os dispositivos a serem incluídos devem ser especificados no corpo da solicitação. Para obter mais informações sobre o esquema de solicitação e respostas, veja a [{{site.data.keyword.iot_short_notm}} documentação da API de gateway limitado ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_add){: new_window}.

Para remover múltiplos dispositivos de um grupo de recursos, use a API a seguir:

```
PUT /bulk/devices/{groupId}/remove
```

Os dispositivos especificados no corpo da solicitação serão removidos do grupo especificado no caminho da solicitação. Para obter mais informações sobre o esquema de solicitação e a resposta, veja a [{{site.data.keyword.iot_short_notm}} documentação da API de gateway limitado ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_remove){: new_window}.

## Localizando um grupo de recursos
{: #finding_groups}

Os grupos de recursos podem ter tags de procura associadas. As tags de procura podem ser usadas para recuperar os detalhes de um grupo de recursos usando a API a seguir:

```
GET /groups
```

Essa API retorna os grupos de recursos associados à tag de procura usada. Se nenhuma tag de procura for especificada, todos os grupos de recursos serão retornados. <!-- For more information about the request schema, response, and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

O ID de um grupo de recursos designado a um gateway pode ser localizado usando a
API a seguir, em que *${clientID}* é o identificador de cliente codificado
pela URL no formato *d:${orgId}:${typeId}:${deviceId}* para dispositivos ou
*g:${orgId}:${typeId}:${deviceId}* para gateways:

```
GET /authorization/devices/${clientId}
```

Essa API retorna o identificador exclusivo do(s) grupo(s) de recursos designado(s) a este dispositivo. Mais informações sobre essa API podem ser localizadas na [{{site.data.keyword.iot_short_notm}} documentação da API de gateway limitado ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_authorization_devices_deviceId){: new_window}.


## Consultando um grupo de recursos
{: #querying_groups}

Os grupos de recursos podem ser consultados dentro de vários parâmetros para retornar as propriedades completas de todos os dispositivos no grupo, os identificadores exclusivos de todos os dispositivos no grupo ou as propriedades do grupo de recursos.

Para retornar as propriedades completas de todos os dispositivos no grupo de recursos especificado, use a API a seguir:

```
GET /bulk/devices/{groupId}
```

Essa API retorna a lista de propriedades completas para todos os membros do grupo de recursos especificado. Para obter mais informações sobre o esquema de solicitação, respostas e como percorrer os resultados, veja a [{{site.data.keyword.iot_short_notm}} documentação da API de gateway limitado ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_bulk_devices_groupId){: new_window}.

Para retornar somente os identificadores exclusivos dos membros do grupo de recursos, use a API a seguir:

```
GET /bulk/devices/{groupId}/ids
```

Essa API retorna os identificadores exclusivos de todos os dispositivos que são membros do grupo de recursos. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para retornar as propriedades do grupo de recursos, use a API a seguir:

```
GET /groups/{groupId}
```

Essa API retorna as propriedades do grupo de recursos (nome, descrição, tags de procura e identificador exclusivo) especificado no caminho, mas não retorna a lista de membros do grupo de recursos. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Criando e excluindo um grupo de recursos.
{: #crt_del_groups}

Os grupos de recursos podem ser criados e excluídos independentemente da conexão de gateways. Para criar um grupo de recursos, use a API a seguir:

```
POST /groups
```

Essa API cria um grupo de recursos e retorna os detalhes do grupo. <!-- For details on the request schema and the responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para excluir um grupo de recursos, use a API a seguir:

```
DELETE /groups/{groupId}
```

Essa API exclui o grupo de recursos especificado. Dispositivos que eram membros do grupo são removidos do grupo, mas os próprios dispositivos não são afetados de qualquer outra forma. <!-- For more information, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Atualizando propriedades do grupo
{: #update_groups}

  - /groups/{groupId}
    - PUT: atualiza as propriedades do grupo especificado.

## Recuperando e atualizando propriedades do dispositivo.
{: #fdevice_group_props}

Há várias maneiras de recuperar propriedades do dispositivo usando a API; cada API retorna informações diferentes. Para recuperar as propriedades de dispositivo de todos os dispositivos existentes em sua
organização do {{site.data.keyword.iot_short_notm}}, use a API a seguir:

```
GET /authorization/devices

```

Esta API retorna as propriedades de todos os dispositivos existentes na
organização, incluindo suas propriedades relevantes de controle de acesso (função,
status, data de expiração). <!-- For more information on responses and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para recuperar as propriedades de um único dispositivo na
organização, use a API a seguir, em que *${clientID}* é o identificador de
cliente codificado pela URL no formato *d:${orgId}:${typeId}:${deviceId}*
para dispositivos ou *g:${orgId}:${typeId}:${deviceId}* para gateways:

```
GET /authorization/devices/${clientId}
```

Esta API retorna todas as propriedades do dispositivo especificado. <!-- For more information, see the [{{site.data.keyword.iot_short_notm}} device model documentation](LINK TO DEVICE MODEL) and [API documentation](LINK TO CORRECT API). -->

Para recuperar apenas as informações de controle de acesso de um dispositivo
específico, use a API a seguir, em que *${clientID}* é o identificador de
cliente codificado pela URL no formato *d:${orgId}:${typeId}:${deviceId}*
para dispositivos ou *g:${orgId}:${typeId}:${deviceId}* para gateways:

```
GET /authorization/devices/${clientId}/roles
```

Essa API recupera as informações relevantes de controle de acesso para o dispositivo especificado sem retornar outras propriedades de dispositivo. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

As propriedades do dispositivo podem ser atualizadas de duas maneiras. As propriedades podem ser atualizadas sem mudar as propriedades de controle de acesso ou as propriedades de controle de acesso podem ser atualizadas diretamente.

Para atualizar as propriedades do dispositivo sem afetar as propriedades de
controle de acesso, use a API a seguir, em *${clientID}* é o identificador de
cliente codificado pela URL no formato *d:${orgId}:${typeId}:${deviceId}*
para dispositivos ou *g:${orgId}:${typeId}:${deviceId}* para gateways:

```
PUT /authorization/devices/${clientId}
```

Essa API só atualizará propriedades do dispositivo que não estiverem associadas ao controle de acesso. <!-- For more information on request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Para atualizar apenas as propriedades de controle de acesso do dispositivo
especificado, use a API a seguir, em que *${clientID}* é o identificador de
cliente codificado pela URL no formato *d:${orgId}:${typeId}:${deviceId}*
para dispositivos ou *g:${orgId}:${typeId}:${deviceId}* para gateways:

```
PUT /authorization/devices/${clientId}/withroles
```

Essa API somente atualizará as propriedades de controle de acesso do dispositivo especificado. <!-- For more information on the request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->
