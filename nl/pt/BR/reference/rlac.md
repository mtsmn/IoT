---

copyright:
  years: 2017
lastupdated: "2017-07-19"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurando o controle de acesso no nível de recursos (Beta)
{: #configure_RLAC}

**Importante:** o recurso de controle de acesso de nível de recursos de {{site.data.keyword.iot_full}} está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [nos forneça sua opinião ![External link icon](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

O controle de acesso de nível de recursos permite controlar o acesso do usuário e da chave API para gerenciar dispositivos. Você usa grupos de recursos para definir quais dispositivos em uma organização cada usuário ou chave API pode gerenciar. Os usuários e as chaves API podem ser designados a um par de "função para grupos", que define se eles podem executar apenas as operações que são cobertas pela função especificada em dispositivos que estão nos grupos especificados. Para obter mais informações sobre o controle de acesso no nível de recursos, consulte [Visão geral do controle de acesso no nível de recursos](rlac_overview.md) e a [documentação da API de controle de acesso do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Configurando o controle de acesso no nível de recursos - fluxo do processo
{: #RLAC_process}

O fluxo do processo a seguir é geralmente usado para ativar e usar o controle de acesso no nível de recursos:
1. [Criar uma organização](../iotplatform_overview.html#organizations).
2. [Criar usuários](../add_users.html#adding-new-users) e [Chaves API](../platform_authorization.html#api-key).
3. [Criar grupos de recursos](rlac.html#create_delete_group).
4. [Designar mapeamentos de função para grupos para os usuários e as chaves API](rlac.html#assign_roletoegroup).
5. [Incluir dispositivos nos grupos de recursos](rlac.html#add_device).
6. [Ativar controle de acesso no nível de recursos](rlac.html#RLAC_enable).

O controle de acesso no nível de recursos é imposto em várias APIs relacionadas a
dispositivo. Para obter uma lista de APIs afetadas, consulte
[APIs em que o controle de acesso no
nível de recursos é imposto](rlac_overview.html#RLAC_enforced_APIs).

## Criando e excluindo grupos de recursos
{: #create_delete_group}

Os grupos de recursos podem ser criados e excluídos independentemente da conexão de gateways.

Para criar um grupo de recursos e retornar os detalhes do grupo, use a API a seguir:

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Dispositivos no grupo vermelho",
        "searchTags": ["red"]
    }

Quando você exclui um grupo de recursos, os dispositivos que estavam no grupo são
removidos do grupo, mas os próprios dispositivos não são afetados de qualquer outra
forma. Para excluir um grupo de recursos, use a API a seguir:

    DELETE /api/v0002/groups/{groupUid}


## Designando pares de "funções para grupos" a usuários ou chaves API
{: #assign_roletogroup}

A designação de um par de "função para grupos" a um usuário ou a uma chave API é
necessária para restringir o usuário ou a chave API a um grupo de dispositivos. Usuários
e chaves API sem quaisquer grupos de recursos podem gerenciar todos os dispositivos em
sua organização sem restrição. Uma chave API pode ter apenas uma função se um par de
"funções para grupos" for usado. A função que é especificada em "rolesToGroups" deve
corresponder à função que é especificado em "funções".

Para designar um par de "função para grupos", use a API a seguir:

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



Para designar um par de "função para grupos" a uma chave API, use a API a seguir:

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## Incluindo e removendo dispositivos de grupos de recursos
{: #add_device}

Para que um usuário ou chave API com um par de "função para grupos" possa gerenciar
um dispositivo, o dispositivo deve ser membro do grupo de recursos que é designado ao
usuário ou chave API. Quando você inclui dispositivos em um grupo de recursos, o grupo no
qual você está incluindo dispositivos deve ser especificado no caminho da solicitação e
os dispositivos que são incluídos devem ser especificados no corpo da solicitação. Para
incluir vários dispositivos em um grupo de recursos ao mesmo tempo, use a API a seguir:

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}", "deviceId":"{deviceUid}" }
    ]

Quando você remove dispositivos de um grupo de recursos, os dispositivos
especificados no corpo da solicitação são removidos do grupo especificado no caminho da
solicitação. Para remover múltiplos dispositivos de um grupo de recursos, use a API a seguir:

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}", "deviceId":"{deviceUid}" }
    ]

Para obter mais informações sobre esquema de solicitação e resposta, consulte a [documentação da API de controle de acesso do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Ativando o controle de acesso no nível de recursos
{: #RLAC_enable}

A ativação do controle de acesso no nível de recursos para uma organização
permitirá que os usuários e as chaves API fiquem restritos a seus grupos de recursos
designados. Para usar o controle de acesso no nível de recursos, deve-se ativar uma
sinalização de configuração no nível da organização usando a API a seguir:

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Localizando grupos de recursos
{: #find_group}

Os grupos de recursos podem ter tags de procura associadas. As tags de procura podem ser usadas para recuperar os detalhes de um grupo de recursos usando a API a seguir:

    GET /api/v0002/groups

Esta API retorna os grupos de recursos associados à tag de procura que é usada. Se nenhuma tag de procura for especificada, todos os grupos de recursos serão retornados. 

Para localizar o ID exclusivo de grupos de recursos que são designados a um usuário, use a API a seguir:

    GET /api/v0002/authorization/users/{userUid}

Para localizar o ID exclusivo de grupos de recursos que são designados a uma chave API, use a API a seguir:

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## Consultando grupos de recursos
{: #query_group}

Os grupos de recursos podem ser consultados usando vários parâmetros para retornar
as propriedades completas de todos os dispositivos no grupo, os identificadores
exclusivos de todos os dispositivos no grupo ou as propriedades do grupo de recursos.

Para retornar as propriedades completas de todos os dispositivos no grupo de recursos especificado, use a API a seguir:

    GET /api/v0002/bulk/devices/{groupUid}

Para retornar somente os identificadores exclusivos dos membros do grupo de recursos, use a API a seguir:

    GET /api/v0002/bulk/devices/{groupUid}/ids

Para retornar as propriedades do grupo de recursos, incluindo o nome, a descrição,
as tags de procura e o identificador exclusivo que são especificados no caminho, use a API a
seguir:

    GET /api/v0002/groups/{groupUid}

Essa API não retorna a lista de membros do grupo de recursos.

## Atualizando propriedades do grupo
{: #update_group}

Para atualizar as propriedades de um grupo, use a API a seguir:

    PUT /api/v0002/groups/{groupId}
