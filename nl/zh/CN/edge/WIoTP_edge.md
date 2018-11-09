---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} Edge 概述（预览）
{: #edge_overview}

**重要信息：**{{site.data.keyword.iot_full}} Edge 功能只作为受限预览程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} Edge 支持您将 {{site.data.keyword.iot_short_notm}} 功能扩展到边缘设备。边缘设备位于组织内的网络边缘上。例如，某个物理位置（如工厂）的传感器和工业控制器。通过 {{site.data.keyword.iot_short_notm}} Edge，数据可以在设备内部进行处理后，再发送到云。

要使用 {{site.data.keyword.iot_short_notm}} Edge，请创建边缘节点，并使用要在其中运行的服务来配置这些节点。然后，WIoTP Edge 会将这些服务部署到节点。边缘服务在其中运行时，边缘节点可以通过边缘网关从设备发送消息。网关会处理消息，转换数据，然后根据接口的配置方式将数据发送到云。

{{site.data.keyword.iot_short_notm}} Edge（预览）提供了 Core IoT 缺省服务，以及创建定制服务（例如，用于预测制造扫地机器人中故障的服务）并发送故障通知的功能。例如，边缘设备可为传感器，用于监视 CPU 使用率并在使用率超过特定百分比时发送警报。

## {{site.data.keyword.iot_short_notm}} Edge 组件

**边缘节点**
边缘节点由位于网络边缘的网关和设备组成。

**边缘设备**
位于网络边缘的设备（例如，传感器），用于运行服务以处理数据后再将数据发送到云。例如，边缘设备可为传感器，用于监视 CPU 使用率并在使用率超过特定百分比时发送警报。

**边缘网关**
网关是专用设备，集成了应用程序和设备的功能，从而可充当其他设备的访问点。启用了边缘后，边缘网关支持无法直接连接到因特网的设备通过先连接到 Edge 上的网关设备来访问 {{site.data.keyword.iot_short_notm}} 服务。

**边缘网关类型**
网关类型对共享属性（例如，型号或位置）的网关设备进行分组。如果启用了边缘功能，并且向 {{site.data.keyword.iot_short_notm}} 添加了边缘网关设备，那么会将边缘网关类型中的属性用作新边缘网关设备的模板。

**边缘服务**
服务是添加到 {{site.data.keyword.iot_short_notm}} Edge 网关并执行数据处理等流程的功能。您可以构建使用这些服务的应用程序。

**边缘服务目录**
缺省 Core IoT 服务包含在该目录中，并已添加到所有边缘节点。您可以根据自己的业务需求添加定制服务。边缘服务目录包含所有可用的定制服务。

**文件服务器存储库**
文件服务器存储库保存 Docker 容器和映像。所有边缘处理功能（即服务）都在 Docker 容器中工作。您可选择要在设备内部运行的 Docker 映像。

## 查找更多信息
{: #more_info}

有关配置、安装和开发 {{site.data.keyword.iot_short_notm}} Edge 的更多信息，请参阅以下主题：
 - [配置 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
 - [在边缘设备上安装 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_install.html#edge_install_device)
 - [针对 {{site.data.keyword.iot_short_notm}} Edge 进行开发](WIoTP_edge_dev.html#edge_dev)
