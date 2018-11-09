---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 裝置的 HTTP API
{: #api}

有兩種類型的 HTTP API 可以與 {{site.data.keyword.iot_full}} 搭配使用。您可以使用 HTTP REST API 來建立、更新、列出及刪除裝置。使用「HTTP 傳訊 API」將事件資訊從裝置傳送至雲端，以及接受來自雲端中應用程式的指令。 

## 用戶端連線
{: #client_connections}

如需用戶端安全以及如何將用戶端連接至 {{site.data.keyword.iot_short_notm}} 中裝置的相關資訊，請參閱[將應用程式、裝置及閘道連接至 {{site.data.keyword.iot_short_notm}}](../reference/security/connect_devices_apps_gw.html)。

**附註：**依預設，會停用 HTTP 埠 1883。如需變更預設值的相關資訊，請參閱[配置安全原則](../reference/security/set_up_policies.html#set_up_policies.md)。

## 鑑別

所有鑑別要求都必須包括授權標頭。基本鑑別是唯一支援的方法。裝置透過 {{site.data.keyword.iot_short_notm}} HTTP REST API 提出 HTTP 要求時，需要下列認證：

|認證|必要輸入|
|:---|:---|
|使用者名稱|`use-token-auth`
|密碼|在登錄裝置時自動產生或手動指定的鑑別記號。





## Content-Type 要求標頭

`Content-Type` 要求標頭必須隨要求提供。下表顯示支援的類型如何對映至 {{site.data.keyword.iot_short_notm}} 內部格式。

|Content-Type 標頭|{{site.data.keyword.iot_short_notm}} 中的格式|
|:---|:---|
|text/plain|"text"
|application/json|"json"
|application/xml|"xml"
|application/octet-stream|"bin"



## 服務品質

與 MQTT 服務品質「最多一次」遞送服務水準 0 類似，HTTP REST 傳訊提供非持續訊息遞送，但會驗證要求正確無誤，並驗證要求會先遞送至伺服器，再傳送 HTTP 回應。包含 HTTP 狀態碼 200 的回覆會確認已將訊息遞送至伺服器。當您使用「最多一次」的 MQTT 服務品質水準或 HTTP 對等項目來遞送事件訊息時，裝置或應用程式必須實作重試邏輯以保證遞送。

如需 {{site.data.keyword.iot_short_notm}} 的 MQTT 通訊協定及服務品質水準的相關資訊，請參閱 [MQTT 傳訊](../reference/mqtt/index.html)。

您可以使用 HTTP 傳訊 API 將事件從裝置發佈至雲端，以及接收來自雲端中應用程式的指令。

## 發佈事件
{: #event_publication}

除了使用 MQTT 傳訊通訊協定之外，您也可以使用「HTTP 傳訊 API」指令來配置裝置，以透過 HTTP 將事件發佈至 {{site.data.keyword.iot_short_notm}}。

請使用下列其中一個 URL，來提交來自連接至 {{site.data.keyword.iot_short_notm}} 之裝置的 ``POST`` 要求：

### 用於發佈事件的不安全 POST 要求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### 用於發佈事件的安全 POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**附註：**您也可以指定埠 443（預設 SSL 埠）來進行安全 HTTP API 呼叫。

## 接收指令
{: #receive_commands}

除了使用 MQTT 傳訊通訊協定之外，您也可以使「HTTP 傳訊 API」指令來配置裝置，透過 HTTP 接收來自 {{site.data.keyword.iot_short_notm}} 的指令。裝置可以接收指向自己的指令。

請使用下列其中一個 URL，來提交來自連接至 {{site.data.keyword.iot_short_notm}} 之裝置的 ``POST`` 要求：

### 用於接收指令的不安全 POST 要求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### 用於接收指令的安全 POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**附註：**您也可以指定埠 443（預設 SSL 埠）來進行安全 HTTP API 呼叫。

您可以選擇性地在 HTTP 要求主體中包括參數 *waitTimeSecs* 來指定一個整數，這個整數代表等待指令的秒數上限：
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**重要注意事項：**
- *waitTimeSecs* 的值必須是 0 - 3600 秒範圍內的整數。預設值為 0。
- 若要接收任何指令，請針對 {command} 元件使用「任何」萬用字元 (+)。如果使用萬用字元，則會在回應標頭欄位 *X-commandId* 中包含指令 ID。
- 如果 HTTP 回應狀態碼是 200，則會在回應主體中包含指令資料。請檢閱回應標頭欄位 *Content-Type*，以尋找內容類型。
- 如果 HTTP 回應狀態碼是 204，則沒有可用的指令資料。

若要存取 {{site.data.keyword.iot_short_notm}} HTTP REST API，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}。

若要存取「{{site.data.keyword.iot_short_notm}} HTTP 傳訊 API」，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP 傳訊 API ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}。

如需開發發置的相關資訊，請參閱[在 {{site.data.keyword.iot_short_notm}} 上開發裝置](../devices/device_dev_index.html)。

## 安全 POST 要求的範例

下列範例使用 HTTP 通訊協定，將訊息從裝置傳送至 {{site.data.keyword.iot_short_notm}}。應用程式透過訂閱發佈訊息的主題，來接收訊息。下表提供裝置的相關資訊。 

|參數|值|
|:---|:---|
|組織 ID|myOrgID
|裝置類型|TestDevices
|裝置 ID|TestPublishEvent
|鑑別方法|use-token-auth
|鑑別記號|passw0rd


下列範例使用 curl，透過 HTTP 通訊協定將稱為 *TestMessage* 的事件從裝置 *TestPublishEvent* 傳送至 {{site.data.keyword.iot_short_notm}}： 

```
curl -v -X POST -H "Content-Type: application/json" -u "use-token-auth:passw0rd" -d @message.txt  https://myOrgID.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/TestDevices/devices/TestPublishEvent/events/TestMessage
```

訊息包含在稱為 *message.txt* 的文字檔中。*message.txt* 檔案的內容如下列範例所示： 
```
{ "d": { "myName": "Publish Test","cputemp": 46,"sine": -10,"cpuload": 1.45 }  } 
```
訊息發佈於主題 *iot-2/evt/TestMessage/fmt/json*。建立稱為 *testSubscriber* 的應用程式，用來訂閱主題過濾器 *iot-2/type/+/id/+/evt/+/fmt/+*。主題過濾器會比對發佈訊息的主題。

您可以使用 [Python 程式庫 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://github.com/ibm-watson-iot/iot-python/tree/master/samples/simpleApp) 中所提供的 Python 樣本 *simpleApp.py*，協助您配置應用程式訂閱主題過濾器 *iot-2/type/+/id/+/evt/+/fmt/+*。 

下列 Python Script 會訂閱與發佈訊息之主題字串相符的主題過濾器：

```
python simpleApp.py -o "myOrgID" -i "testSubscriber" -k "a-myOrgID-jg95dkpscn" -t "L(r4gfPa3bLP2kz4Nb"
```
其中：

- **-o** 是組織名稱。
- **-i** 是應用程式的名稱。
- **-k** 是建立應用程式時所產生的 API 金鑰。  
- **-t** 是建立應用程式時所產生的鑑別記號。   


下列範例顯示收到來自 *TestPublishEvent* 裝置之已發佈訊息的成功回應。 
```
=============================================================================
Timestamp                        Device                        Event
2017-12-06 15:09:40,438   ibmiotf.application.Client  INFO    Connected successfully: a:myOrgID:testSubscriber
=============================================================================
2017-12-06T15:09:59.691290+00:00 TestDevices:TestPublishEvent  TestMessage: {"d": {"myName": "Publish Test", "cputemp": 46, "sine": -10, "cpuload": 1.45}}
2017-12-06T15:10:01.056000+00:00 TestDevices:TestPublishEvent  Connect 199.999.99.99
2017-12-06T15:10:01.446000+00:00 TestDevices:TestPublishEvent  Disconnect 199.999.99.99 (None)

```

## 前次事件快取
{: #last-event-cache}

您可以使用前次事件快取，來儲存已連接裝置傳送至 {{site.data.keyword.iot_short_notm}} 的前次事件的相關資訊。使用 [API ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}，可以擷取裝置之特定 event-id 的前次記錄值，或特定裝置所報告之每個 event-id 的前次記錄值。 

### 配置前次事件快取

依預設，會停用前次事件快取特性，但您可以使用下列方式啟用此功能： 

-	使用 API，將 *enabled* 參數設為 **true**。
-	在 {{site.data.keyword.iot_short_notm}} 儀表板的*設定* 頁面上，將**啟用 LEC** 設為**開啟**。

事件資料儲存在快取中的時間長度，是由存活時間 (TTL) 值所指定。依預設，任何特定事件的最後一個事件資料都會儲存 7 天。您可以使用 API 來更新 **ttlDays** 參數，或在 {{site.data.keyword.iot_short_notm}} 儀表板之*設定* 頁面上選取**事件資料 TTL** 欄位值，以變更持續時間。

對於「精簡」方案，您可以儲存最少 1 天、最多 7 天的資訊。對於其他方案，您可以儲存最少 1 天、最多 45 天的資訊。

使用下列資訊，可協助您使用 API 來配置前次事件快取設定。

若要擷取組織前次事件快取的現行配置，請使用下列 API：

```
    GET /api/v0002/config/lec
```   

   
下列範例顯示 GET 方法的成功回應：
```
{
    "enabled": false,
    "ttlDays": 7
}
```
組織中的任何人員或應用程式都有權存取此端點。 

如果 {{site.data.keyword.iot_short_notm}} 管理者停用組織的前次事件快取，則會傳回下列「HTTP 403 已禁止」狀態碼，以回應 GET 方法：

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: Last event cache is disabled for this organization. For more information, contact your support team."
}
```

如果前次事件快取未啟用，則會傳回 HTTP 404 狀態碼，以回應 GET 方法。若要啟用組織的前次事件快取特性，請使用下列 API：

```
    PUT /api/v0002/config/lec
```

與下列範例有效負載： 
```
{
    "enabled": true,
    "ttlDays": 5
}
```
其中： 

- **enabled** 是必要項目，指定是否啟用組織的前次事件快取。值必須是布林值。 

- **ttlDays** 是選用項目，指定事件保留在組織前次事件快取中的天數。值必須是 1 與 45（含）之間的整數。
   
 
只有管理者、操作員使用者或操作員應用程式才有權存取此端點。如果參數或 JSON 無效，則會傳回 HTTP 401 BAD REQUEST 狀態碼，以回應 PUT 方法。 

**附註：**可能需要 30 秒才能完成配置變更。

### 從前次事件快取中擷取資訊

使用 API，即可擷取裝置前次傳送的事件。不論裝置的實體位置或使用狀態為何，您都可以擷取裝置狀態。您可以擷取特定裝置之事件 ID 的前次記錄值，或特定裝置所報告之每一個事件 ID 的前次記錄值。可針對最多 45 天之前發生的任何特定事件，擷取裝置的前次事件資料。

若要要求特定事件 ID 的最新值，請使用下列 API 要求，其會傳回 "power" 事件 ID 的前次記錄值。

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

傳回的回應具有下列 JSON 格式：

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

**附註：**API 回應為 JSON 格式時，可以使用任何格式來撰寫事件有效負載。「前次事件快取 API」所傳回的有效負載會使用 base64 進行編碼。

若要要求裝置所報告之每個事件 ID 的最新值，請使用下列 API 要求：

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

回應包括裝置所傳送的所有事件 ID。在下列範例中，會傳回 "power" 及 "temperature" 事件的值。

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

若要存取「{{site.data.keyword.iot_short_notm}} 前次事件快取 API」，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP 資訊管理 API ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}。
