---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-09"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 瞭解對映表示式語言
{: #mapping_expression}

您可以使用對映表示式語言來操作及結合資料，以及格式化您可能對已處理資料執行之任何查詢的結果。對映表示式語言是 [JSONata ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.jsonata.org/index.html){:new_window} 的子集，而且可以在定義[對映](ga_im_definitions.html#definitions_resources)時使用。JSONata 是 JSON 資料的輕量型查詢及轉換語言。

下列資訊顯示目前支援的重要運算子和函數，以及如何使用它們的一些範例。 

## 支援的 JSONata 運算子
{: #operators}

下列是支援的 JSONata 運算子子集： 

運算子類型                         | 支援的運算子            | 附註

------------- | ------------- | -------------
算術| *+* - / * % | % 運算子會傳回餘數。
比較| < <= > >= != = | 相等運算子是 =，因為它是在 JSONata 中。
布林| *in*、*and* | 布林常數為 *true* 或 *false*。
條件式三元| ? | ? 運算子會根據測試條件結果來評估兩個替代表示式的其中一個。此運算子採用下列格式 *expression*  ? *value_if_true* : *value_if_false*
字串| & | & 運算子會將運算元的字串值結合至單一產生的字串。
序列產生器| .. | 建立遞增整數陣列，而此陣列的開頭為 LHS 上的數字且結尾為 RHS 上的數字（例如，[1..4] -> [1,2,3,4]）。運算元必須評估為整數。序列產生器只能用於陣列建構子 [] 內。
其他| . | 點運算子用於具有文字索引鍵的物件存取（例如 $event.object.hh）。*
e：* 左側表示式受限於特定內容：在事件 ($event) 或狀態 ($state) 或 meta 資料 ($instance) 中。

**附註：** 
- 使用括弧 () 進行表示式分組，以及變更運算子優先順序
- 使用單引號括住包含空格的內容名稱（例如 $event.object）。'a b' 

## 擴充對映表示式語言

已擴充對映表示式語言，透過引進 $event、$state 及 $instance 變數以與資料管理特性搭配使用。評估表示式之前，JSON 會連結至這些變數。下表提供這些變數的概觀，這些變數是定義為用於表示式：

變數| 範例輸入 JSON          | 作為表示式的範例            | 用來...
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | 選取要在表示式中使用之入埠事件的內容。
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | 選取要在表示式中使用之裝置狀態的內容。
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | 選取要在表示式中使用的裝置或裝置類型屬性。

您也可以定義一個結合這些變數使用的表示式。在下列範例中，*temp_adjustment* 內容定義在裝置 meta 資料中，並且用來校準事件讀數。此內容定義在一個對映中，但可以套用至許多裝置。 

```
"propertyMappings" : {
            "tevt" : {
               "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## 支援的 JSONata 函數
{: #functions}
下列是支援的 JSONata 函數子集： 

函數類型|函數| 說明| 範例
------------- | ------------- | ------------- 
字串| $substring(string, start_index[, length]) | 字串子字串（例如，*$substring("Hello World", 3, 5) => "lo Wo"*）。
字串| $string(arg) | 將引數強制轉型為字串值（例如，*$string(2) => "2"*）。
數字| $number(arg) | 可能的話，將引數強制轉型為數值（例如，*$number(2) => 2*）。
數字| $sum(array) | 傳回數字陣列的算術總和（例如，*([1,2,3]) = 6*）。
數字| $average(array) | 傳回數字陣列的平均值（例如，*([1,2,3]) = 2.0*）。
布林| $exists(expression) | 如果表示式中有此內容，則傳回 *true*，否則傳回 *false*。
陣列| $count(array) | 傳回 array 參數中的項目數（例如，*([1,2,3,4]) = 4*）。如果 array 參數不是陣列，而是另一個 JSON 類型的值，則會將此參數視為包含該值的單態陣列，而此函數會傳回 *1*。
陣列| $append(array1, array2) | 傳回包含 *array1* 中的值且後面接著 *array2* 中的值的陣列（例如，*([1,2], [3,4]) = [1,2,3,4]*）。如果任一參數不是陣列，則會將它視為包含該值的單態陣列（例如，*$append("Hello", "World") => ["Hello", "World"]*）。

## 陣列
使用 JSON 陣列，依指定的順序放置值的集合。陣列會將陣列中的每一個值與索引或位置相關聯。若要處理陣列中的個別值，您必須在陣列的欄位名稱後面使用方括弧來指定索引。如果方括弧包含數字，或求值為數字的表示式，則該數字代表要選取之值的索引。數字陣列也可以用來作為索引（例如 *["a","b","c"][[1,2]] -> ["b", "c"]*）。陣列 *[1,2]* 是用來作為索引，以識別要從陣列 *["a","b","c"]* 中選取的值。 

索引是零偏移，因此，陣列 *arr* 中的第一個值是 *arr[0]*（例如，*[1,2,3][0] -> 1*）。如果數字不是整數，則會將它捨入為整數（例如，*[1,2,3][0.9] -> [1]*）。

使用從陣列結尾開始計算的負索引（例如，*[1,2,3][-1] -> 3*）。 

如果指定的索引超過陣列大小，則不會選取任何項目。

## 建構輸出
您可以使用陣列建構子或物件建構子，來指定已處理資料在輸出中的呈現方式。

### 支援的陣列建構子
您可以用一組方括弧 [] 括住以逗點區隔的文字或表示式清單，來建構陣列。逗點是用來區隔陣列建構子內的多個表示式（例如，*[1, 3-1] = [1,2]*）。

### 支援的物件建構子
{: #constructors}
您可以在輸出中使用一組大括弧 {} 來建構 JSON 物件，其中包含以逗點區隔的索引鍵值或配對，而且每一個索引鍵及值都是以冒號區隔（例如，*{key1 : value1, key2:value2}* 或 *{"hello" : "world"}*）。物件索引鍵必須是字串。  


## 範例：使用陣列來處理及報告溫度資料
下列各節是以[開始使用資料管理](ga_im_example.html)中的範例為建置基礎，以顯示如何使用陣列來維護溫度讀數的滑動視窗，以及計算該視窗內所含之讀數的現行總和或平均值。 

滑動視窗會依到達順序來儲存資料。滑動視窗可以配置成以漸進式形式收回資料，而不是保持所有資料的插入狀態。滑動視窗填滿時，任何未來的插入都會導致在該視窗中收回最舊的資料項目。

下列範例顯示已配置大小為 5 之計數型收回原則的滑動視窗：
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

下列範例顯示如何藉由新增稱為 *tempReadings* 的陣列，來修改[逐步手冊](ga_im_index_scenario.html#step4)的步驟 7 中所顯示的邏輯介面綱目檔配置。此陣列用來維護最後 5 個值的視窗，這些值是從裝置針對最後 5 個事件所傳送。如果 *tempReadings* 中未儲存任何值，則此陣列會設為 [0]，而下一個收到的讀數會附加至此陣列，此陣列持續成長直到收到 5 個讀數。收到 5 個讀數之後，新的讀數會導致從視窗中移除最舊的讀數。 

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

