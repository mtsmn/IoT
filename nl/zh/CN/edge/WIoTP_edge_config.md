---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 配置 {{site.data.keyword.iot_short_notm}} Edge（预览）
{: #edge_configure}

**重要信息：**{{site.data.keyword.iot_full}} Edge 功能只作为受限预览程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

必须将网关连接到 {{site.data.keyword.iot_short_notm}}，然后才能开始从连接到边缘节点的设备接收数据。将网关连接到 {{site.data.keyword.iot_short_notm}} 涉及创建网关设备类型和向 {{site.data.keyword.iot_short_notm}} 注册网关。然后，可以使用注册信息将网关设备连接到 {{site.data.keyword.iot_short_notm}}。


## 高级别配置流程

1. 在 {{site.data.keyword.iot_short_notm}} Edge（预览）测试环境中[创建组织](#edge_org)。
2. [启用 {{site.data.keyword.iot_short_notm}} 预览](#edge_enable)。
3. 通过[创建启用了边缘的网关](#edge_gw_type)、[添加网关设备](#edge_gw)并选择要在边缘节点上运行的边缘服务，配置组织以将边缘网关节点与 {{site.data.keyword.iot_short_notm}} 配合使用。您可以根据需要[创建定制服务](WIoTP_edge_dev.html#create_service)。
4. [配置边缘服务](#config_service)。
5. [在设备上安装 Edge](#edge_install_device)。
6. [管理和监视边缘服务](#monitor_service)。

## 创建组织
{: #edge_org}

通过完成以下步骤，在 {{site.data.keyword.iot_short_notm}} Edge（预览）测试环境中创建组织：
1. 注册 {{site.data.keyword.Bluemix_notm}} 帐户，并在您的 {{site.data.keyword.Bluemix_notm}} 组织中创建 {{site.data.keyword.iot_short_notm}} 服务的实例。可以直接从 [IBM Cloud 服务目录中的 {{site.data.keyword.iot_short_notm}} 页面 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}，创建 {{site.data.keyword.iot_short_notm}} 实例。  
2. 填写相关信息以供应 IBM Cloud 组织。**重要信息：**必须将 IBM Cloud 组织部署在**美国南部**位置，才能使用 {{site.data.keyword.iot_short_notm}} Edge（预览）。
3. 单击**创建**。
4. 在打开的 {{site.data.keyword.iot_short_notm}} 实例中，单击**启动**。这将创建新的 {{site.data.keyword.iot_short_notm}} 组织。组织标识会显示在 URL 的开头以及仪表板中您的用户名下。现在，您可为组织启用 {{site.data.keyword.iot_short_notm}} Edge。

## 启用 {{site.data.keyword.iot_short_notm}} Edge（预览）
{: #edge_enable}

缺省情况下，{{site.data.keyword.iot_short_notm}} Edge（预览）处于禁用状态。要启用 {{site.data.keyword.iot_short_notm}} Edge，请执行以下操作：

1. 在组织的 {{site.data.keyword.iot_short_notm}} 仪表板中，选择**设置**。
2. 在**试验性功能**部分中，激活试验性功能，包括 {{site.data.keyword.iot_short_notm}} Edge（预览）。
3. 刷新浏览器以在仪表板导航菜单中查看新的**边缘服务**功能和快捷方式。**注：**如果**边缘服务**图标未显示在仪表板导航菜单中，请确保为您的位置选择的是**美国南部**。

该标志会添加到本地浏览器存储器中。

## 创建启用了边缘功能的网关类型
{: #edge_gw_type}

连接到 {{site.data.keyword.iot_short_notm}} 的每个网关都必须与一种网关类型相关联。网关类型是共享公共特征的设备组。

1. 从 {{site.data.keyword.iot_short_notm}} 仪表板主导航菜单中，选择**设备**。
2. 在**设备类型**选项卡上，单击**添加设备类型**。
3. 在**添加类型**页面的**身份**选项卡上，选择**网关**作为类型，并输入网关类型名称（如 `my_gateway_type`）和网关类型的描述。**重要信息**：网关类型名称不得超过 36 个字符，仅可包含以下字符：
<ul>
 <li>字母数字字符（a-z、A-Z 和 0-9）</li>
 <li>连字符（-）</li>
 <li>下划线 (&lowbar;)</li>
 <li>句点 (.)</li>
 </ul>
3. 可选：输入网关类型属性和元数据。
4. 打开**边缘功能**按钮。
5. 选择网关类型的体系结构类型。体系结构类型必须与设备的体系结构相匹配。例如，Odroid 设备在 ARM64 体系结构上运行。
6. 单击**下一步**。
7. 可选：在**设备信息**选项卡上，输入与网关设备类型相关的其他详细信息和元数据。
8. 选择要添加到边缘设备的边缘服务。这会将 Core IoT 服务添加到所有设备，但您还可以通过完成以下步骤从目录添加其他服务：
 1. 单击**添加服务**。
 2. 在**添加边缘服务**窗口中，选择要添加的服务的相应版本，然后单击**完成**。
9. 在**添加网关类型**窗口中，单击**完成**。您可以继续到[向此网关类型添加网关设备](#edge_gw)。

## 添加网关设备
{: #edge_gw}

1. 从 {{site.data.keyword.iot_short_notm}} 仪表板主导航菜单中，选择**设备**。
2. 单击**添加设备**。
3. 在**身份**选项卡上，选择要向其添加设备的网关设备类型，并输入要添加的网关设备的唯一标识。设备标识用于在 {{site.data.keyword.iot_short_notm}} 仪表板中标识网关设备；设备标识也是用于将网关设备连接到 {{site.data.keyword.iot_short_notm}} 的必需参数。
**重要信息**：设备标识不得超过 36 个字符，仅可包含以下字符：
 <ul>
 <li>字母数字字符（a-z、A-Z 和 0-9）</li>
 <li>连字符 (-)</li>
 <li>下划线 (&lowbar;)</li>
 <li>句点 (.)</li>
 </ul>
 **提示：**对于连接网络的设备，设备标识可以是（例如）不带任何分隔冒号的设备 MAC 地址。
4. 单击**下一步**。
5. 可选：在**设备信息**选项卡上，输入与网关设备相关的其他详细信息和元数据。
6. 单击**下一步**。
7. 在**添加设备**页面的**安全性**选项卡上，自动生成认证令牌，或为此设备提供自己的认证令牌。如果选择创建自己的令牌，请确保令牌的长度介于 8 到 36 个字符之间，包含大小写字母、数字、连字符、下划线或句点的混合。此令牌不得包含重复的字符序列、字典单词、用户名或其他预定义序列。
9. 单击**下一步**。
10. 单击**完成**。
11. 在设备信息页面中，复制并保存以下设备信息：
 - 组织标识，例如 `tubo8x`
 - 设备类型，例如 `my_gateway_type`
 - 设备标识。**提示：**例如，对于网络连接的设备，这可能是不带任何分隔冒号的 MAC 地址。
 - 认证方法，例如 `token`
 - 认证令牌，例如 `PtBVriRqIg4uh)_-Kl`
  使用此信息将网关连接到 {{site.data.keyword.iot_short_notm}}，然后开始从连接到该网关的设备接收数据。

## 配置边缘服务
{: #config_service}

创建启用了边缘功能的新设备类型后，可以选择要在边缘节点中安装的其他服务。

1. 在 {{site.data.keyword.iot_short_notm}} 仪表板主导航菜单中，选择**设备**。
2. 单击**设备类型**。
3. 选择要配置的边缘设备类型。
4. 在**边缘服务**选项卡上，选择画笔图标以编辑记录。
6. 单击**添加边缘服务**以查看已为此组织上传的服务的列表。
7. 在**添加边缘服务**窗口中，选择要添加的服务的相应版本，然后单击**完成**。
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## 管理和监视边缘服务
{: #monitor_service}

安装边缘节点后，{{site.data.keyword.iot_short_notm}} 允许您监视在边缘节点上运行的服务的状态。

1.	在 {{site.data.keyword.iot_short_notm}} 仪表板主导航菜单中，选择**设备**。
2.	选择与边缘节点相对应的设备
3.	单击**边缘服务**以查看在边缘节点上安装的服务的状态。

## 查找更多信息
{: #more_info}

有关将网关连接到 {{site.data.keyword.iot_short_notm}} 的信息，请参阅[网关的 MQTT 连接](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt)。

有关 {{site.data.keyword.iot_short_notm}} 的更详细信息，请参阅 [Watson IoT Platform 入门](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp)。

有关 {{site.data.keyword.iot_short_notm}} Edge 的更多信息，请参阅以下主题：
- [{{site.data.keyword.iot_short_notm}} Edge 概述](WIoTP_edge.html#edge_overview)
- [在边缘设备上安装 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_install.html#edge_install_device)
- [针对 {{site.data.keyword.iot_short_notm}} Edge 进行开发](WIoTP_edge_dev.html#edge_dev)
