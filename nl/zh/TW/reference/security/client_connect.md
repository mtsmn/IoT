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

# 監視用戶端連線功能（測試版）
{: #connect_status}

{{site.data.keyword.iot_full}} 可讓您查詢及監視用戶端的連線狀態。您可以即時瞭解是否已連接用戶端，以及您是否可以對所有連線問題進行疑難排解。
{:shortdesc}

例如，您可以查看用戶端上次處於作用中的時間及其中斷連線的原因，也可以檢視過去一天未連接的所有用戶端，以解決任何問題。

**重要事項：**監視用戶端連線功能特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## 查詢用戶端連線狀態
{: #query_status}

「用戶端連線狀態 API」可讓您擷取及查詢任何已連接至 {{site.data.keyword.iot_short_notm}} 之用戶端的連線狀態。

查詢連線狀態時，您可以檢視下列資訊：

 - 連線功能狀態（已連接或已斷線）
 - 前次連線的時間（ISO 8601 格式）
 - 前次斷線的時間（ISO 8601 格式）
 - 前次活動事件（例如 MQTT 發佈或 MQTT 訂閱）
 - 前次活動的時間（ISO 8601 格式）
 - 如果用戶端斷線（用戶端、伺服器或網路），則為導致斷線的實體
 - 如果用戶端斷線，則為斷線的 MQTT 原因碼

**範例**

若要擷取單一用戶端的用戶端連線狀態，請執行下列指令：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

若要擷取所有用戶端的連線狀態，請執行下列指令：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

若要擷取所有連接的用戶端，請執行下列指令：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

若要擷取過去兩天內處於作用中的所有用戶端，請執行下列指令：

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**附註：**用戶端連接的時間與 API 中之連線狀態反映正確狀態的時間之間，可能會有短暫的延遲。

如需 API（包括所有可用的查詢過濾器）的相關資訊，請參閱[用戶端連線狀態 API Swagger 文件 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}。

## 監視用戶端連線狀態
{: #monitor_status}

 所有用戶端都有一個監視主題，可讓您訂閱以接收用戶端的即時連線功能狀態更新。如需相關資訊，請參閱[訂閱裝置狀態訊息](../../applications/mqtt.html#subscribe_device_commands)。

## 疑難排解用戶端連線功能問題

「連線日誌 API」可讓您擷取裝置的連線日誌事件清單，以診斷連線功能問題。這些項目會記錄成功連線、不成功的連線嘗試、故意斷線以及伺服器起始的斷線。

如需相關資訊，請參閱[連線日誌 API Swagger 文件 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}。
