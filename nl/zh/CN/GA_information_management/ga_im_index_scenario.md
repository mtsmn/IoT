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


# 逐步指南 1：有关如何通过公共接口使用设备的详细示例
{: #scenario}

使用以下信息创建一个场景：两个温度设备将事件发布到 {{site.data.keyword.iot_full}}。一个设备以摄氏度为单位测量温度。另一个设备以华氏度为单位测量温度。这两个读数都会映射到以摄氏度为单位的单个温度读数。这两个设备发布新的温度读数时，与设备状态关联的属性的值会更改。
{: shortdesc}

## 开始之前

创建[资源](ga_im_definitions.html#definitions_resources)时，该资源将创建为草稿版本。草稿版本是资源的工作副本，您可以使用 API 对其直接进行查询、更新和删除。使用 REST API 时，前缀 **draft/** 用于标识处于草稿状态的资源。 

有关资源的草稿和活动版本的更多信息，请参阅[了解数据管理](ga_im_definitions.html)。

## 先决条件

您必须具有 {{site.data.keyword.iot_short_notm}} [组织实例](../iotplatform_overview.html#organizations)以及该组织的 API 密钥和认证令牌，才能对请求进行认证。 

有关生成 API 密钥的信息，请参阅 [API](../reference/api.html) 文档。有关生成 API 令牌的信息，请参阅[入门教程](../getting-started.html)文档。


## 关于此任务

在此场景中，创建了两个设备。

一个设备名为 *tSensor*。此设备将发布以摄氏度为单位测量的温度事件。温度事件会在主题 `iot-2/evt/tevt/fmt/json` 上发布，并具有以下示例有效内容：
```
{
  "t" : 34.5
}
```

**注：**事件标识为 *tevt*。向物理接口添加此类型的温度事件时，以及定义映射以用于将与此类型的入站事件关联的属性映射到逻辑接口中的属性时，此标识是必需的。在此场景中，逻辑接口中定义的属性名为 **temperature**。

另一个设备名为 *tempSensor*。此设备将发布以华氏度为单位测量的温度事件。温度事件会在主题 `iot-2/evt/tempevt/fmt/json` 上发布，并具有以下示例有效内容：
```
{
  "temp" : 72.55
}
```

**注：**事件标识为 *tempevt*。向物理接口添加此类型的温度事件时，以及定义映射以用于将与此类型的入站事件关联的属性映射到逻辑接口中的属性时，此标识是必需的。在此场景中，逻辑接口中定义的属性名为 **temperature**。

此外，还会配置逻辑接口。此逻辑接口表示此类型的设备的状态，结构如下：
```
{
  "temperature" : <current temperature value in Celsius>
  }
```
此配置表示您可以将应用程序配置为处理与 **temperature** 关联的值，而不是将应用程序配置为处理与 **t** 关联的值以及与 **temp** 关联的值（在该值转换为摄氏度后）。

![{{site.data.keyword.iot_short_notm}} 上温度传感器设备与应用程序之间的映射。](../information_management/images/Information Management Device example.svg "{{site.data.keyword.iot_short_notm}} 上温度传感器设备与应用程序之间的映射")  

使用以下示例场景来设置您自己的接口环境。

**重要说明：**必须将 curl 响应中生成的标识保存为完成其他任务所需的标识。
[逐步指南 1 和 2 的其他信息 - 资源名称和标识](../information_management/im_id_reference.html)中记录了列出本指南中使用的资源属性名称、值和标识的表。

## 根据需要添加设备类型和设备
{: #step14}

在此场景中，假定有两种设备类型和两个设备实例。设备实例 *tSensor* 与设备类型 *TSensor* 相关联。设备实例 *tempSensor* 与设备类型 *TempSensor* 相关联。 

可以使用 [{{site.data.keyword.iot_short_notm}} 仪表板 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://internetofthings.ibmcloud.com){: new_window} 或使用 REST API 来创建设备类型和设备。有关使用 {{site.data.keyword.iot_short_notm}} 仪表板添加设备类型和设备的更多信息，请参阅[使用 Web 界面管理数据入门](im_ui_flow.html)文档。

以下示例显示了如何使用 REST API 来创建名为 *TSensor* 的设备类型：

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TSensor", "description" : "The Celsius sensor device type", "metadata": {"tempThresholdMax": 44,
    "tempThresholdMin": 10}}' \
 ```
 
 以下示例显示了如何使用 REST API 来创建名为 *TempSensor* 的设备类型：

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TempSensor", "description" : "The Fahrenheit sensor device type"}' \
 ```

**注：**可以在创建设备类型和设备时添加元数据。在此场景中，将向设备类型 *TSensor* 添加以下元数据：
```
{
    "tempThresholdMax": 44,
    "tempThresholdMin": 10 
}
```
创建将在 {{site.data.keyword.iot_short_notm}} 从 *tSensor* 设备收到导致设备状态的 *temperature* 属性超过摄氏 44 度的温度事件时触发的[规则](../information_management/im_rules.html)时，将使用此元数据。 


然后，需要注册与设备类型关联的设备实例。以下示例显示了如何使用 REST API 来注册与设备类型 *TSensor* 关联的名为 *tSensor* 的设备实例：
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tSensor", "authToken": "password"}' \
```

以下示例显示了如何使用 REST API 来注册与设备类型 *TempSensor* 关联的名为 *tempSensor* 的设备实例：
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tempSensor", "authToken": "password"}' \
```

有关使用 REST API 来添加设备类型和设备的信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window} 文档。

**注：**设备通过 Watson IoT Platform HTTP REST API 发出 HTTP 请求时，需要用户名和密码。密码是注册设备时自动生成或手动指定的认证令牌的值。如果使用的是 MQTT 客户机，那么必须记录设备的认证令牌，因为需要该令牌通过预订主题字符串来检索设备或事物状态。

## 步骤 1：创建事件模式文件
{: #step1}

对于此场景，创建两个事件模式文件以定义每个入站温度事件的结构。

以下示例显示了如何创建名为 *tEventSchema.json* 的模式文件。此文件定义来自以摄氏度为单位测量温度的温度设备的入站事件的结构：

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tEventSchema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
      "description" : "temperature in degrees Celsius",
      "type" : "number",
      "minimum" : -273.15,
      "default" : 0.0
    }
  },
  "required" : ["t"]
}
  ```
**提示：**使用 **required** 参数将一个或多个属性标记为必需属性。设备消息中必须包含必需属性，{{site.data.keyword.iot_short_notm}} 才能使用设备数据更新设备状态。不会对不包含必需属性的消息进行处理。   

为事件类型创建事件模式资源时，将使用模式文件名 *tEventSchema*。

以下示例显示了如何创建名为 *tempEventSchema.json* 的模式文件。此文件定义来自以华氏度为单位测量温度的温度设备的入站事件的结构：

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tempEventSchema",
  "description" : "defines the structure of a temperature event in degrees Fahrenheit",
  "properties" : {
    "temp" : {
      "description" : "temperature in degrees Fahrenheit",
      "type" : "number",
      "minimum" : -459.67,
      "default" : 0.0
    }
  },
  "required" : ["temp"]
}
  ```
