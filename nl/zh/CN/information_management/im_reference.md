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

# 数据管理参考信息

使用以下参考信息可了解使用 {{site.data.keyword.iot_full}} 的数据管理组件时适用的限制。 

## 模式限制

数据管理中使用以下模式类型：
- 事件类型模式
- 逻辑接口模式
- 事物类型模式

下表列出了适用于每种模式类型的限制：

模式类型|约束
------------------- | -------------
全部|所有模式都必须符合有效的 JSON。
全部|所有模式都必须符合有效的 JSON 模式。
全部|所有模式的根的类型都必须为“object”。
全部|所有模式都具有最多 7 个嵌套级别。
逻辑接口|所有指定为必需的属性都必须定义缺省值。
逻辑接口|如果在模式中定义了对象，那么必须为该对象定义至少一个属性。
逻辑接口|如果在模式中定义了数组，那么关联项只能为一种类型，例如类型只能为“string”。
事物类型|只允许使用顶级属性。除第一个级别外，不允许嵌套。
事物类型|顶级属性的类型必须为“object”。
事物类型|顶级属性必须引用 $logicalInterfaceRef 和 type。$logicalInterfaceRef 的值是逻辑接口的标识或别名。type 必须设置为“object”或“array”。

## 有效和无效模式的示例

### 示例 1 - 有效的逻辑接口模式定义
以下示例定义的逻辑接口模式符合“模式限制”部分中所概述的约束：

  - 模式符合有效 JSON 和有效 JSON 模式。
  - 模式的类型为“object”。
  - 模式的嵌套级别小于 7。 
  - 为模式定义了两个属性。 
  - 必需属性 **a** 和 **b** 已指定缺省值。

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


### 示例 2 - 无效的逻辑接口模式定义
以下示例显示了无效的逻辑接口模式。模式无效的原因是，定义的数组中包含类型为“string”或类型为“number”的项。如果在模式中定义了数组，那么关联项只能为一种类型，例如“string”。

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
### 示例 3 - 有效的事物类型模式定义
以下示例定义的事物类型模式符合“模式限制”部分中所概述的约束：

  - 模式符合有效 JSON 和有效 JSON 模式。
  - 模式的类型为“object”。
  - 模式的嵌套级别小于 7。 
  - 模式只定义了顶级属性。 
  - 顶级属性引用 $logicalInterfaceRef 和设置为“array”或“object”的 type。类型“array”可用于聚合多个设备或事物体，例如多个温度传感器。类型“object”可用于引用单个设备或事物，例如单个湿度传感器。   
  - 必需属性不需要指定缺省值。此约束仅适用于逻辑接口模式。不能在此模式类型中指定缺省值。 

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
