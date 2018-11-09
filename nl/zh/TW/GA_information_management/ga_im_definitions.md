---

copyright:
years: 2016, 2018
lastupdated: "2018-03-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 瞭解資料管理
{: #definitions_resources}
您可能有若干不同的裝置或物品要連接至 {{site.data.keyword.iot_full}}，而且這些裝置或物品可能會以不同的格式來發佈資料。使用資料管理元件的裝置對應項及資產對應項特性，即可將裝置及「物品」中的資料輸出正規化及轉換為單一邏輯視圖，以供應用程式輕鬆取用。使用單一邏輯視圖，就不需要配置應用程式來瞭解每個裝置或「物品」所輸出的不同資料格式。然後，您可以聚集多個裝置或「物品」，以在 {{site.data.keyword.iot_short_notm}} 中定義新的「物品」。使用「物品」可協助您從某範圍的輸入組織及分析送入 {{site.data.keyword.iot_short_notm}} 的資料。 

{: shortdesc}

## 概觀
{: #overview}

使用裝置對應項特性來建立裝置的邏輯模型，然後使用資產對應項特性來聚集這些邏輯模型以定義新的「物品」。這些邏輯模型可協助您改善程式碼的重複使用和維護，以及藉由隔離應用程式與資料變更來管理 IoT 生態系統的複雜性。 

應用程式可以使用 HTTP API 或是訂閱 IoT 主題字串，以在要求時存取裝置或「物品」的現行狀態。狀態包含邏輯介面所定義的一組狀態內容。如果裝置或「物品」的狀態因將事件發佈至 {{site.data.keyword.iot_short_notm}} 而變更，則會更新這些內容的值，並將其儲存在 {{site.data.keyword.iot_short_notm}}。

使用裝置及資產對應項特性，即可達到下列好處：
- 將狀態內容對映至事件訊息資料。
- 聚集多個裝置或「物品」，以定義新的「物品」。
- 定義您喜好的資料結構。
- 定義裝置或「物品」狀態的多個表示法或視圖。
- 訂閱裝置或「物品」狀態，或隨時透過 HTTP API 進行查詢。

實作裝置及資產對應項特性的一些常見使用案例包括：
- 提供一致的介面，讓應用程式開發人員透過 REST 類似方式來存取事件驅動裝置資料。
- 正規化不同廠牌或型號之裝置的資料，而這些裝置會以不同的格式來發佈資料。
- 修改及轉換資料格式，使其符合您的應用程式模型。
- 格式化某範圍裝置或「物品」的海量資料，來分析資料並以最有效的方式呈現資料，以協助預測故障、排定維護、追蹤資產以及改善作業效率。

## 範例
{: #examples}
下列範例說明兩個可能的解決方案。範例 1 說明如何使用裝置對應項特性，而範例 2 說明如何使用資產對應項特性。 

### 範例 1：將異質溫度感應器對映至邏輯介面
{: #device-type-example}
在此範例中，所建立的邏輯介面會以某種格式提供同質溫度狀態資料，而不論實際裝置事件訊息有效負載格式為何。*tSensor* 裝置會將 `{ "t" : 34.5 }` 的攝氏溫度讀數發佈至 {{site.data.keyword.iot_short_notm}}。*tempSensor* 裝置會發佈 `{ "temp" : 72.55 }` 的華氏溫度讀數。溫度讀數會發佈為個別事件。

如需可說明此範例的詳細完整情境，請參閱[逐步手冊 1](ga_im_index_scenario.html)。

![{{site.data.keyword.iot_short_notm}} 上溫度感應器裝置與應用程式之間的對映](../information_management/images/Information Management Device example.svg "{{site.data.keyword.iot_short_notm}} 上溫度感應器裝置與應用程式之間的對映")

在邏輯介面資料流程過程中，您可以對送入的資料執行計算，以將這些讀數正規化為一致形式來進行處理。這表示您不需要撰寫應用程式，即可瞭解或轉換不同的溫標。應用程式會接收單一正規化狀態，並使用 **temperature** 狀態內容，而非裝置特定 **t** 及 **temp** 內容。

### 範例 2：將多個氣候裝置對映至一個「物品」類型邏輯介面
{: #thing-type-example}  
在此範例中，我們會以不同濕度計裝置的形式新增一組濕度感應器，來展開裝置類型範例。使用「物品」類型邏輯介面，即可順暢地將不同裝置類型的資料合併成一個代表某會議室內所有裝置及感應器的邏輯介面。應用程式現在可以藉由連接至與 "RoomType"「物品」類型相關聯的邏輯介面，來取得針對某會議室收集到的氣候資料。下圖顯示「會議室 1」的配置。

如需可說明此範例的詳細完整情境，請參閱[逐步手冊 2](../information_management/im_index_scenario_thing.html)。

![{{site.data.keyword.iot_short_notm}} 上溫度及濕度物品與應用程式之間的對映](../information_management/images/Information Management Thing example scenario.svg "在一個房間中的多部環境裝置，與 {{site.data.keyword.iot_short_notm}} 上的應用程式之間的對映")

稱為 *tSensor* 的溫度裝置及稱為 *humiditySensor1* 的濕度裝置會發佈在*會議室 1* 會議室中收集到的環境資料。溫度及濕度感應器資料分別對映至兩個裝置類型邏輯介面：一個用於溫度計裝置類型，一個則用於濕度計裝置類型。我們現在要建立稱為 *RoomType* 的「物品」類型，並實例化稱為*會議室 1* 的會議室「物品」實例。

在第二個會議室中，稱為 *tempSensor* 的溫度裝置及稱為 *humiditySensor2* 的濕度裝置會發佈在*會議室 2* 會議室中收集到的環境資料。根據 *RoomType*「物品」類型，建立稱為*會議室 2* 的另一個會議室「物品」實例。

我們現在可以設定包括溫度計及濕度計邏輯介面的組合，然後將正確的環境感應器對映至每個會議室實例（例如，*tSensor* 及 *humiditySensor1* 會對映至*會議室 1*，而 *tempSensor* 及 *humiditySensor2* 會對映至*會議室 2*。

一般使用者應用程式現在可以要求特定會議室「物品」ID 的狀態，以及取得會議室溫度及濕度狀態，而不需要知道基礎裝置架構。

## 定義及資源
{: #resources}

下列各圖說明在使用邏輯介面時，{{site.data.keyword.iot_short_notm}} 上裝置與應用程式之間的邏輯對映。

![{{site.data.keyword.iot_short_notm}} 上裝置與應用程式之間的邏輯對映。](../information_management/images/im_resources_things.svg "{{site.data.keyword.iot_short_notm}} 上裝置、「物品」與應用程式之間的對映")

### 概念

概念| 說明              
------------- | ------------- | -------------  
事件|事件是裝置用來將資料發佈至 {{site.data.keyword.iot_short_notm}} 的機制。裝置會控制事件的內容，並指派名稱給它傳送的每個事件。
內容|帶有裝置事件有效負載一部分的資料。
狀態|實體裝置狀態的最新呈現，可包括已跨多個入埠事件對映的所有內容。
組合|定義與「物品」類型相關聯之邏輯介面的邏輯建構。組合是透過「物品」類型綱目所指定。

### 資料管理資源
您可以使用 REST API 來管理資源。如需 REST API 的相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html) 文件。 

類型資源| 說明              
------------- | ------------- | -------------  
事件類型|使用事件類型資源，以建立裝置所發佈事件的模型。事件類型必須參照事件綱目資源。綱目資源定義所發佈事件的結構。</br>**重要事項：**邏輯介面中所使用的入埠事件必須為 JSON 格式。
裝置類型|使用裝置類型資源，以將共用性質或行為的裝置群組在一起。在資料管理中，裝置類型會擴充成包括裝置的一個實體介面以及一個以上用來擷取裝置狀態的邏輯介面。</br>如需相關資訊，請參閱[裝置機型](../reference/device_model.html#id_and_device_types)主題中的「ID 及裝置類型」小節。
物品類型|程式化建構，可代表一個以上不同裝置類型及（或）「物品」類型的集合。</br>**重要事項：**「測試版」支援「物品」類型邏輯介面的 10 層的巢狀層次。
綱目資源|使用綱目資源，以定義事件、裝置或「物品」狀態的結構。會使用下列 [JSON 綱目 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://json-schema.org/){:new_window}：<ul><li>與事件類型相關聯的綱目。此綱目是用來定義裝置發佈至 {{site.data.keyword.iot_short_notm}} 之事件的結構。這些綱目稱為事件綱目。<li>與邏輯介面相關聯的綱目。此綱目用來定義 {{site.data.keyword.iot_short_notm}} 上所儲存之裝置或「物品」狀態的結構。這些綱目稱為邏輯介面綱目。</ul>  </ul>

介面資源|說明
------------- | ------------- | -------------  
邏輯介面|程式化建構，應用程式可連接或訂閱以查看裝置的狀態。邏輯介面用來將正規化視圖定義為 {{site.data.keyword.iot_short_notm}} 中的裝置狀態。邏輯介面必須與邏輯介面綱目相關聯。會更新狀態，以回應入埠裝置事件。**附註：**您可以選擇性地指定有意義的邏輯介面別名。在用來擷取裝置狀態的 API 呼叫或主題字串訂閱中，可以參照別名，而不使用自動產生的邏輯介面 ID。
實體介面|實體介面用來建立實體裝置與 {{site.data.keyword.iot_short_notm}} 間之介面的模型。事件類型可以與實體介面相關聯。

實例資源| 說明              
------------- | ------------- | -------------  
裝置|裝置代表向 {{site.data.keyword.iot_short_notm}} 登錄並以事件形式傳送 IoT 資料的資產、系統或元件。
物品|程式化建構，可邏輯地代表「物品」類型的唯一實例。「物品」實例所提供的用途與裝置類型的已登錄裝置相同。


支援資源| 說明              
------------- | -------------   
對映|使用對映，以定義如何將與入埠事件相關聯的內容對映至邏輯介面上所定義的內容。</br>**重要事項：**至少一個邏輯介面必須先與裝置或「物品」類型相關聯，才能定義任何對映。

## 資源的命名限制
{: #naming_restrictions}
綱目、事件類型、邏輯介面及實體介面具有下列命名限制：
- 名稱必須介於 1 - 128 個字元之間 
- 名稱必須由 Unicode 字元組成 
- 有效的特殊字元包含空格、連字號 (-)、底線 (_)、句點 (.)
- 名稱不能僅包含空格

## 建立、更新、啟動及取消啟動資源
{: #draft_active_resources}

資源可以有兩種版本：初版及作用中版本。在建立資源時，會將該資源建立為初版。
{: shortdesc}

初版是資源的工作副本，您可以使用 API 直接查詢、更新及刪除。藉由啟動草稿裝置類型、草稿「物品」類型或草稿邏輯介面，來建立草稿資源的作用中版本。若要啟動其他資源（例如綱目），您必須啟動用於參照您要啟動之資源的草稿裝置類型、草稿「物品」類型或草稿邏輯介面。

若要在使用 REST API 時區分草稿與作用中資源，則會使用字首 *draft/* 來識別處於草稿狀態的資源。

下列範例使用指定的 ID 來擷取草稿綱目定義的 meta 資料：

```
GET /api/v0002/draft/schemas/{schemaId}
```
下列範例使用指定的 ID 來擷取作用中綱目定義的 meta 資料：
```
GET /api/v0002/schemas/{schemaId}
```
*附註：*給定資源之草稿及作用中版本的 ID 是相同的。





- 啟動資源
{: #activate_resources}  

使用 **activate-configuration** 作業，以驗證及啟動與裝置或「物品」類型相關聯的配置。此配置包括您的草稿綱目、事件類型、實體介面、邏輯介面及對映。必須對邏輯介面、裝置類型或「物品」類型的初版執行 **activate-configuration** 作業。

下列範例顯示對裝置類型的初版執行 **activate-configuration** 作業的 PATCH 要求：
```
PATCH /api/v0002/draft/device/types/TSensor
```
其中，PATCH 主體的有效負載包含下列內容：
```
  {
    "operation": "activate-configuration"
  }   
```
若要啟動「物品」類型的初版，請使用下列 PATCH 方法：
```
PATCH /api/v0002/draft/thing/types/RoomType
```

- 列出差異
{: #list_differences}  

使用 **list-differences** 作業，以傳回邏輯介面、裝置類型或「物品」類型資源的作用中與草稿配置之間的所有差異清單。必須對邏輯介面、裝置或「物品」類型的初版執行 **list-differences** 作業。下列範例顯示對裝置類型的初版執行 **list-differences** 作業的 PATCH 要求：
```
PATCH /api/v0002/draft/device/types/TSensor
```
其中，PATCH 主體的有效負載包含下列內容：
```
  {
    "operation": "list-differences"
  }
```
若要傳回「物品」類型資源的作用中與草稿配置之間的所有差異清單，請使用下列 PATCH 方法：
```
PATCH /api/v0002/draft/thing/types/meetingroom1
```

- 取消啟動資源  
{: #deactivate_resources}  

使用 **deactivate-configuration** 作業，以移除與資源相關聯的作用中配置。只能對邏輯介面、裝置類型或「物品」類型的作用中版本執行 deactivate-configuration 作業。下列範例顯示對裝置類型的作用中版本執行 **deactivate-configuration** 作業的 PATCH 要求：
```
PATCH /api/v0002/device/types/TSensor
```
其中，PATCH 主體的有效負載包含下列內容：
```
  {
    "operation": "deactivate-configuration"
  }
```
若要取消啟動「物品」類型，請使用下列 PATCH 方法：
```
PATCH /api/v0002/thing/types/RoomType
```

*附註：*
- 作用中資源是唯讀的。您可以使用查詢參數，來過濾及排序草稿和作用中資源。
- 如果有與該裝置類型相關聯的裝置實例，則無法刪除該裝置類型。刪除裝置實例時，會清除裝置的狀態。 
- 如果有與該「物品」類型相關聯的裝置或「物品」實例，則無法刪除該「物品」類型。刪除裝置或物品實例時，會清除「物品」的狀態。 
- 您只能使用 API 直接啟動邏輯介面、裝置類型及「物品」類型。其他資源（例如綱目、實體介面、「物品」類型介面及事件類型）會在作用中的邏輯介面、裝置類型或「物品」類型參照時啟動。  
- 必須對與裝置或「物品」類型相關聯之邏輯介面的初版，或者裝置或「物品」類型本身執行 **activate-configuration** 作業。**activate-configuration** 作業會先確認資源配置是有效的，然後再啟動資源。啟動順利完成之後，即會針對裝置或「物品」類型的每個裝置或「物品」實例產生狀態。

## 配置疑難排解
{: #troubleshooting}
如果啟動失敗，請確認已提供給定裝置或「物品」類型的所有必要配置。 

必須提供下列配置，並將其與裝置類型相關聯：
  - 至少與一個事件相關聯的實體介面
  - 至少一個邏輯介面
  - 至少一個關聯邏輯介面的對映
  
必須提供下列配置，並將其與「物品」類型相關聯：
  - 至少與一個裝置或「物品」類型相關聯的「物品」介面
  - 至少一個邏輯介面
  - 至少一個關聯邏輯介面的對映  

您也可以對裝置類型、「物品」類型或邏輯介面資源的初版執行 **validate-configuration** 作業，以確保關聯的 meta 資料是有效的。如果 meta 資料無效，則回應主體中會傳回問題清單。  

下列範例顯示對稱為 "TSensor" 之裝置類型的初版執行 **validate-configuration** 作業的 PATCH 要求：  
```
PATCH /api/v0002/draft/device/types/TSensor
```
其中，PATCH 主體的有效負載包含下列內容：
```
  {
    "operation": "validate-configuration"
  }
```  
下列範例顯示 PATCH 要求的不成功回應：  
```
{
"message": "CUDIM0303I: State update configuration for Device Type 'TSensor' is not valid.",
"details": {
  "id": "CUDIM0303I",
  "properties": [
    "Device Type",
    "Sensor"
  ]
},
"failures": [
  {
    "message": "CUDVS0301E: The device type 'TSensor' does not have any mappings defined for it",
    "details": {
      "id": "CUDVS0301E",
      "properties": [
        "TSensor"
      ]
    }
  }
]
}
```  
下列範例顯示 PATCH 要求的成功回應：  
```  
{
"message": "CUDIM0303I: State update configuration for Device Type 'TSensor' is valid.",
"details": {
  "id": "CUDIM0303I",
  "properties": [
    "Device Type",
    "TSensor"
  ]
},
 "failures": []
}
```  
如果所有必要資源都與裝置或「物品」類型相關聯，請確認內容對映都是有效的。下列範例顯示可能發生的可能錯誤：



  - 表示式參照事件上未由事件綱目所定義的內容
  - 表示式參照狀態上未由邏輯介面綱目所定義的內容
  - 對映是針對未由邏輯介面綱目所定義的內容所定義


您可以參考下列錯誤日誌，以協助您診斷裝置類型的運行環境錯誤：
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
您可以參考下列錯誤日誌，以協助您診斷「物品」類型的運行環境錯誤：
```
iot-2/type/${typeId}/id/${thingId}/err/data
```

### 資源限制

下表顯示可根據方案類型所配置的資源數目上限。 

資源|標準方案                       | 精簡方案  
------------- | ------------- | ------------- 
邏輯介面|1000 |10
實體介面|1000 |5
事件類型|1000 |10
綱目|2000 |20
邏輯介面參照（裝置類型可對映至的邏輯介面數目）|20 |5
事件類型參照（實體介面可具有之事件類型關聯的事件 ID 數目）|40 |10


