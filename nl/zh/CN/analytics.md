---

copyright:
  years: 2016, 2018
lastupdated: "2017-03-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 实时 IoT 数据分析
{: #analytics}  

**重要信息：**我们即将推出的 Beta 版中提供了对 IoT 设备数据定义规则的新方法，这是更为广泛的变更计划的一部分，旨在改进 {{site.data.keyword.iot_full}} 交付规则和操作的方式。

要了解更多信息，请查看博客帖子 [An alternative approach to defining Rules on IoT data ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}。

要开始定义您自己的规则，请查看[创建嵌入式规则 (Beta)](information_management/im_rules.html) 文档。

## 关于实时 IoT 数据分析

使用 Watson {{site.data.keyword.iot_short_notm}} Analytics 可从设备生成的原始数据中获取所需的实时分析信息。  
{: shortdesc}

通过使用[板和卡](data_visualization.html)，可以查看表示来自一个或多个设备的数据集值的图形，以快速概览和了解设备数据。

通过分析规则，可指定用于触发操作的条件。例如，可以创建一条规则，用于在用户设备落下或设备温度达到峰值时，向该设备上的仪表板发送警报并向管理员发送电子邮件。

使用[云规则](cloud_analytics.html)可为直接连接到云中 {{site.data.keyword.iot_short_notm}} 的设备触发规则；使用[边缘规则](edge_analytics.html)可为连接到支持边缘分析的网关的设备触发规则。
