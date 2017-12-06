---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-03"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iot_short_notm}} 故障诊断
{: #ts}

此处提供了有关在 {{site.data.keyword.Bluemix_notm}} 上使用 {{site.data.keyword.iot_full}} 的常见故障诊断问题的解答。
{:shortdesc}

## 访问 {{site.data.keyword.iot_short_notm}} 组织时发生问题
{: #access-expiry-problem}

您无法登录到您拥有的 {{site.data.keyword.iot_short_notm}} 组织。
{:shortdesc}

您无法使用组织 URL 或使用 `https://internetofthings.ibmcloud.com` 直接登录到您的 {{site.data.keyword.iot_short_notm}} 组织。
{: tsSymptoms}

您对 {{site.data.keyword.iot_short_notm}} 组织的访问权可能已到期。缺省情况下，使用 {{site.data.keyword.Bluemix}} 创建的 {{site.data.keyword.iot_short_notm}} 组织使用临时用户概要文件。
{: tsCauses}

可以通过使用 {{site.data.keyword.Bluemix_notm}} 并更改您用户概要文件的到期设置来访问 {{site.data.keyword.iot_short_notm}} 组织，从而解决此问题。要更改用户到期设置，请执行以下操作：

1. 在 {{site.data.keyword.Bluemix_notm}}“仪表板”中，打开 {{site.data.keyword.iot_short_notm}} 服务。
2. 单击导航栏中的**成员**。
3. 单击**编辑**图标。
4. 清除**访问权到期**框，然后单击**保存**。
{: tsResolve}

## 连接到 {{site.data.keyword.iot_short_notm}} 时发生问题
{: #connection_problem}

您与 {{site.data.keyword.iot_short_notm}} 的连接意外断开。
{:shortdesc}

尝试连接到 {{site.data.keyword.iot_short_notm}} 时，设备或应用程序收到错误。
{: tsSymptoms}

可能有两个设备尝试使用相同的 clientID 和凭证进行连接。每个 clientID 仅允许一个唯一连接。不能有两个并行连接使用相同的标识。应用程序可共享相同 API 密钥，但 MQTT 需要客户机标识始终唯一。
{: tsCauses}

您可以通过确认不会有两个设备尝试使用相同凭证进行连接来解决此问题。
{: tsResolve}

## 设备从 {{site.data.keyword.iot_short_notm}} 间歇性断开连接
{: #disconnection_problem}

您设备到 {{site.data.keyword.iot_short_notm}} 的连接意外间歇性断开。
{:shortdesc}

连接到 {{site.data.keyword.iot_short_notm}} 服务的设备间歇性断开连接。设备会重新连接，但很快再次意外断开连接。
{: tsSymptoms}

可能是因为您在连接时，所使用的 MQTT ping 选项值太低，这导致看上去像连接超时。例如，如果客户机 MQTT 设置不正确，那么不会及时收到 ping，并且连接会关闭。
{: tsCauses}

您可以通过确认为连接正确设置 ping 和 KeepAlive 参数来解决此问题。   
{: tsResolve}

