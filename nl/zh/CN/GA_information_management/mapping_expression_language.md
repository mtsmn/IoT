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

# 了解映射表达式语言
{: #mapping_expression}

您可以使用映射表达式语言来操作和组合数据，以及格式化可能对已处理数据运行的任何查询的结果。映射表达式语言是 [JSONata ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://docs.jsonata.org/index.html){:new_window} 的子集，可在定义[映射](ga_im_definitions.html#definitions_resources)时使用。JSONata 是用于 JSON 数据的轻量级查询和转换语言。

以下信息显示当前受支持的关键运算符和函数，以及您可以如何使用它们的一些示例。 

## 受支持的 JSONata 运算符
{: #operators}

支持以下 JSONata 运算符子集： 

运算符的类型                   | 受支持的运算符    | 注释

------------- | ------------- | -------------
算术 | *+* - / * % | % 运算符返回余数。
比较 | < <= > >= != = | 等式运算符为 =，就像在 JSONata 中一样。
布尔 | *in*、*and* | 布尔常量为 *true* 或 *false*。
条件三元 | ? | ? 运算符基于测试条件的结果，求两个可选表达式其中一个的值。运算符采用以下格式 *expression*？*value_if_true* : *value_if_false*
字符串| & | & 运算符将操作数的字符串值连接到单个结果字符串中。
序列生成器| .. | 创建递增整数的数组，从 LHS 上的数字开始，到 RHS 上的数字结束，例如 [1..4] -> [1,2,3,4]。操作数必须求值为整数。序列生成器只能在数组构造函数 [] 中使用。
其他 | . | 点运算符用于带有字面值键的对象访问，例如 $event.object.hh。*
e:* 左侧的表达式受限于事件 ($event) 或状态 ($state) 或元数据 ($instance) 中的特定属性。

**注：** 
- 对表达式分组使用括号 ( ) 并更改运算符优先顺序
- 使用单引号将包含空格的属性名称括起来，例如 $event.object.'a b' 

## 扩展映射表达式语言

已扩展映射表达式语言，通过引入 $event、$state 和 $instance 变量，与数据管理功能配合使用。在对表达式求值之前，JSON 会绑定到这些变量。下表提供了这些变量的概述，这些变量定义为在表达式中使用：

变量| 示例输入 JSON| 作为表达式的示例| 用于...
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | 选择要在表达式中使用的入站事件的属性。
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | 选择设备状态上要在表达式中使用的属性。
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | 选择要在表达式中使用的设备或设备类型属性。

您还可以定义一个将这些变量的使用组合在一起的表达式。在以下示例中，*temp_adjustment* 属性在设备元数据中定义，用于校准事件读取。属性在一个映射中定义，但可以应用到许多设备。 

```
"propertyMappings" : {
    "tevt" : {
       "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## 受支持的 JSONata 函数
{: #functions}
支持以下 JSONata 函数子集： 

函数的类型 |函数 | 描述
| 示例
------------- | ------------- | ------------- 
字符串| $substring(string, start_index[, length]) | 提取子字符串。例如 *$substring("Hello World", 3, 5) => "lo Wo"*。
字符串| $string(arg) | 将自变量强制转换为字符串值，例如 *$string(2) => "2"*。
数值 | $number(arg) | 果可能，将参数强制转换为数字值，例如 *$number(2) => 2*。
数值 | $sum(array) | 返回数字数组的算术和，例如 *([1,2,3]) = 6*。
数值 | $average(array) | 返回数字数组的平均值，例如 *([1,2,3]) = 2.0*。
布尔 | $exists(expression) | 如果属性在表达式中存在则返回 *true*，否则返回 *false*。
数组 | $count(array) | 回数组参数中的项数，例如 *([1,2,3,4]) = 4*。如果数组参数不是数组，而是另一 JSON 类型的值，那么该参数将被视为包含该值的单个数组，并且此函数返回 *1*。
数组 | $append(array1, array2) | 返回一个数组，该数组包含 *array1* 中值，后跟 *array2* 中的值，例如 *([1,2], [3,4]) = [1,2,3,4]*。如果任一参数不是数组，那么会将其视为包含该值的单个数组，例如 *$append("Hello", "World") => ["Hello", "World"]*。

## 数组
使用 JSON 数组按指定顺序放置值的集合。数组使数组中的每个值与索引或位置相关联。要处理数组中的各个值，必须使用数组的字段名称后的方括号来指定索引。如果方括号包含数字或者求值为数字的表达式，那么该数字代表要选择的值的索引。数字数组也可以用作索引，例如 *["a","b","c"][[1,2]] -> ["b", "c"]*。数组 *[1,2]* 用作标识要从数组 *["a","b","c"]* 中选择的值的索引。 

索引为零偏移量，因此数组 *arr* 中的第一个值为 *arr[0]*，例如 *[1,2,3][0] -> 1*。如果数字不是整数，那么会向下舍入为整数，例如 *[1,2,3][0.9] -> [1]*。

使用负索引从数组末尾进行计数，例如 *[1,2,3][-1] -> 3*。 

如果指定的索引超过数组的大小，那么不会选择任何内容。

## 构造输出
您可以使用数组构造函数或对象构造函数，来指定如何在输出中显示处理的数据。

### 受支持的数组构造
通过将以逗号分隔的文字或表达式列表括在方括号 [] 中，可以构造数组。逗号用来分隔数组构造函数中的多个表达式，例如 *[1, 3-1] = [1,2]*。

### 受支持的对象构造函数
{: #constructors}
您可以使用一对花括号 {} 在输出中构造 JSON 对象，方法是在花括号中包含由逗号分隔的键值或对，并以冒号分隔每个键和值，例如 *{key1 : value1, key2:value2}* 或 *{"hello" : "world"}*。对象键必须是字符串。  


## 示例：使用数组处理和报告温度数据
以下各部分以[数据管理入门](ga_im_example.html)中的示例为基础构建，用于说明如何使用数组来维护温度读数的滑动窗口，并计算该窗口中包含的读数的当前总和或平均值。 

滑动窗口以到达顺序存储数据。滑动窗口可以配置为以递增方式逐出数据，而不是保留所有插入的数据。当滑动窗口填满时，任何未来插入都会导致逐出该窗口中最旧的数据项。

以下示例显示了一个滑动窗口，其配置了大小为 5 的基于计数的逐出策略：
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

以下示例显示了如何通过添加名为 *tempReadings* 的数组，来修改[逐步指南](ga_im_index_scenario.html#step4)步骤 7 中显示的逻辑接口模式文件配置。此数组用于针对最后 5 个事件，维护从设备发送的最后 5 个值的窗口。如果没有值存储在 *tempReadings* 中，那么会将该数组设置为 [0]，并且会将下一个接收的读数追加到该数组，该数组会增大，直到接收到 5 个读数为止。接收到 5 个读数后，新读数会使最早的读数从窗口中除去。 

**重要注意事项：**必须将 *tempReadings* 设置为“required”，并在缺省情况下将其设置为数组。 

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
以下部分显示了一个示例，该示例说明如何配置映射资源，以基于当前滑动窗口中包含的读数，计算平均温度读数，以及所有温度读数总和：
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

