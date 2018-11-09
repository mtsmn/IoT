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

# APIs do sistema de mensagens HTTP para dispositivos de gateway
{: #api}

## Acessando a documentação da API de sistema de mensagens HTTP para dispositivos de gateway
{: #rest_messaging_api}

Para acessar a documentação da API do sistema de mensagens HTTP do {{site.data.keyword.iot_short_notm}} e encontrar mais informações sobre o envio de eventos dos dispositivos de gateway, consulte [API do sistema de mensagens HTTP do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Conexões do cliente
{: #client_connections}

Para obter informações sobre a segurança do cliente e como conectar clientes a dispositivos de gateway no {{site.data.keyword.iot_short_notm}}, veja [Conectando aplicativos, dispositivos e gateways ao {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).


## Publicando eventos
{: #event_publication}

Além de usar o protocolo de sistema de mensagens MQTT, também é possível configurar seus dispositivos de gateway para publicar eventos no {{site.data.keyword.iot_short_notm}} sobre HTTP usando comandos da API do sistema de mensagens HTTP.

Para enviar uma solicitação de ``POST`` de um dispositivo que está conectado ao {{site.data.keyword.iot_short_notm}}, use uma das URLs a seguir:

### Solicitação de POST não segura para eventos de publicação

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Solicitação de POST segura para eventos de publicação

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Notas importantes:**
- É possível enviar eventos de dispositivo de gateway apenas usando o sistema de mensagens HTTP. Use o protocolo de sistema de mensagens MQTT para enviar solicitações para outros recursos de gerenciamento de dispositivo de gateway e controle.
- Porta 443, a porta SSL padrão, também pode ser especificada para proteger as chamadas API HTTP.
- Se um gateway não estiver designado à função *Gateway padrão*, ele poderá publicar eventos em nome de qualquer dispositivo na organização. Se o dispositivo que está conectado ao gateway não estiver registrado, o gateway registrará esse dispositivo automaticamente.
- Designe a função *Gateway padrão* se você desejar verificar os níveis de autorização do dispositivo.

Para obter mais informações sobre a função de gateways e grupos de recursos, consulte [Controle de acesso ao gateway](../gateways/gateway-access-control.html).

### Authentication

Todas as solicitações devem incluir um cabeçalho de autorização. Autenticação Básica é o único método suportado. Quando um dispositivo faz uma solicitação de HTTP usando a API de REST HTTP do {{site.data.keyword.iot_short_notm}}, as credenciais a seguir são necessárias:

|Credencial|Entrada requerida|
|:---|:---|
|User name| `g/{orgId}/{gwType}/{gwDevId}` ou `g-{orgId}-{gwType}-{gwDevId}`
|Password| O token de autenticação que era gerado automaticamente ou especificado manualmente quando você registrava o dispositivo de gateway.

Em que:

<dl>
<dt>orgId</dt>  
<dd>O nome da organização, que deve corresponder ao nome especificado no cabeçalho do host.</dd>

<p></p>
<dt>gwType</dt>  
<dd>O tipo de gateway. </dd>
<dd>Se você usar o caractere hífen "-" como um delimitador no nome de usuário, esse valor não deverá incluir um caractere de hífen. </dd>
<p></p>
<dt>gwDevId</dt>  
<dd>O identificador de dispositivo de gateway. </dd>
</dl>


### Cabeçalhos de solicitação Content-Type

Uma solicitação `Content-Type` deve ser fornecida com a solicitação se o conteúdo não é JSON. A tabela a seguir mostra como os tipos suportados são mapeados para os formatos internos do {{site.data.keyword.iot_short_notm}}:

|cabeçalho Content-Type|Formato no {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|texto/simples|"texto"
|aplicativo/json| "json"
|aplicativo/xml | "xml"
|application/octet-stream|"bin"

### Qualidade de Serviço

Semelhante ao nível 0 de serviço de entrega de qualidade de serviço MQTT "no máximo uma vez", o sistema de mensagens REST HTTP fornece entrega de mensagem não persistente, mas valida se a solicitação está correta e se pode ser entregue ao servidor antes de enviar a resposta HTTP. Uma resposta que contém um código de status HTTP 200 confirma que a mensagem foi entregue ao servidor. Ao usar o nível de qualidade de serviço MQTT "no máximo uma vez" ou o equivalente de HTTP (Protocolo de Transporte de Hipertexto) para entregar mensagens de eventos, o dispositivo ou o aplicativo deve implementar a lógica de nova tentativa para garantir a entrega.

Para obter mais informações sobre o protocolo MQTT e os níveis de qualidade de serviço para o {{site.data.keyword.iot_short_notm}}, consulte [Sistema de mensagens MQTT](../reference/mqtt/index.html).

Para obter mais informações sobre como gerenciar dispositivos de gateway usando APIs, veja [APIs de REST HTTP para dispositivos de gateway](../gateways/gw_api.html).

## Recebendo comandos
{: #receive_commands}

Além de usar o protocolo de sistema de mensagens MQTT, também é possível configurar seus dispositivos de gateway para receber comandos do {{site.data.keyword.iot_short_notm}} sobre HTTP usando comandos da API do sistema de mensagens HTTP. Um dispositivo de gateway pode receber comandos que são direcionados para dispositivos dentro de seu grupo de recursos associado. Para obter mais informações sobre grupos de recursos de gateway, consulte [Controle de acesso ao gateway](../gateways/gateway-access-control.html).

Use uma das URLs a seguir para enviar uma solicitação de ``POST`` de um gateway que está conectado ao {{site.data.keyword.iot_short_notm}}:

### Solicitação de POST não segura para comandos de recebimento

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Solicitação de POST segura para comandos de recebimento

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
