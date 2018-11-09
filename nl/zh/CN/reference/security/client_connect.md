---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 监视客户机连接 (Beta)
{: #connect_status}

通过 {{site.data.keyword.iot_full}}，可以查询和监视客户机的连接状态。您可以实时了解客户机是否已连接，并且可以对任何连接问题进行故障诊断。{:shortdesc}

例如，可以查看客户机上次处于活动状态的时间以及该客户机断开连接的原因，或者可以查看过去一天未连接的所有客户机，以便您可以解决任何问题。

**重要信息：**监视客户机连接功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## 查询客户机连接状态
{: #query_status}

通过客户机连接状态 API，可以检索和查询与 {{site.data.keyword.iot_short_notm}} 连接的任何客户机的连接状态。

查询连接状态时，可以查看以下信息：

 - 连接状态（已连接或已断开连接）
 - 上次连接的时间（ISO 8601 格式）
 - 上次断开连接的时间（ISO 8601 格式）
 - 最后一个活动事件（如 MQTT 发布或 MQTT 预订）
 - 最后一个活动的时间（ISO 8601 格式）
 - 如果客户机已断开连接，将显示导致断开连接的实体（客户机、服务器或网络）
 - 如果客户机已断开连接，将显示断开连接的 MQTT 原因码

**示例**

检索单个客户机的客户机连接状态：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

检索所有客户机的连接状态：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

检索所有已连接的客户机：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

检索过去两天内处于活动状态的所有客户机：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**注：**客户机连接的时间和 API 中的连接状态反映正确状态的时间之间可能存在短暂的延迟。

有关该 API 的更多信息（包括所有可用的查询过滤器），请参阅[客户机连接状态 API Swagger 文档 ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}。

## 监视客户机连接状态
{: #monitor_status}

 所有客户机都有一个监视主题，使您能够预订以接收客户机的实时连接状态更新。有关更多信息，请参阅[预订设备状态消息](../../applications/mqtt.html#subscribe_device_commands)。

## 对客户机连接问题进行故障诊断

通过连接日志 API，可以检索设备的连接日志事件列表，以诊断连接问题。这些条目会记录成功连接、失败连接尝试次数、有意断开连接和服务器发出的断开连接。

有关更多信息，请参阅[连接日志 API Swagger 文档 ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}。
