---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 逐步指南 2 的其他信息 - 配置湿度设备的逻辑接口

使用以下信息创建一个场景：两个湿度设备上的传感器将事件发布到 IBM Watson™ IoT Platform。这两个湿度设备的读数会映射到一个湿度读数。一个设备位于会议室 1 中，另一个设备位于会议室 2 中。


## 关于此任务

使用在[逐步指南 1](../GA_information_management/ga_im_index_scenario.html) 中所用的相同 {{site.data.keyword.iot_short_notm}} 组织实例以及该组织的 API 密钥或令牌。 

此配置是[逐步指南 2](im_index_scenario_thing.html) 文档中所述教程的先决条件。

在此场景中，创建了一种设备类型和两个设备实例。

设备实例名为 *humiditySensor1* 和 *humiditySensor2*。这两个设备发布湿度事件。湿度事件具有以下示例有效内容：
```
{
  "hum" : 35
}
```
湿度事件在主题 `iot-2/evt/humevt/fmt/json` 上发布。 

**注：**事件标识为 *humevt*。向物理接口添加这些类型的湿度事件时，以及定义映射以用于将与此类型的入站事件关联的属性映射到逻辑接口中的属性时，此标识是必需的。在此场景中，逻辑接口中定义的属性名为 **humidity**。此逻辑接口表示此类型的设备的状态，结构如下：
```
{
  "humidity" : <current humidity value in Celsius>
}
```

使用以下示例场景来设置您自己的接口环境。

**重要说明：**必须将 curl 响应中生成的标识保存为完成其他任务所需的标识。


