---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# APIs HTTP para dispositivos de gateway
{: #api_link}

Há dois tipos de APIs HTTP que podem ser usadas com o {{site.data.keyword.iot_full}}. É possível usar as APIs de REST HTTP para
criar, atualizar, listar e excluir dispositivos de gateway. Use as APIs do Sistema de Mensagens HTTP para enviar informações de evento dos dispositivos de gateway para a nuvem e para aceitar comandos de aplicativos na nuvem. 


## Conexões do cliente
{: #client_connections}

Para obter informações sobre a segurança do cliente e como conectar clientes a dispositivos de gateway no {{site.data.keyword.iot_short_notm}}, veja [Conectando aplicativos, dispositivos e gateways ao {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

**Nota:** a porta HTTP 1883 está desativada por padrão. Para obter informações sobre como mudar a configuração padrão, consulte [Configurando políticas de segurança](../reference/security/set_up_policies.html#set_up_policies.md).


## Authentication

Todas as solicitações devem incluir um cabeçalho de autorização. Autenticação Básica é o único método suportado. Quando um dispositivo faz uma solicitação de HTTP usando a API de REST HTTP do {{site.data.keyword.iot_short_notm}}, as credenciais a seguir são necessárias:

|Credencial|Entrada requerida|
|:---|:---|
|User name| `g/{orgId}/{gwType}/{gwDevId}` ou `g-{orgId}-{gwType}-{gwDevId}`
|Password| O token de autenticação que era gerado automaticamente ou especificado manualmente quando você registrava o dispositivo de gateway.

em
que:

<dl>
<dt>orgId</dt>  
<dd>O nome da organização, que deve corresponder ao nome especificado no cabeçalho do host.</dd>

<p></p>
<dt>gwType</dt>  
<dd>O tipo de gateway.</dd> 
<dd>Se você usar o caractere hífen "-" como um delimitador no nome de usuário, esse valor não deverá incluir um caractere de hífen. </dd>
<p></p>
<dt>gwDevId</dt>  
<dd>O identificador de dispositivo de gateway. </dd>
</dl>


## Cabeçalhos de solicitação Content-Type

Um cabeçalho de solicitação `Content-Type` deve ser fornecido com a solicitação. A tabela a seguir mostra como os tipos suportados são mapeados para os formatos internos do {{site.data.keyword.iot_short_notm}}:

|cabeçalho Content-Type|Formato no {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|texto/simples|"texto"
|aplicativo/json| "json"
|aplicativo/xml | "xml"
|application/octet-stream|"bin"

## Qualidade de Serviço

Semelhante ao nível 0 de serviço de entrega de qualidade de serviço MQTT "no máximo uma vez", o sistema de mensagens REST HTTP (Protocolo de Transporte de Hipertexto) fornece entrega de mensagem não persistente, mas valida se a solicitação está correta e se pode ser entregue ao servidor antes de enviar a resposta HTTP. Uma resposta que contém um código de status HTTP (Protocolo de Transporte de Hipertexto) 200 confirma que a mensagem foi entregue ao servidor. Ao usar o nível de qualidade de serviço MQTT "no máximo uma vez" ou o equivalente de HTTP (Protocolo de Transporte de Hipertexto) para entregar mensagens de eventos, o dispositivo ou o aplicativo deve implementar a lógica de nova tentativa para garantir a entrega.

Para obter mais informações sobre o protocolo MQTT e os níveis de qualidade de serviço para o {{site.data.keyword.iot_short_notm}}, consulte [Sistema de mensagens MQTT](../reference/mqtt/index.html).

É possível usar as APIs do sistema de mensagens HTTP para publicar eventos dos dispositivos de gateway para a nuvem e para receber comandos de aplicativos na nuvem.

## Publicando eventos
{: #event_publication}

Além de usar o protocolo de sistema de mensagens MQTT, também é possível configurar seus dispositivos de gateway para publicar eventos no {{site.data.keyword.iot_short_notm}} sobre HTTP usando comandos da API do sistema de mensagens HTTP.

Para enviar uma solicitação de ``POST`` de um dispositivo que está conectado ao {{site.data.keyword.iot_short_notm}}, use uma das URLs a seguir:

### Solicitação POST não segura para eventos de publicação

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Solicitação de POST seguro para eventos de publicação

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Notas importantes:**
- Porta 443, a porta SSL padrão, também pode ser especificada para proteger as chamadas API HTTP.
- Se um gateway não estiver designado à função *Gateway padrão*, ele poderá publicar eventos em nome de qualquer dispositivo na organização. Se o dispositivo que está conectado ao gateway não estiver registrado, o gateway registrará esse dispositivo automaticamente.
- Designe a função *Gateway padrão* se você desejar verificar os níveis de autorização do dispositivo.

Para obter mais informações sobre a função de gateways e grupos de recursos, consulte [Controle de acesso ao gateway](../gateways/gateway-access-control.html).

## Recebendo comandos
{: #receive_commands}

Além de usar o protocolo de sistema de mensagens MQTT, também é possível configurar seus dispositivos de gateway para receber comandos do {{site.data.keyword.iot_short_notm}} sobre HTTP usando comandos da API do sistema de mensagens HTTP. Um dispositivo de gateway pode receber comandos que são direcionados para dispositivos dentro de seu grupo de recursos associado. Para obter mais informações sobre grupos de recursos de gateway, consulte [Controle de acesso ao gateway](../gateways/gateway-access-control.html).

Use uma das URLs a seguir para enviar uma solicitação de ``POST`` de um gateway que está conectado ao {{site.data.keyword.iot_short_notm}}:

### Solicitação POST não segura para receber comandos

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Solicitação de POST seguro para receber comandos

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Nota:** porta 443, a porta SSL padrão, também pode ser especificada para proteger as chamadas API HTTP.

É possível incluir opcionalmente o parâmetro *waitTimeSecs* no corpo da solicitação de HTTP para especificar um número inteiro que represente o número máximo de segundos a aguardar por um comando:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Notas importantes:**
- O valor de *waitTimeSecs* deve ser um número inteiro no intervalo de 0 a 3600 segundos. O valor
padrão é 0.
- Para receber comandos para qualquer tipo de dispositivo, use o caractere curinga "qualquer" (+) para o componente `typeId`. Se o caractere curinga for usado, o tipo de dispositivo estará contido no campo de cabeçalho de resposta *X-deviceType*.
- Para receber comandos para qualquer dispositivo, use o caractere curinga "qualquer" (+) para o componente `deviceId`. Se o caractere curinga for usado, o identificador de dispositivo estará contido no campo de cabeçalho de resposta *X-deviceId*.
- Para receber qualquer comando, use o caractere curinga "qualquer" (+) para o componente `command`. Se o caractere curinga for usado, o identificador de comando estará contido no campo de cabeçalho de resposta *X-commandId*.
- Se o código de status de resposta de HTTP for 200, os dados do comando estarão contidos no corpo da resposta. Revise o campo de cabeçalho de resposta *Content-Type* para localizar o tipo de conteúdo.
- Se o código de status de resposta de HTTP for 204, nenhum dado de comando estará disponível.

Para acessar as APIs de REST HTTP do {{site.data.keyword.iot_short_notm}}, consulte [API de REST HTTP do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

Para acessar as APIs do Sistema de mensagens HTTP do {{site.data.keyword.iot_short_notm}}, consulte [API do Sistema de mensagens HTTP do {{site.data.keyword.iot_short_notm}}![Ícone de link externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.

## Último cache de eventos
{: #last-event-cache}

É possível usar o último cache de eventos para armazenar informações sobre o último evento que foi enviado para o {{site.data.keyword.iot_short_notm}} por um dispositivo conectado. Ao usar uma [API ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}, é possível recuperar o último valor registrado de um ID de evento específico para um dispositivo ou o último valor registrado para cada ID de evento relatado por um dispositivo específico. 

### Configurando o último cache de eventos

O recurso de último cache de eventos fica desativado por padrão, mas é possível ativar a funcionalidade das seguintes maneiras: 

-	Configure o parâmetro *enabled* como **true** usando uma API.
-	Na página *Configurações* do painel do {{site.data.keyword.iot_short_notm}}, configure **Ativar LEC** como **Ativo**.

O período de tempo que os dados do evento ficam armazenados no cache é especificado pelo valor de tempo de vida (TTL). Os últimos dados do evento de qualquer evento específico são armazenados por sete dias, por padrão.  É possível mudar a duração usando uma API para atualizar o parâmetro **ttlDays** ou selecionando um valor para o campo **TTL de dados do evento** na página *Configurações* do painel do {{site.data.keyword.iot_short_notm}}.

Para planos Lite, é possível armazenar informações por um mínimo de um dia e um máximo de sete dias. Para outros planos, é possível armazenar informações por um mínimo de um dia e um máximo de 45 dias.

Use as informações a seguir para ajudá-lo a configurar as definições do último cache de eventos usando APIs.

Para recuperar a configuração atual do último cache de eventos de uma organização, use a API a seguir:

```
    GET /api/v0002/config/lec
```   

   
O exemplo a seguir mostra uma resposta bem-sucedida para o método GET:
```
{
    "enabled": false, "ttlDays": 7
}
```
Qualquer pessoa ou aplicativo em uma organização está autorizado a acessar esse terminal. 

Se o último cache de eventos for desativado para uma organização pelo administrador do {{site.data.keyword.iot_short_notm}}, o código de status HTTP 403 FORBIDDEN a seguir será retornado em resposta ao método GET:

```
{
    "exceção": {
        "id": "CUDCS0105E", "properties": ["myOrgID"]
    }, "message": "CUDCS0105E: Last event cache is disabled for this organization. Para obter mais informações, entre em contato com a equipe de suporte.
}
```

Se o último cache de eventos não estiver ativado, um código de status HTTP 404 será retornado em resposta ao método GET. Para ativar o recurso de último cache de eventos de uma organização, use a API a seguir:

```
    PUT /api/v0002/config/lec
```

com a carga útil de exemplo a seguir: 
```
{
    "enabled": true, "ttlDays": 5
}
```
em
que: 

- **enabled** é necessário e especifica se o último cache de eventos está ativado para uma organização. O valor deve ser um valor booleano. 

- **ttlDays** é opcional e especifica o número de dias que um evento fica retido no último cache de eventos de uma organização. O valor deve ser um número inteiro entre 1 e 45, inclusive.
   
 
Apenas o administrador, os usuários do operador ou os aplicativos do operador estão autorizados a acessar esse terminal. Se os parâmetros ou o JSON for inválido, um código de status HTTP 401 BAD REQUEST será retornado em resposta ao método PUT. 

**Nota:** a conclusão da mudança na configuração pode levar até 30 segundos.

### Recuperando informações do último cache de eventos

Ao usar uma API, é possível recuperar o último evento enviado por um dispositivo. É possível recuperar o status do dispositivo, independentemente do local físico do dispositivo ou do status de uso. É possível recuperar o último valor registrado de um ID de evento para um dispositivo específico ou o último valor registrado para cada ID do evento que foi relatado por um dispositivo específico. Os últimos dados do evento de um dispositivo podem ser recuperados para qualquer evento específico que tenha ocorrido até 45 dias atrás.

Para solicitar o valor mais recente para um ID de evento específico, use a solicitação de API a seguir, que retorna o último valor registrado para o ID do evento “energia”.

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

A resposta é retornada no formato JSON a seguir:

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**Observação:** enquanto a resposta da API está no formato JSON, cargas úteis do evento podem ser gravadas em qualquer formato. As cargas úteis retornadas pela API do Último cache de eventos são codificadas em base64.

Para solicitar o valor mais recente para cada ID do evento que foi relatado por um dispositivo, use a solicitação de API a seguir:

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

A resposta inclui todos os IDs de evento que foram enviados pelo dispositivo. No exemplo a seguir, os valores são retornados para os eventos “energia” e “temperatura”.

```
[
    {
        "deviceId": "<device-id>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

Para acessar as APIs de Último cache de eventos do {{site.data.keyword.iot_short_notm}}, consulte [API do Information Management HTTP do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}.
