---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guia 3: simulando um grande número de dispositivos
No primeiro guia, você configurou um simulador de dispositivo básico para simular manualmente uma ou mais esteiras transportadoras. Neste guia, expandimos essa simulação, incluindo grandes números de simuladores de auto-execução em seu ambiente para testar a análise e o monitoramento em um ambiente mais realista, com vários dispositivos.
{:shortdesc}

**Importante:** o aplicativo requer 512 MB de memória, que é mais do que o alocado por padrão e que também excede a quantia disponível para contas de avaliação grátis, incluindo a Conta para teste e a Conta padrão do {{site.data.keyword.Bluemix}}. Os titulares das contas de assinatura e pagamento conforme o uso podem aumentar a memória alocada para 512 MB. Os titulares das contas de avaliação grátis precisam fazer upgrade para uma conta de Assinatura ou Pagamento conforme o uso. Para obter mais informações sobre os tipos de contas do {{site.data.keyword.Bluemix_notm}}, consulte [Tipos de contas](/docs/pricing/index.html#pricing).

** Nota: **  O Bluemix é agora IBM Cloud. Veja o [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo") para obter mais detalhes.

## Visão geral e objetivo
{: #overview}

Neste guia, você configurará um aplicativo {{site.data.keyword.Bluemix_notm}} para simular vários dispositivos de esteira transportadora usando um "backend" do [Node-RED ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://nodered.org){: new_window}.

O aplicativo contém três fluxos que:   
1. Criam diversos dispositivos.   
2. Simulam publicação de eventos simultâneos.
3. Excluem os dispositivos.   

![Criação e exclusão de dispositivo](images/device_creation.png "Criação e exclusão de dispositivo")

![Simulação de evento de dispositivo](images/device_event_simulation.png "Simulação de evento de dispositivo")

Ao concluir as instruções que estão neste guia, você:
- Usará o Cloud Foundry para implementar um aplicativo simulador de dispositivo baseado em Node-RED e ativado por webhook.
- Usará chamadas API para criar e registrar dispositivos, publicar eventos de dispositivo e excluir dispositivos.

**Importante:** a simulação de grandes números de dispositivos que simultaneamente enviam dados do dispositivo para o {{site.data.keyword.iot_full}} poderá usar uma grande quantidade de dados. 

## Pré-requisito
{: #prereqs}  
Você precisará das contas e ferramentas a seguir:

* [Conta do {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/registration/) com    
 - Mais de 512 MB de RAM   
 - Mais de 1 GB de cota do disco  
 - Dois serviços disponíveis
* [Cloud Foundry Command Line Interface (cf CLI) ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Use a CLI do cf para implementar e gerenciar seus aplicativos {{site.data.keyword.Bluemix_notm}}.
* Opcional: [Git ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://git-scm.com/downloads){: new_window}  
Se você escolhe usar o Git para fazer download das amostras de código, deve-se ter também uma [conta GitHub.com ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com){: new_window}. Também é possível fazer download do código como um arquivo compactado sem uma conta GitHub.com.

Para qualquer uma das chamadas REST nas etapas a seguir, é possível utilizar cURL
ou o plug-in de complemento do cliente REST que está disponível no Mozilla.  
{: tip}

## Etapa 1 - Implementar o aplicativo no {{site.data.keyword.Bluemix_notm}}
{: #step1}

Siga as etapas abaixo para criar e implementar seu aplicativo manualmente.   

1. Clone o repositório GitHub do aplicativo de amostra *Lesson4*.  
Use sua ferramenta git favorita para clonar o repositório a seguir:  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
Em Shell do Git, use o comando a seguir:
```bash
Clone de $git https://github.com/ibm-Watson-iot/guide-transporyor/tree/master/device-simulator
```
3. Configure o aplicativo para seu ambiente editando o arquivo manifest.yml.  
O que editar:
 - Para usar um serviço do {{site.data.keyword.iot_short_notm}} existente,
atualize todas as instâncias de `lesson4-simulate-iotf-service` para
refletir o nome do serviço. Por exemplo, se você estiver usando o serviço
{{site.data.keyword.iot_short_notm}} do guia 1, use
`iotp-for-conveyor` como nome do serviço.    
 - Configure o nome do dispositivo e o host.   
Na seção de aplicativos, mude as entradas `name` e
`host` para algo que seja exclusivo, como
`YOUR_NAME-lesson4-simulate`.   
**Dica:** a URL de ROTAS que você usa para acessar o aplicativo é criada na entrada `host`, por exemplo:
`https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plano: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
1. Na linha de comandos, configure seu terminal de API executando o comando cf api.   
Substitua o valor `API-ENDPOINT` pelo terminal de API para sua região.
   ```
cf api API-ENDPOINT
   ```
Exemplo: `cf api https://api.ng.bluemix.net`  
<table>
<tr>
<th>Região</th>
<th>Terminal de API</th>
</tr>
<tr>
<td>Sul dos Estados Unidos</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>Reino Unido</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. Efetue login em sua conta do {{site.data.keyword.Bluemix_notm}}.
  ```
cf login
  ```
Se solicitado, selecione a organização e o espaço em que você deseja implementar a {{site.data.keyword.iot_short_notm}} e o aplicativo de amostra.
6. Crie os serviços necessários no {{site.data.keyword.Bluemix_notm}}.   
 1. Use o comando a seguir para criar o serviço cloudantNoSQLDB Lite:
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. Use o comando a seguir para criar o serviço do {{site.data.keyword.iot_short_notm}}
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**Importante:** Se você estiver usando um serviço do
{{site.data.keyword.iot_short_notm}} existente e tiver atualizado o arquivo
manifest.yml apropriadamente, certifique-se de usar o nome do serviço existente. Por
exemplo, no guia 1, use `iotp-for-conveyor` em vez de
`lesson4-simulate-iotf-service` na chamada cf.
7. Execute o comando `cf push` para criar o projeto e enviar por push à sua organização.  
Seu aplicativo de amostra é implementado no {{site.data.keyword.Bluemix_notm}}.  
Quando a implementação é concluída, uma mensagem é exibida para indicar que seu app está em execução.   
Exemplo:  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. Obtenha as credenciais de serviço do seu aplicativo.
 1. Efetue login no {{site.data.keyword.Bluemix_notm}} em:  
 [https://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net){: new_window}.
 2. Selecione a conta e o espaço no qual você implementou o app.
 3. No menu, selecione **Apps** e, em seguida, selecione **Painel**.
 4. Em Aplicativos Cloud Foundry, clique no nome do aplicativo que você acabou de implementar.
 4. Selecione **Conexões**.
 5. Localize o serviço iotf que você está usando com seu aplicativo e clique em **Visualizar credenciais**.  
 6. Na página de credenciais de serviço, localize `apiKey` e `apiToken`.   
A chave API e o token de autenticação são usados como credenciais quando você usa a API
do aplicativo para criar e executar seus dispositivos simulados.

## Etapa 2 - Proteger seu fluxo do Node-RED
{: #step2}

Embora esta etapa seja opcional, é recomendável proteger seu fluxo do Node-RED.

1. Proteja seu editor para que apenas usuários autorizados possam acessá-lo.
2. Para proteger o fluxo do Node-RED, acesse o painel do
{{site.data.keyword.Bluemix_notm}} e selecione o aplicativo que você
implementou anteriormente.
3. Clique em **Tempo de execução** e, em seguida, clique em **Variáveis de ambiente**.
4. Inclua as seguintes variáveis de ambiente definidas pelo usuário e seus respectivos valores:
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. Salve o fluxo.

Quando o fluxo do Node-RED é seguro, se você planejar usar os comandos da API de REST
para criar, excluir ou simular vários dispositivos, deverá fornecer as credenciais de
nome do usuário e senha do Node-RED durante a autenticação básica.

## Etapa 3 - Criar e conectar dispositivos
{: #step3}

É possível usar a interface de fluxo do Node-RED ou a API de REST do aplicativo para concluir as tarefas a seguir.

### Criando e conectando dispositivos usando o Node-RED  
Para registrar múltiplos dispositivos:  
1. Efetue login no {{site.data.keyword.Bluemix_notm}} em:  
[https://bluemix.net ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net){: new_window}.
2. Selecione a conta e o espaço no qual você implementou o app.
3. No menu, selecione **Apps** e, em seguida, selecione **Painel**.
4. Em aplicativos Cloud Foundry, clique na URL de **ROTA** do aplicativo que você acabou de implementar.  
A ROUTE_URL é criada da entrada `host` que você usou no arquivo
manifest.yml: `HOST.mybluemix.net`  
Exemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
A interface Node-RED é aberta.
5. Clique em **Acessar o editor de fluxos do Node-RED**.
6. No editor de fluxo, selecione a guia **Tipo de dispositivo e instância**.
7. Para registrar cinco dispositivos, clique no nó de injeção rotulado **Registrar 5 dispositivos motorController**.

### Criando e conectando dispositivos usando a API de REST  
Para registrar múltiplos dispositivos:  

1. Faça uma solicitação HTTP POST para a URL a seguir: `ROUTE_URL/rest/devices`  
Exemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - Configure 'Content-Type' e 'Accept' para 'application/json'  
 - Use a carga útil JSON a seguir:  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
  Em que:  
    - numberDevices é o número de dispositivos a serem criados e registrados.
    - Opcional: typeId é o tipo de dispositivo como o qual os dispositivos serão registrados. Se
typeId não for fornecido, ele será padronizado como "iotp-starter para transporte". **Dica:**
É possível inserir qualquer nome de tipo de dispositivo, mas
os outros guias da série esperam dispositivos do tipo `iotp-for-conveyor`. Se
você usa um tipo de dispositivo diferente, deve-se modificar apropriadamente as
configurações nos guias.
    - Opcional: authToken é o token de autorização com o qual os dispositivos serão registrados.
    - Opcional: se chunkSize não for fornecido, será configurado como 500 por padrão. O 'chunksize' deve ser menor que e um fator de numberDevices.
    - deviceName é o padrão para o deviceID para os dispositivos criados.

## Etapa 4 - Simular eventos de dispositivo
{: #step4}

Como os dispositivos simulados são registrados no
{{site.data.keyword.iot_short_notm}}, agora é possível executar o simulador para
começar a enviar eventos de dispositivo.

### Simulando eventos de dispositivo usando Node-RED  
Para enviar eventos de dispositivo:  
1. No editor de fluxo do Node-RED, selecione a guia **Simular vários dispositivos**.
7. Para simular cinco dispositivos, clique no nó de injeção que está rotulado como
**Simular 5 dispositivos**.
 


### Simulando eventos de dispositivo usando a API de REST  
Para enviar eventos de dispositivo:

1. Faça uma solicitação HTTP POST para a URL a seguir: `ROUTE_URL/rest/runtest`  
Exemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - Configure 'Content-Type' e 'Accept' para 'application/json'  
 - Use a carga útil JSON a seguir:   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
Em que:
    - numberDevices é o número de dispositivos para os quais você deseja simular dados.
    - numberEvents é o número de eventos que cada dispositivo simulado envia.
    - timeInterval é o espaçamento dos eventos em milissegundos.
    - deviceType é o tipo de dispositivo para o qual você criou dispositivos simulados.
    - deviceName é o padrão para o deviceID para os dispositivos criados.
 

## Etapa 5 - Excluindo dispositivos
{: #deleting}

### Node-RED  
Para excluir dispositivos:  
1. No editor de fluxo do Node-RED, selecione a guia **Tipo de dispositivo e instância**.
2. Para excluir cinco dispositivos, clique no nó de injeção que está rotulado como **Excluir 5 dispositivos**.



### API Rest  

1. Faça uma solicitação HTTP POST para a URL a seguir: `ROUTE_URL/rest/deleteDevices`  
Exemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - Configure 'Content-Type' e 'Accept' para 'application/json'  
 - Use a carga útil JSON a seguir:      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName":"belt"  
}</code></pre>
  


## O que Vem a Seguir?
{: @whats_next}  
Vá para outro tópico do seu interesse:
- [ Guia 2: Monitorando os dados do dispositivo ](getting-started-iot-monitoring.html)  
Agora que você conectou um ou mais dispositivos e começou a fazer bom uso dos dados do dispositivo, é hora de começar a monitorar uma coleção de dispositivos.
- [Saiba mais sobre a {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Saiba mais sobre APIs de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/reference/api.html){:new_window}
