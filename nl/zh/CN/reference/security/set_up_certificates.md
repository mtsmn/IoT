---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-15"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# 配置证书
{: #set_up_certificates}

证书用于设备认证，或者替换 MQTT 消息传递的缺省 {{site.data.keyword.iot_full}} 服务器证书。没有有效签名证书的任何设备将被拒绝访问，无法与服务器通信。

要为设备配置证书和服务器访问权，系统操作员应将关联的认证中心 (CA) 证书注册到 {{site.data.keyword.iot_short_notm}} 平台，还可以选择将消息服务器证书注册到该平台。

有关使用 API 来管理 CA 证书和消息传递服务器证书的信息，请参阅 [IBM Watson IoT Platform 认证和授权 API ![外部链接图标](../../../../icons/launch-glyph.svg)（外部链接图标）](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}。

## CA 证书
通过 CA 证书，组织可以将设备上的客户机证书识别为可信证书，以便设备可以连接到服务器。

如果您添加 CA 证书或者替换消息传递服务器证书，那么所有设备必须使用支持服务器名称指示 (SNI) 的 MQTT 客户机进行连接，以便服务器可以使用相应的 CA 来认证设备。

您可以通过配置连接安全策略，来设置连接安全级别。有关如何执行此操作的更多信息，请参阅[配置安全策略](set_up_policies.html)。

## 客户机证书

单独的客户机（设备或网关）证书会保留在设备上，不会上传到平台。用于对所有设备和网关证书签名的 CA 签名证书是您上传到平台的唯一证书。如果是使用自签名消息传递服务器证书，那么必须上传用于对客户机证书 (cert.pem) 签名的根和中间证书。

使用 CA 证书签名的单个设备或网关证书必须将设备标识或网关标识输入为证书中的 Common Name (CN) 或 SubjectAltName。

对于设备来说，**CN** 字段格式为 `CN=d:devtype:devid`，而 **SubjectAltName** 字段格式为 `SubjectAltName=email:d:*devtype:devid*`，其中 `email:d` 是常量，`*devtype*` 是设备的设备类型，`*devid*` 是平台中设备的标识。

对于网关来说，**CN** 字段格式为 `CN=g:typeId:deviceId`，而 **SubjectAltName** 字段格式为 SubjectAltName=email:g:*devtype:devid*

注：对于设备或网关证书，请勿在 **CN** 或 **SubjectAltName** 字段中包含 `orgId`。`orgId` 应该作为连接到消息传递服务器时，客户机实现所提供的 SNI 信息的一部分提供。

有关客户机证书的更多信息，请参阅[使用客户机端证书将 Raspberry Pi 连接到 IBM Watson IoT Platform 诀窍 ![外部链接图标](../../../../icons/launch-glyph.svg)（外部链接图标）](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}

## 消息传递服务器证书

消息传递服务器证书接受缺省域 internetofthings.ibmcloud.com。证书 CN 或 SubjectAltName 必须遵循以下格式：

- `orgId.messaging.internetofthings.ibmcloud.com`（IoTP 域）

以下示例显示服务器证书的有效 CN：

`mtxpd0.messaging.internetofthings.ibmcloud.com`

有关消息传递服务器证书的更多信息，请参阅[使用自签名服务器证书将 Raspberry Pi 连接到 IBM Watson IoT Platform 诀窍 ![外部链接图标](../../../../icons/launch-glyph.svg)（外部链接图标）](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}

### 定制域 (Beta)
{: #custom_domains}

**重要信息**：用于消息传递服务器证书的定制域功能只作为受限 Beta 程序的一部分提供。要启用定制域，请在**设置**页面上开启**试验性功能**。

作为 Beta 功能的一部分，消息传递服务器证书接受定制域。证书 CN 或 SubjectAltName 必须遵循以下格式：

- `orgId.messaging<custom domain>`

对于定制域，**CN** 字段接受通配符，如以下示例所示：

- `CN=*.messaging.fab-iot.com`

**重要信息**：对于定制域，需要外部 DNS 服务才能将定制域解析到 {{site.data.keyword.iot_full}} 消息传递服务器。平台不提供此 DNS 服务。

## 注册认证中心 (CA) 证书进行设备认证 
{: #reg_ca_cert}

1. 登录到 {{site.data.keyword.iot_short_notm}}，并浏览至**常规设置**。
2. 在**安全性**部分的 **CA 证书**下，单击**添加证书**。
3. 浏览以选择要上传的证书文件，或者将文件拖放到**添加证书**窗口中。该文件中只能有一个证书，并且证书日期必须有效。仅接受格式为 .pem 或 .der 的证书。您可以预览所选文件中的证书信息。
4. 输入证书文件的描述。
5. 确认已选择正确的文件，然后单击**保存**。所选证书会在表中列出，缺省情况下，该证书处于活动状态。

## 注册消息传递服务器证书
{: #reg_msg_cert}

平台随附了缺省服务器证书。您可以使用缺省证书或者从组织上传一个证书。如果您尚没有要使用的证书，您可以创建新证书请求。接收到新证书之后，必须进行签名，然后上传回平台。

**注：**平台仪表板页面可建立与消息传递服务器的内部连接以检索设备信息。为组织设置自签名服务器证书时，仪表板用户必须在其浏览器中将服务器证书添加为可信证书，以避免连接问题，因为缺省情况下，浏览器不会将消息传递服务器识别为可信服务器。

## 从组织上传消息传递服务器证书
{: #upload_cert}
1. 在**常规设置**的**安全性**部分中的**消息传递服务器证书**下，单击**添加证书**。
2. 浏览以选择要上传的证书文件，或者将文件拖放到**添加证书**窗口中。
3. 浏览以选择要上传的专用密钥文件，或者将文件拖放到**添加证书**窗口中。
4. 如果专用密钥已使用口令进行加密，请输入专用密钥口令。
5. 确认已选择正确的文件，然后单击**保存**。

## 激活消息传递服务器证书

要激活缺省证书或者是已经上传的其他证书，请从**消息传递服务器证书**下的表中的**缺省消息传递服务器证书**下拉列表中选择要使用的证书。所选证书将作为活动证书列示在表中。

## 请求新消息传递服务器证书
{: #request_cert}

如果要使用新的消息传递服务器证书，可以生成证书签名请求 (CSR) 来请求新证书。

1. 在**常规设置**的**安全性**部分中的**消息传递服务器证书**下，单击**生成 CSR**。
2. 输入详细信息，以为服务器请求 CSR，然后单击**生成**。CSR 将显示在表中。
3. 下载请求，并将其提交到认证中心以进行签名。
4. 在您获取证书之后，返回到表中的 CSR 条目，并上传新证书。证书上传后，表中的 CSR 将替换为已上传的证书。
