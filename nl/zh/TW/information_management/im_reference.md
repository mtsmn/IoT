---

copyright:
years: 2017, 2018
lastupdated: "2018-03-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 資料管理參照資訊

使用下列參照資訊，以瞭解使用 {{site.data.keyword.iot_full}} 資料管理元件時所套用的限制。 

## 綱目限制

下列是資料管理中使用的綱目類型：
- 事件類型綱目
- 邏輯介面綱目
- 物品類型綱目

下表列出套用至每個綱目類型的限制：

綱目類型          |        限制
------------------- | -------------
全部       | 所有綱目都必須符合有效的 JSON。
全部       | 所有綱目都必須符合有效的 JSON 綱目。
全部       | 所有綱目的根都必須是 "object" 類型。
全部       | 所有綱目的巢狀層次最多為 7 層。
邏輯介面| 指定為必要項目的所有內容都必須已定義預設值。
邏輯介面| 如果綱目中已定義物件，則必須至少為該物件定義一個內容。
邏輯介面| 如果綱目中定義陣列，則關聯的項目只能為一種類型（例如，只能為 "string" 類型）。
物品類型| 只容許最上層內容。在第一層之後不容許任何巢狀處理。
物品類型| 最上層內容必須為 "object" 類型。
物品類型| 最上層內容必須參照 $logicalInterfaceRef 及類型。$logicalInterfaceRef 的值是邏輯介面的 ID 或別名。類型必須設為 "object" 或 "array"。

## 有效及無效綱目範例

### 範例 1 - 有效的邏輯介面綱目定義
下列範例定義可符合「綱目限制」一節中所概述限制的邏輯介面綱目：

  - 綱目符合有效的 JSON 及有效的 JSON 綱目。
  - 綱目的類型為 "object"。
  - 綱目的巢狀層次低於 7。 
  - 已定義綱目的兩個內容。 
  - 必要內容 **a** 及 **b** 已指定預設值。

```
{
 "type": "object",
 "properties":{
    "a":{
       "type":"string"
    },
    "b":{
       "type": "number"
      }
  },
  "default":{"a":"HelloWorld", "b":2},
  "required": ["a", "b"]
}
```


### 範例 2 - 無效的邏輯介面綱目定義
下列範例顯示無效的邏輯介面綱目。綱目無效，因為陣列已定義為包含 "string" 類型或 "number" 類型的項目。如果綱目中定義陣列，則關聯的項目只能為一種類型（例如，"string"）。

```
{
 "type": "object",
 "properties":{
    "a":{
      "type":"array",
       "items":{
          "type": ["string", "number"]
       }
    }
  }
}
```
### 範例 3 - 有效的物品類型綱目定義
下列範例定義可符合「綱目限制」一節中所概述限制的物品類型綱目：

  - 綱目符合有效的 JSON 及有效的 JSON 綱目。
  - 綱目的類型為 "object"。
  - 綱目的巢狀層次低於 7。 
  - 綱目只能定義最上層內容。 
  - 最上層內容參照 $logicalInterfaceRef 以及設為 "array" 或 "object" 的類型。"array" 類型可以用來聚集數個裝置或「物品」（例如，數個溫度感應器）。"object" 類型可以用來參照單一裝置或「物品」（例如，單一濕度感應器）。   
  - 必要內容不需要指定預設值。此限制只會套用至邏輯介面綱目。無法在此綱目類型中指定預設值。 

```
{
   "type" : "object",
   "properties" : {
       "temperatureSensors": {
           "description": "Aggregated temperature sensors",
           "$logicalInterfaceRef": "IThermometer",
           "type" : "array"
       },
       "humiditySensor": {
           "description": "The humidity sensor device",
           "$logicalInterfaceRef": "5846cd7c6522050001db0e24"
            "type" : "object"
       }
   },
   "required" : [
       "temperatureSensors",
       "humiditySensor"
   ]
  }
```
