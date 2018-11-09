---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-01"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 使用 Data Science Experience 分析数据
{: #DSX_integration}

您可以将 {{site.data.keyword.iot_full}} 与 Data Science Experience (DSX) 配合使用，以可视化并了解从连接到平台的设备发送的数据。
{: shortdesc}

## 概述和目标

本指南逐步指导您完成通过将 DSX 用作分析工具来可视化 {{site.data.keyword.iot_short_notm}} 设备事件数据的过程。

DSX 提供了一种基于云的交互协作环境，您可以在此环境中使用多种工具来激活其洞察。使用 IBM DSX 中提供的 Jupyter 配置页来装入历史 {{site.data.keyword.iot_short_notm}} 数据，并使用这些数据来创建可视化，以帮助数据分析和异常识别。您可以从历史数据派生阈值，并使用这些值在 {{site.data.keyword.iot_short_notm}} 中创建云规则。当与规则关联的 IoT 设备发布的读数超出所配置的阈值限制时，云规则向用户发出警报。

可以使用 {{site.data.keyword.cloudantfull}} NoSQL DB 服务来收集发送到 {{site.data.keyword.iot_short_notm}} 的设备数据并将其存储在 {{site.data.keyword.Bluemix}} 中。要收集数据，必须首先将 {{site.data.keyword.iot_short_notm}} 连接到 {{site.data.keyword.cloudant_short_notm}} 服务。根据配置的存储区时间间隔，设备数据存储在 {{site.data.keyword.cloudant_short_notm}} 每日、每周或每月数据库中。


![使用 DSX 分析数据的概述](images/DSX_overview.png)

作为本指南的一部分，您将了解：

 - 如何配置平台数据存储，以便将 Cloudant NoSQL DB 用作历史服务。
 - 如何使用天气传感器模拟器来生成要由平台使用的数据。
 - 如何导出数据，然后将数据导入 DSX 以分析数据。
 - 如何可视化 IoT 数据、检测异常并配置警报。


## 先决条件

要完成这些步骤，您必须具有对安装了 [Cloudant NoSQL DB ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window} 的 [{{site.data.keyword.iot_short_notm}} ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} 的访问权，对 [Apache Spark ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} 服务的访问权，以及对 [DSX Account ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window} 的访问权。


## 步骤 1. 设置模拟器
{:#DSX_sensor_data}

为了执行有意义的分析，您必须具有有意义的数据。您可以模拟真实的传感器数据，以了解如何使用 DSX 来分析 Watson IoT Platform 设备数据。此步骤提供以下内容的指示信息：
 - [使用 **{{site.data.keyword.iot_short_notm}} 的现有实例**设置模拟器](#sim_existing_platorm)
 - [使用 **{{site.data.keyword.iot_short_notm}} 的新实例**设置模拟器](#sim_new_platform)


### 使用 {{site.data.keyword.iot_short_notm}} 的现有实例设置天气传感器模拟器
{: #sim_existing_platform}

要使用天气传感器模拟器，针对组织来模拟实时传感器数据事件，必须首先设置模拟器。以下步骤假设您已经具有已启动并在运行的 {{site.data.keyword.iot_short_notm}} 实例。

1. [生成运行模拟器所需的 API 密钥和令牌。![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [部署天气传感器模拟器 Web 应用程序 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}，并遵循详细指示信息。

   有关天气传感器的更多信息，请参阅[天气传感器模拟器指南 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}。
3. 继续执行[步骤 2. 配置数据库连接器](#DSX_config_db)。


### 使用 {{site.data.keyword.iot_short_notm}} 的新实例设置天气传感器模拟器
{: #sim_new_platform}

要使用天气传感器模拟器，针对组织来模拟实时传感器数据事件，必须首先设置模拟器。以下步骤包括用于随模拟器创建 {{site.data.keyword.iot_short_notm}} 实例的指示信息。

1. [使用 {{site.data.keyword.iot_short_notm}} 的实例部署天气传感器模拟器 Web 应用程序 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window}，并执行详细步骤。

   有关天气传感器的更多信息，请参阅[天气传感器模拟器指南 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}。
2. 等待部署完成，然后浏览到 IBM Cloud“仪表板”。
3. 启动由部署过程创建的 {{site.data.keyword.iot_short_notm}} 服务“wiotp-for-weather-sensors-simulator”。
4. 继续执行[步骤 2. 配置数据库连接器](#DSX_config_db)。


## 步骤 2 . 配置数据库连接器
{: #DSX_config_db}

可以在每天、每周或每月数据库中按所选存储区时间间隔选项，将设备数据存储在 Cloudant 中。在同一存储区时间间隔（日、周或月）从所有设备收集的数据存储在同一数据库中。

无论存储区时间间隔配置如何，连接器都会自动创建三个数据库。针对当前存储区时间间隔，会创建一个数据库，针对即将到来的时间间隔和配置数据库也会各创建一个数据库。等到时间间隔结束时，设备数据会针对新时间间隔存储在存储区数据库中，并会为后续存储区创建一个新的数据库。

要将 {{site.data.keyword.cloudant_short_notm}} 与 DSX 一起使用，必须配置平台数据存储，以便将 Cloudant NoSQL DB 用作历史服务。

1. 在 {{site.data.keyword.cloudant_short_notm}} 仪表板上，单击导航栏中的**扩展**。
2. 在**历史数据存储**下，单击**设置**。**配置历史数据存储**部分列出了 {{site.data.keyword.cloudant_short_notm}} 所在的 IBM Cloud 空间中可用的所有 Cloudant NoSQL DB 服务。
3. 选择要连接的 Cloudant NoSQL DB 服务。
4. 指定以下 Cloudant NoSQL 数据库配置选项：
  - 存储区时间间隔 = 天
  - 时区 = UTC
  - 数据库名称 = 缺省值
5. 单击**完成**并确认与 Cloudant 服务的连接的授权。请确保浏览器中启用了弹出窗口，以便能够访问确认窗口。成功配置 Cloudant NoSQL DB 后，“历史数据存储”状态将更改为“已配置”，并且设备数据会存储在 {{site.data.keyword.cloudant_short_notm}} NoSQL DB 中。
6. 继续执行[步骤 3. 运行模拟器](#run_simulator)。


## 步骤 3. 运行模拟器
{: #run_simulator}

模拟器会将位于海法地区的 17 个气象站中的实际天气传感器数据发布到您的 {{site.data.keyword.iot_short_notm}} 组织。

1. 浏览到模拟器。
2. 如果使用绑定的 {{site.data.keyword.iot_short_notm}} 实例部署了模拟器，请继续执行步骤 3。如果部署了独立版本的模拟器，请输入以下详细信息：
   - Watson IoT Platform 组织
   - API 密钥
   - 认证令牌

3. 单击**运行模拟器**。生成数据需要几分钟时间。
4. 在模拟器运行时转至 Watson IoT Platform，并验证是否已创建设备，以及事件是否要传入这些设备。
5. 继续执行[步骤 4. 设置 DSX 并可视化数据](#DSX_visualize_data)。


## 步骤 4. 设置 DSX 并可视化数据
{: #DSX_visualize_data}

要设置 DSX 并开始可视化数据，请执行以下操作：

1. [设置预配置的 Jupyter 配置页](#setup_jupyter_notebook)，以获取对数据的洞察并检测异常情况。
2. [运行分析。](#run_analysis)
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. 设置预配置的 Jupyter 配置页
{: #setup_jupyter_notebook}

Jupyter 配置页是一个 Web 应用程序，它允许用户创建和共享包含可执行代码、数学公式、图形/可视化项 (matplotlib) 和解释性文本的文档。

要设置预配置的 Jupyter 配置页以获取数据的洞察并检测异常：
1. 通过受支持的浏览器，使用 IBM Cloud 标识登录到 [DSX ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://datascience.ibm.com/){: new_window}。
2. 单击 **+ 新建项目**以创建新项目，然后选择 **Jupyter 配置页**磁贴。项目为您创建一个空间，供您收集和共享配置页、连接至数据源、创建管道以及添加数据集。
3. 单击 **+ 添加到项目**下拉列表，并选择**连接**。从服务列表中，选择 **Cloudant**，接着输入在 {{site.data.keyword.cloudant_short_notm}} 服务页面的“服务凭证”选项卡中找到的 Cloudant URL、用户名和密码，然后在显示的字段中输入这些项。指定连接的名称，然后单击**创建**。
4. 检查该连接是否已在仪表板的“数据资产”部分下变为可用。
5. 单击 **+ 新建配置页**以创建新的 Jupyter 配置页。
6. 从“服务”下的“选择运行时”下拉列表中，选择 **spark-iw**。如果此项不存在，表示尚未正确供应 Apache Spark 服务。请在 IBM Cloud“仪表板”上检查 Apache Spark 服务是否存在，如果不存在，请访问服务页面以供应此服务。
7. 将“语言”设置为 **Python 2**，将 Spark 版本设置为 **2.1**。
8. 选择**从 URL** 以装入现有配置页，然后指定该配置页的描述性名称，并输入以下 URL 以打开样本配置页：
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. 单击**创建配置页**。检查配置页是否是使用元数据和代码创建的。
10. 选择以“!pip install --upgrade pixiedust”开头的单元，然后单击**运行**以运行代码。
11. 完成安装时，通过单击**重新启动内核**图标或从“内核”菜单中选择**重新启动**，重新启动 Spark 内核。
12. 等待大约 10 秒钟让内核重新启动，然后单击带有注释的空代码单元以将其选中。
13. 通过完成以下步骤，将 Cloudant 凭证导入到该单元：

    1. 单击 ![查找并添加数据](images/find_add_data_icon.png)。
    2. 选择**连接**选项卡。
    3. 单击**插入到代码**。
此时，将使用您的 Cloudant 凭证创建名为“credentials_1”的字典。如果名称未指定为“credentials_1”，请将该字典重命名为“credentials_1”，因为这是要运行的配置页代码所需的名称。
14. 在具有数据库名称 (dbname) 的单元中，输入作为数据源的 Cloudant 数据库的名称，例如 `iotp_yourWIoTPorgId_default_Year-month-day`。
15. 保存配置页。配置页已准备就绪，可以执行。


### 2. 运行分析
{: #run_analysis}

1.	选择包含 Cloudant 凭证的单元。
2.	单击**播放**，以执行单元代码。

3.	检查执行结果并分析每个单元中使用的 Python 代码。
4.	对每个单元重复步骤 2 和 3。对于指定了**需要用户输入**的单元，必须向下一个代码单元中定义的变量提供新的输入值，该代码单元才能执行。

**注：**某些单元运行后台 Spark 作业，可能需要更长时间才能完成。当单元中的代码执行完成时，星号 `*` 将变成一个数字，例如，In `[*]` 变为 In `[1]`。完成这些步骤后，您可以在 {{site.data.keyword.iot_short_notm}} 中创建云规则，以在检测到异常时自动生成警报。


<!-- ### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails. -->


## 后续步骤

有关 DSX 的更多信息，请参阅以下资源：

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [DSX社区配置页和教程 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window}，请遵循链接来了解更多有关 Jupyter 配置页的信息。
 - [Watson IoT Platform手册中的分析诀窍 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
