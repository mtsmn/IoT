---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Monitorando a conectividade do cliente (Beta)
{: #connect_status}

O {{site.data.keyword.iot_full}} permite consultar e monitorar o status da conexão de clientes. É possível saber em tempo real se um cliente está conectado e resolver problemas de conexão.
{:shortdesc}

Por exemplo, é possível ver quando um cliente esteve ativo pela última vez e por que ele se desconectou ou é possível visualizar todos os clientes que não se conectaram no dia anterior para que você possa tratar de quaisquer problemas.

**Importante:** o recurso de conectividade do cliente de monitoramento está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Consultando o Status da Conexão do Cliente
{: #query_status}

A API do Estado de Conexão do Cliente permite recuperar e consultar o status da conexão para quaisquer clientes que tenham se conectado ao {{site.data.keyword.iot_short_notm}}.

Quando você consulta o status da conexão, é possível visualizar as informações a seguir:

 - o status de conectividade (conectado ou desconectado)
 - o horário da última conexão, no formato ISO 8601
 - o horário da última desconexão, no formato ISO 8601
 - o último evento de atividade (como a publicação MQTT ou a assinatura MQTT)
 - o horário da última atividade, no formato ISO 8601
 - a entidade que causou a desconexão, se o cliente estiver desconectado (cliente, servidor ou rede)
 - o código de razão MQTT para a desconexão, se o cliente estiver desconectado

**Exemplos**

Para recuperar o status da conexão do cliente para um único cliente:

` GET {0}{0}{0}{2}/clientconnectionstates/ { `

Para recuperar o status da conexão de todos os clientes:

` GET {0}{0}{0}{2}/clientconnectionstates `

Para recuperar todos os clientes conectados:

` GET {0}{0}{0}{2}/clientconnectionstates?connectionStatus = conectado `

Para recuperar todos os clientes que estiveram ativos nos últimos dois dias:

` GET {0}{0}{0}{2}/clientconnectionstates?lastConnectedAfter = {2}D } `

**Nota:** pode haver um breve atraso entre o horário em que um cliente se conecta e o horário em que o status da conexão da API reflete o status correto.

Para obter mais informações sobre a API, incluindo todos os filtros de consulta disponíveis, veja [o documento Swagger da API do Estado de Conexão do Cliente ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}.

## Monitorando status de conexão do cliente
{: #monitor_status}

 Todos os clientes têm um tópico de monitor que permite que você assine para receber atualizações de status de conectividade em tempo real para o cliente. Para obter mais informações, veja [Assinando mensagens de status do dispositivo](../../applications/mqtt.html#subscribe_device_commands).

## Resolução de problemas de conectividade do cliente

A API de Logs de Conexão permite recuperar uma lista de eventos de log de conexão para um dispositivo para diagnosticar problemas de conectividade. As entradas registram conexões bem-sucedidas, tentativas de conexão malsucedidas, desconexões intencionais e desconexões iniciadas pelo servidor.

Para obter mais informações, veja o [documento Swagger da API de Logs de Conexão ![Ícone de link externo](../../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}.
