---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# {{site.data.keyword.iot_short_notm}} 入门指南
{: #getting-started}

“入门指南”旨在指导您全面了解使用 {{site.data.keyword.iot_full}} 开发现成端到端 IoT 原型系统的基本知识，编写这些指南是为了供 {{site.data.keyword.iot_short_notm}} 的新手开发者使用。
{:shortdesc}

每个指南都提供了一个逐步的过程，可快速引导您了解已部署并在运行的解决方案，其演示了一个或多个 {{site.data.keyword.iot_short_notm}} 功能。

## 使用指南  
{: #using-guides}
在每个指南中，您至少获得一个根据 {{site.data.keyword.iot_short_notm}} 最佳做法进行编码的 GitHub 源样本。这些样本可进行扩展，安全地与系统集成，并且通常适合您的 devOps 开发和构建过程。

从技术角度来说，可以通过将样本代码调整为适合自己的环境，来扩展快速解决方案，或者通过更紧密地模拟您在生产环境中遇到的硬件示例，来补充快速解决方案。这些指南提供了指向任务相关文档、API 文档、客户机库 (SDK)、教学视频和其他培训材料的其他链接。

您可以按照顺序执行指南，其中第一个指南提供了后续指南的基础。如果要跳过并遵循自己的路径来了解系列指南，那么每个指南还随附了需求列表。这些需求告诉您这些指南需要的设备数据的格式，并且您可以相应地设置自己的环境。
{: tip}

## 指南列表
{: #list-of-guides}  

提供了以下指南：

|指南|描述|    
| ----- | ---- |   
|[1. 连接传送带设备](getting-started-iot-conveyor.html) |通过安装 {{site.data.keyword.iot_short_notm}} 组织开始，然后连接来自样本 GitHub 的传送带模拟应用程序，或者如果您更喜欢一些更实际的东西，那么可以使用自己的基于 Raspberry-Pi 的传送带模拟器。</br> 然后，设置一些基本的 {{site.data.keyword.iot_short_notm}} 仪表板卡，以可视化和监视设备数据。|   
|[2. 监视设备数据](getting-started-iot-monitoring.html)|连接 Node.js 或基于窗口小部件库的监视应用程序，以实时查看传送带数据。  
|[3. 模拟大量设备](getting-started-iot-large-scale-simulation.html)|通过向环境中添加大量设备模拟器来扩展简单设备模拟。</br>**重要信息：**应用程序需要 512 MB 内存，缺省情况下会超过分配的内存量，同时也会超出免费试用帐户的可用量，因此必须首先升级到“预订”或“现买现付”帐户。|   
