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

# 入门教程
{: #getting-started-with-iotp}

在此 {{site.data.keyword.iot_full}} 入门教程中，我们将 IoT 设备连接到 {{site.data.keyword.iot_short_notm}} 并设置分析，以探索实时数据。
{:shortdesc}

<div id="prerequisites"></div>

## 开始之前
{: #prereqs}

您将需要 [IBM Cloud 帐户 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/registration/){: new_window}、{{site.data.keyword.iot_short_notm}} 服务的实例和满足以下需求的设备：

*	您的设备必须能够使用 HTTP 或 MQTT 协议进行通信。

* 设备消息必须符合 {{site.data.keyword.iot_short_notm}} 消息有效内容需求。

有关更多信息，请参阅[在 Watson IoT Platform 上开发设备](/docs/services/IoT/devices/device_dev_index.html)。

## 步骤 1：注册设备

可以从 [{{site.data.keyword.iot_short_notm}} 仪表板 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://internetofthings.ibmcloud.com){: new_window} 一次添加一个设备，也可使用 [Watson IoT Platform API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} 添加多个设备。

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

## 步骤 3：创建板和卡以跟踪设备数据

通过使用板和卡，可以查看表示来自一个或多个设备的数据集值的图形，以快速概览和了解设备数据。

1. 在 {{site.data.keyword.iot_short_notm}} 仪表板中，单击**创建新板**。

2. 输入板的名称和描述。

3. 单击**下一步**，然后单击**创建**。

4. 单击刚创建的板，然后单击**添加新卡**。选择“设备”作为卡类型。下表描述可以选择的可视化选项。

|类型|显示的数据|
|---------------|---------------|
|通用可视化|一个或多个数据集的值。选择较大的窗口小部件大小，以在小型表中查看最多三个数据点值。|
|折线图|实时滚动图表中的一个或多个数据集。使用“设置”菜单可设置数据范围和保留时间、图形外观等。|
|条形图|以带标签的条形显示的数据集值。使用“设置”菜单可切换水平或垂直条形方向。|
|圆环图|以圆环图显示的两个或更多数据集。|
|值|一个或多个数据集的原始值。|
|量表|显示为量表的数据集值。使用“设置”菜单可选择设置低、中、高数据范围的量表阈值。|
|设备属性|一个或多个设备的特定属性。|
|所有设备属性|一个或多个设备的所有属性。|
|设备列表|要监视的多个设备的列表。列表可用作其他卡的数据源。|
|设备信息|单个设备的基本信息。|
|设备地图|设备列表中设备的位置。|

{: caption="表 1. 可视化选项" caption-side="top"}

## 步骤 4：创建设备类型模式

此时，您需要创建设备类型模式和映射设备属性，从而创建规则。基于来自所映射设备属性的数据点，可以触发这些规则。

1. 在 {{site.data.keyword.iot_short_notm}} 仪表板中，转至**设备 > 管理模式**，并单击**添加模式**。

2. 选择要与此消息模式关联的设备类型。对于一种设备类型，仅可定义一个模式。

3. 单击**添加属性**并添加一个或多个属性。

    您可以从连接的设备选择属性，创建用于修改或组合现有属性的虚拟属性，或手动添加属性。

    设备所发送的消息的有效内容中定义了可用属性。
    {: tip}

    * 要手动添加属性，请选择**手动**选项卡，然后定义以下属性详细信息：
        * 名称
        * 数据类型
        * 属性
        * 数据单位（可选）
        * 小数位（可选）

    * 要创建虚拟属性，请选择**虚拟属性**选项卡，然后定义以下属性详细信息：
        * 名称
        * 数据类型
        * 属性
        * 计算
        * 数据单位（可选）
        * 小数位

    * 要从所连接的设备选择属性，请选择**来自连接的设备**选项卡，然后选择要添加到模式的一个或多个属性。已添加所选属性，并且描述已设置为属性的名称。

4. 单击**完成**以创建属性。

## 步骤 5：创建规则和操作

规则是基于条件的决策点，用于使实时设备数据与预定义的阈值或其他属性数据相匹配，以在满足条件时触发警报。除了在 {{site.data.keyword.iot_short_notm}} 仪表板中显示的警报外，还可以添加一个或多个操作，用于在触发规则时运行业务逻辑。

1. 在 {{site.data.keyword.iot_short_notm}} 仪表板中，转至**规则**，并单击**创建云规则**。

2. 输入规则的名称和描述，选择规则适用的设备类型，然后单击**下一步**。

3. 要设置规则逻辑，请添加一个或多个 IF 条件以用作规则的触发器。

    可以通过并列行的方式添加条件以将其应用为 OR 条件，也可以通过顺序列的方式添加条件以将其应用为 AND 条件。请参考以下示例：

      * 如果参数值大于指定的值，可能触发简单的规则：条件 = `temp_cpu>80`
      * 当满足阈值组合时，可能触发更复杂的规则：条件 = `temp_cpu>60 AND cpu_load>90`

    要触发用于比较两个属性的条件，或者触发使用 AND 以顺序方式组合的两个或更多属性条件，请在同一设备消息中包含触发数据点。如果数据是在多个消息中收到的，那么不会触发该条件或这些顺序条件。
    {: tip}

4. 为规则配置有条件触发需求。

    您可以使用有条件触发需求，来控制一段时间内为某个规则触发的警报数。有条件触发将作用于该规则中的任何条件。例如，如果某个规则使用 OR 设置了 5 个并行条件，那么每个为 true 的条件都会计入有条件触发器计数。

      1. 在规则编辑器中，单击缺省的**每次满足条件时触发**链接，以打开“设置频率需求”对话框。
      2. 选择并配置要在规则中使用的有条件触发器。

        * 每次满足条件时触发
        * 在 M _时间单位_内条件满足 N 次时触发

5. 创建或选择在满足规则条件时执行的一个或多个操作。

    例如，操作可以是发送电子邮件或发布 Webhook。

6. 可选：为规则选择警报优先级。

    优先级用于对**板 > 基于规则的分析**板中显示的警报分类。缺省优先级为“低”。

7. 单击**保存**以保存而不激活规则，或者单击**激活**以保存并激活规则。

    如果激活规则，那么在满足条件时，将向**基于规则的分析**板中添加警报，并运行任何规则操作。

## 后续步骤

通过创建并连接您自己的应用程序以使用实时和历史设备数据，从而扩展数据分析功能。

  * 检查[客户机库](/docs/services/IoT/iot_platform_client_lib.html)，以获取构建代码的工具，用于集成和连接设备和应用程序。

  * 探索 [Watson IoT Platform 诀窍 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window}，以获取将高级分析嵌入到所连接设备和应用程序的更多教程。
