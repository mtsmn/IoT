---

copyright:
  years: 2017
lastupdated: "2017-10-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Visão geral do controle de acesso no nível de recursos (Beta)
{: #RLAC_overview}

O controle de acesso de nível de recursos permite controlar o acesso do usuário e da chave API para gerenciar dispositivos. Use
grupos de recursos para especificar os dispositivos em uma organização que cada usuário ou
chave API pode gerenciar. Para obter informações sobre como configurar o controle de
acesso no nível de recursos, consulte [Configurando
o controle de acesso no nível recursos](rlac.html#configure_RLAC).

**Importante:** o recurso de controle de acesso de nível de recursos de {{site.data.keyword.iot_full}} está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [nos forneça sua opinião ![External link icon](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Conceitos do controle de acesso no nível de recursos 
{: #RLAC_concepts}

Os grupos de dispositivos fazem parte dos controles de acesso no nível de recursos
do {{site.data.keyword.iot_short_notm}}. É possível usar o recurso de grupos de
dispositivos para gerenciar o número de dispositivos que a equipe pode acessar de acordo
com sua função. Antes de implementar grupos de dispositivos, determine e registre a
intenção e o escopo dos grupos de dispositivos para aumentar a eficiência da sua solução. 

É possível implementar grupos de dispositivos como parte de sua solução de IoT para
permitir que um usuário ou aplicativo acesse um subconjunto de dispositivos em vez de
todos os dispositivos que estão associados a uma organização. Para aumentar a eficiência
dos grupos de dispositivos, use esses grupos quando uma pessoa precisar de acesso a muitos dispositivos.

Por exemplo, uma empresa que emprega vários engenheiros de campo e a equipe de
operações quer implementar uma solução IoT no Reino Unido. A empresa configura um grupo
de dispositivos para cada uma das 69 cidades, 9 grupos de dispositivos para cada uma das
regiões geográficas e um grupo de dispositivos para todo o Reino Unido. Os dispositivos
são alocados para esses grupos e os grupos são designados a 15 engenheiros de campo e à
equipe de operações, alguns dos quais têm acesso a mais de um grupo. Os usuários que são
responsáveis por apenas um ou dois dispositivos podem continuar a usar aplicativos móveis
e arquiteturas empresariais que são externas ao
{{site.data.keyword.iot_short_notm}}, mas complementam a solução IoT geral.

Perguntas sobre grupos de dispositivos podem ser feitas em
[Perguntas
no espaço do Internet of Things![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link
externo")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window}.

## Limites de grupo de recursos para controle de acesso

São impostos limites de grupo de recursos para assegurar que o recurso de controle
de acesso no nível de recursos funcione efetivamente. Os limites restringem o número de
grupos de recursos que são designados a um sujeito, o número de dispositivos nos grupos
e o número de grupos ao qual um recurso pode pertencer.

Os limites a seguir são impostos:

 - No máximo 10 grupos de recursos podem ser designados a uma chave API, usuário ou gateway.
 - Um grupo de recursos pode ter no máximo 300 recursos.
 - Um recurso pode pertencer a no máximo 10 grupos.

## Terminologia de controle de acesso no nível de recursos

É possível usar as seções a seguir para ajudar a entender a terminologia que é
usada no recurso de controle de acesso no nível de recursos. 

### Sujeitos
{: #subjects}

Um sujeito é uma entidade autenticada que solicita o acesso à plataforma, que deve
ser autorizado. Os seguintes tipos de sujeitos são válidos:

- *Usuários*: usuários finais que são as pessoas usando os aplicativos. Um membro é um usuário cuja conta não tem data de expiração e um convidado é um usuário cuja conta tem uma data de expiração.
- *Dispositivos*: dispositivos físicos que acessam a plataforma {{site.data.keyword.iot_short_notm}} e usam o tipo Dispositivo.
- *Gateways*: dispositivos físicos que acessam a plataforma {{site.data.keyword.iot_short_notm}} e usam o tipo Gateway.
- *Aplicativos*: aplicativos ou serviços que acessam a plataforma {{site.data.keyword.iot_short_notm}} usando chaves API para autorização e controle de acesso.

### Recursos
{: #resources}

Um recurso é uma entidade sobre a qual o sujeito executa a ação. O sujeito
solicita acesso autorizado à plataforma para o recurso. Os dispositivos são os
únicos recursos que são suportados na liberação Beta do controle de acesso no nível de
recursos.

### Ações
{: #actions}

A ação é o que o sujeito executa, que na maioria das vezes é Criar, Ler,
Atualizar, Excluir ou Executar. As operações são usadas para agrupar ações em recursos.

### Aplicativos
{: #applications}

Um aplicativo pode agir em nome de um sujeito que é desconhecido para a plataforma,
como um aplicativo móvel que controla um dispositivo interno e um aplicativo pode agir
em nome de um dispositivo real ou simulado que é conhecido ou desconhecido para a
plataforma. Pode ser também um verdadeiro aplicativo do lado do servidor que não está
agindo em nome de qualquer outro tipo de dispositivo.

A autenticação é fornecida e o acesso à plataforma {{site.data.keyword.iot_short_notm}} é fornecido com base na chave API ou token.

### Operations
{: #operations}

Uma operação contém uma ou mais ações que são executadas em determinados tipos de
recursos usando interfaces com o usuário e interfaces programáticas, como serviços REST e
interfaces MQTT. São utilizadas por diferentes sujeitos, incluindo membros, aplicativos e
dispositivos. As operações são identificadas por seus IDs (OID).

### Funções
{: #roles}

Funções são conjuntos de operações que são utilizadas para fornecer permissão para
usar as operações. Um sujeito pode ter várias funções e a função é um atributo de um
sujeito.

**Formato de funções**

Matriz de 0..n funções

    { "roles": [ "PD_ADMIN_APP" ] }

**Formato de funções para grupos**

Mapa de 0..n pares de <sequência, lista<sequência>>

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### Permissões
{: #permissions}

As permissões autorizam um sujeito a executar operações em um recurso ou grupo de
recursos.

### Grupos de recursos
{: #resource_groups}

Um grupo de recursos é uma coleção de dispositivos como recursos. Os dispositivos
podem ser incluídos em grupos de recursos e grupos de recursos podem ser designados a
sujeitos em pares de função para grupos. Esses relacionamentos permitem que os
sujeitos executem operações para uma determinada função em grupos específicos de dispositivos.

### Usuários
{: #users}

Os usuários efetuam login no painel da plataforma
{{site.data.keyword.iot_short_notm}} usando um ID que é membro de uma
organização. Os usuários recebem funções que concedem a eles autorização para chamar
certos conjuntos de APIs HTTP. Para obter mais informações sobre as funções de usuário,
consulte [Níveis de acesso
para funções de usuário](roles_access.html#levels-of-access-for-user-roles).

### Chaves API (interface de programação de aplicativos)
{: #API_keys}

Chaves API são tokens usados para chamar as APIs HTTP da plataforma
{{site.data.keyword.iot_short_notm}}. As chaves API recebem funções que
concedem a elas autorização para chamar certos conjuntos de APIs HTTP. Para obter mais
informações sobre funções de chave API, consulte
[Níveis de
acesso para funções do aplicativo](app_roles_access.html#levels-of-access-for-application-roles).

## APIs em que o controle de acesso no nível de recursos é imposto
{: #RLAC_enforced_APIs}
Quando o controle de acesso no nível de recursos está ativado, as APIs relacionadas a
dispositivos que estão incluídas nessa seção só funcionarão se o dispositivo
especificado estiver acessível ao responsável pela chamada.

### APIs de dispositivo (individual)

**Listar dispositivos de tipo**

    GET /api/v0002/device/types/${typeId}

Somente o subconjunto de dispositivos que pertencem aos grupos apropriados é retornado.

**Obter dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Obter detalhes do gerenciamento de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Obter dispositivos registrados por um gateway**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

Retornará um erro 404 se o gateway não estiver acessível ao responsável pela
chamada. Somente o subconjunto de dispositivos que pertencem aos grupos apropriados é retornado. Os
dispositivos e os gateways devem estar no grupo do responsável pela chamada.

**Atualizar dispositivo**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Excluir dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

### APIs de dispositivo (em massa)

**Listar dispositivos**

    GET /api/v0002/bulk/devices

Somente o subconjunto de dispositivos que pertencem aos grupos apropriados é retornado.

**Excluir dispositivos**

    DELETE /api/v0002/bulk/devices/remove

Somente o subconjunto de dispositivos que pertencem aos grupos apropriados é
excluído. Os dispositivos que não estão acessíveis ainda retornam com sucesso =
verdadeiro.

**Atualizar dispositivos:**

    PUT /api/v0002/bulk/devices/update

### APIs de gerenciamento de dispositivo

**Iniciar solicitação**

    POST /api/v0002/mgmt/requests

Falha se algum dispositivo não está acessível para o responsável pela chamada

**Excluir solicitação**

    DELETE /api/v0002/mgmt/requests/${requestId}

Falha se algum dispositivo não está acessível para o responsável pela chamada

**Visualizar solicitação**

    GET /api/v0002/mgmt/requests/${requestId}

**Listar solicitações**

    GET /api/v0002/mgmt/requests

**Obter uma solicitação**

    GET /api/v0002/mgmt/requests/${requestId}

**Obter detalhes do dispositivo de solicitação**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**Obter detalhes do dispositivo de solicitação para um dispositivo**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### APIs de determinação de problema

**Logs de conexão para um dispositivo**

    GET /api/v0002/logs/connection

Retornará uma matriz vazia se o dispositivo não estiver acessível.

**Incluir código de erro de dispositivo**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Listar códigos de erro de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Limpar códigos de erro de dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Incluir log de diagnóstico de dispositivo**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Listar log de diagnósticos de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Obter um log de diagnósticos de dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Limpar log de diagnósticos de dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Excluir um log de diagnósticos de dispositivo**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

###APIs de último cache de eventos

**Obter eventos para o dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.

**Obter eventos para o dispositivo**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

Retorna um erro 404 se o dispositivo não é acessível pelo responsável pela chamada.
