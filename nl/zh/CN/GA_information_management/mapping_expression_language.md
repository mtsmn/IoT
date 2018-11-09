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

# 了解映射表达式语言
{: #mapping_expression}

您可以使用映射表达式语言来处理和组合数据，以及格式化可能对已处理数据运行的任何查询的结果。映射表达式语言是 [JSONata ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/index.html){:new_window} 的子集，可在定义[映射](ga_im_definitions.html#resources)或创建[规则](../information_management/im_rules.html)时使用。 

JSONata 是用于 JSON 数据的轻量级查询和转换语言。此外，还提供了 [JSONata Exerciser ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://try.jsonata.org/){:new_window} 工具，用于快速、方便地试用 JSONata。

以下信息显示当前受支持的关键运算符和函数，以及您可以如何使用它们的一些示例。 

## JSONata 运算符
{: #operators}

支持所有 [JSONata 运算符 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/operators.html){:new_window}，但以下运算符除外：

- ~>（函数链接）
- ^(…)（排序依据）
- :=（变量绑定）

**注：** 
- 对表达式分组使用括号 ( ) 并更改运算符优先顺序
- 使用单引号或双引号将字符串和属性名括起，例如 event.object.'ab' 
- 使用反引号将包含特殊字符（包括空格）的属性名括起，例如： 
```
{"x y":22}.`x y`
```
- 使用反引号将以数字开头的属性名括起，例如：
```
`7emperature`
```

## JSONata 函数
{: #functions}
支持以下 JSONata 函数： 

 - 所有[字符串 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/string-functions.html){:new_window} 函数
 - 所有[数字 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/numeric-functions.html){:new_window} 函数 	
 - 所有[数字汇总 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 函数 
 - 所有[布尔值 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/boolean-functions.html){:new_window} 函数  
 - [$count 和 $append 数组 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/array-functions.html){:new_window} 函数 	

## 扩展映射表达式语言

已扩展映射表达式语言，通过引入 $event、$state 和 $instance 变量，与数据管理功能配合使用。在对表达式求值之前，JSON 会绑定到这些变量。下表提供了这些变量的概述，这些变量定义为在表达式中使用：

变量|示例输入 JSON|作为表达式的示例|用于...
------------- | ------------- | -------------
$event |*{"t": 34.5}*  |$event.t |选择要在表达式中使用的入站事件的属性。
$state |*{"temperature": 34.5,"humidity": 78 }*  |$state.temperature |选择设备状态上要在表达式中使用的属性。
$instance |*{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*|$instance.metadata.temp_adjustment |选择要在表达式中使用的设备或设备类型属性。

以下示例使用以下对象作为事件的输入：  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
通过使用以下表达式将对象转换为数组：

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
此表达式将生成以下输出：
```
 [
    35,
    72,
    1024
  ]
```

您可以反转此示例，并将数组从输入转换为对象。以下示例使用以下数组作为事件的输入：
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
通过使用以下表达式将数组转换为对象：
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
 此表达式将生成以下输出：
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
您还可以定义一个将这些变量的使用组合在一起的表达式。在以下示例中，*temp_adjustment* 属性在设备元数据中定义，用于校准事件读数。属性在一个映射中定义，但可以应用到许多设备。

```
"propertyMappings" : {
    "tevt" : {
       "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


点运算符“.”用于带有字面值键的对象访问，例如 $event.object.hh。左侧的表达式受限于事件 ($event)、状态 ($state) 或元数据 ($instance) 中的特定属性。右侧的表达式包含您可能要访问的信息。

有关点运算符的更多信息，请参阅 JSONata 文档的[运算符 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/operators.html){:new_window} 部分。


## 语言指南

- 支持所有[基本选择 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/basic.html){:new_window}。
- 支持所有[复杂选择 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/complex.html){:new_window}，但通配符除外。
- 支持条件表达式和带括号的表达式作为[编程构造 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/programming.html){:new_window} 的一部分。通过将 $instance、$state 和 $event 变量作为扩展映射语言（特定于 Watson IoT Platform 数据管理）的一部分，支持变量。当前不支持在 JSONata 中使用的“$”和“$$”变量。 

## 构造输出
您可以使用数组构造函数或对象构造函数来指定如何在输出中显示处理的数据。

### 支持的数组构造函数
通过将以逗号分隔的文字或表达式列表括在方括号 [] 中，可以构造数组。逗号用来分隔数组构造函数中的多个表达式，包括序列生成，例如：*[1, 3-1] = [1,2]*、*[1..3] -> [1,2,3]* 和 *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### 支持的对象构造函数
{: #constructors}
您可以使用一对花括号 {} 在输出中构造 JSON 对象，方法是在花括号中包含由逗号分隔的键值或对，并以冒号分隔每个键和值，例如 *{key1 : value1, key2:value2}* or *{"hello" : "world"}*。对象键必须是字符串。


## 示例：使用数组处理和报告温度数据
以下场景基于[使用 REST API 开始进行数据管理](ga_im_example.html) 中的示例构建，显示可如何使用数组来维护温度读数的滑动窗口，并计算该窗口中所包含读数的当前总和或平均值。

滑动窗口以到达顺序存储数据。滑动窗口可以配置为以递增方式逐出数据，而不是保留所有插入的数据。当滑动窗口填满时，任何未来插入都会导致逐出该窗口中最旧的数据项。

以下示例显示了一个滑动窗口，其配置了大小为 5 的基于计数的逐出策略：
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

以下示例显示了如何通过添加名为 *tempReadings* 的数组，修改[逐步指南 1](ga_im_index_scenario.html#step4) 的步骤 7 中显示的逻辑接口模式文件配置。此数组用于针对最后 5 个事件，维护从设备发送的最后 5 个值的窗口。如果没有值存储在 *tempReadings* 中，那么会将该数组设置为 [0]，并且会将下一个接收的读数追加到该数组，该数组会增大，直到接收到 5 个读数为止。接收到 5 个读数后，新读数会使最早的读数从窗口中除去。

**重要说明：**必须将 *tempReadings* 设置为“required”，并在缺省情况下将其设置为数组。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature": {
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
  "required": [
      "tempReadings"
  ],
  "type": "object"
}
```
以下部分显示了一个示例，该示例说明如何配置映射资源，以基于当前滑动窗口中包含的读数，计算平均温度读数，以及所有温度读数总和：
```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt": {
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
**注：**在 $average 和 $sum 函数中使用 *$state.tempReadings* 数组之前，会重新计算该数组。由于无法控制映射表达式的顺序，因此在对 *tempAverage* 或 *tempSum* 表达式求值时，需要重新计算数组以确保数组包含当前值。

以下示例显示了基于 5 个温度读数的滑动窗口所计算的平均温度和总温度：
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

## 处理映射表达式与输入数据之间的不匹配问题

当某个映射表达式包含对所发布事件中未指定的输入数据的引用时，状态更新可能会失败。

例如，您可能配置了以下表达式：
```
temperature = $event.t 
humidity = $event.hum
```
其中，*t* 是事件的可选属性。

如果收到仅包含湿度数据的事件（例如 `{"humidity":22}`），那么表达式 `temperature = $event.t` 会求值失败，因为在发布的设备事件中未指定可选的 *t* 属性。

状态更新失败。湿度状态属性未更新，并且会将错误消息发布到设备的 MQTT 错误主题：
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
要防止由于未指定的可选数据而导致状态更新失败，可以将 $exists 函数与一个三元条件配合使用，以指定可选属性的缺省值。以下示例为 *t* 属性定义了缺省值 *0*：
```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

通过以这种方式为可选属性定义缺省值，表达式将成功求值，即使在发布的设备事件中未指定 *t* 属性也是如此。

因此，如果收到事件 `{"humidity":22}`，说明状态更新成功完成，并且设备状态会设置为 `{"humidity":22, "temperature":0}`。
