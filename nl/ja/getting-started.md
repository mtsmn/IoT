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

# 概説のチュートリアル
{: #getting-started-with-iotp}

この {{site.data.keyword.iot_full}} の概説のチュートリアルでは、IoT デバイスを {{site.data.keyword.iot_short_notm}} に接続します。
{:shortdesc}

<div id="prerequisites"></div>

## 始めに
{: #prereqs}

[IBM Cloud アカウント ![外部リンク・アイコン ](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/registration/){: new_window}、{{site.data.keyword.iot_short_notm}} サービスのインスタンス、以下の要件を満たすデバイスが必要になります。

*	デバイスは、HTTP プロトコルまたは MQTT プロトコルを使用して通信できるようでなければなりません。

* デバイス・メッセージは、{{site.data.keyword.iot_short_notm}} のメッセージ・ペイロードの要件に準拠していなければなりません。

詳細については、[Watson IoT Platform でのデバイスの開発](/docs/services/IoT/devices/device_dev_index.html)を参照してください。

## 手順 1: デバイスの登録

[{{site.data.keyword.iot_short_notm}} ダッシュボード ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://internetofthings.ibmcloud.com){: new_window} から一度に 1 つずつデバイスを追加するか、[Watson IoT プラットフォーム API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} を使用して複数のデバイスを追加することができます。

{{site.data.keyword.iot_short_notm}} ダッシュボードからデバイスを追加するには、以下のようにします。

1. {{site.data.keyword.Bluemix_notm}} コンソールで、{{site.data.keyword.iot_short_notm}} サービスの詳細ページの**「起動」**をクリックします。

    新しいブラウザー・タブで以下の URL の {{site.data.keyword.iot_short_notm}} Web コンソールが開きます。

    ```
    https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    ここで、*org_id* は [{{site.data.keyword.iot_short_notm}} 組織](iotplatform_overview.html#organizations){: new_window}の ID です。

2. 「概要」ダッシュボードのメニュー・ペインから**「デバイス」**を選択し、**「デバイスの追加」**をクリックします。

3. 追加するデバイスのデバイス・タイプを作成します。

    {{site.data.keyword.iot_short_notm}} に接続する各デバイスには、デバイス・タイプを関連付ける必要があります。 デバイス・タイプとは、共通の特性を共有するデバイス・グループのことです。

    1. **「デバイス・タイプの作成」**をクリックします。
    2. `my_device_type` などのデバイス名と、デバイス・タイプの説明を入力します。
    
        > **注意:** デバイス・タイプ名は 36 文字以内にする必要があり、英数字の文字 (a-z, A-Z, 0-9) と、`_`、`.`、および `-`のいずれかのみを含めることができます。

    3. オプション: デバイス・タイプの属性やメタデータを入力します。

        属性とメタデータは、後で追加および編集できます。
        {: tip}

4. **「次へ」**をクリックして、選択したデバイス・タイプのデバイスを追加するプロセスを開始します。

5. `my_first_device` などのデバイス ID を入力します。

    デバイス ID は、{{site.data.keyword.iot_short_notm}} ダッシュボードでデバイスを識別するために使用され、デバイスを {{site.data.keyword.iot_short_notm}} に接続するための必須パラメーターでもあります。

    > **注意:** デバイス・タイプ名は 36 文字以内にする必要があり、英数字の文字 (a-z, A-Z, 0-9) と、`_`、`.`、および `-`のいずれかのみを含めることができます。

    ネットワークに接続されたデバイスの場合は、デバイス MAC アドレス (区切り文字のコロンは付けない) をデバイス ID として入力できます。

5. **「次へ」**をクリックすると、プロセスが完了します。

6. 認証トークンを指定するか、自動生成されたトークンを受け入れます。 自分でトークンを作成する場合は、長さを 8 文字から 36 文字の間にして、英数字と、`_`、`.`、`!`、`&`、`@`、`?`、`\*`、`+`、`(`、`)`、`-` だけを使用してください。

    反復した文字シーケンスや、辞書に出てくる単語やユーザー名などの事前定義シーケンスをトークンに含めないでください。

7. 要約情報が正しいことを確認してから、**「追加」**をクリックして接続を追加します。

8. デバイス情報のページで、以下の詳細をコピーして保存します。

    * 組織 ID
    * デバイス・タイプ
    * デバイス ID
    * 認証方式
    * 認証トークン

    {{site.data.keyword.iot_short_notm}} へのデバイスの接続を構成するときに、組織 ID、デバイス・タイプ、デバイス ID、および認証トークンが必要になります。

## 手順 2: {{site.data.keyword.iot_short_notm}} とデバイスの接続

1. MQTT メッセージングを使用するようにデバイスをセットアップし、組織 ID、デバイス・タイプ、デバイス ID、および認証トークンを使用して認証します。

2. MQTT プロトコルを使用して、デバイス・メッセージを {{site.data.keyword.iot_short_notm}} 組織に送信します。

    デバイスを接続するときには、以下の情報が必要になります。

    * URL: *org_id*.messaging.internetofthings.ibmcloud.com

      *org_id* は、{{site.data.keyword.iot_short_notm}} 組織の ID です。

    * ポート:
      * 1883
      * 8883 (暗号化)
      * 443 (Web ソケット)
    * デバイス ID: d:_org_id:device_type:device_id_
    * ユーザー名: use-token-auth
    * パスワード: _Authentication token_
    * イベント・トピック形式: iot-2/evt/_event_id/fmt/format_string_

      _event_id_ では、{{site.data.keyword.iot_short_notm}} に表示されるイベント名を指定します。_format_string_ は、イベントの形式 (JSON など) です。

    * メッセージ形式: JSON

  詳しくは、[デバイスの MQTT 接続](/docs/services/IoT/devices/mqtt.html)を参照してください。

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

## 次の手順

デバイスのリアルタイム・データと履歴データをコンシュームする独自のアプリを作成して接続します。

  * デバイスとアプリを統合し、接続するためのコードをビルドする場合は、ツールの[クライアント・ライブラリー](/docs/services/IoT/iot_platform_client_lib.html)を参照してください。

  * 接続したデバイスとアプリにより高度な分析を埋め込む方法に関する詳細なチュートリアルについては、[Watson IoT プラットフォームのレシピ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window}を参照してください。
