---

copyright:
years: 2016, 2018
lastupdated: "2018-05-14"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introdução ao Data Management usando a interface da web
{: #gs_web}

O {{site.data.keyword.iot_full}} fornece ferramentas on-line para ajudar a normalizar e transformar dados como parte do recurso Data Management.

## Visão geral
{: #web_overview}

Além da [API de gerenciamento de dados do Watson IoT Platform![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} que é fornecida, é possível usar o Criador de interface simples ou o Criador de interface avançada para configurar recursos e começar a mapear seus dados de dispositivo. Os criadores de interface on-line orientam você pelas etapas na interface com o usuário.

Uma interface física é usada para modelar a interface entre um dispositivo físico e o Watson IoT Platform. Os tipos de eventos podem ser associados a interface física. Uma interface lógica é usada para definir a visualização normalizada para o estado do dispositivo no Watson IoT Platform e geralmente é consumida por aplicativos. Uma interface lógica deve ser associada a um esquema de interface lógica. O estado é atualizado em resposta a eventos de dispositivo de entrada.

Para obter mais informações sobre o recurso Data Management e seus benefícios, veja a [ Introdução ao Data Management](../GA_information_management/ga_im_device_twin.html#device_twins).

Para obter informações sobre os recursos que você precisa configurar, veja [Entendendo o Data Management](../GA_information_management/ga_im_definitions.html#definitions_resource).

Com o Criador de Interface Simples, você inclui uma interface física e, em seguida, uma interface lógica é criada automaticamente e as duas são mapeadas juntas. Com o Criador de
interface avançada, você tem mais controle sobre a configuração. Você inclui uma
interface física e uma ou mais interfaces lógicas; em seguida, inclui mapeamentos entre
elas.

As interfaces e os mapeamentos são incluídos no formato de rascunho. Depois de
serem incluídos e validados, deve-se ativá-los. Para obter mais informações sobre recursos ativos e de rascunho, consulte [Criando, atualizando, ativando e desativando seus recursos](../GA_information_management/ga_im_definitions.html#draft_active_resources).

As bibliotecas de interface contêm interfaces físicas e lógicas de modelo que podem ser reutilizadas.

## Fluxo de alto nível
{: #interface_flow}

Use as etapas a seguir para ajudar a configurar os recursos necessários para começar a usar o recurso de gerenciamento de dados.

### Antes de iniciar

Este procedimento supõe que você tenha um tipo de dispositivo com pelo menos um dispositivo registrado e conectado. Para obter mais informações sobre esses pré-requisitos e para obter instruções sobre como criar um tipo de dispositivo e registrar um dispositivo, veja [Conectando dispositivos](../iotplatform_task.html#iotplatform_task).

### Etapas

1. Selecione o Criador de Interface Simples ou o Criador de Interface Avançada:
  - ** Fluxo de Criador de Interface Simples **
    1. [Crie uma interface física de rascunho](#create_physical_interface_simple).
  - ** Fluxo do Advanced Interface Creator **
    1. [Crie uma interface física de rascunho](#create_physical_interface_advanced).
    2. [Crie uma ou mais interfaces lógicas de rascunho](#create_logical_interface). (Esta etapa é concluída automaticamente quando você usa o fluxo simples.)
    3. [Mapeie a interface física para as interfaces lógicas](#create_interface_mappings).
2. [Configurar preferências de notificação](#set_notifications).
3. [Ativar a configuração](#validate_activate).

Também é possível configurar o recurso Data Management usando as APIs. Para obter informações detalhadas sobre como configurar o recurso Data Management para normalizar e transformar os dados do dispositivo usando as APIs, veja [Introdução ao Data Management](ga_im_example.html#im_example). O [Guia passo a passo: um exemplo detalhado sobre como trabalhar com dispositivos por meio de uma interface comum](../GA_information_management/ga_im_index_scenario.html#scenario) guia você pelas etapas de criação de uma interface lógica de tipo de dispositivo para dispositivos de termômetro heterogêneos. Para obter detalhes sobre a API, consulte a documentação da [API de REST HTTP do {{site.data.keyword.iot_short_notm}}![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.
{: tip}


## Criando uma interface física de rascunho (fluxo simples)
{: #create_physical_interface_simple}

1. No menu de navegação principal, clique em **Dispositivos**.
2. Clique em **Tipos de dispositivo** e selecione o tipo de dispositivo para o qual você deseja criar uma interface. Também é possível criar um novo tipo de dispositivo. Consulte  [ Conectando Dispositivos ](../iotplatform_task.html#iotplatform_task)  para obter mais informações.
3. Visualize as informações de tipo de dispositivo e clique em **Interface**.
4. Clique em **Fluxo simples** para abrir o Criador de interface simples.
5. Clique em **Criar interface**.
6. Clique em **Incluir propriedade** para iniciar a inclusão de eventos e propriedades para a interface física.
   a. Na janela **Incluir propriedades na interface**, selecione
os eventos que você deseja incluir na interface. O sistema atende aos eventos ativos para
dispositivos conectados do tipo de dispositivo selecionado. É possível selecionar o ID do
dispositivo do último evento de cache ou selecionar outro evento na lista.
   b. Selecione as propriedades do evento e clique em **Incluir**.
   c. Inclua mais propriedades conforme necessário.
7. Clique em **Pronto**. A interface física é criada. Se você desejar incluir mais detalhes, será possível [fazer upgrade para uma interface avançada](#create_physical_interface_advanced) selecionando o link **Usar o criador de interface avançada**.


## Criando uma interface física de rascunho (fluxo avançado)
{: #create_physical_interface_advanced}

1. No menu de navegação principal, clique em **Dispositivos**.
2. Clique em **Tipos de dispositivo** e selecione o tipo de dispositivo para o qual você deseja criar uma interface. Também é possível criar um novo tipo de dispositivo. Consulte  [ Conectando Dispositivos ](../iotplatform_task.html#iotplatform_task)  para obter mais informações.
2. Visualize as informações de tipo de dispositivo e clique em **Interface**.
3. Clique em **Fluxo avançado** para abrir o Criador de
interface avançada.
4. Selecione uma das opções a seguir:
 - Para criar uma nova interface física, clique em **Criar interface**.
 - Para usar um modelo de interface da biblioteca, clique em **Incluir da biblioteca**, selecione a interface e, em seguida, acesse [Criando uma interface lógica de rascunho](#create_logic_interface).
5. Na guia **Identidade** da página **Criar interface
física**, digite um nome e uma descrição da interface física.
6. Opcional: ative o comutador do Edge se estiver usando dispositivos ativados pelo Edge com a interface. Para obter mais informações sobre o Edge, veja [Analítica do Edge](../edge_analytics.html#edge_analytics).
7. Clique em **Avançar**.
8. Defina a interface física incluindo e modificando tipos de eventos e cargas úteis:
   a. Selecione um tipo de evento na lista para modificar um tipo de evento existente ou clique em **Criar tipo de evento**.
   b. Selecione o último evento em cache, selecione um evento na lista ou inclua um evento manualmente e, em seguida, clique em **Incluir**.
9. Clique em **Pronto**. A interface física é criada em
formato de rascunho. Você pode prosseguir para incluir uma ou mais interfaces lógicas.

## Criando Interfaces Lógicas de Rascunho (Fluxo Avançado
{: #create_logical_interface}

1. Depois de ter criado a interface física de rascunho, escolha uma das opções a seguir:
 - Clique em **Incluir interface lógica** para criar uma nova interface lógica.
 - Clique em **Incluir da biblioteca** para usar um modelo de interface da biblioteca de interface. Selecione a interface e, em seguida, vá para a etapa 7.
2. Na guia **Identidade** da página **Criar interface lógica**, digite um nome e uma descrição da interface lógica.
3. Clique em **Avançar**.
4. Clique em **Incluir propriedade** para começar a definir as propriedades da interface lógica.
5. Na janela **Criar propriedade**, selecione as propriedades da interface física que você deseja incluir na interface lógica e, em seguida, salve as mudanças.
6. Clique em **Incluir objeto**.
7. Inclua interfaces lógicas adicionais conforme necessário e, em seguida, clique em **Avançar**.
8. {: #set_notifications}Configure as preferências de notificação para a interface. Selecione uma das opções a seguir para determinar quando deseja que notificações de estado do dispositivo sejam enviadas:
 - Nenhum notificador de evento - nenhuma notificação é enviada. É possível usar a
API de REST para recuperar informações de mudança de estado do dispositivo.
 - Na mudança de estado - você será notificado quando o estado do dispositivo mudar.
 - Em cada evento - você será notificado sempre que a plataforma processar um evento
para o dispositivo, mesmo que o evento não resulte em uma mudança de estado.
8. Clique em **Pronto**. As interfaces são criadas e o ![Ícone de status de rascunho](images/draft_icon.png) indica o status de rascunho. É possível continuar para modificar ou ativar as interfaces.

## Ativando a Configuração
{: #validate_activate}

Depois de ter criado as interfaces físicas e lógicas de rascunho, deve-se ativar a configuração.

- Clique em  ** Activate **  para ativar a configuração. O status da configuração é configurado como ativado.
![Implementação ativa](images/active_deployment.png) Se você faz mudanças nas interfaces, as interfaces se tornam rascunhos novamente e deve-se reativá-las.
