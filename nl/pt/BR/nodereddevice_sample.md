---

copyright:
  years: 2016, 2018
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Criando e conectando um simulador de dispositivo Node-RED
Use o Node-RED para criar um simulador de dispositivo para enviar dados do dispositivo simulado para sua organização {{site.data.keyword.iot_full}}  
{:shortdesc}

## Visão geral
Node-RED é uma ferramenta que é usada para ligar dispositivos de hardware, APIs e serviços on-line de maneiras novas e interessantes. Para obter mais informações, veja o website do [Node-RED ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://nodered.org/){: new_window}.  

É possível executar sua instância Node-RED em seu próprio ambiente ou usá-la como
um aplicativo {{site.data.keyword.Bluemix_notm}}. As etapas a seguir incluem instruções para o {{site.data.keyword.Bluemix_notm}}.

Crie e conecte o simulador de dispositivo Node-RED para enviar mensagens do dispositivo MQTT para o {{site.data.keyword.iot_short_notm}}. No exemplo a seguir, o simulador de dispositivo simula o envio de dados de um contêiner de frete para um broker MQTT, por exemplo, {{site.data.keyword.iot_short_notm}}.

## Etapas

1. Crie o simulador do dispositivo Node-RED concluindo as etapas a seguir:   
    1. Efetue login no {{site.data.keyword.Bluemix_notm}} em: https://console.ng.bluemix.net
    2. Selecione a guia **Catálogo**.
    3. Localize a seção Texto padrão do catálogo de serviços e clique em **Iniciador do Internet of Things Platform**. **Dica:** Clique [aqui ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window} para ir diretamente até a página Iniciador do Internet of Things Platform.
    4. Na página Iniciador do Internet of Things Platform, selecione o espaço no qual deseja implementar o Node-RED, verifique as seleções Criar um app e clique em **Criar** para incluir o Node-RED na sua organização Bluemix. Por exemplo:
    <ul>
     <li> Espaço: dev
     <li> Nome do app: myDevice
     <li> Nome do host: myDevice  
    </ul>  
Deixe as outras opções com os valores padrão. Após o aplicativo ser implementado, a página Iniciar codificação com o Node-RED é exibida.
**Nota:** o processo de preparação pode levar alguns minutos.  

2. Clique no link Rotas para abrir o Node-RED ou em visite a URL do app, por exemplo, `http://myDevice.mybluemix.net`  
3. Conclua as etapas a seguir para **Proteger seu editor do Node-RED**:
    1. Clique em **Avançar**
    2. Inclua um nome de usuário e senha.  
    **Nota:** Para a senha, deve-se atender às regras decomplexidade necessária; caso contrário, o botão **Avançar** permanecerá esmaecido.  
    3. Opcional. Marque **Permitir que qualquer pessoa visualize o editor, mas não faça nenhuma mudança** ou **Não recomendado: permitir que qualquer pessoa acesse o editor e faça mudanças**
    4. Clique em **Concluir** para concluir a configuração.
4. Clique em **Acessar o editor de fluxo do Node-RED**
5. Insira o nome de usuário e a senha que você criou anteriormente.  
O fluxo do simulador de dispositivo é disponibilizado ao editor de fluxo. O fluxo simula um **Termostato** que envia seus dados de localização, temperatura e umidade medidas para o {{site.data.keyword.iot_short_notm}}.  
6. Confirme se o dispositivo está conectado verificando se um ponto com a mensagem conectada é exibido para o nó **Enviar para o IBM IoT Platform**. A verificação também é válida para o nó **Entrada de app IBM IoT** para o subfluxo **Monitor de temperatura**.  
7. Envie e receba mensagens do dispositivo MQTT concluindo as etapas a seguir:  
    1. Clique no nó de injeção **Enviar dados** para acionar dados a serem enviados ao {{site.data.keyword.iot_short_notm}}.
       **Nota:** É possível ativar o nó de depuração **Depurar carga útil de saída** para ver quais dados são enviados e verificar o nó de função **Carga útil do dispositivo** para ver o código que constrói a carga útil. 
    2. Depois que você clica em **Enviar dados**, as mensagens MQTT são enviadas para o {{site.data.keyword.iot_short_notm}} e são recebidas pelo nó **Entrada de app IBM IoT**. O subfluxo **Monitor de temperatura** estabelece se a temperatura está dentro dos limites definidos e exibe uma mensagem na guia de depuração. 
       **Nota:** Clique no nó de comutação **limite de temperatura** para verificar e mudar os valores do limite
    3. Verifique a guia de depuração. Uma mensagem é exibida, por exemplo, **Temperatura (17) dentro dos limites de segurança**.
    
Agora você conectou seu dispositivo IoT de amostra ao {{site.data.keyword.iot_short_notm}} e pode ver os dados do dispositivo.
