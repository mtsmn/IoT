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

# 入門指導教學
{: #getting-started-with-iotp}

在此 {{site.data.keyword.iot_full}} 入門指導教學中，我們將 IoT 裝置連接至 {{site.data.keyword.iot_short_notm}}。
{:shortdesc}

<div id="prerequisites"></div>

## 開始之前
{: #prereqs}

您需要有一個 [IBM Cloud 帳戶 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/registration/){: new_window}、一個 {{site.data.keyword.iot_short_notm}} 服務實例，以及一個符合下列需求的裝置：

*	您的裝置必須能夠使用 HTTP 或 MQTT 通訊協定進行通訊。

* 裝置訊息必須符合 {{site.data.keyword.iot_short_notm}} 訊息有效負載需求。

如需相關資訊，請參閱[在 Watson IoT Platform 上開發裝置](/docs/services/IoT/devices/device_dev_index.html)。

## 步驟 1：登錄裝置

您可以從 [{{site.data.keyword.iot_short_notm}} 儀表板 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://internetofthings.ibmcloud.com){: new_window} 一次新增一個裝置，或是使用 [Watson IoT Platform API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} 新增多個裝置。

若要從 {{site.data.keyword.iot_short_notm}} 儀表板新增裝置，請執行下列動作：

1. 在 {{site.data.keyword.Bluemix_notm}} 主控台中，按一下 {{site.data.keyword.iot_short_notm}} 服務詳細資料頁面上的**啟動**。

    {{site.data.keyword.iot_short_notm}} Web 主控台會在新瀏覽器分頁中開啟，其 URL 為：

    ```
 https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
 ```

    其中 *org_id* 是[您的 {{site.data.keyword.iot_short_notm}} 組織](iotplatform_overview.html#organizations){: new_window}的 ID。

2. 在「概觀」儀表板中，從功能表窗格中選取**裝置**，然後按一下**新增裝置**。

3. 建立您要新增之裝置的裝置類型。

    連接至 {{site.data.keyword.iot_short_notm}} 的每一個裝置都必須與裝置類型相關聯。裝置類型是一群共用相同性質的裝置。

    1. 按一下**建立裝置類型**。
    2. 輸入裝置名稱（例如 `my_device_type`）及裝置類型的說明。
    
        > **附註：**裝置類型名稱不得超過 36 個字元，且只能包含英數字元（a-z、A-Z、0-9）及下列任何字元：`_`、`.` 和 `-`。



    3. 選用項目：輸入裝置類型屬性和 meta 資料。  
 

        您可以稍後再新增及編輯屬性和 meta 資料。
        {: tip}

4. 按**下一步**，開始新增所選取裝置類型的裝置。

5. 輸入裝置 ID（例如 `my_first_device`）。

    裝置 ID 可用來識別 {{site.data.keyword.iot_short_notm}} 儀表板中的裝置，它也是將裝置連接至 {{site.data.keyword.iot_short_notm}} 的必要參數。

    > **附註：**裝置類型名稱不得超過 36 個字元，且只能包含英數字元（a-z、A-Z、0-9）及下列任何字元：`_`、`.` 和 `-`。



    若為連接網路的裝置，裝置 ID 可以是不含任何分隔冒號的裝置 MAC 位址。

5. 按**下一步**，以完成處理程序。

6. 提供鑑別記號，或接受自動產生的記號。如果您選擇建立自己的記號，請確定其長度為 8 到 36 個字元，並且只包含英數字元及下列字元：`_`、`.`、`!`、`&`、`@`、`?`、`\*`、`+`、`(`、`)` 及 `-`。

    記號不得包含重複的字元順序、字典單字、使用者名稱或其他預先定義的順序。

7. 驗證摘要資訊正確無誤，然後按一下**新增**，以新增連線。

8. 在裝置資訊頁面中，複製並儲存下列詳細資料：

    * 組織 ID
  
    * 裝置類型
    * 裝置 ID
    * 鑑別方法
  
    * 鑑別記號
  

    您需要有「組織 ID」、「裝置類型」、「裝置 ID」及「鑑別記號」，才能配置您的裝置來連接至 {{site.data.keyword.iot_short_notm}}。

## 步驟 2：將您的裝置連接至 {{site.data.keyword.iot_short_notm}}

1. 設定裝置來進行 MQTT 傳訊，並使用「組織 ID」、「裝置類型」、「裝置 ID」及「鑑別記號」進行鑑別。

2. 使用 MQTT 通訊協定將裝置訊息傳送至 {{site.data.keyword.iot_short_notm}} 組織。

    連接裝置時，需要下列資訊：

    * URL：*org_id*.messaging.internetofthings.ibmcloud.com

      其中 *org_id* 是 {{site.data.keyword.iot_short_notm}} 組織的 ID。

    * 埠：
      * 1883
      * 8883（已加密）
      * 443 (websockets)
    * 裝置 ID：d:_org_id:device_type:device_id_
    * 使用者名稱：use-token-auth
    * 密碼：_鑑別記號_
    * 事件主題格式：iot-2/evt/_event_id/fmt/format_string_

      其中 _event_id_ 指定顯示在 {{site.data.keyword.iot_short_notm}} 中的事件名稱，而 _format_string_ 是事件的格式，例如 JSON。

    * 訊息格式：JSON

  如需相關資訊，請參閱[裝置的 MQTT 連線功能](/docs/services/IoT/devices/mqtt.html)。

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

## 後續步驟

建立並連接您自己的應用程式來使用即時和歷程裝置資料。

  * 移出工具的[用戶端程式庫](/docs/services/IoT/iot_platform_client_lib.html)來建置程式碼，以整合及連接裝置和應用程式。

  * 探索 [Watson IoT Platform 秘訣 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window}，以取得將進階分析內含至所連接裝置及應用程式的指導教學。