## 根据需要添加设备类型和设备
{: #step14}

在此场景中，假定有一种设备类型和两个设备实例。设备实例 *humiditySensor1* 和 *humiditySensor2* 与设备类型 *HumiditySensor* 相关联。 

可以使用 [{{site.data.keyword.iot_short_notm}} 仪表板 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://internetofthings.ibmcloud.com){: new_window} 或使用 REST API 来创建设备类型和设备。有关使用 {{site.data.keyword.iot_short_notm}} IoT Platform 仪表板添加设备类型和设备的更多信息，请参阅[使用 Web 界面管理数据入门](../GA_information_management/im_ui_flow.html)文档。

以下示例显示了如何使用 REST API 来创建名为 *HumiditySensor* 的设备类型：

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**重要说明：**可以在创建设备类型和设备时添加元数据。在此场景中，将向设备类型 *HumiditySensor* 添加以下元数据：
```
{
    "maxHumidityThreshold": 70
}
```
[创建规则](im_rules.html)（在 {{site.data.keyword.iot_short_notm}} 从 *humiditySensor1* 或 *humiditySensor2* 收到导致设备状态 *humidity* 属性超过值 70% 的湿度事件时触发）时，可以使用此元数据。 

## 注册设备

以下示例显示了如何注册名为 *humiditySensor1* 的设备实例：
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
以下示例显示了如何注册称为 *humiditySensor2* 的设备实例：
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**注：**设备通过 Watson IoT Platform HTTP REST API 发出 HTTP 请求时，需要用户名和密码。密码是注册设备时自动生成或手动指定的认证令牌的值。如果使用的是 MQTT 客户机，那么必须记录设备的认证令牌，因为需要该令牌通过预订 IoT 主题字符串来检索设备或事物状态。

## 步骤 1：创建事件模式文件
{: #step1}

对于此场景，创建一个事件模式文件以定义入站湿度事件的结构。

以下示例显示了如何创建名为 *humEventSchema.json* 的模式文件。此文件定义来自湿度设备的入站事件的结构：

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "defines the structure of a humidity event",
    "properties" : {
        "humidity" : {
            "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
   },
   "required" : [
       "humidity"
    ]
}
```
**提示：**使用 **required** 参数将一个或多个属性标记为必需属性。设备消息中必须包含必需属性，{{site.data.keyword.iot_short_notm}} 才能使用设备数据更新设备状态。不会对不包含必需属性的消息进行处理。   


## 步骤 2：为事件类型创建事件模式资源
{: #step2}

要创建事件模式资源，请使用以下 API：

```
POST /draft/schemas
```

模式定义文件将在多部分 POST (multipart/form-data) 中传递到 Watson IoT Platform。该 POST 的主体必须至少包含两个部分：

- 一个部分的名称为 **schemaFile**，包含作为该部分主体的文件的实际内容。
- 一个部分的名称为 **name**，包含用于定义该部分主体中模式资源名称的字符串。

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文档。



以下示例显示了如何使用 cURL 创建事件模式资源 *humEventSchema.json*：

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**注：**示例授权值 `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` 由以下信息组成：
`{API Key}:{authorization token}`，随后会对该信息进行基本 64 位编码。

以下示例显示了对 POST 方法的响应：

```
{
  "name" : "humEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "humEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e20",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
向事件类型添加事件模式时，需要 POST 方法的响应中返回的模式标识 *5846cd7c6522050001db0e20*。



## 步骤 3：创建引用事件模式的事件类型
{: #step3}

每种事件类型都会引用在先前示例中，使用在对用于创建事件模式资源的 POST 方法的响应中返回的模式标识创建的相关事件模式。

要创建事件类型，请使用以下 API：

```
POST /draft/event/types
```
草稿事件类型必须引用定义入站 MQTT 事件结构的模式定义。


有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) 文档。




以下示例显示了如何使用 cURL 为湿度事件创建事件类型：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

模式标识 *5846cd7c6522050001db0e20* 用于向事件类型添加事件模式。此标识是在用于创建事件模式资源 *humEventSchema.json* 的 POST 方法的响应中返回的。

以下示例显示了对 POST 方法的响应：

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e20",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20"
  },
  "name" : "humEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e21",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

在 POST 方法的响应中返回的事件类型标识 *5846cd7c6522050001db0e21* 用于向物理接口添加事件类型。


## 步骤 4：创建物理接口
{: #step7}

要创建物理接口，请使用以下 API：

```
POST /draft/physicalinterfaces
```

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文档。



在此场景中，我们需要一个用于先前创建的事件类型的物理接口。

以下示例显示了如何使用 cURL 创建物理接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

在响应中返回的物理接口标识 *5846cd7c6522050001db0e22* 将在为了向物理接口添加以摄氏度为单位测量的温度事件而调用的 POST 方法的 URL 中使用。


## 步骤 5：向物理接口添加事件类型
{: #step8}

要向物理接口添加事件类型，请使用以下 API：

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文档。



在此场景中，将向指定的物理接口添加以下事件类型：
- 湿度事件 *humevt* 将使用来自主题的 *eventId* 以及通过创建事件模式资源而生成的 *eventTypeId*，添加到标识为 *5846cd7c6522050001db0e22* 的物理接口。

以下示例显示了如何使用 cURL 向标识为 *5846cd7c6522050001db0e22* 的物理接口添加湿度事件 *humevt*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## 步骤 6：更新设备类型以连接物理接口
{: #step9}

要更新设备类型，请使用以下 API：

```
POST /draft/device/types/{typeId}/physicalinterface
```

其中，*typeId* 是设备类型标识。


有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



在此场景中，设备类型 *HumiditySensor* 更新为连接到物理接口 *5846cd7c6522050001db0e22*。

以下示例显示了如何使用 cURL 更新设备类型 *HumiditySensor*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

以下示例显示了对 POST 方法的响应：
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
添加物理接口和逻辑接口时，设备标识 *HumiditySensor* 是必需的。
  


## 步骤 7：创建逻辑接口模式文件
{: #step4}

以下示例显示了如何创建名为 *hygrometer.json* 的逻辑接口模式文件。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema that defines the canonical interface for a hygrometer",
    "properties" : {
        "humidity" : {
            "description" : "percentage humidity",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## 步骤 8：创建逻辑接口模式资源
{: #step5}
以下示例显示了如何使用 cURL 创建逻辑接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
以下示例显示了对 POST 方法的响应：
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometer",
  "version" : "draft"
}
```

## 步骤 9：创建引用逻辑接口模式的逻辑接口
{: #step6}

要创建逻辑接口，请使用以下 API：

```
POST /draft/logicalinterfaces
```

您可以选择为逻辑接口指定有意义的别名。别名可以在用于检索设备状态的 API 调用或主题字符串预订中引用，而不使用自动生成的逻辑接口标识。

**注：**别名的长度必须为 1 到 36 个字符，并且可以包含字母数字、连字符、句点和下划线字符。别名不能是 24 个字符的十六进制字符串。 

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 文档。



在此场景中，使用在先前响应中返回的模式标识 *5846cd7c6522050001db0e23*，向逻辑接口添加逻辑接口模式。

以下示例显示了如何使用 cURL 创建逻辑接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometermometer",
  "version" : "draft"
}
```
在此场景中，使用在 POST 方法的响应中返回的逻辑接口标识 *5846cd7c6522050001db0e24*，向设备类型添加逻辑接口。此外，还将使用此标识将入站设备事件映射到由逻辑接口定义的属性。

您可以使用 HTTP REST API 或预订主题字符串，通过逻辑接口别名 *IHygrometer* 来检索设备的状态。


## 步骤 10：向设备类型添加逻辑接口

以下示例显示了如何使用 cURL 向设备类型 HumiditySensor 添加引用逻辑接口模式标识 5846cd7c6522050001db0e23 的逻辑接口 5846cd7c6522050001db0e24：
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
以下示例显示了对 POST 方法的响应：
```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Hygrometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846cd7c6522050001db0e24",
  "schemaId" : "5846cd7c6522050001db0e23"
}
```

## 步骤 11：向设备类型添加映射

    
以下示例显示了如何使用 cURL 向设备类型 *HumiditySensor* 添加映射：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

以下示例显示了对 POST 方法的响应：
```
{
  "logicalInterfaceId": "5846cd7c6522050001db0e24",
  "propertyMappings": {
    "humevt": {
      "humidity": "$event.hum"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## 步骤 12：激活设备类型的配置


    
以下示例显示了如何使用 cURL 验证并激活设备类型 *HumiditySensor* 的配置：
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
以下示例显示了对 PATCH 方法的响应：

```
{
 "message": "CUDRS0520I: State update configuration for device type 'HumiditySensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["HumiditySensor"]
  },
 "failures": []
}
```
