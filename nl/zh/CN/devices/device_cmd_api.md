---

copyright:
 years: 2015, 2017
lastupdated: "2017-06-08"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 针对设备的 HTTP 消息传递 API (Beta)
{: #api}

**重要信息：**针对设备的 {{site.data.keyword.iot_full}} HTTP 消息传递 API 功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。


## 访问针对设备的 HTTP 消息传递 API 文档
{: #rest_messaging_api}

要访问 {{site.data.keyword.iot_short_notm}} HTTP 消息传递 API 文档，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP 消息传递 API ![外部链接图标](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}。


## 客户机连接
{: #client_connections}

有关客户机安全性以及如何将客户机连接到 {{site.data.keyword.iot_short_notm}} 中设备的信息，请参阅[将应用程序、设备和网关连接到 {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html)。

## 发布事件
{: #event_publication}

除了使用 MQTT 消息传递协议外，还可以配置设备，以使用 HTTP REST API 命令通过 HTTP 将事件发布到 {{site.data.keyword.iot_short_notm}}。

使用以下某个 URL 提交来自连接到 {{site.data.keyword.iot_short_notm}} 的设备的 `POST` 请求：

### 非安全 POST 请求
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### 安全 POST 请求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**注：**还可以为安全 HTTP API 调用指定端口 443（缺省 SSL 端口）。

### 认证

所有请求都必须包含授权头。基本认证是唯一受支持的方法。设备通过 {{site.data.keyword.iot_short_notm}} HTTP REST API 发起 HTTP 请求时，以下凭证是必需的：

|凭证|必需的输入|
|:---|:---|
|用户名|`use-token-auth`
|密码| 注册设备时自动生成或手动指定的认证令牌。

### Content-Type 请求头

必须为请求提供 `Content-Type` 请求头。下表显示了如何将受支持的类型映射到 {{site.data.keyword.iot_short_notm}} 内部格式。

|Content-Type 头|{{site.data.keyword.iot_short_notm}} 中的格式|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

## 接收命令
{: #receive_commands}

除了使用 MQTT 消息传递协议外，还可以配置设备，以使用 HTTP 消息传递 API 命令通过 HTTP 从 {{site.data.keyword.iot_short_notm}} 接收命令。设备可以接收自行导向的命令。

使用以下某个 URL 提交来自连接到 {{site.data.keyword.iot_short_notm}} 的设备的 `POST` 请求：

### 非安全 POST 请求
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### 安全 POST 请求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**注：**还可以为安全 HTTP API 调用指定端口 443（缺省 SSL 端口）。

要从 {{site.data.keyword.iot_short_notm}} 接收命令，请使用以下 API：

<pre class="pre"><code class="hljs">/device/types/{deviceType}/devices/{deviceId}/commands/{command}/request</code></pre>

您可以选择性地在 HTTP 请求的主体中包含 *waitTimeSecs* 参数，以指定整数，代表等待命令的最大秒数：
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**重要说明：**
- *waitTimeSecs* 的值必须是 0 - 3600 秒之间的整数。缺省值为 0。
- 要接收任何命令，请对 {command} 组件使用“任意”通配符 (+)。如果使用通配符，那么命令标识包含在响应头字段 *X-commandId* 中。
- 如果 HTTP 响应状态码为 200，那么命令数据包含在响应的主体中。请复查响应头字段 *Content-Type*，以找到内容类型。
- 如果 HTTP 响应状态码为 204，那么没有可用的命令数据。


有关开发设备的信息，请参阅[在 {{site.data.keyword.iot_short_notm}} 上开发设备](../devices/device_dev_index.html)。
