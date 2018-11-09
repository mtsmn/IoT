---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guias de introdução do {{site.data.keyword.iot_short_notm}}
{: #getting-started}

Os guias de Introdução são projetados para conduzir você pelos princípios básicos
de desenvolvimento de um sistema de protótipo IoT de ponta a ponta pronto para produção
com o {{site.data.keyword.iot_short_notm}} e são escritos para desenvolvedores
que são novos no trabalho com o {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

Cada guia fornece um processo passo a passo que rapidamente leva você para uma
solução implementada e em execução que demonstra um ou mais recursos do
{{site.data.keyword.iot_short_notm}}.

## Usando os guias  
{: #using-guides}
Em cada guia, você obtém pelo menos uma amostra originada no GitHub que é codificada de
acordo com as melhores práticas do {{site.data.keyword.iot_short_notm}}. Essas
amostras podem ser escaladas, protegidas, integradas com sistemas e geralmente podem se
ajustar a vários processos devOps de desenvolvimento e construção.

A tendência pode expandir tecnicamente a solução rápida ao adaptar o código de
amostra para seu próprio ambiente ou por complementar a solução rápida com exemplos de
hardware que mais de perto simula o que você pode encontrar em seu ambiente de produção. Os
guias fornecem links adicionais para a documentação de tarefas relevantes, os
documentos de API, bibliotecas do cliente (SDKs), vídeos instrutivos e outros materiais
de treinamento.

É possível seguir os guias na ordem, no qual o primeiro deles fornece uma base sobre a
qual os guias seguintes são construídos. Se você quer percorrer e seguir seu próprio
caminho pela série, cada guia também é fornecido com uma lista de requisitos. Por
exemplo, se você já tiver uma organização do {{site.data.keyword.iot_short_notm}}
configurada, poderá ir direto para os guias número dois e três e começar a explorar
regras, ações e um aplicativo de monitoramento de amostra. Os requisitos indicam o
formato dos dados do dispositivo que esses guias requerem e você pode configurar seu
próprio ambiente adequadamente.
{: tip}

## Lista de guias
{: #list-of-guides}  

Os guias a seguir estão disponíveis:

| Guia | Descrição |    
| ----- | ---- |   
| [1. Conectando um dispositivo de esteira
transportadora](getting-started-iot-conveyor.html) | Comece instalando uma organização do {{site.data.keyword.iot_short_notm}} e, em seguida, conecte um aplicativo de simulação de esteira transportadora originado pelo GitHub de amostra ou, se você preferir algo mais físico, seu próprio simulador de esteira transportadora baseado no Raspberry-Pi. </br> Em seguida, configure alguns cartões do painel do {{site.data.keyword.iot_short_notm}} para visualizar e monitorar os dados do dispositivo. | 
| [2. Usando regras e ações básicas em tempo
real](getting-started-iot-rules.html) | Com um ou mais dispositivos conectados à sua organização do {{site.data.keyword.iot_short_notm}} e enviando dados, agora é possível configurar regras de negócios e disparar ações para, por exemplo, notificar um operador da fábrica se a velocidade da sua esteira transportadora cair abaixo de um determinado limite.
| [3. Monitorando seus dados do dispositivo](getting-started-iot-monitoring.html) | Expanda
os painéis integrados de monitoramento de dispositivo do {{site.data.keyword.iot_short_notm}} conectando um aplicativo Node.js ou de monitoramento baseado na biblioteca de widgets para ver dados da esteira transportadora em tempo real.  
| [4. Simulando um grande número de dispositivos](getting-started-iot-large-scale-simulation.html) | Expanda
a simulação de dispositivo simples, incluindo grandes números de simuladores de
dispositivos em seu ambiente para testar a análise e o monitoramento dos guias
anteriores em um ambiente mais realista, com vários dispositivos. </br>**Importante:** o aplicativo requer 512 MB de memória, que é mais do
que é alocada por padrão e que também excede a quantidade disponível para contas de avaliação grátis, que devem primeiro ser atualizadas para uma conta de Assinatura ou de Pagamento conforme o uso. |   
