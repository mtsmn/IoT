---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 针对网关设备的 HTTP API
{: #api_link}

有两种类型的 HTTP API 可以与 {{site.data.keyword.iot_full}} 配合使用。可以使用 HTTP REST API 来创建、更新、列出和删除网关设备。使用 HTTP 消息传递 API 可将事件信息从网关设备发送到云，也可以接受来自云中应用程序的命令。 


## 客户机连接
{: #client_connections}

有关客户机安全性以及如何将客户机连接到 {{site.data.keyword.iot_short_notm}} 中网关设备的信息，请参阅[将应用程序、设备和网关连接到 {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html)。

**注：**缺省情况下，HTTP 端口 1883 已禁用。有关更改缺省设置的信息，请参阅[配置安全策略](../reference/security/set_up_policies.html#set_up_policies.md)。


## 认证

所有请求都必须包含授权头。基本认证是唯一受支持的方法。设备使用 {{site.data.keyword.iot_short_notm}} HTTP REST API 发起 HTTP 请求时，以下凭证是必需的：

|凭证|必需的输入|
|:---|:---|
|用户名|`g/{orgId}/{gwType}/{gwDevId}` 或 `g-{orgId}-{gwType}-{gwDevId}`
|密码|注册网关设备时自动生成或手动指定的认证令牌。



其中：

<dl>
<dt>orgId</dt>  
<dd>组织名称，必须与主机头中指定的名称相匹配。</dd>

<p></p>
<dt>gwType</dt>  
<dd>网关的类型。</dd> 
<dd>如果您在用户名中使用连字符“-”字符作为分隔符，那么此值不得包含连字符字符。</dd>
<p></p>
<dt>gwDevId</dt>  
<dd>是网关的设备标识。</dd>
</dl>


## Content-Type 请求头

必须为请求提供 `Content-Type` 请求头。下表显示了如何将受支持的类型映射到 {{site.data.keyword.iot_short_notm}} 内部格式：

|Content-Type 头|{{site.data.keyword.iot_short_notm}} 中的格式|
|:---|:---|
|text/plain|"text"
|application/json|"json"
|application/xml|"xml"
|application/octet-stream|"bin"



## 服务质量

与 MQTT 服务质量“最多一次”传递服务级别 0 类似，HTTP REST 消息传递提供了非持久性消息传递，但它会验证请求是否正确，并在发送 HTTP 响应之前，验证其是否可以传递到服务器。包含 HTTP 状态码 200 的回复将确认消息已传递到服务器。使用“最多一次”MQTT 服务质量级别或 HTTP 同等级别来传递事件消息时，设备或应用程序必须实现重试逻辑以保证传递。

有关 {{site.data.keyword.iot_short_notm}} 的 MQTT 协议和服务质量级别的更多信息，请参阅 [MQTT 消息传递](../reference/mqtt/index.html)。

可以使用 HTTP 消息传递 API 将事件从网关设备发送到云，也可以接受来自云中应用程序的命令。

## 发布事件
{: #event_publication}

除了使用 MQTT 消息传递协议外，还可以配置网关设备，以使用 HTTP 消息传递 API 命令通过 HTTP 将事件发布到 {{site.data.keyword.iot_short_notm}}。

要从连接到 {{site.data.keyword.iot_short_notm}} 的设备提交 ``POST`` 请求，请使用以下其中一个 URL：

### 用于发布事件的非安全 POST 请求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### 用于发布事件的安全 POST 请求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**重要说明：**
- 还可以为安全 HTTP API 调用指定端口 443（缺省 SSL 端口）。
- 如果未向网关分配*标准网关*角色，那么它可以代表组织中的任何设备发布事件。如果连接到网关的设备未注册，网关会自动注册该设备。
- 如果想要检查设备授权级别，请分配*标准网关*角色。

有关网关和资源组角色的更多信息，请参阅[网关访问控制](../gateways/gateway-access-control.html)。

## 接收命令
{: #receive_commands}

除了使用 MQTT 消息传递协议外，还可以配置网关设备，以使用 HTTP 消息传递 API 命令通过 HTTP 从 {{site.data.keyword.iot_short_notm}} 接收命令。网关设备可以接收导向到其相关联资源组内设备的命令。有关网关资源组的更多信息，请参阅[网关访问控制](../gateways/gateway-access-control.html)。

使用以下某个 URL 提交来自连接到 {{site.data.keyword.iot_short_notm}} 的网关的 ``POST`` 请求：

### 用于接收命令的非安全 POST 请求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### 用于接收命令的安全 POST 请求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**注：**还可以为安全 HTTP API 调用指定端口 443（缺省 SSL 端口）。

您可以选择性地在 HTTP 请求的主体中包含 *waitTimeSecs* 参数，以指定整数，代表等待命令的最大秒数：
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**重要说明：**
- *waitTimeSecs* 的值必须是 0 - 3600 秒之间的整数。缺省值为 0。
- 要接收任何设备类型的命令，请对 `typeId` 组件使用“任意”通配符 (+)。如果使用通配符，那么设备类型包含在响应头字段 *X-deviceType* 中。
- 要接收任何设备的命令，请对 `deviceId` 组件使用“任意”通配符 (+)。如果使用通配符，那么设备标识包含在响应头字段 *X-deviceId* 中。
- 要接收任何命令，请对 `command` 组件使用“任意”通配符 (+)。如果使用通配符，那么命令标识包含在响应头字段 *X-commandId* 中。
- 如果 HTTP 响应状态码为 200，那么命令数据包含在响应的主体中。请复查响应头字段 *Content-Type*，以找到内容类型。
- 如果 HTTP 响应状态码为 204，那么没有可用的命令数据。

要访问 {{site.data.keyword.iot_short_notm}} HTTP REST API，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部链接图标](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}。

要访问 {{site.data.keyword.iot_short_notm}} HTTP 消息传递 API，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP 消息传递 API ![外部链接图标](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}。

## 上次事件高速缓存
{: #last-event-cache}

可以使用最后一个事件高速缓存来存储有关已连接设备发送到 {{site.data.keyword.iot_short_notm}} 的最后一个事件的信息。通过使用 [API ![外部链接图标](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}，可以检索设备特定事件标识的最后一个记录值，或检索特定设备报告的每个事件标识的最后一个记录值。 

### 配置最后一个事件高速缓存

缺省情况下，最后一个事件高速缓存功能已禁用，但可以通过以下方式启用该功能： 

-	使用 API 将 *enabled* 参数设置为 **true**。
-	在 {{site.data.keyword.iot_short_notm}} 仪表板的*设置*页面上，将**启用 LEC** 设置为**开启**。

事件数据在高速缓存中存储的时间长度通过生存时间 (TTL) 值指定。缺省情况下，任何特定事件的最后一个事件数据都会存储 7 天。可以通过使用 API 来更新 **ttlDays** 参数，或通过在 {{site.data.keyword.iot_short_notm}} 仪表板的*设置*页面上为**事件数据 TTL** 字段选择值来更改持续时间。

对于轻量套餐，可以将信息最短存储 1 天，最长存储 7 天。对于其他套餐，可以将信息最短存储 1 天，最长存储 45 天。

使用以下信息可帮助您使用 API 来配置最后一个事件高速缓存设置。

要检索组织的最后一个事件高速缓存的当前配置，请使用以下 API：

```
    GET /api/v0002/config/lec
```   

   
以下示例显示了对 GET 方法的成功响应：

```
{
    "enabled": false,
    "ttlDays": 7
}
```
组织中的任何人员或应用程序都有权访问此端点。 

如果 {{site.data.keyword.iot_short_notm}} 管理员禁用了组织的最后一个事件高速缓存，那么会在对 GET 方法的响应中返回以下“HTTP 403 已禁止”状态码：

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: Last event cache is disabled for this organization. For more information, contact your support team."
}
```

如果未启用最后一个事件高速缓存，那么会在对 GET 方法的响应中返回 HTTP 404 状态码。要为组织启用最后一个事件高速缓存功能，请使用以下 API：

```
    PUT /api/v0002/config/lec
```

使用以下示例有效内容： 
```
{
    "enabled": true,
    "ttlDays": 5
}
```
其中： 

- **enabled** 是必需的，用于指定是否为组织启用了最后一个事件高速缓存。该值必须是布尔值。 

- **ttlDays** 是可选的，用于指定事件在组织的最后一个事件高速缓存中保留的天数。该值必须是 1 到 45（包括 1 和 45）之间的整数。
   
 
只有管理员、操作员用户或操作员应用程序才有权访问此端点。如果参数或 JSON 无效，那么会在对 PUT 方法的响应中返回“HTTP 401 错误的请求”状态码。 

**注：**完成配置更改可能需要最长 30 秒时间。

### 从最后一个事件高速缓存中检索信息

通过使用 API，可以检索设备发送的最后一个事件。无论设备的物理位置或使用状态如何，都可以检索设备状态。您可以检索特定设备的事件标识的上次记录值，或检索特定设备报告的每个事件标识的上次记录值。对于过去最多 45 天内发生的任何特定事件，可检索设备的最后一个事件数据。

要请求特定事件标识的最新值，请使用以下 API 请求，此请求将返回“power”事件标识的上次记录值。

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

返回的响应为以下 JSON 格式：

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**注：**虽然 API 响应为 JSON 格式，但是可以采用任何格式写入事件有效内容。最后一个事件高速缓存 API 返回的有效内容以基本 64 位格式编码。

要请求设备报告的每个事件标识的最新值，请使用以下 API 请求：

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

响应包含设备发送的所有事件标识。在以下示例中，将返回“power”和“temperature”事件的值。

```
[
    {
        "deviceId": "<device-id>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

要访问 {{site.data.keyword.iot_short_notm}} 最后一个事件高速缓存 API，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP 信息管理 API ![外部链接图标](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}。
