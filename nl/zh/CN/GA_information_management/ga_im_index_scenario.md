---

copyright:
years: 2016, 2017
lastupdated: "2017-09-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 逐步指南：有关如何通过公共接口使用设备的详细示例
{: #scenario}

使用以下信息创建一个场景：两个温度传感器将事件发布到 {{site.data.keyword.iot_full}}。一个传感器以摄氏度为单位测量温度。另一个传感器以华氏度为单位测量温度。这两个读数都会映射到以摄氏度为单位的单个温度读数。这两个设备发布新的温度读数时，与设备状态关联的属性的值会更改。
{: shortdesc}

## 先决条件

您必须具有 {{site.data.keyword.iot_short_notm}} 组织实例以及该组织的 API 密钥或令牌。

## 关于此任务

在此场景中，配置了两个设备。

一个设备名为 *TemperatureSensor1*。此设备将发布以摄氏度为单位测量的温度事件。温度事件会在主题 `iot-2/evt/tevt/fmt/json` 上发布，并具有以下示例有效内容：
```
{
  "t" : 34.5
}
```

**注：**事件标识为 *tevt*。向物理接口添加此类型的温度事件时，以及定义映射以用于将与此类型的入站事件关联的属性映射到逻辑接口中的属性时，此标识是必需的。在此场景中，逻辑接口中定义的属性名为 **temperature**。

另一个设备名为 *TemperatureSensor2*。此设备将发布以华氏度为单位测量的温度事件。温度事件会在主题 `iot-2/evt/tempevt/fmt/json` 上发布，并具有以下示例有效内容：
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
![{{site.data.keyword.iot_short_notm}} 上温度传感器设备与应用程序之间的映射。](images/Information)  

使用以下示例场景来设置您自己的接口环境。

## 根据需要添加设备类型和设备
{: #step14}

在此场景中，假定有两种设备类型和两个设备实例。设备实例 *TemperatureSensor1* 与设备类型 *EnvSensor1* 相关联。设备实例 *TemperatureSensor2* 与设备类型 *EnvSensor2* 相关联。

有关使用 REST API 来添加设备类型的信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html) 文档。

## 步骤 1：创建草稿事件模式文件
{: #step1}

对于此场景，创建两个草稿事件模式文件以定义每个入站温度事件的结构。

以下示例显示了如何创建名为 *tEventSchema.json* 的草稿模式文件。此文件定义来自以摄氏度为单位测量温度的温度传感器的入站事件的结构：

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor1 tEvent Schema",
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
**提示：**使用“required”参数将一个或多个属性设为必需属性。设备消息中必须包含必需属性，{{site.data.keyword.iot_short_notm}} 才能使用设备数据更新设备状态。不会对不包含必需属性的消息进行处理。   

为事件类型创建事件模式资源时，将使用模式文件名 *tEventSchema*。

以下示例显示了如何创建名为 *tempEventSchema.json* 的草稿模式文件。此文件定义来自以华氏度为单位测量温度的温度传感器的入站事件的结构：

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor2 tempEvent Schema",
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

## 步骤 2：为事件类型创建草稿事件模式资源
{: #step2}

要创建事件模式资源，请使用以下 API：

```
POST /draft/schemas
```

以下参数是必需的：  

参数|	描述
------	|	-----  
name	|	为您要创建的事件模式提供名称。
schemaFile	|	到本地事件模式 JSON 文件的路径。



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

## 步骤 3：创建引用事件模式的草稿事件类型
{: #step3}

每种事件类型都会引用在先前示例中，使用在对用于创建事件模式资源的 POST 方法的响应中返回的模式标识创建的相关事件模式。

要创建草稿事件类型，请使用以下 API：

```
POST /draft/event/types
```

以下参数是必需的：  

参数|	描述
------	|	-----
name	|	为您要创建的事件类型提供名称。

schemaId	|	为事件模式资源创建的标识。



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

## 步骤 4：创建草稿物理接口
{: #step7}

要创建草稿物理接口，请使用以下 API：

```
POST /draft/physicalinterfaces
```

以下参数是必需的：  

参数|	描述
------	|	-----
name	|	为您要创建的物理接口提供名称。



有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文档。



在此场景中，需要两个物理接口，分别用于每种事件类型。

以下示例显示了如何使用 cURL 创建第一个物理接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Env sensor physical interface 1"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "Env sensor physical interface 1",
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
  --data '{"name" : "Env sensor physical interface 2"}'
```

以下示例显示了对 POST 方法的响应：

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

在响应中返回的物理接口标识 *5847d1df6522050001db0e1b* 将在为了向物理接口添加以华氏度为单位测量的温度事件而调用的 POST 方法的 URL 中使用。   

## 步骤 5：向草稿物理接口添加事件类型
{: #step8}

要向物理接口添加事件类型，请使用以下 API：

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

以下参数是必需的：  

参数|	描述
------	|	-----
eventId	|	输入来自设备事件有效内容的事件名称。

eventTypeId	|	为事件类型创建的标识。



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

## 步骤 6：更新草稿设备类型以连接草稿物理接口
{: #step9}

要更新草稿设备类型，请使用以下 API：

```
POST /draft/device/types/{typeId}/physicalinterface
```

其中，*typeId* 是设备类型标识。


有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



在此场景中，设备类型 *EnvSensor1* 更新为连接到物理接口 *5847d1df6522050001db0e1a*，并且设备类型 *EnvSensor2* 更新为连接到物理接口 *5847d1df6522050001db0e1b*。

以下示例显示了如何使用 cURL 更新设备类型 *EnvSensor1*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/physicalinterface \
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
  "name" : "Env sensor physical interface 1",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
添加物理接口和逻辑接口时，设备标识 *EnvSensor1* 是必需的。

以下示例显示了如何使用 cURL 更新设备类型 *EnvSensor2*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/physicalinterface \
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
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
添加物理接口和逻辑接口时，设备标识 *EnvSensor2* 是必需的。

## 步骤 7：创建草稿逻辑接口模式文件
{: #step4}

以下示例显示如何创建名为 *envSensor.json* 的草稿逻辑接口模式文件。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Environment Sensor Schema",
    "description" : "Schema to represent a canonical environment sensor device",
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

## 步骤 8：创建草稿逻辑接口模式资源
{: #step5}

要创建草稿逻辑接口模式资源，请使用以下 API：

```
POST /draft/schemas
```

以下参数是必需的：  

参数|	描述
------	|	-----
name	|	为您要创建的逻辑接口模式提供名称。
schemaFile	|	到本地逻辑接口模式 JSON 文件的路径。



有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文档。



以下示例显示了如何使用 cURL 创建逻辑接口模式：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=temperatureEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/envSensor.json"'
```

以下示例显示了对 POST 方法的响应：

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "temperatureEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "envSensor.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
使用在对 POST 方法的响应中返回的模式标识 *5846ec826522050001db0e11*，向逻辑接口添加逻辑接口模式。

## 步骤 9：创建引用草稿逻辑接口模式的草稿逻辑接口
{: #step6}

要创建草稿逻辑接口，请使用以下 API：

```
POST /draft/logicalinterfaces
```

以下参数是必需的：  

参数|	描述
------	|	-----
name	|	提供您要创建的逻辑接口的名称。
schemaId	|	为逻辑接口模式资源创建的标识。



有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 文档。



在此场景中，使用在先前响应中返回的模式标识 *5846ec826522050001db0e11*，向逻辑接口添加逻辑接口模式。

以下示例显示了如何使用 cURL 创建逻辑接口：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "environment sensor interface", "schemaId" : "5846ec826522050001db0e11"}'
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
  "name" : "environment sensor interface",
  "version" : "draft"
}
```
在此场景中，使用在对 POST 方法的响应中返回的逻辑接口标识 *5846ed076522050001db0e12*，向设备类型添加逻辑接口。此外，还将使用此标识将入站设备事件映射到由逻辑接口定义的属性。

## 步骤 10：向设备类型添加逻辑接口
{: #step10}

要向设备类型添加草稿逻辑接口，请使用以下 API：

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

其中，*typeId* 是设备类型的名称。 

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。

  
**注：**在此场景中，同一逻辑接口 *5846ed076522050001db0e12* 与 *EnvSensor1* 和 *EnvSensor2* 关联。

以下示例显示了如何使用 cURL 向设备类型 *EnvSensor1* 添加引用逻辑模式标识 *5846ec826522050001db0e11* 的逻辑接口 *5846ed076522050001db0e12*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/logicalinterfaces \
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
  "name" : "environment sensor interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

以下示例显示了如何使用 cURL 向设备类型 *EnvSensor2* 添加与逻辑模式标识 *5846ec826522050001db0e11* 关联的逻辑接口 *5846ed076522050001db0e12*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/logicalinterfaces \
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
  "name" : "environment sensor interface",
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

以下参数是必需的：  

参数|	描述
------	|	-----
logicalInterfaceId	|	为逻辑接口创建的标识。
propertyMappings	|	有效的 JSON 结构，将为逻辑接口定义的属性映射到设备事件有效内容的属性。
您可以选择性地设置 *notificationStrategy* 参数。  

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



在此场景中，将针对设备类型 *EnvSensor1* 定义映射以用于将入站事件 *tevt* 中的 **t** 属性映射到逻辑接口上的 **temperature** 属性。此外，还将针对设备类型 *EnvSensor1* 定义映射以用于将入站事件 *tempevt* 中的 **temp** 属性映射到逻辑接口上的 **temperature** 属性。通知设置设定为“on-state-change”。 

以下示例显示了如何使用 cURL 向设备类型 *EnvSensor1* 添加映射：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**重要信息：**对于已结构化的 [JSON ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://json-schema.org/){:new_window} 事件有效内容，请确保 $event.*property* 条目正确匹配您属性的 JSON 结构。例如，如果有效内容为  `{"d":{"t":<temp>}}`，请使用 `$event.d.t`

指定在对用于创建逻辑接口和设备类型 *EnvSensor1* 的 POST 方法的响应中返回的逻辑接口标识 *5846ed076522050001db0e12*。

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
以下示例显示了如何使用 cURL 向设备类型 *EnvSensor2* 添加映射：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

指定在对用于创建逻辑接口和设备类型 *EnvSensor2* 的 POST 方法的响应中返回的逻辑接口标识 *5846ed076522050001db0e12*。将应用转换，使值从华氏测量单位转换为摄氏测量单位。


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

验证并激活与每种草稿设备类型的设备状态更新相关的配置。此配置包括草稿模式、事件类型、物理接口、逻辑接口和映射。

要验证并激活草稿设备类型配置，请使用以下 API：

```
PATCH /draft/device/types/{typeId}
```

其中，*typeId* 是设备类型标识。

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



在此场景中，我们需要验证并激活两种草稿设备类型的配置。

以下示例显示了如何使用 cURL 验证并激活草稿设备类型 *EnvSensor1* 的配置：

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

以下示例显示了对 PATCH 方法的响应：

```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor1' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor1"]
  },
 "failures": []
}
```
以下示例显示了如何使用 cURL 验证并激活草稿设备类型 *EnvSensor2* 的配置：

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
以下示例显示了对 PATCH 方法的响应：

```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor2' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor2"]
  },
 "failures": []
}
```

## 步骤 13：发布入站设备事件
{: #step12}

使用以下示例有效内容，在主题 `iot-2/evt/tevt/fmt/json` 上发布来自 *TemperatureSensor1* 的温度事件：
```
{
  "t" : 34.5
}
```
使用以下示例有效内容，在主题 `iot-2/evt/tempevt/fmt/json` 上发布来自 *TemperatureSensor2* 的温度事件：
```
{
  "temp" : 72.55
}
```

有关从设备发布入站事件的信息，请参阅[应用程序的 MQTT 连接](../applications/mqtt.html#publishing_device_events)。


## 步骤 14：检索设备的状态
{: #step13}

您可以使用 HTTP REST API 或预订主题来检索设备的状态。

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
logicalInterfaceId	|	为逻辑接口创建的标识。

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文档。



以下示例显示了如何使用 cURL 通过引用所创建逻辑接口的标识来检索 *TemperatureSensor1* 的当前状态：
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor1/devices/TemperatureSensor1/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

逻辑接口标识 *5846ed076522050001db0e12* 会在 GET 方法中使用。此标识在对用于创建逻辑接口的 POST 方法的响应中返回。以下示例显示了对 GET 方法的响应：
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
}
}
```
以下示例显示了如何使用 cURL 通过引用所创建逻辑接口的标识来检索 *TemperatureSensor2* 的当前状态：
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor2/devices/TemperatureSensor2/state/5846ed076522050001db0e12 \
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
请注意，返回的温度读数以摄氏度而不是华氏度为单位。

您的应用程序可以处理此规范化数据，而无需用于识别或转换不同温标的配置。


