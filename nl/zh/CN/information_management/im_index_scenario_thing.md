---

copyright:
years: 2016, 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 逐步指南 2：有关如何通过公共接口使用事物的详细示例 (Beta)
{: #scenario}

此场景基于先前的[逐步指南 1：有关如何通过公共接口使用设备的详细示例](../GA_information_management/ga_im_index_scenario.html)进行构建。

**重要信息：**用于数据管理的 {{site.data.keyword.iot_full}} 事物功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

在此场景中，温度和湿度设备发布在两个会议室（会议室 1 和会议室 2）中收集的环境数据。对于每个会议室，温度和湿度设备数据分别映射到两种设备类型逻辑接口。 

对于会议室 1，与 *TSDensor* 设备类型关联的温度设备数据会映射到*温度计接口*逻辑接口，与 *HumiditySensor* 设备类型关联的湿度设备数据会映射到*湿度计接口*逻辑接口。 

对于会议室 2，与 TempSensor 设备类型关联的温度设备数据会映射到*温度计接口*逻辑接口，与 *HumiditySensor* 设备类型关联的湿度设备数据会映射到*湿度计接口*逻辑接口。 

然后会创建一个名为 *RoomType* 的事物类型，以及两个房间事物实例：*meetingroom1* 和 *meetingroom2*。

此场景设置一个组合，其中包含温度计和湿度计逻辑接口，然后将正确的环境设备映射到每个房间实例，例如，*tSensor* 和 *humiditySensor1* 映射到 *meetingroom1*。

## 先决条件
{: #pre_req}

继续操作之前，请先确保：
- 使用在“逐步指导 1”中所用的 {{site.data.keyword.iot_full}} 组织实例及该组织的 API 密钥或令牌。有关 API 密钥和令牌的更多信息，请参阅[针对应用程序的 HTTP REST API](../applications/api.html#authentication) 文档的“认证”部分。
- 已配置两个逻辑接口，一个用于温度设备，一个用于湿度设备。有关配置温度设备逻辑接口的信息，请参阅[逐步指南 1：有关如何通过公共接口使用设备的详细示例](../GA_information_management/ga_im_index_scenario.html#step4)文档。有关配置湿度设备逻辑接口的信息，请参阅[逐步指南 2 的其他信息：配置湿度设备的逻辑接口](im_hygrometer.html)。

## 关于此任务
{: #about}

在 {{site.data.keyword.iot_short_notm}} 中，事物可以由多个设备和事物组成。事物类型定义如何编写事物的实例。 

逻辑接口与事物类型相关联。此关联定义为事物类型实例生成的状态的结构。映射用于定义如何将来自聚合设备和事物的属性映射到事物状态上的属性。

通过使用逻辑接口，应用程序就无需了解设备或事物是如何配置的。例如，您可能会使用单个设备测量房间的温度，或者，可能会通过将多个设备的读数取平均值来计算房间的温度。应用程序需要了解一个或多个房间的状态，其中之一就是温度属性。至于如何计算应用程序接收到的温度值则无关紧要。

在此场景中，两个温度设备和两个湿度设备将事件发布到 {{site.data.keyword.iot_short_notm}}。一个温度设备和一个湿度设备位于办公区块的会议室 1 中。另一个温度和湿度设备位于会议室 2 中。下图说明了会议室 1 的配置：

![在 {{site.data.keyword.iot_short_notm}} 上温度和湿度事物与应用程序之间的映射。](images/Information Management Thing example scenario.svg "在 {{site.data.keyword.iot_short_notm}} 上温度和湿度事物与应用程序之间的映射。")

名为 *RoomType* 的事物类型用于定义如何编写各房间的实例。一个逻辑接口与 *RoomType* 相关联，并定义将入站事件映射到同时显示温度和湿度的单个读数。此单个读数即是事物状态。映射用于定义如何将来自温度和湿度设备的属性映射到此事物状态。在这些设备发布新的读数时，与事物状态关联的属性的值会更改。

下表显示了示例中使用的四个设备、每个设备发布的主题以及每个设备的示例有效内容。

设备/类型|事件|事件有效内容/属性

------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1)|`iot-2/evt/`*`tevt`*`/fmt/json`|`{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2)|`iot-2/evt/`*`tempevt`*`/fmt/json`|`{ "temp" : 34.5 }`/ **temperature2**
*humiditySensor1*/HumiditySensor (meetingroom1)|`iot-2/evt/`*`humevt`*`/fmt/json`|`{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2)|`iot-2/evt/`*`humevt`*`/fmt/json`|`{  "hum" : 75}`/ **humidity2**

**注：**定义映射以将与该类型的入站事件关联的属性与逻辑接口中的某个属性相关联时，需要事件标识 *tevt*、*tempevt* 和 *humevt*。在此场景中，逻辑接口中定义了两个属性 - *temperature* 和 *humidity*。

此外，还会配置逻辑接口。逻辑接口表示的事物状态的结构如下：
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

使用以下示例场景来设置您自己的接口环境。 

**注：**[逐步指南 1 和 2 文档中引用的资源属性和标识](im_id_reference.html)中记录了列出本指南中使用的资源属性名称、值和标识的表。

## 根据需要添加设备类型和设备  
{: #add_device}  
在此场景中，会使用三种设备类型和四个设备实例。设备实例 *tSensor* 与设备类型 *TSensor* 相关联。设备实例 *tempSensor* 与设备类型 *TempSensor* 相关联。设备实例 *humiditySensor1* 和 *humiditySensor2* 与设备类型 *HumiditySensor* 相关联。

可以使用 [{{site.data.keyword.iot_short_notm}} 仪表板 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://internetofthings.ibmcloud.com){: new_window} 或使用 REST API 来创建设备类型和设备。 

有关使用 {{site.data.keyword.iot_short_notm}} IoT Platform 仪表板添加设备类型和设备的更多信息，请参阅[使用 Web 界面管理数据入门](../GA_information_management/im_ui_flow.html)文档。

有关使用 REST API 来添加设备类型和设备的信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window} 文档。



## 步骤 1：创建事物类型模式文件。  
{: #crt_composition_file}  
创建事物类型模式文件，该文件引用温度和湿度设备类型的设备逻辑接口标识。  

以下示例显示了如何创建名为 *roomTypeSchema* 的事物类型模式文件。   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON Schema that defines the structure of the Room Thing Type.",
   "properties" : {
       "thermometer": {
            "type": "object",
            "description": "The thermometer device",
            "$logicalInterfaceRef": "IThermometer"
        },
        "hygrometer": {
            "type": "object",
            "description": "The hygrometer device",
            "$logicalInterfaceRef": "IHygrometer"
        }
    },
   "required" : [
       "temperature",
       "humidity"
   ]
}
```
**提示：**使用 **required** 参数将一个或多个属性标记为必需属性。设备消息中必须包含必需属性，Watson IoT Platform 才能使用设备数据更新设备状态。不会对不包含必需属性的消息进行处理。   
**注：**创建事物类型时，必须指定在创建事物类型模式文件时生成的模式标识。  

## 步骤 2：创建事物模式资源。  
{: #crt_composition_resource}  

通过上传在上一步中生成的事物类型模式文件来创建模式资源。  
使用以下 API 上传事物类型模式文件：  
```
POST /draft/schemas
```  
模式定义文件将在多部分 POST (multipart/form-data) 中传递到 Watson IoT Platform。该 POST 的主体必须至少包含两个部分：

  - 一个部分的名称为 **schemaFile**，包含作为该部分主体的文件的实际内容。
  - 一个部分的名称为 **name**，包含用于定义该部分主体中模式资源名称的字符串。


有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文档。

  

以下示例显示了如何使用 cURL 创建事物类型模式资源：  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
以下示例显示了对 POST 方法的响应：
```
{
  "name" : "roomTypeSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "roomType.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5a72ea48d60180002c4f5e58",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
创建事物类型时，需要在对 POST 方法的响应中返回的模式标识 *5a72ea48d60180002c4f5e58*。


## 步骤 3：创建事物类型  
{: #crt_thing_type}  

事物类型用于对事物实例建模。必须先在组织中创建事物类型，然后才能创建事物实例。对于此场景，请创建一种事物类型。  
与事物类型关联的模式可定义那些聚合在一起以形成事物实例的对象的类型。事物类型必须引用在上一步中创建的事物类型模式资源。  
使用以下 API 创建事物类型：
```
POST /draft/thing/types
```
POST 方法的主体中需要以下参数：  
<table>
<tr><th>参数</th><th>描述</th></tr>
<tr><td>id</td><td>为要创建的事物类型提供标识。</td></tr>
<tr><td>name</td><td>为要创建的事物类型提供名称。</td></tr>
<tr><td>schemaId</td><td>为组合模式资源创建的标识。</td></tr>
</table>

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文档。

  

以下示例显示了如何使用 cURL 创建名为 *RoomType* 的事物类型。
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

响应： 
```
{
 "name": "RoomType",
 "description": "Room Thing Type",
 "id": "RoomType",
 "schemaId": "5a72ea48d60180002c4f5e58",
 "metadata": {},
  "refs": {
    "schema": "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58",
    "mappings": "/api/v0002/draft/thing/types/RoomType/mappings",
    "logicalInterfaces": "/api/v0002/draft/thing/types/RoomType/logicalinterfaces"
   },
 "version": "draft",
 "created": "2018-02-01T10:22:43Z",
 "createdBy": "ANOther",
 "updated": "2018-02-01T10:22:43Z",
 "updatedBy": "ANOther"
}
```

## 步骤 4：创建逻辑接口模式文件  
{: #crt_ai_schema_file}
在逻辑接口中，可以定义存储为事物状态的数据的结构。对于此场景，请创建用于定义温度和湿度属性的逻辑接口。通过引用在创建逻辑接口资源时生成的逻辑接口标识，将逻辑接口与事物类型 *RoomType* 相关联。  
以下示例显示了如何创建名为 *roomSchema* 的逻辑接口模式文件。

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON Schema that defines the canonical room state structure",
    "properties" : {
        "temperature" : {
            "description" : "Temperature in degrees celsius",
           "type" : "number",
           "minimum" : -273.15,
           "default" : 0.0
       },
       "humidity" : {
           "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
   },
   "required" : [
       "temperature",
       "humidity"
   ]
}
```  
## 步骤 5：创建逻辑接口模式资源  
{: #crt_ai_schema_resource}  
使用以下 API 上传在上一步中创建的逻辑接口模式文件，以创建事物类型的逻辑接口模式资源：  
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
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
以下示例显示了对 POST 方法的响应：
```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "roomSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "room.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5a4b9847d60180002efce645/content"
  },
  "id" : "5a4b9847d60180002efce645"
}
```
使用在对 POST 方法的响应中返回的模式标识 *5a4b9847d60180002efce645*，向事物类型的逻辑接口添加逻辑接口模式资源。  


## 步骤 6：创建事物类型的逻辑接口  
{: #crt_thing_ai}  
逻辑接口必须引用在上一步中创建的逻辑接口模式资源。  
要创建逻辑接口，请使用以下 API：  
```
POST draft/logicalinterfaces
```  
您可以选择为逻辑接口指定有意义的别名。别名可以在用于检索事物状态的 API 调用或主题字符串预订中引用，而不使用自动生成的逻辑接口标识。

**注：**别名的长度必须为 1 到 36 个字符，并且可以包含字母数字、连字符、句点和下划线字符。别名不能是 24 个字符的十六进制字符串。

在此场景中，使用在先前响应中返回的模式标识 *5a4b9847d60180002efce645*，向逻辑接口添加逻辑接口模式。

以下示例显示了如何使用 cURL 创建别名为 *IRoom* 的逻辑接口：
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
以下示例显示了对 POST 方f法的响应：
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58"
  },
  "schemaId" : "5a72ea48d60180002c4f5e58",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5a4b9847d60180002efce645",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "IRoom",
  "alias" : "IRoom",
  "version" : "draft"
}
```
在此场景中，使用在对 POST 方法的响应中返回的逻辑接口标识 *5a4b9847d60180002efce645*，向设备类型添加逻辑接口。此外，还将使用此标识将入站设备事件映射到由逻辑接口定义的属性。



有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 文档。

  


## 步骤 7：向事物类型添加逻辑接口  
{: #add_thing_ai}  
要向事物类型添加逻辑接口，请使用以下 API：  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文档。

  
在此场景中，逻辑接口与事物类型 *RoomType* 相关联。

以下示例显示了如何使用 cURL 将事物逻辑接口 *IRoom* 添加到事物类型 *RoomType*：  
```
{   
  "id": "5a4b9847d60180002efce645"
}
```
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id": "5a4b9847d60180002efce645"}'
```

以下示例显示了对 POST 方法的响应：
```
{
 "name": "Room Logical Interface",
 "description": "This is a Room logical interface",
 "id": "5a4b9847d60180002efce645",
 "schemaId": "5a4b9817d60180002efce644",
 "refs": {
   "schema": "/api/v0002/draft/schemas/5a4b9817d60180002efce644"
 },
 "version": "draft",
 "created": "2018-01-02T14:33:43Z",
 "createdBy": "ANOther",
 "updated": "2018-01-02T14:33:43Z",
 "updatedBy": "ANOther"
}
```

## 步骤 8：定义映射
{: #define_Thing_type_mappings}

定义事物类型的映射，用于描述如何将来自聚合设备或事物的状态的属性映射到在事物类型逻辑接口上定义的属性。

要映射事件，请使用以下 API：  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

其中，*thingtypeId* 是在创建事物类型时对 POST 请求的响应中返回的标识。 

POST 请求的主体中需要以下参数：  
<table>
<tr>
<th>	参数</th><th>	描述</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	有效的 JSON 结构，用于将为逻辑接口定义的属性映射到设备事件有效内容的属性。</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	有效内容主体中需要该逻辑接口标识。</td>
</tr>
</table>  

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文档。



以下示例显示了如何使用 cURL 向事物类型 *RoomType* 添加映射：

```
{
  "logicalInterfaceId": "5a4b9847d60180002efce645",
  "notificationStrategy": "on-state-change",
  "propertyMappings": {
       "thermometer": {
         "temperature": "$event.temperature"
       },
       "hygrometer": {
         "humidity": "$event.humidity"
       }
   },
}
```  
*thermometer* 设备在 [roomTypeSchema](#crt_composition_file) 中定义。*$event.temperature* 属性在标识为 *5846ed076522050001db0e12* 且别名为 *IThermometer* 的逻辑接口模式中定义。  
*hygrometer* 设备在 [roomTypeSchema](#crt_composition_file) 中定义。*$event.humidity* 属性在标识为 *5846cd7c6522050001db0e24* 且别名为 *IHygrometer* 的逻辑接口模式中定义。


## 步骤 9：验证并激活配置
{: #activate}

验证并激活与每种事物类型的事物状态更新相关的配置。此配置包括模式、逻辑接口和映射。 

要验证并激活事物类型配置，请使用以下 API：
```
PATCH /draft/thing/types/{thingTypeId}
```
其中，*thingTypeId* 是事物类型标识。 

以下示例显示了如何使用 cURL 验证并激活与事物类型 *RoomType* 关联的配置：
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

以下示例显示了对 PATCH 方法的响应：
```
{
 "message": "CUDIM0300I: State update configuration for Thing Type 'Room Thing Type' has been successfully submitted for  activation.",
"details": {
  "id": "CUDIM0300I",
   "properties": [
     "Thing Type",
     "Room Thing Type"
   ]
 },
 "failures": []
}
```

## 步骤 10：创建事物类型的实例
{: #create_Thing_instances}

事物是事物类型的实例。通过事物，您可以将某个设备或事物的一个或多个实例聚合成一个对象。

要创建事物，请使用以下 API：

```
POST /thing/types/{thingTypeId}/things
```
其中，*thingtypeId* 是在创建事物类型时对 POST 请求的响应中返回的标识。 

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things) 文档。



在此场景中，需要创建事物类型为 *RoomType* 的两个事物实例。其中一个事物实例名为 *meetingroom1*，另一个事物实例名为 *meetingroom2*。

以下示例显示了如何使用 cURL 创建名为 *meetingroom1* 的事物实例。*meetingroom1* 事物实例与 *tSensor* 和 *humiditySensor1* 设备实例相关联。

```
thingId = "meetingroom1"
 meetingroom1AggregatedObjects = {
   "thermometer": {
     "type": "device",
     "typeId": "TSensor",
     "id": "tSensor"
   },
   "hygrometer": {
     "type": "device",
     "typeId": "HumiditySensor",
     "id": "humiditySensor1"
   }
 }
``` 

创建的事物标识在 POST 方法的 URL 中使用，调用该方法时，可将温度事件添加到事物逻辑接口。

以下示例显示了如何使用 cURL 创建名为 *meetingroom2* 的事物实例。*meetingroom2* 与 *tempSensor* 和 *humiditySensor2* 设备实例相关联。

```
thingId = "meetingroom2"
   meetingroom2AggregatedObjects = {
    "thermometer": {
      "type": "device",
      "typeId": "TempSensor",
      "id": "tempSensor"
    },
    "hygrometer": {
      "type": "device",
      "typeId": "HumiditySensor",
      "id": "humiditySensor2"
    }
  }

``` 

## 步骤 11：验证映射的设备事件是否已发布到逻辑接口  
{: #publish_event}  
发布已聚合到事物 *meetingroom1* 中的各个设备的以下事件：  
 - 在主题 `iot-2/evt/tevt/fmt/json` 上发布来自 *tSensor* 的温度事件  
 - 在主题 `iot-2/evt/humevt/fmt/json` 上发布来自 *humiditySensor1* 的湿度事件  
 
发布已聚合到事物 *meetingroom2* 中的各个设备的以下事件：  
 - 在主题 `iot-2/evt/tempevt/fmt/json` 上发布来自 *tempSensor* 的温度事件  
 - 在主题 `iot-2/evt/hevt/fmt/json` 上发布来自 *humiditySensor2* 的湿度事件  
 
有关从设备发布入站事件的信息，请参阅[应用程序的 MQTT 连接](../applications/mqtt.html#publishing_device_events)。  

## 步骤 12：检查事物状态是否已更改  
{: #verify_Thing_state}  

您可以使用 HTTP REST API 或预订主题来检索事物的状态。

如果您具有 MQTT 客户机应用程序，那么可以预订以下主题字符串：
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

或者，可以使用以下 HTTP REST API 来检索最新的事物状态：

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

以下参数是必需的：  
<table>
<tr>
<th>参数</th><th>	描述</th>
</tr>
<tr>
<td>thingTypeId</td><td>事物类型标识。</td>
</tr>
<tr>
<td>thingId	</td><td>	事物标识。</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>为逻辑接口创建的标识。</td>
</tr>
</table>  

有关更多详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文档。

  


## 后续步骤

创建规则，以用于在 {{site.data.keyword.iot_short_notm}} 收到的事件导致设备或事物状态发生更改时启动操作。有关创建规则的信息，请参阅[创建嵌入式规则 (Beta)](im_rules.html)。
