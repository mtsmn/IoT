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

# 使用 Web 界面 (Beta) 开始进行数据管理
{: #gs_web}

**重要信息：**{{site.data.keyword.iot_full}} 数据管理 Web 接口功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} 提供联机工具来帮助您将数据标准化并作为数据管理功能部件的一部分进行转换。除了提供的 [Watson IoT Platform 数据管理 API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 外，您还可以使用简单接口创建程序或高级接口创建程序，来配置资源并开始映射设备数据。联机接口创建者将指导您完成用户界面中的步骤。

有关数据管理功能及其优点的信息，请参阅[数据管理简介](../GA_information_management/ga_im_device_twin.html#device_twins)。

有关需要配置的资源的更多信息，请参阅[了解数据管理](../GA_information_management/ga_im_definitions.html#definitions_resource)。

使用“简单接口创建程序”，您添加自动创建的一个物理接口和一个逻辑接口，且这两个会映像在一起。使用“高级接口创建程序”，您可以对配置进行更多控制。添加一个物理接口和一个或多个逻辑接口，然后在它们之间添加映射。

接口和映射会以草稿形式添加。添加并验证后，必须将其激活。有关草稿和活动资源的更多信息，请参阅[创建、更新、激活和取消激活资源](../GA_information_management/ga_im_definitions.html#draft_active_resources)。



## 高级流程
{: #interface_flow}

使用以下步骤来帮助您配置开始使用数据管理功能所需的资源。

**开始之前**
本过程假设您具有至少有一个已连接和已注册设备的设备类型。有关这些先决条件的更多信息以及有关如何创建设备类型和注册设备的指示信息，请参阅[配置设备以与数据管理功能配合使用](im_config_devices.html)。

1. 选择要规范化并转换数据的设备类型。
2. 选择“简单接口创建程序”或“高级接口创建程序”：

a. 简单流程  
   i. [创建草稿物理接口](#create_physical_interface_simple)。  
   
b. 高级流程  
   i. [创建草稿物理接口](#create_physical_interface_advanced)。  
   ii. [创建一个或多个草稿逻辑接口](#create_logical_interface)。（使用简单流程时会自动填写此步骤。）  
   iii. [将物理接口映射到逻辑接口](#create_interface_mappings)。  
     
     
3. [设置通知首选项](#set_notifications)。
4. [激活配置](#validate_activate)。

*注：*您还可以使用 API 来配置“数据管理”功能。有关使用 API 配置“数据管理”功能以规范化并转换设备数据的详细信息，请参阅[数据管理入门]{ga_im_example.html#im_sexample}。[逐步指南：有关如何通过公共接口使用设备的详细示例](../GA_information_management/ga_im_index_scenario.html#scenario)将指导您完成为异构温度计设备创建设备类型逻辑接口的步骤。

有关 API 的详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 文档。



## 创建草稿物理接口（简单流程）
{: #create_physical_interface_simple}

1. 从主导航菜单中，选择**设备**。
2. 选择**设备类型**，然后选择要为其创建接口的设备类型。您也可以[创建新的设备类型]。
3. 根据需要查看和更新设备类型信息并保存任何更改。
4. 单击**接口**。
5. 单击**简单流程**以打开“简单接口创建程序”。
6. 单击**创建接口**。
7. 单击**添加属性**以开始向物理接口添加事件和属性。  
 a. 在“**向接口添加属性**”窗口中，选择要添加到接口的事件。系统将侦听所选设备类型的已连接设备的活动事件。您可以选择最后一个高速缓存的事件的设备标识，或从列表中选择另一个事件。  
  b. 从事件中选择属性，然后单击**添加**。  
  c. 根据需要添加更多属性。
8. 单击**完成**。物理接口已创建。如果要添加更多详细信息，可以通过选择**使用高级接口创建程序**链接来升级接口。


## 创建草稿物理接口（高级流程）
{: #create_physical_interface_advanced}

1. 从主导航菜单中，选择**设备**。
2. 选择**设备类型**，然后选择要为其创建接口的设备类型。您也可以[创建新的设备类型]。
2. 查看设备类型信息并单击**接口**。
4. 单击**高级流程**以打开“高级接口创建程序”。
5. 单击**创建接口**
7. 在**创建物理接口**页面的**身份**选项卡上，输入物理接口的名称和描述。
7. 可选：如果要将启用了 Edge 的设备与接口配合使用，请打开 Edge Analytics 开关。
8. 单击**下一步**。
9. 通过添加事件类型和有效内容来定义物理接口：
 a. 单击**创建事件类型**。 
 b. 选择最后一个高速缓存的事件、从列表中选择事件或手动添加事件。
10. 单击**完成**。以草稿格式创建物理接口。您可以继续添加一个或多个逻辑接口。

## 创建草稿逻辑接口
{: #create_logical_interface}

1. 创建草稿物理接口后，单击**添加逻辑接口**。
2. 在**创建逻辑接口**页面的**身份**选项卡上，输入逻辑接口的名称和描述。
3. 单击**下一步**。
4. 单击**添加属性**以开始定义逻辑接口的属性。
5. 在**创建属性**窗口中，从物理接口中选择要添加至逻辑接口的属性，然后保存更改。
6. 根据需要添加其他逻辑接口，然后单击**下一步**。
7. {: #set_notifications}设置接口的通知首选项。选择以下某个选项以确定何时希望发送设备状态通知：
 - 没有事件通知程序 - 不发送通知。可使用该 REST API 来检索设备状态更改信息。
 - 在状态更改时 - 设备状态更改时通知您。
 - 在发生每个事件时 - 每次平台处理设备的事件时都通知您，即使事件不会导致状态更改也是如此。
8. 单击**完成**。以草稿格式创建接口。
9. {: #validate_activate} 单击**部署**以验证配置。
10. 单击**部署**以激活配置。配置状态设置为“已部署”。 