为事件类型创建事件模式资源时，将使用模式文件名 *tempEventSchema*。   

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



以下示例显示了如何使用 cURL 创建事件模式资源 *tEventSchema.json*：

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

**提示：**示例授权值 `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` 由以下信息组成：
`{API Key}:{authorization token}`，随后会对该信息进行基本 64 位编码。

以下示例显示了对 POST 方法的响应：

```
{
  "name" : "tEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "tEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e0d",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
向事件类型添加事件模式时，需要在对 POST 方法的响应中返回的模式标识 *5846cd7c6522050001db0e0d*。



以下示例显示了如何使用 cURL 创建事件模式资源 *tempEventSchema.json*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

以下示例显示了对 POST 方法的响应：

```
{
  "schemaType" : "json-schema",
  "schemaFileName" : "tempEventSchema.json",
  "updated" : "2016-12-06T14:44:51Z",
  "name" : "tempEventSchema",
  "version" : "draft",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "created" : "2016-12-06T14:44:51Z",
  "id" : "5846cee36522050001db0e0e",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e/content"
  },
  "contentType" : "application/octet-stream",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
向事件类型添加事件模式时，需要在对 POST 方法的响应中返回的模式标识 *5846cee36522050001db0e0e*。



## 步骤 3：创建引用事件模式的事件类型
{: #step3}

每种事件类型都会引用在先前示例中，使用在对用于创建事件模式资源的 POST 方法的响应中返回的模式标识创建的相关事件模式。

要创建事件类型，请使用以下 API：

```
POST /draft/event/types
```
草稿事件类型必须引用定义入站 MQTT 事件结构的模式定义。


有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) 文档。




