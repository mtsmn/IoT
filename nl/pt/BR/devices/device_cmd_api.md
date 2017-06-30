---

copyright:
 years: 2015, 2017
lastupdated: "2017-06-08"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# APIs do sistema de mensagens HTTP para dispositivos (beta)
{: #api}

**Importante:** o recurso API de sistema de mensagens HTTP do {{site.data.keyword.iot_full}} para dispositivos está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.


## Acessando a documentação da API de sistema de mensagens HTTP para dispositivos
{: #rest_messaging_api}

Para acessar a documentação da API de sistema de mensagens HTTP para dispositivos do {{site.data.keyword.iot_short_notm}}, veja [API de sistema de mensagens HTTP do {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}.


## Conexões do cliente
{: #client_connections}

Para obter informações sobre segurança do cliente e como conectar clientes a dispositivos no {{site.data.keyword.iot_short_notm}}, veja [Conectando aplicativos, dispositivos e gateways ao {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html).

## Publicando eventos
{: #event_publication}

Além de usar o protocolo de sistema de mensagens MQTT, também é possível configurar seus dispositivos para publicar eventos no {{site.data.keyword.iot_short_notm}} por HTTP (Protocolo de Transporte de Hipertexto) usando comandos da API (interface de programação de aplicativos) REST HTTP (Protocolo de Transporte de Hipertexto).

Use uma das URLs (Localizadores Uniformes de Recursos) a seguir para enviar uma solicitação de `POST` de um dispositivo que está conectado ao {{site.data.keyword.iot_short_notm}}:

### Solicitação de POST não segura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### Solicitação de POST segura

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**Nota:** porta 443, a porta SSL padrão, também pode ser especificada para proteger as chamadas API HTTP.

### Authentication

Todas as solicitações devem incluir um cabeçalho de autorização. Autenticação Básica é o único método suportado. Quando um dispositivo faz qualquer solicitação de HTTP (Protocolo de Transporte de Hipertexto) por meio da API (interface de programação de aplicativos) REST HTTP (Protocolo de Transporte de Hipertexto) do {{site.data.keyword.iot_short_notm}}, as credenciais a seguir são necessárias:

|Credencial|Entrada requerida|
|:---|:---|
|User name|`use-token-auth`
|Password| O token de autenticação que foi gerado automaticamente ou manualmente especificado quando você registrou o dispositivo.


### Cabeçalhos de solicitação Content-Type

Um cabeçalho de solicitação `Content-Type` deve ser fornecido com a solicitação. A tabela a seguir mostra como os tipos suportados são mapeados para os formatos internos do{{site.data.keyword.iot_short_notm}}.

|cabeçalho Content-Type|Formato no {{site.data.keyword.iot_short_notm}}|
|:---|:---|
|texto/simples|"texto"
|aplicativo/json| "json"
|aplicativo/xml | "xml"
|application/octet-stream|"bin"


## Recebendo comandos
{: #receive_commands}

Além de usar o protocolo de sistema de mensagens MQTT, também é possível configurar seus dispositivos para receber comandos do {{site.data.keyword.iot_short_notm}} sobre HTTP usando comandos da API de sistema de mensagens HTTP. Um dispositivo pode receber comandos que são direcionados a si mesmo.

Use uma das URLs (Localizadores Uniformes de Recursos) a seguir para enviar uma solicitação de `POST` de um dispositivo que está conectado ao {{site.data.keyword.iot_short_notm}}:

### Solicitação de POST não segura
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### Solicitação de POST segura

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**Nota:** porta 443, a porta SSL padrão, também pode ser especificada para proteger as chamadas API HTTP.

Para receber um comando do {{site.data.keyword.iot_short_notm}}, use a API a seguir:

<pre class="pre"><code class="hljs">/device/types/{deviceType}/devices/{deviceId}/commands/{command}/request</code></pre>

É possível incluir opcionalmente o parâmetro *waitTimeSecs* no corpo da solicitação de HTTP para especificar um número inteiro que represente o número máximo de segundos a aguardar por um comando:
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**Notas importantes:**
- O valor de *waitTimeSecs* deve ser um número inteiro no intervalo de 0 a 3600 segundos. O valor
padrão é 0.
- Para receber qualquer comando, use "qualquer" caractere curinga (+) para o componente {command}. Se o caractere curinga for usado, o identificador de comando estará contido no campo de cabeçalho de resposta *X-commandId*.
- Se o código de status de resposta de HTTP for 200, os dados do comando estarão contidos no corpo da resposta. Revise o campo de cabeçalho de resposta *Content-Type* para localizar o tipo de conteúdo.
- Se o código de status de resposta de HTTP for 204, nenhum dado de comando estará disponível.


Para obter informações sobre desenvolvimento de dispositivos, veja [Desenvolvendo dispositivos no {{site.data.keyword.iot_short_notm}}](../devices/device_dev_index.html).
