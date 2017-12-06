---

copyright:
years: 2017
lastupdated: "2017-08-10"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introdução ao gerenciamento de dados usando a interface da web (Beta)
{: #gs_web}

**Importante:** o recurso de interface da web {{site.data.keyword.iot_full}} Data Management está disponível somente como parte de um programa beta limitado. Atualizações futuras podem incluir mudanças incompatíveis com a versão atual desse recurso. Experimente e [informe-nos o que acha ![Ícone de link externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

O {{site.data.keyword.iot_short_notm}} fornece ferramentas on-line para ajudar a normalizar e transformar dados como parte do recurso de gerenciamento de dados. Além da [API de gerenciamento de dados do Watson IoT Platform![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} que é fornecida, é possível usar o Criador de interface simples ou o Criador de interface avançada para configurar recursos e começar a mapear seus dados de dispositivo. Os criadores de interface on-line orientam você pelas etapas na interface com o usuário.

Para obter informações sobre o recurso de gerenciamento de dados e seus benefícios, consulte a [Introdução ao gerenciamento de dados](../GA_information_management/ga_im_device_twin.html#device_twins).

Para obter informações sobre os recursos que você precisa configurar, consulte [Entendendo o gerenciamento de dados](../GA_information_management/ga_im_definitions.html#definitions_resource).

Com o Criador de interface simples, você inclui uma interface física e uma
interface lógica é automaticamente criada e as duas são mapeadas juntas. Com o Criador de
interface avançada, você tem mais controle sobre a configuração. Você inclui uma
interface física e uma ou mais interfaces lógicas; em seguida, inclui mapeamentos entre
elas.

As interfaces e os mapeamentos são incluídos no formato de rascunho. Depois de
serem incluídos e validados, deve-se ativá-los. Para obter mais informações sobre recursos ativos e de rascunho, consulte [Criando, atualizando, ativando e desativando seus recursos](../GA_information_management/ga_im_definitions.html#draft_active_resources).



## Fluxo de alto nível
{: #interface_flow}

Use as etapas a seguir para ajudar a configurar os recursos necessários para começar a usar o recurso de gerenciamento de dados.

**Antes de iniciar**
Este procedimento presume que você tenha um tipo de dispositivo com pelo menos um dispositivo conectado e registrado. Para obter mais informações sobre esses pré-requisitos e para obter instruções sobre como criar um tipo de dispositivo e registrar um dispositivo, consulte [Configurando dispositivos para uso com o recurso de gerenciamento de dados](im_config_devices.html).

1. Selecione o tipo de dispositivo que você deseja normalizar e transformar dados.
2. Selecione o Criador de interface simples ou o Criador de interface avançada:

a. Fluxo simples  
   i. [Crie uma interface física de rascunho](#create_physical_interface_simple).  
   
b. Fluxo avançado  
   i. [Crie uma interface física de rascunho](#create_physical_interface_advanced).  
   ii. [Crie uma ou mais interfaces lógicas de rascunho](#create_logical_interface). (Esta etapa é concluída automaticamente quando você usa o fluxo simples.)  
   iii. [Mapeie a interface física para as interfaces lógicas](#create_interface_mappings).  
     
     
3. [Configurar preferências de notificação](#set_notifications).
4. [Ativar a configuração](#validate_activate).

*Nota:* Você também pode configurar o recurso de gerenciamento de dados usando as APIs. Para obter informações detalhadas sobre como configurar o recurso de gerenciamento de dados para normalizar e transformar seus dados de dispositivo usando as APIs, consulte [Introdução ao gerenciamento de dados]{ga_im_example.html#im_example}. O [Guia passo a passo: um exemplo detalhado sobre como trabalhar com dispositivos por meio de uma interface comum](../GA_information_management/ga_im_index_scenario.html#scenario) guia você pelas etapas de criação de uma interface lógica de tipo de dispositivo para dispositivos de termômetro heterogêneos.

Para obter detalhes sobre a API, consulte a documentação da [API de REST HTTP do {{site.data.keyword.iot_short_notm}}![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Criando uma interface física de rascunho (fluxo simples)
{: #create_physical_interface_simple}

1. No menu de navegação principal, selecione **Dispositivos**.
2. Selecione **Tipos de dispositivo** e selecione o tipo de dispositivo para o qual você deseja criar uma interface. Também é possível [criar um novo tipo de dispositivo].
3. Visualize e atualize as informações de tipo de dispositivo, conforme
necessário, e salve quaisquer mudanças.
4. Clique em **Interface**.
5. Clique em **Fluxo simples** para abrir o Criador de interface simples.
6. Clique em **Criar interface**.
7. Clique em **Incluir propriedade** para iniciar a
inclusão de eventos e propriedades na interface física.  
 a. Na janela **Incluir propriedades na interface**, selecione
os eventos que você deseja incluir na interface. O sistema atende aos eventos ativos para
dispositivos conectados do tipo de dispositivo selecionado. É possível selecionar o ID do
dispositivo do último evento de cache ou selecionar outro evento na lista.  
  b. Selecione as propriedades do evento e clique em **Incluir**.  
  c. Inclua mais propriedades conforme necessário.
8. Clique em **Pronto**. A interface física é criada. Se você
deseja incluir mais detalhes, pode fazer upgrade da interface selecionando o link
**Usar criador de interface avançada**.


## Criando uma interface física de rascunho (fluxo avançado)
{: #create_physical_interface_advanced}

1. No menu de navegação principal, selecione **Dispositivos**.
2. Selecione **Tipos de dispositivo** e selecione o tipo de dispositivo para o qual você deseja criar uma interface. Também é possível [criar um novo tipo de dispositivo].
2. Visualize as informações de tipo de dispositivo e clique em **Interface**.
4. Clique em **Fluxo avançado** para abrir o Criador de
interface avançada.
5. Clique em **Criar interface**
7. Na guia **Identidade** da página **Criar interface
física**, digite um nome e uma descrição da interface física.
7. Opcional: ligue o Edge Analytics se você estiver usando dispositivos ativados para Edge com a interface.
8. Clique em **Avançar**.
9. Defina a interface física, incluindo tipos de eventos e cargas úteis:
 a. Clique em **Criar tipo de evento**. 
 b. Selecione o último evento armazenado em cache, selecione um evento na lista ou
inclua um evento manualmente.
10. Clique em **Pronto**. A interface física é criada em
formato de rascunho. Você pode prosseguir para incluir uma ou mais interfaces lógicas.

## Criando uma interface lógica de rascunho
{: #create_logical_interface}

1. Depois de criar a interface física de rascunho, clique em **Incluir interface lógica**.
2. Na guia **Identidade** da página **Criar interface lógica**, digite um nome e uma descrição da interface lógica.
3. Clique em **Avançar**.
4. Clique em **Incluir propriedade** para começar a definir as propriedades da interface lógica.
5. Na janela **Criar propriedade**, selecione as propriedades da interface física que você deseja incluir na interface lógica e, em seguida, salve as mudanças.
6. Inclua interfaces lógicas adicionais conforme necessário e, em seguida, clique em **Avançar**.
7. {: #set_notifications}Configure as preferências de notificação para a interface. Selecione uma das opções a seguir para determinar quando deseja que notificações de estado do dispositivo sejam enviadas:
 - Nenhum notificador de evento - nenhuma notificação é enviada. É possível usar a
API de REST para recuperar informações de mudança de estado do dispositivo.
 - Na mudança de estado - você será notificado quando o estado do dispositivo mudar.
 - Em cada evento - você será notificado sempre que a plataforma processar um evento
para o dispositivo, mesmo que o evento não resulte em uma mudança de estado.
8. Clique em **Pronto**. As interfaces são criadas no formato de rascunho.
9. {: #validate_activate} Clique em **Implementar** para validar a configuração.
10. Clique em **Implementar** para ativar a configuração. O status de configuração é definido como implementado. 

