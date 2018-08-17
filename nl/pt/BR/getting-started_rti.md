---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutorial Introdução
{: #getting-started-with-iotp}

Neste tutorial de introdução ao {{site.data.keyword.iot_full}}, conectamos um dispositivo IoT ao {{site.data.keyword.iot_short_notm}} e configuramos a análise de dados para explorar dados em tempo real.
{:shortdesc}

<div id="prerequisites"></div>

## Antes de iniciar
{: #prereqs}

Uma [conta do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/registration/){: new_window}, uma instância do serviço {{site.data.keyword.iot_short_notm}} e um dispositivo que atenda aos requisitos a seguir são necessários:

*	Seu dispositivo deve poder se comunicar usando os protocolos HTTP ou MQTT.

* As mensagens do dispositivo devem se adequar aos requisitos de carga útil da mensagem do {{site.data.keyword.iot_short_notm}}.

Para obter mais informações, veja [Desenvolvendo dispositivos no Watson IoT Platform](/docs/services/IoT/devices/device_dev_index.html).

## Etapa 1: registrar seu dispositivo

É possível incluir dispositivos, um a cada vez, no painel [{{site.data.keyword.iot_short_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://internetofthings.ibmcloud.com){: new_window} ou é possível usar a [API da Watson IoT Platform ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} para incluir múltiplos dispositivos.

Para incluir um dispositivo a partir do painel do {{site.data.keyword.iot_short_notm}}:

1. No console do {{site.data.keyword.Bluemix_notm}}, clique em **Ativar** na página de detalhes do serviço {{site.data.keyword.iot_short_notm}}.

    O console da web do {{site.data.keyword.iot_short_notm}} é aberto em uma nova guia do navegador na URL a seguir:

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    Em que *org_id* é o ID de [sua organização do {{site.data.keyword.iot_short_notm}}](iotplatform_overview.html#organizations){: new_window}.

2. No painel Visão geral, na área de janela de menu, selecione **Dispositivos** e, em seguida, clique em **Incluir dispositivo**.

3. Crie um tipo de dispositivo para o dispositivo que você está incluindo.

    Cada dispositivo conectado ao {{site.data.keyword.iot_short_notm}} deve estar associado a um tipo de dispositivo. Tipos de dispositivos são grupos de dispositivos que compartilham características.

    1. Clique em **Criar tipo de dispositivo**.
    2. Insira um nome de dispositivo, como `my_device_type`, e uma descrição para o tipo de dispositivo.
    
        > ** Nota:** o nome do tipo de dispositivo não deve ter mais de 36 caracteres e pode conter somente caracteres alfanuméricos (a-z, A-Z, 0-9) e qualquer um dos caracteres a seguir: `_`, `.` e `-`.

    3. Opcional: insira atributos e metadados de tipo de dispositivo.

        Será possível incluir e editar atributos e metadados posteriormente.
        {: tip}

4. Clique em **Avançar** para iniciar o processo de incluir seu dispositivo com o tipo de dispositivo selecionado.

5. Insira um ID do dispositivo, por exemplo `my_first_device`.

    O ID do dispositivo é usado para identificar o dispositivo no painel do {{site.data.keyword.iot_short_notm}} e também é um parâmetro necessário para conectar seu dispositivo ao {{site.data.keyword.iot_short_notm}}.

    > ** Nota:** o nome do tipo de dispositivo não deve ter mais de 36 caracteres e pode conter somente caracteres alfanuméricos (a-z, A-Z, 0-9) e qualquer um dos caracteres a seguir: `_`, `.` e `-`.

    Para dispositivos conectados à rede, o ID do dispositivo poderia ser o endereço MAC do dispositivo sem dois-pontos separando.

5. Clique em **Avançar** para concluir o processo.

6. Forneça um token de autenticação ou aceite um token gerado automaticamente. Se você optar por criar seu próprio token, certifique-se de que ele tenha entre 8 e 36 caracteres e consista apenas em caracteres alfanuméricos e os seguintes caracteres: `_`, `.`, `!`, `&`, `a`, `?`, `\ *`, `+`, `(`, `)` e `-`.

    O token não deve conter sequências de caracteres repetidos, palavras de dicionário, nomes de usuário ou outras sequências predefinidas.

7. Verifique se as informações de resumo estão corretas e, em seguida, clique em **Incluir** para incluir a conexão.

8. Na página de informações sobre o dispositivo, copie e salve os detalhes a seguir:

    * ID de Organização
    * Tipo do Dispositivo
    * ID do dispositivo
    * Método de Autenticação
    * Token de Autenticação

    Você precisará do ID da organização, Tipo de Dispositivo, ID do dispositivo e Token de autenticação para configurar seu dispositivo para se conectar ao {{site.data.keyword.iot_short_notm}}.

## Etapa 2: conectar seu dispositivo ao {{site.data.keyword.iot_short_notm}}

1. Configure seu dispositivo para o sistema de mensagens MQTT e autentique usando o ID da organização, Tipo de dispositivo, ID do dispositivo e Token de autenticação.

2. Envie mensagens do dispositivo para sua organização do {{site.data.keyword.iot_short_notm}} usando o protocolo MQTT.

    As informações a seguir são necessárias ao conectar seu dispositivo:

    * URL: *org_id*.messaging.internetofthings.ibmcloud.com

      Em que *org_id* é o ID de sua organização do {{site.data.keyword.iot_short_notm}}.

    * 1Port:
      * 1883
      * 8883 (criptografada)
      * 443 (soquetes da web)
    * ID do dispositivo: d:_org_id:device_type:device_id_
    * Nome do usuário: use-token-auth
    * Senha: _Token de autenticação_
    * Formato do tópico de evento: iot-2/evt/_event_id/fmt/format_string_

      Em que _event_id_ especifica o nome do evento que é mostrado no {{site.data.keyword.iot_short_notm}} e _format_string_ é o formato do evento, como JSON.

    * Formato da mensagem: JSON

  Para obter mais informações, veja [Conectividade MQTT para dispositivos](/docs/services/IoT/devices/mqtt.html).

## Etapa 3: criar placas e cartões para manter o controle de dados do dispositivo

Usando placas e cartões, é possível visualizar gráficos que representam valores do conjunto de dados de um ou mais dispositivos para uma visão geral e um entendimento rápidos dos dados do dispositivo.

1. Em seu painel do {{site.data.keyword.iot_short_notm}}, clique em **Criar nova placa**.

2. Insira um nome e uma descrição para a placa.

3. Clique em **Avançar** e, em seguida, **Criar**.

4. Clique na placa que você acabou de criar e, em seguida, clique em **Incluir novo cartão**. Selecione Dispositivos como o tipo de cartão. A tabela a seguir descreve as opções de visualização entre a quais é possível escolher.

| Tipo | Dados exibidos |
|---------------|---------------|
| Visualização genérica | O valor de um ou mais conjuntos de dados. Escolha o tamanho de widget grande para ver até três valores de ponto de dados em uma tabela pequena. |
| Gráfico de Linhas | Um ou mais conjuntos de dados em um gráfico de rolagem em tempo real. Use o menu Configurações para configurar o intervalo de dados e a retenção, a aparência dos gráficos e mais. |
| gráfico de barras | Valores do conjunto de dados em barras rotuladas. Use o menu Configurações para alternar a direção da barra horizontal ou vertical. |
| Gráfico de rosca | Dois ou mais conjuntos de dados em uma representação circular. |
| Value | O valor bruto de um ou mais conjuntos de dados. |
| Medidor | O valor de um conjunto de dados mostrado como um medidor. Use o menu Configurações para opcionalmente configurar limites do medidor para intervalos de dados inferior, intermediário e superior. |
| Propriedades do Dispositivo | Propriedades específicas para um ou mais dispositivos. |
| Todas as propriedades do dispositivo | Todas as propriedades para um ou mais dispositivos. |
| Lista de dispositivos | Uma lista para monitorar vários dispositivos. Uma lista pode ser usada como uma origem de dados para outros cartões. |
| Informações do dispositivo | Informações básicas para um único dispositivo. |
| Mapa de dispositivos | O local de dispositivos em uma lista de dispositivos. |

{: caption="Tabela 1. Opções de visualização" caption-side="top"}

## Etapa 4: criar esquemas de tipo de dispositivo

Neste ponto, é necessário criar um esquema de tipo de dispositivo e mapear propriedades do dispositivo para então criar regras que são acionadas com base nos pontos de dados de suas propriedades do dispositivo mapeadas.

1. No painel do {{site.data.keyword.iot_short_notm}}, acesse **Dispositivos > Gerenciar esquemas** e clique em **Incluir esquema**.

2. Selecione um tipo de dispositivo para associar ao esquema de mensagem. Somente um esquema pode ser definido para um tipo de dispositivo.

3. Clique em **Incluir propriedade** e inclua uma ou mais propriedades.

    É possível selecionar propriedades de um dispositivo conectado, criar propriedades virtuais que modifiquem ou combinem propriedades existentes ou incluir propriedades manualmente.

    As propriedades disponíveis estão definidas na carga útil das mensagens que são enviadas por um dispositivo.
    {: tip}

    * Para incluir uma propriedade manualmente, selecione a **guia Manual** e defina os detalhes da propriedade a seguir:
        * Name
        * Tipo de Dados
        * Propriedade
        * Unidade de dados (opcional)
        * Casas decimais (opcional)

    * Para criar uma propriedade virtual, selecione a guia **Propriedade virtual** e defina os detalhes da propriedade a seguir:
        * Name
        * Tipo de Dados
        * Propriedade
        * Cálculo
        * Unidade de dados (opcional)
        * Casas decimais

    * Para selecionar propriedades de um dispositivo conectado, selecione a guia **De conectado** e, em seguida, selecione uma ou mais propriedades para incluir no esquema. As propriedades selecionadas são incluídas e a descrição é configurada como o nome da propriedade.

4. Clique em **Concluir** para criar as propriedades.

## Etapa 5: criar regras e ações

Regras são pontos de decisão baseados em condições que correspondem a dados do dispositivo de tempo real com valores de limites predefinidos ou outros dados de propriedade para acionar um alerta se uma condição for atendida. Além do alerta que é exibido no painel do {{site.data.keyword.iot_short_notm}}, será possível incluir uma ou mais ações para executar a lógica de negócios quando uma regra for acionada.

1. No painel do {{site.data.keyword.iot_short_notm}}, acesse **Regras** e clique em **Criar regra de nuvem**.

2. Insira um nome e uma descrição para a regra, selecione um tipo de dispositivo ao qual a regra se aplica e clique em **Avançar**.

3. Para configurar a lógica de regra, inclua uma ou mais condições IF para usar como acionadores da regra.

    É possível incluir condições em linhas paralelas para aplicá-las como condições OR ou incluir condições em colunas sequenciais para aplicá-las como condições AND. Dê uma olhada nos exemplos a seguir:

      * Uma regra simples poderá acionar um alerta se um valor de parâmetro for maior que um valor especificado:
Condição = `temp_cpu>80`
      * Uma regra mais complexa pode ser acionada quando uma combinação de limites é atendida:
Condição = `temp_cpu>60 AND cpu_load>90`

    Para acionar uma condição que compara duas propriedades ou para acionar duas ou mais condições de propriedade que são combinadas sequencialmente usando AND, inclua os pontos de dados de acionamento na mesma mensagem do dispositivo. Se os dados forem recebidos em mais de uma mensagem, a condição ou condições sequenciais não serão acionadas.
    {: tip}

4. Configure os requisitos de acionador condicional para a sua regra.

    É possível usar os requisitos de acionador condicional para controlar o número de alertas que são acionados para uma regra durante um período de tempo. O acionamento condicional age em qualquer condição na regra. Por exemplo, se uma regra tiver cinco condições paralelas configuradas usando OR, cada condição verdadeira será incluída na contagem do acionador condicional.

      1. No editor de regras, clique no link **Acionar cada vez que as condições forem atendidas** padrão para abrir o diálogo para configurar o requisito de frequência.
      2. Selecione e configure o acionador condicional que você deseja usar na regra.

        * Acionar cada vez que as condições forem atendidas.
        * Acionar se a condições forem atendidas N vezes em _Unidade de tempo_ M

5. Crie ou selecione uma ou mais ações que ocorrem se as condições da regra forem atendidas.

    Por exemplo, uma ação pode ser enviar um e-mail ou postar uma webhook.

6. Opcional: selecione uma prioridade de alerta para a regra.

    A prioridade é usada para classificar os alertas que são exibidos na placa **Placas > Análise de dados baseada em regra**. A prioridade padrão é Baixa.

7. Clique em **Salvar** para salvar sem ativar ou clique em **Ativar** para salvar e ativar sua regra.

    Quando você ativa a regra, um alerta é incluído na placa **Análise de dados baseada em regras** quando as condições são atendidas e qualquer ação de regra é executada.

## Próximas etapas

Estenda os recursos de análise de dados criando e conectando seus próprios apps para consumir dados do dispositivo em tempo real e históricos.

  * Consulte as [bibliotecas do cliente](/docs/services/IoT/iot_platform_client_lib.html) para construir código para integrar e conectar seus dispositivos e apps.

  * Explore as [orientações do Watson IoT Platform ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} para mais tutoriais sobre como integrar a análise de dados avançados a seus dispositivos e apps conectados.
