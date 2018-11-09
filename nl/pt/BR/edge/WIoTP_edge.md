---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}}  Visão Geral do Edge (Visualização)
{: #edge_overview}

**Importante:** o recurso {{site.data.keyword.iot_full}} Edge está disponível apenas como parte de um programa de visualização limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

O {{site.data.keyword.iot_short_notm}} Edge permite estender os recursos do {{site.data.keyword.iot_short_notm}} para os dispositivos Edge. Os dispositivos Edge residem na borda de uma rede dentro de uma organização. Exemplos incluem sensores e controladores industriais em um local físico, como uma fábrica. Com o {{site.data.keyword.iot_short_notm}} Edge, os dados podem ser processados dentro dos dispositivos antes de serem enviados para a nuvem.

Para usar o {{site.data.keyword.iot_short_notm}} Edge, você cria os nós Edge e configura esses nós com os serviços que devem ser executados dentro deles. O WIoTP Edge então implementa esses serviços para os nós. Com os serviços Edge em execução neles, os nós Edge podem enviar mensagens de dispositivos com o gateway Edge. O gateway processa as mensagens, transforma os dados e, em seguida, pode enviar os dados para a nuvem, dependendo de como as interfaces são configuradas.

A visualização do {{site.data.keyword.iot_short_notm}} fornece o serviço padrão Core IoT, juntamente com a capacidade de criar serviços customizados, como um serviço para prever falhas em um robô de chão de fábrica e enviar notificações de falha. Um exemplo de dispositivo de borda é um sensor que monitora o uso da CPU e envia um alerta quando o uso excede uma determinada porcentagem.

## {{site.data.keyword.iot_short_notm}}  Componentes do Edge

**Nó Edge**
Um nó Edge consiste em um gateway e em um dispositivo que reside na borda da rede.

**Dispositivo Edge**
Um dispositivo, tal como um sensor, o reside na borda de uma rede e executa serviços que processam dados antes de serem enviados para a nuvem. Um exemplo de dispositivo de borda é um sensor que monitora o uso da CPU e envia um alerta quando o uso excede uma determinada porcentagem.

**Gateway Edge**
Os gateways são dispositivos especializados que têm os recursos combinados de um aplicativo e um dispositivo, o que permite que eles sirvam pontos de acesso para outros dispositivos. Quando o Edge for ativado, os gateways Edge ativarão dispositivos que não puderem se conectar diretamente com a Internet para acessar o serviço {{site.data.keyword.iot_short_notm}} primeiro conectando-se ao dispositivo de gateway no Edge.

**Tipo de gateway Edge**
Um tipo de gateway agrupa dispositivos de gateway que compartilham atributos, como o número do modelo ou o local. Quando os recursos do Edge são ativados e um dispositivo de gateway de borda é incluído no {{site.data.keyword.iot_short_notm}}, os atributos do tipo de gateway de borda são usados como um modelo para o novo dispositivo de gateway de borda.

**Serviços Edge**
Os serviços consistem em recursos que são incluídos em um gateway {{site.data.keyword.iot_short_notm}} Edge e executam processos, como a manipulação de dados. Você constrói aplicativos que usam os serviços.

**Catálogo de Serviços Edge**
O serviço padrão Core IoT está incluído no catálogo e incluído em todos os nós Edge. É possível incluir serviços customizados com base em suas necessidades de negócios. O Catálogo de Serviços Edge contém todos os serviços customizados disponíveis.

**Repositório do Servidor de Arquivos**
O Repositório do Servidor de Arquivos mantém contêineres de docker e imagens. Todos os recursos, ou serviços, de processamento do Edge trabalham dentro de contêineres de docker. Você seleciona as imagens do docker a serem executadas dentro dos dispositivos.

## Localizando Mais Informações
{: #more_info}

Para obter mais informações sobre como configurar, instalar e desenvolver para o {{site.data.keyword.iot_short_notm}} Edge, veja os tópicos a seguir:
 - [ Configurando o  {{site.data.keyword.iot_short_notm}}  Edge ](WIoTP_edge_config.html#edge_configure)
 - [ Instalando  {{site.data.keyword.iot_short_notm}}  Edge em Dispositivos de Borda ](WIoTP_edge_install.html#edge_install_device)
 - [ Desenvolvendo para  {{site.data.keyword.iot_short_notm}}  Edge ](WIoTP_edge_dev.html#edge_dev)
