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

# 使用 Web 界面开始进行数据管理
{: #gs_web}

{{site.data.keyword.iot_full}} 提供联机工具来帮助您将数据标准化并作为数据管理功能部件的一部分进行转换。

## 概述
{: #web_overview}

除了提供的 [Watson IoT Platform 数据管理 API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 外，您还可以使用简单接口创建程序或高级接口创建程序，来配置资源并开始映射设备数据。联机接口创建者将指导您完成用户界面中的步骤。

物理接口用于对物理设备与 Watson IoT Platform 之间的接口进行建模。事件类型可以与物理接口相关联。逻辑接口用于将规范化视图定义到 Watson IoT Platform 中的设备状态，并且逻辑接口通常由应用程序使用。逻辑接口必须与逻辑接口模式相关联。状态会进行更新以对入站设备事件做出响应。

有关数据管理功能及其优点的更多信息，请参阅[数据管理简介](../GA_information_management/ga_im_device_twin.html#device_twins)。

有关需要配置的资源的更多信息，请参阅[了解数据管理](../GA_information_management/ga_im_definitions.html#definitions_resource)。

使用“简单接口创建程序”，您可以添加一个物理接口，然后系统会自动创建一个逻辑接口，并将这两个接口映射在一起。使用“高级接口创建程序”，您可以对配置进行更多控制。添加一个物理接口和一个或多个逻辑接口，然后在它们之间添加映射。

接口和映射会以草稿形式添加。添加并验证后，必须将其激活。有关草稿和活动资源的更多信息，请参阅[创建、更新、激活和取消激活资源](../GA_information_management/ga_im_definitions.html#draft_active_resources)。

接口库保存模板物理接口和逻辑接口，您可以复用这些接口。

## 高级流程
{: #interface_flow}

使用以下步骤来帮助您配置开始使用数据管理功能所需的资源。

### 开始之前

本过程假设您有一种设备类型，其中至少有一个已注册且已连接的设备。有关这些先决条件的更多信息以及有关如何创建设备类型和注册设备的指示信息，请参阅[连接设备](../iotplatform_task.html#iotplatform_task)。

### 步骤

1. 选择“简单接口创建程序”或“高级接口创建程序”：
  - **简单接口创建程序流程**
    1. [创建草稿物理接口](#create_physical_interface_simple)。
  - **高级接口创建程序流程**
    1. [创建草稿物理接口](#create_physical_interface_advanced)。
    2. [创建一个或多个草稿逻辑接口](#create_logical_interface)。（使用简单流程时会自动填写此步骤。）
    3. [将物理接口映射到逻辑接口](#create_interface_mappings)。
2. [设置通知首选项](#set_notifications)。
3. [激活配置](#validate_activate)。

您还可以使用 API 来配置“数据管理”功能。有关使用 API 配置“数据管理”功能以规范化并转换设备数据的详细信息，请参阅[数据管理入门](ga_im_example.html#im_example)。[逐步指南：有关如何通过公共接口使用设备的详细示例](../GA_information_management/ga_im_index_scenario.html#scenario)将指导您完成为异构温度计设备创建设备类型逻辑接口的步骤。有关 API 的详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 文档。
{: tip}


## 创建草稿物理接口（简单流程）
{: #create_physical_interface_simple}

1. 在主导航菜单中，单击**设备**。
2. 单击**设备类型**，然后选择要为其创建接口的设备类型。您还可以创建新的设备类型。请参阅[连接设备](../iotplatform_task.html#iotplatform_task)以获取更多信息。
3. 查看设备类型信息并单击**接口**。
4. 单击**简单流程**以打开“简单接口创建程序”。
5. 单击**创建接口**。
6. 单击**添加属性**以开始向物理接口添加事件和属性。
   a. 在“**向接口添加属性**”窗口中，选择要添加到接口的事件。系统将侦听所选设备类型的已连接设备的活动事件。您可以选择最后一个高速缓存的事件的设备标识，或从列表中选择另一个事件。
   b. 从事件中选择属性，然后单击**添加**。
   c. 根据需要添加更多属性。
7. 单击**完成**。物理接口已创建。如果要添加更多详细信息，可以通过选择**使用高级接口创建程序**链接来[升级到高级接口](#create_physical_interface_advanced)。


## 创建草稿物理接口（高级流程）
{: #create_physical_interface_advanced}

1. 在主导航菜单中，单击**设备**。
2. 单击**设备类型**，然后选择要为其创建接口的设备类型。您还可以创建新的设备类型。请参阅[连接设备](../iotplatform_task.html#iotplatform_task)以获取更多信息。
2. 查看设备类型信息并单击**接口**。
3. 单击**高级流程**以打开“高级接口创建程序”。
4. 选择下列其中一个选项：
 - 要创建新的物理接口，请单击**创建接口**。
 - 要使用库中的接口模板，请单击**从库添加**，选择接口，然后转至[创建草稿逻辑接口](#create_logic_interface)。
5. 在**创建物理接口**页面的**身份**选项卡上，输入物理接口的名称和描述。
6. 可选：如果要将启用了边缘的设备与接口配合使用，请打开边缘开关。有关边缘的更多信息，请参阅 [Edge Analytics](../edge_analytics.html#edge_analytics)。
7. 单击**下一步**。
8. 通过添加和修改事件类型及有效内容来定义物理接口：
   a. 从列表中选择事件类型以修改现有事件类型，或者单击**创建事件类型**。
   b. 选择最后一个高速缓存的事件、从列表中选择事件或手动添加事件，然后单击**添加**。
9. 单击**完成**。以草稿格式创建物理接口。您可以继续添加一个或多个逻辑接口。

## 创建草稿逻辑接口（高级流程）
{: #create_logical_interface}

1. 创建草稿物理接口后，选择下列其中一个选项：
 - 单击**添加逻辑接口**以创建新的逻辑接口。
 - 单击**从库添加**以使用接口库中的接口模板。选择接口，然后转至步骤 7。
2. 在**创建逻辑接口**页面的**身份**选项卡上，输入逻辑接口的名称和描述。
3. 单击**下一步**。
4. 单击**添加属性**以开始定义逻辑接口的属性。
5. 在**创建属性**窗口中，从物理接口中选择要添加至逻辑接口的属性，然后保存更改。
6. 单击**添加对象**。
7. 根据需要添加其他逻辑接口，然后单击**下一步**。
8. {: #set_notifications}设置接口的通知首选项。选择以下某个选项以确定何时希望发送设备状态通知：
 - 没有事件通知程序 - 不发送通知。可使用该 REST API 来检索设备状态更改信息。
 - 在状态更改时 - 设备状态更改时通知您。
 - 在发生每个事件时 - 每次平台处理设备的事件时都通知您，即使事件不会导致状态更改也是如此。
8. 单击**完成**。这将创建接口，并且 ![“草稿状态”图标](images/draft_icon.png) 会指示草稿状态。您可以继续修改或激活接口。

## 激活配置
{: #validate_activate}

创建草稿物理接口和逻辑接口后，必须激活配置。

- 单击**激活**以激活配置。配置状态会设置为“已激活”。
![活动部署](images/active_deployment.png)如果您对这些接口进行了更改，那么这些接口将再次成为草稿，并且您必须将其重新激活。
