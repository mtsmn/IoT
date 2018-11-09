---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 瞭解對映表示式語言
{: #mapping_expression}

您可以使用對映表示式語言來操作及結合資料，以及格式化您可能對已處理資料執行之任何查詢的結果。對映表示式語言是 [JSONata ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/index.html){:new_window} 的子集，而且可以在定義[對映](ga_im_definitions.html#resources)或建立[規則](../information_management/im_rules.html)時使用。 

JSONata 是 JSON 資料的輕量型查詢及轉換語言。也提供 [JSONata Exerciser ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://try.jsonata.org/){:new_window} 工具，並提供快速且方便的方法來試用 JSONata。

下列資訊顯示目前支援的重要運算子和函數，以及如何使用它們的一些範例。 

## JSONata 運算子
{: #operators}

除了下列運算子之外，還支援所有 [JSONata 運算子 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/operators.html){:new_window}：

- ~>（函數鏈結）
- ^(…)（排序方式）
- :=（變數連結）

**附註：** 
- 使用括弧 () 進行表示式分組，以及變更運算子優先順序
- 使用單引號或雙引號括住字串及內容名稱，例如，$event.object.'ab' 
- 使用反引號括住包含特殊字元（包括空格）的內容名稱，例如 
```
{"x y":22}.`x y`
```
- 使用反引號括住開頭為數字的內容名稱，例如
```
`7emperature`
```

## JSONata 函數
{: #functions}
下列是支援的 JSONata 函數： 

 - 所有[字串 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/string-functions.html){:new_window} 函數
 - 所有[數值 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/numeric-functions.html){:new_window} 函數 	
 - 所有 [數值聚集 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 函數 
 - 所有[布林 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/boolean-functions.html){:new_window} 函數  
 - [$count 及 $append 陣列 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/array-functions.html){:new_window} 函數 	

## 擴充對映表示式語言

已擴充對映表示式語言，透過引進 $event、$state 及 $instance 變數以與資料管理特性搭配使用。評估表示式之前，JSON 會連結至這些變數。下表提供這些變數的概觀，這些變數是定義為用於表示式：

變數|範例輸入 JSON          |作為表示式的範例            |用來...
------------- | ------------- | -------------
$event |*{"t": 34.5}*  |$event.t |選取要在表示式中使用之入埠事件的內容。
$state |*{"temperature": 34.5,"humidity": 78 }*  |$state.temperature |選取要在表示式中使用之裝置狀態的內容。
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  |$instance.metadata.temp_adjustment |選取要在表示式中使用的裝置或裝置類型屬性。

下列範例使用下列物件作為事件的輸入：  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
使用下列表示式，將物件轉換為陣列：

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
此表示式會產生下列輸出：
```
 [
    35,
    72,
    1024
  ]
```

您可以反轉此範例，並將陣列從輸入轉換為物件。下列範例使用下列陣列作為事件的輸入：
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
使用下列表示式，將陣列轉換為物件：
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
此表示式會產生下列輸出：
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
您也可以定義一個結合這些變數使用的表示式。在下列範例中，*temp_adjustment* 內容定義在裝置 meta 資料中，並且用來校準事件讀數。此內容定義在一個對映中，但可以套用至許多裝置。 

```
"propertyMappings" : {
            "tevt" : {
               "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


點運算子 "." 用於具有文字索引鍵的物件存取（例如 $event.object.hh）。左側表示式受限於特定內容：在事件 ($event)、狀態 ($state) 或 meta 資料 ($instance) 中。右側表示式包含您可能要存取的資訊。

 

如需點運算子的相關資訊，請參閱 JSONata 文件的 [Operators ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/operators.html){:new_window} 小節。 


## 語言手冊

- 支援所有[基本選取 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/basic.html){:new_window}。
- 支援所有[複式選取 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/complex.html){:new_window}，萬用字元除外。
- 在[程式設計結構 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/programming.html){:new_window} 當中支援條件式及帶括弧表示式。透過使用 $instance、$state 及 $event 變數，支援變數作為 Watson IoT Platform 資料管理特有延伸對映語言的一部分。目前不支援 JSONata 中所使用的 "$" 及 "$$" 變數。



## 建構輸出
您可以使用陣列建構子或物件建構子，來指定已處理資料在輸出中的呈現方式。

### 支援的陣列建構子
您可以用一組方括弧 [] 括住以逗點區隔的文字或表示式清單，來建構陣列。逗點用來區隔陣列建構子內的多個表示式（包括序列產生）（例如 *[1, 3-1] = [1,2]*、*[1..3] -> [1,2,3]* 及 *[1..3, 7..9] -> [1,2,3,7,8,9]*）。

### 支援的物件建構子
{: #constructors}
您可以在輸出中使用一組大括弧 {} 來建構 JSON 物件，其中包含以逗點區隔的索引鍵值或配對，而且每一個索引鍵及值都是以冒號區隔（例如，*{key1 : value1, key2:value2}* 或 *{"hello" : "world"}*）。物件索引鍵必須是字串。  


## 範例：使用陣列來處理及報告溫度資料
下列各節是以[使用 REST API 開始使用資料管理](ga_im_example.html)中的範例為建置基礎，以顯示如何使用陣列來維護溫度讀數的滑動視窗，以及計算該視窗內所含之讀數的現行總和或平均值。 

滑動視窗會依到達順序來儲存資料。滑動視窗可以配置成以漸進式形式收回資料，而不是保持所有資料的插入狀態。滑動視窗填滿時，任何未來的插入都會導致在該視窗中收回最舊的資料項目。

下列範例顯示已配置大小為 5 之計數型收回原則的滑動視窗：
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

下列範例顯示如何藉由新增稱為 *tempReadings* 的陣列，來修改[逐步手冊 1](ga_im_index_scenario.html#step4) 的步驟 7 中所顯示的邏輯介面綱目檔配置。此陣列用來維護最後 5 個值的視窗，這些值是從裝置針對最後 5 個事件所傳送。如果 *tempReadings* 中未儲存任何值，則此陣列會設為 [0]，而下一個收到的讀數會附加至此陣列，此陣列持續成長直到收到 5 個讀數。收到 5 個讀數之後，新的讀數會導致從視窗中移除最舊的讀數。 

**重要注意事項：**您必須將 *tempReadings* 設為 "required"，而且依預設是陣列。 

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature" : {
            "description" : "latest temperature reading in degrees Celsius",
          "type" : "number",
          "minimum" : -273.15,
          "default" : 0.0
      },
      "tempAverage": {
          "type": "number"
      },
      "tempReadings": {
          "default": [],
          "items": {
              "type": "number"
          },
         "type": "array"
      },
      "tempSum": {
          "type": "number"
      }
  },
   "required" : [
       "tempReadings"
  ],
  "type": "object"
}
```
下節所顯示的範例是有關如何配置對映資源來計算平均溫度讀數，以及根據現行滑動視窗內所含的讀數來計算所有溫度讀數的總和：





```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt" : {
               "tempAverage": "$average($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))",
               "tempReadings": "$count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t)",
               "tempSum": "$sum($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))"
           }
       },
       "updated": "2017-10-13T10:05:40Z",
       "updatedBy": "a-8x7nmj-9iqt56kfil",
       "version": "active"
   }
]
```
**附註：***$state.tempReadings* 陣列在用於 $average 及 $sum 函數之前會先重新計算。需要重新計算陣列，以確保在評估 *tempAverage* 或 *tempSum* 表示式時，陣列包含現行值，因為無法控制對映表示式的順序。



下列範例會根據有 5 個溫度讀數的滑動視窗來顯示平均溫度及總結溫度：
```
{
    "state": {
        "tempAverage": 18557.6,
        "tempReadings": [
            17591,
            10262,
            25621,
            16676,
            22638
        ],
        "tempSum": 92788
    },
    "timestamp": "2017-10-13T11:07:20Z",
    "updated": "2017-10-13T11:05:40Z"
}
```

## 處理對映表示式與輸入資料之間的不符

其中一個對映表示式包含已發佈事件中未指定的輸入資料參照時，狀態更新可能會失敗。



例如，您可以配置下列表示式：

```
temperature = $event.t 
humidity = $event.hum
```
其中 *t* 是事件的選用內容。 

如果收到僅包含濕度資料的事件（例如 `{"humidity":22}`），則無法評估表示式 `temperature = $event.t`，因為已發佈裝置事件中未指定選用性的 *t* 內容。

狀態更新失敗。未更新濕度狀態內容，並且會將錯誤訊息發佈至裝置的 MQTT 錯誤主題：

```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
若要防止因未指定選用資料而發生狀態更新失敗，您可以搭配使用 $exists 函數與三元條件式，來指定選用內容的預設值。下列範例定義 *t* 內容的預設值 *0*： 

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

使用此方式定義選用內容的預設值，可順利評估表示式，即使已發佈裝置事件中未指定 *t* 內容。 

因此，如果收到事件 `{"humidity":22}`，則已順利完成狀態更新，而且裝置狀態設為 `{"humidity":22, "temperature":0}`。
