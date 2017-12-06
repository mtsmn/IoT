---
copyright:
  years: 2016, 2017
lastupdated: "2017-09-06"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 计费

付费 {{site.data.keyword.iot_full}} 服务套餐（非“Lite”套餐）以一个月内交换和分析的兆字节概念为基础。本文档详细描述了 {{site.data.keyword.iot_short_notm}} 如何计量数据，来创建使用情况信息，以确定使用服务的成本。使用情况信息可用来根据设备、应用程序和网关的设计和数量，来估计使用 {{site.data.keyword.iot_short_notm}} 的成本。

有关已交换或分析的每兆字节数据的成本的信息，请参阅 Bluemix 目录中针对所需区域的 {{site.data.keyword.iot_short_notm}} 服务。

您可以使用[定价计算器 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://iot-cost-calculator.ng.bluemix.net/) 来帮助您计算 {{site.data.keyword.iot_short_notm}} 服务的成本。

以下三个项目分别进行测量，并根据使用情况收费： 
- 已交换的数据
- 已分析的数据
- 已分析的边缘数据

## 已交换的数据
*已交换的数据*计算包括通过使用 MQTT 或 HTTP 消息传递，由应用程序、设备和网关交换的帐户数据，以及通过使用 HTTP API，由应用程序交换的数据。

### 已交换的 MQTT 数据
使用 MQTT 时，TCP 流内容将计入*已交换的数据*。任何 MQTT 消息的有效内容都包含在使用 MQTT 协议进行交换的数据的累计中。当发布消息，以及传递消息时，就会累计有效内容数据大小。

MQTT 协议设计为尽可能轻量级。虽然在累计*交换的数据*时，{{site.data.keyword.iot_short_notm}} 会包括 MQTT 软件包的大小，但是由于它是轻量级协议，所以额外开销很低。下表显示了每个 MQTT 软件包的最小大小：

|MQTT 软件包|最小大小（字节数）|变量和关联的最小大小（字节数）|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |客户机标识（如果是 d:xxxxxx:t:i，则为 12），如果设置，则主题 (0)，消息 (0)，用户名（对于设备为 14 -“use-token-auth”）和密码（如果自动生成，则为 18）|
|CONNACK                        |4|无|
|PUBLISH                        |21                  |主题名称（如果是 iot-2/evt/i/fmt/f，则为 17），有效内容（最小值为 0）。适用于入站和出站 PUBLISH 软件包。|
|PUBACK                         |4|无|
|PUBREC                         |4|无|
|PUBREL                         |4|无|
|PUBCOMP                        |4|无|
|SUBSCRIBE                      |26                  |可以包括多个主题过滤器（如果是 iot-2/evt/i/fmt/f，则为 19）和 QoS (1)|
|SUBACK                         |5|在 SUBSCRIBE 中每个主题过滤器为 (1)|
|UNSUBSCRIBE                    |23                  |可以包括多个主题过滤器（如果是 iot-2/evt/i/fmt/f，则为 19）|
|UNSUBACK                       |4|无|
|PINGREQ                        |2|无|
|PINGRESP                       |2|无|
|DISCONNECT                     |2|无|

使用 TLS 时，还要考虑安全的握手。握手大概要 8 kb。因此，保持 MQTT 连接尽可能长会更经济高效。在每个保持活动时间间隔，打开连接仅产生 PINGREQ 和 PINGRESP 额外开销（4 个字节），该时间间隔是特定于客户机的，并且取决于指定的保持活动周期。使用现有 TLS 会话来重新打开 TLS 连接会减少交换的字节数，因为证书不会以任一方向发送。缺省情况下，许多 TLS 客户机都支持此操作，但会话的生存期有限。使用客户机证书会增加 TLS 握手的大小，具体取决于客户机证书的大小。 

不考虑 TLS 记录以及 TCP 和 IP 开销。

**注**：使用 {{site.data.keyword.iot_short_notm}} 仪表板查看设备时，会进行 MQTT 预订，以便仪表板可以显示实时事件。此 MQTT 预订也会计入交换的数据总量。

### 已交换的 HTTP 消息传递数据
使用 HTTP 消息传递时，如果发布事件或接收命令，那么*已交换的数据*的累计中会包括消息有效内容。

与 MQTT 一样，HTTP 开销也计算在内。HTTP 开销明显高于 MQTT 开销。每个请求的 HTTP 开销大约为 300 个字节。还需要加入消息有效内容大小。

使用 TLS 时，会考虑安全握手。握手大概要 8 kb。因此，保持 HTTPS 连接尽可能长会更经济高效。打开连接在*已交换的数据*方面不会产生额外的开销。使用现有 TLS 会话来重新打开 TLS 连接会减少交换的字节数，因为证书不会以任一方向发送。缺省情况下，许多 TLS 客户机都支持此操作，但会话的生存期有限。使用客户机证书会增加 TLS 握手的大小，具体取决于客户机证书的大小。

不考虑 TLS 记录以及 TCP 和 IP 开销。

### 已交换的 TTP API 数据
当应用程序调用任何 API 时，请求和响应主体都会对*已交换的数据*进行累计。每个数据的大小根据 API 的不同而有所不同。

与 MQTT 和 HTTP 消息传递不同，HTTP 开销和 TLS 开销都不考虑已交换的 HTTP API 数据。

**注**：使用 {{site.data.keyword.iot_short_notm}} 仪表板时，仪表板将使用这些 HTTP API 来列示包括设备、设备类型和设备连接日志在内的信息。这些 HTTP API 将计入*已交换的数据*中。

## 已分析的数据
*已分析的数据*计算用于测量平台内规则引擎所处理的事件数据。当根据特定设备和事件类型，由一个或多个规则评估设备事件时，会认为规则引擎已处理数据。 

## 已分析的边缘数据
*已分析的边缘数据*计算用于测量 {{site.data.keyword.iot_short_notm}} Edge Analytics Agent 在网关设备上处理的事件数据。当根据特定设备和事件类型，由一个或多个边缘规则评估设备事件时，会认为边缘代理程序已处理数据。 
