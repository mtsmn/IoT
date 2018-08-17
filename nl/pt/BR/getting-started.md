---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutorial Introdução
{: #getting-started-with-iotp}

Neste tutorial de introdução do {{site.data.keyword.iot_full}}, conectamos
um dispositivo IoT ao {{site.data.keyword.iot_short_notm}}.
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

<!--## Step 3: Create boards and cards to keep track of device data

By using boards and cards, you can view graphics that represent data set values from one or more devices for a quick overview and understanding of the device data.

1. In your {{site.data.keyword.iot_short_notm}} dashboard, click **Create New Board**.

2. Enter a name and description for the board.

3. Click **Next** and then **Create**.

4. Click the board you just created, and then click **Add New Card**. Select Devices as the card type. The following table describes the visualization options you can choose from.

| Type | Data that is displayed |
| Generic visualization | The value of one or more data sets. Choose the large widget size to see up to three data point values in a small table. |
| Line chart | One or more data sets in a real-time scrolling chart. Use the Settings menu to set the data range and retention, the look and feel of the graphs, and more. |
| Bar chart | Data set values in labelled bars. Use the Settings menu to toggle horizontal or vertical bar direction. |
| Donut chart | Two or more data sets in a circular representation. |
| Value | The raw value of one or more data sets. |
| Gauge | The value of a data set shown as a gauge. Use the Settings menu to optionally set gauge thresholds for lower, middle, and upper data ranges. |
| Device properties | Specific properties for one or more devices. |
| All device properties | All properties for one or more devices. |
| Device list | A list to monitor multiple devices. A list can be used as a data source for other cards. |
| Device info | Basic information for a single device. |
| Device map | The location of devices in a device list. |

{: caption="Table 1. Visualization options" caption-side="top"}

## Step 4: Create device type schemas

At this point, you need to create a device type schema and map device properties to then create rules that are triggered based on the datapoints from your mapped device properties.

1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Devices > Manage Schemas** and click **Add Schema**.

2. Select a device type to associate with the message schema. Only one schema can be defined for a device type.

3. Click **Add property** and add one or more properties.

    You can select properties from a connected device, create virtual properties that modify or combine existing properties, or add properties manually.

    The available properties are defined in the payload of the messages that are sent by a device.
    {: tip}

    * To add a property manually, select the **Manual tab** and define the following property details:
        * Name
        * Data type
        * Property
        * Data unit (optional)
        * Decimal places (optional)

    * To create a virtual property, select the **Virtual Property** tab and define the following property details:
        * Name
        * Data type
        * Property
        * Calculation
        * Data unit (optional)
        * Decimal places

    * To select properties from a connected device, select the **From Connected** tab, and then select one or more properties to add to the schema. The selected properties are added and the description is set to the name of the property.

4. Click **Finish** to create the properties.

## Step 5: Create rules and actions

Rules are condition-based decision points that match real-time device data with predefined threshold values or other property data to trigger an alert if a condition is met. In addition to the alert that's displayed in the {{site.data.keyword.iot_short_notm}} dashboard, you can add one or more actions to run business logic when a rule is triggered.

1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Rules** and click **Create Cloud Rule**.

2. Enter a name and description for the rule, select a device type that the rule applies to, and click **Next**.

3. To set up the rule logic, add one or more IF conditions to use as triggers for the rule.

    You can add conditions in parallel rows to apply them as OR conditions, or you can add conditions in sequential columns to apply them as AND conditions. Take a look at the following examples:

      * A simple rule might trigger an alert if a parameter value is larger than a specified value: Condition = `temp_cpu>80`
      * A more complex rule might trigger when a combination of thresholds are met: Condition = `temp_cpu>60 AND cpu_load>90`

    To trigger a condition that compares two properties, or to trigger two or more property conditions that are combined sequentially by using AND, include the triggering data points in the same device message. If the data is received in more than one message, the condition or sequential conditions don't trigger.
    {: tip}

4. Configure conditional trigger requirements for your rule.

    You can use conditional trigger requirements to control the number of alerts that are triggered for a rule over a time period. The conditional triggering acts on any condition in the rule. For example, if a rule has five parallel conditions set by using OR, each condition that's true counts towards the conditional trigger count.

      1. In the rule editor, click the default **Trigger each time conditions are met** link to open the set frequency requirement dialog.
      2. Select and configure the conditional trigger you want to use in the rule.

        * Trigger every time conditions are met
        * Trigger if conditions are met N times in M _Unit of time_

5. Create or select one or more actions that occur if the rule conditions are met.

    For example, an action can be to send an email or post a webhook.

6. Optional: Select an alert priority for the rule.

    The priority is used to classify the alerts that are displayed in the **Boards > Rule-Based Analytics** board. The default priority is Low.

7. Click **Save** to save without activating,  or click **Activate** to save and activate your rule.

    When you activate the rule, an alert is added to the **Rule-Based Analytics** board when the conditions are met, and any rule action is run. -->

## Próximas etapas

Crie e conecte seus próprios apps para consumir dados do dispositivo em tempo real e históricos.

  * Consulte as [bibliotecas do cliente](/docs/services/IoT/iot_platform_client_lib.html) para construir código para integrar e conectar seus dispositivos e apps.

  * Explore as [orientações do Watson IoT Platform ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window} para mais tutoriais sobre como integrar a análise de dados avançados a seus dispositivos e apps conectados.
