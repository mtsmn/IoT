---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configurando o  {{site.data.keyword.iot_short_notm}}  Edge (Preview)
{: #edge_configure}

**Importante:** o recurso {{site.data.keyword.iot_full}} Edge está disponível apenas como parte de um programa de visualização limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Antes de poder começar a receber dados de dispositivos que estão conectados ao nó Edge, deve-se conectar o gateway ao {{site.data.keyword.iot_short_notm}}. Conectar um gateway ao {{site.data.keyword.iot_short_notm}} envolve a criação de um tipo de dispositivo de gateway e o registro do gateway com o {{site.data.keyword.iot_short_notm}}. Em seguida, é possível usar as informações de registro para conectar o dispositivo de gateway ao {{site.data.keyword.iot_short_notm}}.

## Fluxo de configuração de alto nível

1. [Crie uma organização](#edge_org) dentro do ambiente de teste da visualização {{site.data.keyword.iot_short_notm}} Edge.
2. [Ative a visualização {{site.data.keyword.iot_short_notm}}](#edge_enable).
3. Configure sua organização para usar os nós de gateway Edge com o {{site.data.keyword.iot_short_notm}} [criando um tipo de gateway com o Edge ativado](#edge_gw_type), [incluindo dispositivos de gateway](#edge_gw) e selecionando os serviços Edge a serem executados em seus nós Edge. É possível [criar serviços customizados](WIoTP_edge_dev.html#create_service), se necessário.
4. [ Configure os serviços Edge ](#config_service).
5. [ Instale o Edge em seus dispositivos ](#edge_install_device).
6. [ Gerencie e monitore os serviços Edge ](#monitor_service).

## Criar uma organização
{: #edge_org}

Crie uma organização dentro do ambiente de teste da visualização {{site.data.keyword.iot_short_notm}} Edge, concluindo as etapas a seguir:
1. Registre-se para uma conta do {{site.data.keyword.Bluemix_notm}} e crie uma instância do serviço {{site.data.keyword.iot_short_notm}} em sua organização do {{site.data.keyword.Bluemix_notm}}. É possível criar uma instância do {{site.data.keyword.iot_short_notm}} diretamente na página [{{site.data.keyword.iot_short_notm}} no Catálogo de serviços do IBM Cloud ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}.  
2. Conclua as informações para provisionar a organização do IBM Cloud. **Importante:** deve-se implementar sua organização do IBM Cloud no local **Sul dos EUA** para usar a visualização {{site.data.keyword.iot_short_notm}} Edge.
3. Clique em **Criar**.
4. Na instância do {{site.data.keyword.iot_short_notm}} que é aberta, clique em **Ativar**.
Uma nova organização  {{site.data.keyword.iot_short_notm}}  é criada. O ID da organização é mostrado no início da URL e no painel sob seu nome do usuário. Agora é possível ativar o {{site.data.keyword.iot_short_notm}} Edge para a organização.

## Ativar a visualização  {{site.data.keyword.iot_short_notm}}  Edge Preview
{: #edge_enable}

A visualização {{site.data.keyword.iot_short_notm}} Edge está desativada por padrão. Para ativar o  {{site.data.keyword.iot_short_notm}}  Edge:

1. No painel do {{site.data.keyword.iot_short_notm}} para sua organização, selecione **Configurações**.
2. Na seção **Recursos experimentais**, ative os recursos experimentais, incluindo a visualização {{site.data.keyword.iot_short_notm}} Edge.
3. Atualize seu navegador para visualizar o novo recurso **Serviços Edge** e o atalho no menu de navegação do painel. **Nota:** se o ícone **Serviços Edge** não aparecer no menu de navegação do painel, assegure-se de que você tenha selecionado **Sul dos EUA** para seu local.

A sinalização é incluída no armazenamento do navegador local.

## Criar um tipo de gateway com recursos do Edge ativados
{: #edge_gw_type}

Cada gateway que é conectado ao {{site.data.keyword.iot_short_notm}} deve ser associado a um tipo de gateway. Os tipos de gateway são grupos de dispositivos que compartilham características comuns.

1. No menu de navegação principal do painel do {{site.data.keyword.iot_short_notm}}, selecione **Dispositivos**.
2. Na guia **Tipos de dispositivo**, clique em **Incluir tipo de dispositivo**.
3. Na guia **Identidade** da página **Incluir tipo**, selecione o tipo **Gateway** e insira um nome do tipo de gateway, como `my_gateway_type` e uma descrição para o tipo de gateway.
**Importante:** o nome do tipo de gateway não deve ter mais que 36 caracteres e pode conter somente:
<ul>
 <li>Caracteres alfanuméricos (a-z, A-Z, 0-9)</li>
 <li>Hifens (-)</li>
 <li>Sublinhados (&lowbar;)</li>
 <li>Pontos (.)</li>
 </ul>
3. Opcional: insira atributos e metadados de tipo de gateway.
4. Ative o botão  ** Edge Capabilities ** .
5. Selecione o tipo de arquitetura para o tipo de gateway. O tipo de arquitetura deve corresponder à arquitetura de seu dispositivo. Por exemplo, um dispositivo Odroid é executado na arquitetura ARM64.
6. Clique em **Avançar**.
7. Opcional: na guia **Informações sobre o dispositivo**, insira detalhes adicionais e metadados relacionados ao tipo de dispositivo de gateway.
8. Selecione os serviços Edge a serem incluídos em seus dispositivos Edge. O serviço Core IoT é incluído em todos os dispositivos, mas é possível incluir outros serviços do catálogo, concluindo as etapas a seguir:
 1. Clique em  ** Incluir Serviços **.
 2. Na janela **Incluir serviços do Edge**, selecione a versão apropriada dos serviços que você deseja incluir e, em seguida, clique em **Concluído**.
9. Na janela **Incluir tipo de gateway**, clique em **Pronto**.
É possível continuar para [incluir dispositivos de gateway para esse tipo de gateway](#edge_gw).

## Incluindo e Dispositivos de Gateway
{: #edge_gw}

1. No menu de navegação principal do painel do {{site.data.keyword.iot_short_notm}}, selecione **Dispositivos**.
2. Clique em **Incluir dispositivo**.
3. Na guia **Identidade**, selecione o tipo de dispositivo de gateway no qual você está incluindo dispositivos e insira um ID exclusivo para o dispositivo de gateway que está sendo incluído.
O ID do dispositivo é usado para identificar o dispositivo de gateway no painel do {{site.data.keyword.iot_short_notm}} e também é um parâmetro necessário para conectar seu dispositivo de gateway ao {{site.data.keyword.iot_short_notm}}.
**Importante:** O ID do dispositivo deve ter no máximo 36 caracteres e pode conter apenas:
 <ul>
 <li>Caracteres alfanuméricos (a-z, A-Z, 0-9)</li>
 <li>Hifens (-)</li>
 <li>Sublinhados (&lowbar;)</li>
 <li>Pontos (.)</li>
 </ul>
 **Dica:** para dispositivos conectados à rede, o ID do dispositivo poderia, por exemplo, ser o endereço de Controle de Acesso à Mídia do dispositivo sem dois pontos separando.
4. Clique em **Avançar**.
5. Opcional: na guia **Informações sobre o dispositivo**, insira detalhes adicionais e metadados relacionados ao dispositivo de gateway.
6. Clique em **Avançar**.
7. Na guia **Segurança** da página **Incluir dispositivo**, gere automaticamente um token de autenticação ou forneça seu próprio token de autenticação para esse dispositivo. Se você optar por criar seu próprio token, certifique-se de que tenha entre oito e 36 caracteres de comprimento, contenha uma combinação de letras minúsculas e maiúsculas, números e hífen, sublinhado ou ponto. O token não deve conter sequências de caracteres repetidos, palavras de dicionário, nomes de usuário ou outras sequências predefinidas.
9. Clique em **Avançar**.
10. Clique em **Pronto**.
11. Na página de informações sobre o dispositivo, copie e salve as informações sobre o dispositivo a seguir:
 - ID da organização, como `tubo8x`
 - Tipo de dispositivo, como `my_gateway_type`
 - ID do dispositivo. **Dica:** para dispositivos conectados à rede, isso poderia por exemplo ser o endereço de Controle de Acesso à Mídia (MAC) sem dois pontos separando.
 - Método de autenticação, como `token`
 - Token de autenticação, como `PtBVriRqIg4uh)_-Kl`
Use essas informações para conectar o gateway ao {{site.data.keyword.iot_short_notm}} e iniciar o recebimento de dados de dispositivos que estão conectados ao gateway.

## Configurando Serviços de Edge
{: #config_service}

Depois de criar um novo tipo de dispositivo com os recursos do Edge ativados, é possível selecionar diferentes serviços para serem instalados em seu nó Edge.

1. No menu de navegação principal do painel do {{site.data.keyword.iot_short_notm}}, selecione **Dispositivos**.
2. Clique em  ** Tipos de Dispositivo **.
3. Selecione o tipo de dispositivo Edge que você deseja configurar.
4. Na guia **Serviços Edge**, selecione o ícone de lápis para editar o registro.
6. Clique em **Incluir serviços Edge** para ver uma lista de serviços que foram transferidos por upload para essa organização.
7. Na janela **Incluir serviços do Edge**, selecione a versão apropriada dos serviços que você deseja incluir e, em seguida, clique em **Concluído**.
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## Gerenciando e monitorando serviços Edge
{: #monitor_service}

Após os nós Edge serem instalados, o {{site.data.keyword.iot_short_notm}} permite monitorar o status de serviços que estão em execução em nós Edge.

1.	No menu de navegação principal do painel do {{site.data.keyword.iot_short_notm}}, selecione **Dispositivos**.
2.	Selecione o dispositivo correspondente ao seu nó Edge
3.	Clique em **Serviços Edge** para ver o status dos serviços que estão instalados no nó Edge.

## Localizando Mais Informações
{: #more_info}

Para obter informações sobre como conectar seu gateway ao {{site.data.keyword.iot_short_notm}}, consulte [Conectividade MQTT para gateways](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt).

Para obter informações mais detalhadas sobre o {{site.data.keyword.iot_short_notm}}, veja [Introdução ao Watson IoT Platform](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp).

Para obter mais informações sobre o {{site.data.keyword.iot_short_notm}} Edge, consulte os tópicos a seguir:
- [ {{site.data.keyword.iot_short_notm}}  Visão Geral do Edge ](WIoTP_edge.html#edge_overview)
- [ Instalando  {{site.data.keyword.iot_short_notm}}  Edge em Dispositivos de Borda ](WIoTP_edge_install.html#edge_install_device)
- [ Desenvolvendo para  {{site.data.keyword.iot_short_notm}}  Edge ](WIoTP_edge_dev.html#edge_dev)
