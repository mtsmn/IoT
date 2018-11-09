---
copyright:
  years: 2016, 2018
lastupdated: "2018-02-23"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 計費

付費 {{site.data.keyword.iot_full}} 服務方案（非「精簡」的方案）是根據一個月期間進行交換之 MB 的概念。此文件詳述 {{site.data.keyword.iot_short_notm}} 如何測量資料，以建立判斷使用服務成本的用量資訊。根據裝置、應用程式及閘道的設計和數目，用量資訊可用來估計使用 {{site.data.keyword.iot_short_notm}} 的成本。

如需交換每 MB 資料成本的相關資訊，請參閱 {{site.data.keyword.Bluemix_notm}} 型錄中所需地區的 {{site.data.keyword.iot_short_notm}} 服務。

您可以使用[定價計算機 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://iot-cost-calculator.ng.bluemix.net/)，協助您計算 {{site.data.keyword.iot_short_notm}} 服務成本。

下列項目會根據用量進行計量及計費： 

## 交換的資料
*交換的資料* 計算包括使用 MQTT 或 HTTP 傳訊，依應用程式、裝置及閘道進行交換的帳戶資料，以及使用 HTTP API，依應用程式進行交換的資料。

### 交換的 MQTT 資料
使用 MQTT 時，TCP 串流內容會計入*交換的資料*。任何 MQTT 訊息的有效負載都會包括在使用 MQTT 通訊協定進行交換的資料累計中。在發佈訊息時，以及將訊息遞送給訂閱者時，都會累計發生的有效負載資料大小。

MQTT 通訊協定的設計是盡可能小型。雖然 {{site.data.keyword.iot_short_notm}} 會在累計*交換的資料* 時包括 MQTT 封包大小，但是因為它是小型通訊協定，所以這個額外負擔較低。下表指出每一個 MQTT 封包的大小下限：

|MQTT 封包                      |大小位元組下限      |變數及關聯的大小下限（以位元組為單位）|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |用戶端 ID（如果 d:xxxxxx:t:i，則為 12）、設定的主題 (0) 及訊息 (0)、使用者名稱（14 適用於裝置 - 'use-token-auth'）及密碼（如果自動產生，則為 18）|
|CONNACK                        |4|無任何項目|
|PUBLISH                        |21                  |主題名稱（如果 iot-2/evt/i/fmt/f，則為 17）及有效負載（0 是最小值）。同時適用於入埠及出埠 PUBLISH 封包。|
|PUBACK                         |4|無任何項目|
|PUBREC                         |4|無任何項目|
|PUBREL                         |4|無任何項目|
|PUBCOMP                        |4|無任何項目|
|SUBSCRIBE                      |26                  |可以包括主題過濾器（如果 iot-2/evt/i/fmt/f，則為 19）及 QoS (1) 的倍數|
|SUBACK                         |5|(1) 用於 SUBSCRIBE 中的每個主題過濾器|
|UNSUBSCRIBE                    |23                  |可以包括多個主題過濾器（如果 iot-2/evt/i/fmt/f，則為 19）|
|UNSUBACK                       |4|無任何項目|
|PINGREQ                        |2|無任何項目|
|PINGRESP                       |2|無任何項目|
|DISCONNECT                     |2|無任何項目|

使用傳輸層安全 (TLS) 時，也會考慮安全信號交換。信號交換大約是 8 KB。因此，盡可能長時間保持 MQTT 連線開啟會更具成本效益。開啟的連線只會造成 PINGREQ 及 PINGRESP 額外負擔（4 個位元組），每個保持作用中間隔都是用戶端特定的，並且取決於所指定的保持作用中期間。使用現有 TLS 階段作業來重新開啟 TLS 連線可減少交換的位元組數，因為未以任一方向傳送憑證。依預設，許多 TLS 用戶端都支援此動作，但階段作業的生命期限有限。根據用戶端憑證的大小，使用用戶端憑證可能會增加 TLS 信號交換的大小。 

不會考量傳輸層安全 (TLS) 記錄，以及 TCP 和 IP 額外負擔。

**附註** - 使用 {{site.data.keyword.iot_short_notm}} 儀表板檢視裝置時，會進行 MQTT 訂閱，讓儀表板可以顯示即時事件。此 MQTT 訂閱也會計入交換的資料總量。

### 交換的 HTTP 傳訊資料
若使用 HTTP 傳訊，則在累計*交換的資料* 時（發佈事件或接收指令時）會包括訊息有效負載。

與使用 MQTT 相同，會計入 HTTP 額外負擔。HTTP 額外負擔遠高於 MQTT 額外負擔。每個要求的 HTTP 額外負擔大約為 300 個位元組。還需要加上訊息有效負載大小。

使用傳輸層安全 (TLS) 時，會考慮安全信號交換。信號交換大約是 8 KB。因此，盡可能長時間保持 HTTPS 連線開啟會更具成本效益。就*交換的資料* 而言，開啟的連線不會造成任何額外負擔。使用現有 TLS 階段作業來重新開啟 TLS 連線可減少交換的位元組數，因為未以任一方向傳送憑證。依預設，許多 TLS 用戶端都支援此動作，但階段作業的生命期限有限。根據用戶端憑證的大小，使用用戶端憑證可能會增加 TLS 信號交換的大小。

不會考量傳輸層安全 (TLS) 記錄，以及 TCP 和 IP 額外負擔。

### 交換的 HTTP API 資料
應用程式呼叫任何 API 時，要求及回應內文都會累計至*交換的資料*。每一個的大小取決於 API 而有明顯的差異。

與 MQTT 及 HTTP 傳訊不同，針對交換的 HTTP API 資料，不會考量 HTTP 額外負擔，也不會考量 TLS 額外負擔。

**附註** - 使用 {{site.data.keyword.iot_short_notm}} 儀表板時，儀表板會使用 HTTP API 列出包括裝置、裝置類型及裝置連線日誌的資訊。這些 HTTP API 呼叫會計入*交換的資料*。

<!-- ## Data Analyzed
The *data analyzed* calculation measures event data that is processed by the rules engine within the platform.  Data is considered processed by the rules engine when device events are evaluated by one or more rules, based on a specific device and event type. 
## Edge Data Analyzed
The *edge data analyzed* calculation measures event data that is processed on a gateway device by the {{site.data.keyword.iot_short_notm}} Edge Analytics Agent.  Data is considered processed by the edge agent when device events are evaluated by one or more edge rules, based on a specific device and event type.  -->