以下示例显示了如何使用 cURL 为以摄氏度为单位测量的温度事件创建事件类型：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

模式标识 *5846cd7c6522050001db0e0d* 用于向事件类型添加事件模式。此标识在对用于创建事件模式资源 *tEventSchema.json* 的 POST 方法的响应中返回。

以下示例显示了对 POST 方法的响应：

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e0d",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d"
  },
  "name" : "tEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846d0fd6522050001db0e0f",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

事件类型标识 *5846d0fd6522050001db0e0f* 在对用于向物理接口添加事件类型的 POST 方法的响应中返回。

以下示例显示了如何使用 cURL 为以华氏度为单位测量的温度事件创建事件类型：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
模式标识 *5846cee36522050001db0e0e* 用于向事件类型添加事件模式。此标识在对用于创建事件模式资源 *tempEventSchema.json* 的 POST 方法的响应中返回。以下示例显示了对 POST 方法的响应：

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "schemaId" : "5846cee36522050001db0e0e",
  "created" : "2016-12-06T15:00:20Z",
  "id" : "5846d2846522050001db0e10",
  "updated" : "2016-12-06T15:00:20Z",
  "name" : "tempEvent",
  "version" : "draft",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e"
  },
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
事件类型标识 *5846d2846522050001db0e10* 在对用于向物理接口添加事件类型的 POST 方法的响应中返回。



