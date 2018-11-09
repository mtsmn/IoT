---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 配置 {{site.data.keyword.iot_short_notm}} Edge 遞送規則（預覽）
{: #edge_rules}

{{site.data.keyword.iot_full}} Edge 遞送規則定義如何在 Edge 節點內轉遞訊息。

**重要事項：**{{site.data.keyword.iot_full}} Edge 特性僅是有限預覽程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

您可以為 Edge 節點定義多個遞送規則。會針對本端裝置或應用程式直接發佈至 Edge 節點的每則訊息來評估規則。從雲端發佈至 Edge 節點的訊息一律會在本端發佈，不會受到遞送影響。

## 遞送規則格式
{: #edge_rules_format}

遞送規則定義為下列格式的 JSON 物件：

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

在儲存於 Edge 節點之 `/etc/wiotp-edge` 資料夾中的 `routing.json` 檔案中，將遞送規則指定為 JSON 物件陣列。

每個遞送規則的內容都定義如下：

內容|類型| 說明              
------------- | -----------| -----------
rule_id | integer、必要 | 規則的 ID。遞送規則會根據 rule_id 以遞增順序評估。不應該有多個具有相同 rule_id 的規則。
matching_filter | string、必要 | 用來評估是否應該將規則套用至已發佈訊息。它應該是 MQTT 訂閱的格式。支援 MQTT 單一層次 (+) 及多層次 (#) 萬用字元。此過濾器會針對發佈訊息的主題進行評估。此內容定義為 {{site.data.keyword.iot_short_notm}} 應用程式主題格式，並包括 `/type/deviceType/id/deviceId` 欄位。例如：`"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
Destination | string、必要 | 定義訊息的轉遞位置。請設定下列其中一個值：`"CLOUD"`、`"IM"`（將訊息轉遞至「資訊管理」元件）或 `"LOCAL"`（在 Edge 節點本端轉遞訊息）。例如，`"destination" : "CLOUD"`
continue_matching | boolean、選用、預設值 = true | 指定此規則符合一次時，是否應該使用此規則來評估其他訊息。有重疊的遞送規則存在時，此內容可更適當地控制遞送。
forward_skip_levels | integer、選用、預設值 = 0 | 指定重新發佈訊息時，應該從原始訊息主題中移除的層次數。預設值為 0，這表示會保留原始主題。例如，如果 `forward_skip_levels` 設為 `1`，而且訊息發佈於主題 `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json`，則訊息會重新發佈於主題 `iot-2/type/T1/id/D1/evt/E1/fmt/json`。
store | boolean、選用、預設值 = true | 定義是否應該將儲存及轉遞特性套用至以此規則遞送的訊息。只有在 destination 內容設為 `"CLOUD"` 時，才會套用此內容。如果此內容設為 false，則在 Edge 節點與雲端中斷連線時不會儲存符合規則的訊息。

## routing.json 檔案的範例
{: #edge_rules_example}

在下列範例中，配置三個規則：
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

第一個規則將 `AlarmEvent` 類型的所有事件都直接遞送至雲端。

第二個規則將發佈於開頭為 `sendToCloud/iot-2` 之主題的所有事件都直接遞送至雲端。訊息在開頭為 `iot-2` 的主題中轉遞，而且不包括 `sendToCloud/` 字首。

第三個規則是預設規則，可將所有其他訊息遞送至本端 MQTT 分配管理系統。這些訊息隨後可供具有相符訂閱的任何本端消費者使用。Edge 節點的安裝包括在 `/etc/wiotp-edge/routing.json` 本端遞送所有項目的預設規則。

## 編輯遞送規則
{: #edge_rules_edit}

您可以在 `/etc/wiotp-edge/routing.json` 檔案中直接編輯規則。編輯檔案之後，請儲存變更，然後執行 `docker ps` 來判定邊緣連接器容器的 ID。請使用該 ID 重新啟動 Docker 容器。

## 遞送規則疑難排解
{: #edge_rules_ts}

如果邊緣連接器容器在變更 `routing.json` 之後保持重新啟動，則表示遞送規則檔包含語法錯誤。請檢查 `/var/wiotp-edge/trace/edgeConnector_trace.log` 日誌檔，以查看錯誤訊息。
