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

# 入门教程
{: #getting-started-with-iotp}

在此 {{site.data.keyword.iot_full}} 入门教程中，我们将 IoT 设备连接到 {{site.data.keyword.iot_short_notm}}。
{:shortdesc}

<div id="prerequisites"></div>

## 开始之前
{: #prereqs}

我们将需要一个 [IBM Cloud 帐户 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/registration/){: new_window}、{{site.data.keyword.iot_short_notm}}
服务的实例，以及满足以下需求的设备：

*	您的设备必须能够使用 HTTP 或 MQTT 协议进行通信。

* 设备消息必须符合 {{site.data.keyword.iot_short_notm}} 消息有效内容需求。

有关更多信息，请参阅[在 Watson IoT Platform 上开发设备](/docs/services/IoT/devices/device_dev_index.html)。

## 步骤 1：注册设备

您可以从 [{{site.data.keyword.iot_short_notm}} 仪表板 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://internetofthings.ibmcloud.com){: new_window} 一次添加一个设备，也可以使用 [WatsonIoT Platform API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} 添加多个设备。

要从 {{site.data.keyword.iot_short_notm}} 仪表板添加设备，请执行以下操作：

1. 在 {{site.data.keyword.Bluemix_notm}} 控制台中，单击 {{site.data.keyword.iot_short_notm}} 服务详细信息页面上的**启动**。

    此时，{{site.data.keyword.iot_short_notm}} Web 控制台将在新浏览器选项卡中打开，URL 如下：

    ```
        https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    其中，*org_id* 是 [{{site.data.keyword.iot_short_notm}} 组织](iotplatform_overview.html#organizations){: new_window}的标识。

2. 在“概述”仪表板中，从菜单窗格选择**设备**，然后单击**添加设备**。

3. 为要添加的设备创建设备类型。

    连接到 {{site.data.keyword.iot_short_notm}} 的每个设备都必须与一种设备类型相关联。设备类型是共享公共特征的设备组。

    1. 单击**创建设备类型**。
    2. 输入设备名称（如 `my_device_type`）以及设备类型描述。
    
        > **注：**设备类型名称不得超过 36 个字符，仅可包含字母数字字符（a-z、A-Z、0-9）和以下任何字符：`_`、`.` 和 `-`。

    3. 可选：输入设备类型属性和元数据。

        您可以稍后添加和编辑属性及元数据。
        {: tip}

4. 单击**下一步**以开始添加具有所选设备类型的设备的过程。

5. 输入设备标识，例如 `my_first_device`。

    设备标识用于在 {{site.data.keyword.iot_short_notm}} 仪表板中标识设备，还是用于将设备连接到 {{site.data.keyword.iot_short_notm}} 的必需参数。

    > **注：**设备类型名称不得超过 36 个字符，仅可包含字母数字字符（a-z、A-Z、0-9）和以下任何字符：`_`、`.` 和 `-`。

    对于连接网络的设备，设备标识可以是不带任何分隔冒号的设备 MAC 地址。

5. 单击**下一步**以完成该过程。

6. 提供认证令牌或接受自动生成的令牌。如果选择创建自己的令牌，请确保令牌的长度在 8 到 36 个字符之间，并且仅包含字母数字字符和以下字符：`_`、`.`、`!`、`&`、`@`、`?`、`\*`、`+`、`(`、`)` 和 `-`。

    此令牌不得包含重复的字符序列、字典单词、用户名或其他预定义序列。

7. 验证摘要信息是否正确，然后单击**添加**以添加连接。

8. 在设备信息页面中，复制并保存以下详细信息：

    * 组织标识
    * 设备类型
    * 设备标识
    * 认证方法
    * 认证令牌

    您将需要“组织标识”、“设备类型”、“设备标识”和“认证令牌”来配置设备以连接到 {{site.data.keyword.iot_short_notm}}。

## 步骤 2：将设备连接到 {{site.data.keyword.iot_short_notm}}

1. 设置您的设备以实现 MQTT 消息传递并使用“组织标识”、“设备类型”、“设备标识”和“认证令牌”以进行认证。

2. 通过使用 MQTT 协议将设备消息发送到 {{site.data.keyword.iot_short_notm}} 组织。

    连接设备时需要以下信息：

    * URL：*org_id*.messaging.internetofthings.ibmcloud.com

      其中，*org_id* 是 {{site.data.keyword.iot_short_notm}} 组织的标识。

    * 端口：
      * 1883
      * 8883（已加密）
      * 443 (websocket)
    * 设备标识：d:_org_id:device_type:device_id_
    * 用户名：use-token-auth
    * 密码：_认证令牌_
    * 事件主题格式：iot-2/evt/_event_id/fmt/format_string_

      其中，_event_id_ 指定 {{site.data.keyword.iot_short_notm}} 中显示的事件名称，_format_string_ 是事件的格式（例如，JSON）。

    * 消息格式：JSON

  有关更多信息，请参阅[设备的 MQTT 连接](/docs/services/IoT/devices/mqtt.html)。

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

## 后续步骤

创建并连接您自己的应用程序以使用实时和历史设备数据。

  * 检查[客户机库](/docs/services/IoT/iot_platform_client_lib.html)，以获取构建代码的工具，用于集成和连接设备和应用程序。

  * 探索 [Watson IoT Platform 诀窍 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window}，以获取将高级分析嵌入到所连接设备和应用程序的更多教程。