## 步骤 4：创建物理接口
{: #step7}

要创建物理接口，请使用以下 API：

```
POST /draft/physicalinterfaces
```

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文档。



在此场景中，需要两个物理接口，分别用于每种事件类型。

以下示例显示了如何使用 cURL 创建第一个物理接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TSensor Physical Interface"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

在响应中返回的物理接口标识 *5847d1df6522050001db0e1a* 将在为了向物理接口添加以摄氏度为单位测量的温度事件而调用的 POST 方法的 URL 中使用。

以下示例显示了如何使用 cURL 创建第二个物理接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TempSensor Physical Interface"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

在响应中返回的物理接口标识 *5847d1df6522050001db0e1b* 将在为了向物理接口添加以华氏度为单位测量的温度事件而调用的 POST 方法的 URL 中使用。   

## 步骤 5：向物理接口添加事件类型
{: #step8}

要向物理接口添加事件类型，请使用以下 API：

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文档。



在此场景中，将向指定的物理接口添加以下事件类型：
- 摄氏温度事件 *tevt* 将使用来自主题的 *eventId* 以及通过创建事件模式资源而生成的 *eventTypeId*，添加到标识为 *5847d1df6522050001db0e1a* 的物理接口。
- 华氏温度事件 *tempevt* 将使用来自主题的 *eventId* 以及通过创建事件模式资源而生成的 *eventTypeId*，添加到标识为 *5847d1df6522050001db0e1b* 的物理接口。


以下示例显示了如何使用 cURL 向标识为 *5847d1df6522050001db0e1a* 的物理接口添加温度事件 *tevt*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

以下示例显示了如何使用 cURL 向标识为 *5847d1df6522050001db0e1b* 的物理接口添加温度事件 *tempevt*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
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



在此场景中，设备类型 *TSensor* 更新为连接到物理接口 *5847d1df6522050001db0e1b*，并且设备类型 *TempSensor* 更新为连接到物理接口 *5847d1df6522050001db0e1a*。


以下示例显示了如何使用 cURL 更新设备类型 *TSensor*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
添加物理接口和逻辑接口时，设备标识 *TSensor* 是必需的。

以下示例显示了如何使用 cURL 更新设备类型 *TempSensor*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

以下示例显示了对 POST 方法的响应：
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
添加物理接口和逻辑接口时，设备标识 *TempSensor* 是必需的。


## 步骤 7：创建逻辑接口模式文件
{: #step4}

以下示例显示了如何创建名为 *thermometer.json* 的逻辑接口模式文件。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "thermometerSchema",
    "description" : "Schema that defines the canonical interface for a thermometer",
    "properties" : {
        "temperature" : {
            "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## 步骤 8：创建逻辑接口模式资源
{: #step5}

要创建逻辑接口模式资源，请使用以下 API：

```
POST /draft/schemas
```

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文档。



以下示例显示了如何使用 cURL 创建逻辑接口模式：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=thermometerSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/thermometer.json"'
```

以下示例显示了对 POST 方法的响应：

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "thermometerSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "thermometer.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
使用在对 POST 方法的响应中返回的模式标识 *5846ec826522050001db0e11*，向逻辑接口添加逻辑接口模式。



## 步骤 9：创建引用逻辑接口模式的逻辑接口
{: #step6}

要创建逻辑接口，请使用以下 API：

```
POST /draft/logicalinterfaces
```

您可以选择为逻辑接口指定有意义的别名。别名可以在用于检索设备状态的 API 调用或主题字符串预订中引用，而不使用自动生成的逻辑接口标识。

**注：**别名的长度必须为 1 到 36 个字符，并且可以包含字母数字、连字符、句点和下划线字符。别名不能是 24 个字符的十六进制字符串。 

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 文档。



在此场景中，使用在先前响应中返回的模式标识 *5846ec826522050001db0e11*，向逻辑接口添加逻辑接口模式。

以下示例显示了如何使用 cURL 创建逻辑接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Thermometer Interface", "alias" : "IThermometer", "schemaId" : "5846ec826522050001db0e11"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "schemaId" : "5846ec826522050001db0e11",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846ed076522050001db0e12",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Thermometer Interface",
  "alias" : "IThermometer",
  "version" : "draft"
}
```
在此场景中，使用在对 POST 方法的响应中返回的逻辑接口标识 *5846ed076522050001db0e12*，向设备类型添加逻辑接口。此外，还将使用此标识将入站设备事件映射到由逻辑接口定义的属性。

您可以使用 HTTP REST API 或预订主题字符串，通过逻辑接口别名 *IThermometer* 来[检索设备的状态](##step13)。

## 步骤 10：向设备类型添加逻辑接口
{: #step10}

要向设备类型添加逻辑接口，请使用以下 API：

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

其中，*typeId* 是设备类型的名称。 

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。

  
**注：**在此场景中，同一逻辑接口 *5846ed076522050001db0e12* 与 *TSensor* 和 *TempSensor* 关联。

以下示例显示了如何使用 cURL 向设备类型 *TSensor* 添加与逻辑模式标识 *5846ec826522050001db0e11* 关联的逻辑接口 *5846ed076522050001db0e12*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

以下示例显示了如何使用 cURL 向设备类型 *TempSensor* 添加引用逻辑接口模式标识 *5846ec826522050001db0e11* 的逻辑接口 *5846ed076522050001db0e12*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

## 步骤 11：定义映射以用于将入站事件中的属性映射到逻辑接口中的属性
{: #step11}  
**重要信息：**如果您正在使用 MQTT 客户机应用程序，并且要预订接收有关设备状态更新的通知，那么在定义映射时需要配置通知设置。通过以这种方式配置您的通知策略，您可以在多种设备类型之间复用逻辑接口，并且每种设备类型都配置为在您选择的过程中接收状态更改通知。无论 MQTT 客户机是否预订指定的主题字符串，都可以定义通知策略。  
  
可以配置以下通知设置：  

参数|	描述
------	|	-----
never	|	不发送通知 - 此设置是缺省值
on-every-event	|	无论设备状态是否更改，都会在发生每个事件时发送通知
on-state-change	|	仅当事件导致设备状态更改时，才发送通知

**注：**如果选择 *never* 通知设置，那么应用程序从不接收关于设备状态更改的通知。因此，您可能会想要选择 *on-every-event* 或 *on-state-change* 通知策略。  

要映射事件，请使用以下 API：

```
POST /draft/device/types/{typeId}/mappings
```

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



在此场景中，将针对设备类型 *TSensor* 定义映射，以用于将入站事件 *tevt* 中的 **t** 属性映射到逻辑接口上的 **temperature** 属性。此外，还将针对设备类型 *TempSensor* 定义映射，以用于将入站事件 *tempevt* 中的 **temp** 属性映射到逻辑接口上的 **temperature** 属性。通知设置设定为“on-state-change”。 

以下示例显示了如何使用 cURL 向设备类型 *TSensor* 添加映射：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**重要信息：**对于已结构化的 [JSON ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://json-schema.org/){:new_window} 事件有效内容，请确保 $event.*property* 条目正确匹配您属性的 JSON 结构。例如，如果有效内容为  `{"d":{"t":<temp>}}`，请使用 `$event.d.t`

指定在对用于创建逻辑接口和设备类型 *TSensor* 的 POST 方法的响应中返回的逻辑接口标识 *5846ed076522050001db0e12*。

以下示例显示了对 POST 方法的响应：

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt" : {
       "temperature" : "$event.t"
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
以下示例显示了如何使用 cURL 向设备类型 *TempSensor* 添加映射：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

指定在对用于创建逻辑接口和设备类型 *TempSensor* 的 POST 方法的响应中返回的逻辑接口标识 *5846ed076522050001db0e12*。将应用转换，使值从华氏测量单位转换为摄氏测量单位。


以下示例显示了对 POST 方法的响应：

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
     "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
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

## 步骤 12：验证并激活配置
{: #step15}

验证并激活与每种设备类型的设备状态更新相关的配置。此配置包括模式、事件类型、物理接口、逻辑接口和映射。

要验证并激活设备类型配置，请使用以下 API：

```
PATCH /draft/device/types/{typeId}
```

其中，*typeId* 是设备类型标识。

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



在此场景中，我们需要验证并激活两种设备类型的配置。

以下示例显示了如何使用 cURL 验证并激活设备类型 *TSensor* 的配置：

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
以下示例显示了对 PATCH 方法的响应：

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TSensor"]
  },
 "failures": []
}
```

以下示例显示了如何使用 cURL 验证并激活设备类型 *TempSensor* 的配置：

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

以下示例显示了对 PATCH 方法的响应：

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TempSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TempSensor"]
  },
 "failures": []
}
```

## 步骤 13：发布入站设备事件
{: #step12}

使用以下示例有效内容，在主题 `iot-2/evt/tevt/fmt/json` 上发布来自 *tSensor* 的温度事件：
```
{
  "t" : 34.5
}
```
使用以下示例有效内容，在主题 `iot-2/evt/tempevt/fmt/json` 上发布来自 *tempSensor* 的温度事件：
```
{
  "temp" : 72.55
}
```

有关从设备发布入站事件的信息，请参阅[应用程序的 MQTT 连接](../applications/mqtt.html#publishing_device_events)。


## 步骤 14：检索设备的状态
{: #step13}

您可以使用 HTTP REST API 或预订主题字符串来检索设备的状态。如果在创建逻辑接口时指定了别名，那么可以使用别名而不使用 *logicalInterfaceId* 参数来检索设备的状态。 

- 如果您具有 MQTT 客户机应用程序，那么可以预订以下主题字符串：  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- 或者，您可以使用以下 HTTP REST API 来检索最新的设备状态：  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

以下参数是必需的：  

参数|	描述
------	|	-----
typeId	|	设备类型标识符。
deviceId	|	设备标识符。
logicalInterfaceId 或别名|	为逻辑接口创建的标识或用户指定的别名。


有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



以下示例显示了如何使用 cURL 通过引用所创建逻辑接口的标识来检索 *tSensor* 的当前状态：
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/tSensor/devices/tSensor/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

逻辑接口标识 *5846ed076522050001db0e12* 会在 GET 方法中使用。此标识在对用于创建逻辑接口的 POST 方法的响应中返回。以下示例显示了对 GET 方法的响应：
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
}
}
```

以下示例显示了如何使用 cURL 通过引用所创建逻辑接口的别名来检索 *tempSensor* 的当前状态：
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices/tempSensor/state/IThermometer \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

逻辑接口别名 *IThermometer* 会在 GET 方法中使用。此标识在对用于创建逻辑接口的 POST 方法的响应中返回。以下示例显示了对 GET 方法的响应：
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
}
}
```

请注意，返回的温度读数以摄氏度而不是华氏度为单位。



您的应用程序可以处理此规范化数据，而无需用于识别或转换不同温标的配置。

## 后续步骤

您可能希望通过配置事物类型和事物实例基于此场景进行构建，以利用数据管理的双联资产功能。有关配置事物的信息，请参阅[逐步指南 2：有关如何通过公共接口使用事物的详细示例 (Beta)](../information_management/im_index_scenario_thing.html)。

您还可以创建规则，以用于在 {{site.data.keyword.iot_short_notm}} 收到的事件导致设备或事物状态发生更改时触发操作。有关创建规则的信息，请参阅[创建嵌入式规则 (Beta)](../information_management/im_rules.html)。

