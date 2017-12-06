---

copyright:
 years: 2015, 2017
lastupdated: "2017-10-04"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 裝置的 HTTP 傳訊 API
{: #api}


## 存取裝置的 HTTP 傳訊 API 文件
{: #rest_messaging_api}

若要存取「{{site.data.keyword.iot_short_notm}} HTTP 傳訊 API」文件，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP 傳訊 API ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}。


## 用戶端連線
{: #client_connections}

如需用戶端安全以及如何將用戶端連接至 {{site.data.keyword.iot_short_notm}} 中裝置的相關資訊，請參閱[將應用程式、裝置及閘道連接至 {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html)。

## 發佈事件
{: #event_publication}

除了使用 MQTT 傳訊通訊協定之外，您還可以使用 HTTP REST API 指令來配置裝置，讓它透過 HTTP 將事件發佈至 {{site.data.keyword.iot_short_notm}}。

請使用下列其中一個 URL，來提交來自連接至 {{site.data.keyword.iot_short_notm}} 之裝置的 ``POST`` 要求：

### 未受保護的 POST 要求
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### 安全的 POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**附註：**您也可以指定埠 443（預設 SSL 埠）來進行安全 HTTP API 呼叫。

### 鑑別

所有要求都必須包含授權標頭。基本鑑別是唯一支援的方法。裝置透過 {{site.data.keyword.iot_short_notm}} HTTP REST API 提出 HTTP 要求時，需要下列認證：

|認證|必要輸入|
|:---|:---|
|使用者名稱|`use-token-auth`
|密碼| 在登錄裝置時自動產生或手動指定的鑑別記號。


### Content-Type 要求標頭

如果內容不是 JSON，則 `Content-Type` 要求標頭應該隨要求一起提供。下表顯示支援的類型如何對映至 {{site.data.keyword.iot_short_notm}} 內部格式。

|Content-Type 標頭|{{site.data.keyword.iot_short_notm}} 中的格式|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml| "xml"
|application/octet-stream|"bin"

## 接收指令
{: #receive_commands}

除了使用 MQTT 傳訊通訊協定之外，您也可以使用「HTTP 傳訊 API」指令來配置裝置，透過 HTTP 接收來自 {{site.data.keyword.iot_short_notm}} 的指令。裝置可以接收指向自己的指令。

請使用下列其中一個 URL，來提交來自連接至 {{site.data.keyword.iot_short_notm}} 之裝置的 ``POST`` 要求：

### 未受保護的 POST 要求
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### 安全的 POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**附註：**您也可以指定埠 443（預設 SSL 埠）來進行安全 HTTP API 呼叫。

您可以選擇性地在 HTTP 要求主體中包括參數 *waitTimeSecs* 來指定一個整數，這個整數代表等待指令的秒數上限：
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**重要注意事項：**
- *waitTimeSecs* 的值必須是 0 - 3600 秒範圍內的整數。預設值為 0。
- 若要接收任何指令，請針對 {command} 元件使用「任何」萬用字元 (+)。如果使用萬用字元，則會在回應標頭欄位 *X-commandId* 中包含指令 ID。
- 如果 HTTP 回應狀態碼是 200，則會在回應主體中包含指令資料。請檢閱回應標頭欄位 *Content-Type*，以尋找內容類型。
- 如果 HTTP 回應狀態碼是 204，則沒有可用的指令資料。


如需開發發置的相關資訊，請參閱[在 {{site.data.keyword.iot_short_notm}} 上開發裝置](../devices/device_dev_index.html)。
