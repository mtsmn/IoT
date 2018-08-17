---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introdução ao {{site.data.keyword.iot_short_notm}}
{: #gettingstartedtemplate}

O {{site.data.keyword.iot_full}} for {{site.data.keyword.Bluemix_notm}} fornece um kit de ferramentas versátil que inclui dispositivos de gateway, gerenciamento de dispositivo e acesso ao aplicativo poderoso. Usando o {{site.data.keyword.iot_short_notm}}, é possível coletar dados do dispositivo conectados e executar analítica em dados em tempo real a partir de sua organização.
{:shortdesc}

## Antes de iniciar
{: #byb}

Antes de conectar dispositivos e usar dados, registre-se para uma conta do {{site.data.keyword.Bluemix_notm}} e crie uma instância do serviço {{site.data.keyword.iot_short_notm}} em sua organização do {{site.data.keyword.Bluemix_notm}}. É possível criar uma instância do {{site.data.keyword.iot_short_notm}} diretamente na página [{{site.data.keyword.iot_short_notm}} no Catálogo de serviços do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}.  

Para obter informações detalhadas sobre como inscrever-se para uma conta no {{site.data.keyword.Bluemix_notm}}, configurar regiões e outras configurações de gerenciamento de conta, veja [Gerenciando sua conta do IBM Cloud](https://console.ng.bluemix.net/docs/admin/account.html#signup).

É possível instalar e configurar sua instância do {{site.data.keyword.iot_short_notm}} no painel. Para abrir o painel, acesse sua instância de serviço do {{site.data.keyword.iot_short_notm}} no {{site.data.keyword.Bluemix_notm}} e, em seguida, clique em **Ativar**.

## Sobre essa Tarefa

As etapas a seguir descrevem como você pode começar a usar rapidamente seu serviço do {{site.data.keyword.iot_short_notm}}.

Um conjunto mais detalhado de guias de introdução e aplicativos de amostra que passam pelos fundamentos do desenvolvimento de um sistema de protótipo IoT de ponta a ponta pronto para produção com o {{site.data.keyword.iot_short_notm}} também estão disponíveis. Se você for um desenvolvedor novo no trabalho com {{site.data.keyword.iot_short_notm}}, use os processos passo a passo na seção [Guias de introdução](https://console.bluemix.net/docs/services/IoT/getting_started/getting-started-iot-overview.html#getting-started).

## Etapa 1: conectar seus dispositivos
{: #up_and_running}

Para colocar o serviço em funcionamento, explore as opções a seguir, dependendo de sua situação:

|  |   O serviço é implementado | O serviço não é implementado
 | -------------| ------------- | -------------
  |**Eu tenho um dispositivo para conectar** | [Conecte seu dispositivo ao {{site.data.keyword.iot_short_notm}}](iotplatform_task.html#iotplatform_task).| Explore a conexão de dispositivo no [Play
com o {{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play){:new_window}.
  |**Eu não tenho um dispositivo para conectar** | [Crie e conecte um simulador de dispositivo Node-RED](nodereddevice_sample.html){:new_window}. Ou [Conectar
seu smartphone ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play/device/smartphone){:new_window}. | Introdução ao [Watson IoT Platform Starter](https://console.bluemix.net/docs/starters/IoT-starter/iot500.html).
  
Para obter mais informações sobre como conectar tipos de dispositivo específicos ao {{site.data.keyword.iot_short_notm}}, veja [Orientações do developerWorks ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){:new_window}.  

Para obter a documentação para desenvolvedor de conexão de dispositivo, consulte:
- [Conectividade MQTT para dispositivos](devices/mqtt.html).
- [Conectividade MQTT para gateways](gateways/mqtt.html).

<!--
## Step 2: Analyze your device data
{: #analyzing_data}
Start exploring the real-time data that the devices are sending to {{site.data.keyword.iot_short_notm}}.
{{site.data.keyword.iot_short_notm}} includes the following analytics tools:  
- [Boards and cards](data_visualization.html) to visualize your real-time device data.
- [Rules and actions](analytics.html) that are triggered by real-time device data.
For a quick getting started example, see the [Using Rules and Actions with IBM Watson IoT Platform Cloud Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/using-rules-and-actions-with-ibm-watson-iot-platform-cloud-analytics/){:new_window} developerWorks recipe.
-->

## Etapa 2: criar aplicativos para consumir seus dados do dispositivo
{: #develop_applications}

Crie e conecte seus próprios aplicativos para consumir dados do dispositivo.

Para obter informações adicionais, consulte os
seguintes tópicos:   
- Explore a [documentação do desenvolvedor de aplicativos](applications/api.html) e a [Documentação da API do {{site.data.keyword.iot_short_notm}}](reference/api.html).
- Explore as [Bibliotecas do cliente do {{site.data.keyword.iot_short_notm}}](iot_platform_client_lib.html) que fornecem ferramentas e arquivos para construir e desenvolver código para integração e conexão de seus dispositivos e aplicativos.
- [Conecte um serviço do {{site.data.keyword.cloudantfull}}](cloudant_connector.html) a seu {{site.data.keyword.iot_short_notm}} para armazenar dados históricos do dispositivo.
- Crie suas próprias regras usando o novo recurso de [regras integradas (Beta)](information_management/im_rules.html).
