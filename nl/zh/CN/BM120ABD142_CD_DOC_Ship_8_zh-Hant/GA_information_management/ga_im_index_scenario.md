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


# 逐步手冊：如何透過共用介面使用裝置的詳細範例
{: #scenario}

使用下列資訊，以建立兩個溫度感應器將事件發佈至 {{site.data.keyword.iot_full}} 的情境。一個感應器測量攝氏度數的溫度。另一個感應器則測量華氏度數的溫度。這些讀數都會對映成攝氏度數的單一溫度讀數。這些裝置發佈新的溫度讀數時，與裝置狀態相關聯之內容的值就會變更。
{: shortdesc}

## 必要條件

您必須有 {{site.data.keyword.iot_short_notm}} 組織實例以及該組織的 API 金鑰或記號。

## 關於此作業

在此情境下，會配置兩個裝置。

一個裝置稱為 *TemperatureSensor1*。此裝置會發佈以攝氏度數測量的溫度事件。溫度事件發佈於主題 `iot-2/evt/tevt/fmt/json`，並且具有下列範例有效負載：
```
{
  "t" : 34.5
}
```

**附註：**事件 ID 是 *tevt*。將這類型的溫度事件新增至實體介面時，以及定義對映以將與這類型入埠事件相關聯的內容對映至邏輯介面中的內容時，需要此 ID。在此情境下，邏輯介面中所定義的內容稱為 **temperature**。

另一個裝置稱為 *TemperatureSensor2*。此裝置會發佈以華氏度數測量的溫度事件。溫度事件發佈於主題 `iot-2/evt/tempevt/fmt/json`，並且具有下列範例有效負載：
```
{
  "temp" : 72.55
}
```

**附註：**事件 ID 是 *tempevt*。將這類型的溫度事件新增至實體介面時，以及定義對映以將與這類型入埠事件相關聯的內容對映至邏輯介面中的內容時，需要此 ID。在此情境下，邏輯介面中所定義的內容稱為 **temperature**。

也會配置邏輯介面。此邏輯介面以下列結構呈現這類型裝置的狀態：
```
{
  "temperature" : <current temperature value in Celsius>
  }
```
此配置表示您可以配置應用程式來處理與 **temperature** 相關聯的值，而不是配置應用程式來處理與 **t** 相關聯的值，以及在將與 **temp** 相關聯的值轉換成攝氏度數之後處理該值。

![{{site.data.keyword.iot_short_notm}} 上溫度感應器裝置與應用程式之間的對映。](images/Information Management Device example.svg "{{site.data.keyword.iot_short_notm}} 上溫度感應器裝置與應用程式之間的對映")  

使用下列範例情境，以設定您自己的介面環境。

## 必要的話，新增「裝置類型」及「裝置」
{: #step14}

在此情境下，假設兩種裝置類型及兩個裝置實例。裝置實例 *TemperatureSensor1* 與裝置類型 *EnvSensor1* 相關聯。裝置實例 *TemperatureSensor2* 與裝置類型 *EnvSensor2* 相關聯。

如需使用 REST API 來新增裝置類型的相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html) 文件。

## 步驟 1：建立草稿事件綱目檔
{: #step1}

在此情境中，建立兩個草稿事件綱目檔，以定義每一個入埠溫度事件的結構。

下列範例顯示如何建立稱為 *tEventSchema.json* 的草稿綱目檔。此檔案定義以攝氏度數測量溫度的溫度感應器的入埠事件結構：

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
**提示：**使用 "required" 參數，以將一個以上的內容標示為必要。必要內容必須內含在 {{site.data.keyword.iot_short_notm}} 的裝置訊息中，才能使用裝置資料來更新裝置狀態。不會處理未包括 required 內容的訊息。   

建立事件類型的事件綱目資源時，會使用綱目檔名稱 *tEventSchema*。

下列範例顯示如何建立稱為 *tempEventSchema.json* 的草稿綱目檔。此檔案定義以華氏度數測量溫度的溫度感應器的入埠事件結構：

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
建立事件類型的事件綱目資源時，會使用綱目檔名稱 *tempEventSchema*。   

## 步驟 2：建立事件類型的草稿事件綱目資源
{: #step2}

若要建立事件綱目資源，請使用下列 API：

```
POST /draft/schemas
```

以下是必要參數：  

參數|	說明
------	|	-----  
name	|	提供所建立事件綱目的名稱。
schemaFile	|	本端事件綱目 JSON 檔案的路徑。



如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文件。

下列範例顯示如何使用 cURL 來建立事件綱目資源 *tEventSchema.json*：

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

下列範例顯示 POST 方法的回應：

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
將事件綱目新增至事件類型時，需要 POST 方法回應中所傳回的綱目 ID *5846cd7c6522050001db0e0d*。

下列範例顯示如何使用 cURL 來建立事件綱目資源 *tempEventSchema.json*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

下列範例顯示 POST 方法的回應：

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
將事件綱目新增至事件類型時，需要 POST 方法回應中所傳回的綱目 ID *5846cee36522050001db0e0e*。

## 步驟 3：建立參照事件綱目的草稿事件類型
{: #step3}

每一種事件類型都會參照前一個範例中所建立的相關事件綱目，方法是使用用來建立事件綱目資源的 POST 方法回應中所傳回的綱目 ID。

若要建立草稿事件類型，請使用下列 API：

```
POST /draft/event/types
```

以下是必要參數：  

參數|	說明
------	|	-----
name	|	提供所建立事件類型的名稱。
schemaId	|	針對事件綱目資源所建立的 ID。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) 文件。


下列範例顯示如何使用 cURL 來建立以攝氏度數測量的溫度事件的事件類型：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

綱目 ID *5846cd7c6522050001db0e0d* 是用來將事件綱目新增至事件類型。此 ID 是在用來建立事件綱目資源 *tEventSchema.json* 的 POST 方法回應中所傳回。

下列範例顯示 POST 方法的回應：

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

POST 方法回應中所傳回的事件類型 ID *5846d0fd6522050001db0e0f* 是用來將事件類型新增至實體介面。

下列範例顯示如何使用 cURL 來建立以華氏度數測量的溫度事件的事件類型：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
綱目 ID *5846cee36522050001db0e0e* 是用來將事件綱目新增至事件類型。此 ID 是在用來建立事件綱目資源 *tempEventSchema.json* 的 POST 方法回應中所傳回。

下列範例顯示 POST 方法的回應：

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
POST 方法回應中所傳回的事件類型 ID *5846d2846522050001db0e10* 是用來將事件類型新增至實體介面。

## 步驟 4：建立草稿實體介面
{: #step7}

若要建立草稿實體介面，請使用下列 API：

```
POST /draft/physicalinterfaces
```

以下是必要參數：  

參數|	說明
------	|	-----
name	|	提供所建立實體介面的名稱。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文件。

在此情境下，我們需要兩個實體介面 - 一種事件類型一個。

下列範例顯示如何使用 cURL 來建立第一個實體介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Env sensor physical interface 1"}'
```

下列範例顯示 POST 方法的回應：

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

回應中所傳回的實體介面 ID *5847d1df6522050001db0e1a* 是用於呼叫以將以攝氏度數測量的溫度事件新增至實體介面的 POST 方法的 URL 中。

下列範例顯示如何使用 cURL 來建立第二個實體介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Env sensor physical interface 2"}'
```

下列範例顯示 POST 方法的回應：

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

回應中所傳回的實體介面 ID *5847d1df6522050001db0e1b* 是用於呼叫以將以華氏度數測量的溫度事件新增至實體介面的 POST 方法的 URL 中。   

## 步驟 5：將事件類型新增至草稿實體介面
{: #step8}

若要將事件類型新增至實體介面，請使用下列 API：

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

以下是必要參數：  

參數|	說明
------	|	-----
eventId	|	輸入裝置事件有效負載中的事件名稱。
eventTypeId	|	針對事件類型所建立的 ID。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文件。

在此情境下，下列事件類型會新增至指定的實體介面：
- 攝氏溫度事件 *tevt* 會新增至 ID 為 *5847d1df6522050001db0e1a* 的實體介面，方法是使用主題中的 *eventId* 以及建立事件綱目資源的 *eventTypeId*。
- 華氏溫度事件 *tempevt* 會新增至 ID 為 *5847d1df6522050001db0e1b* 的實體介面，方法是使用主題中的 *eventId* 以及建立事件綱目資源的 *eventTypeId*。


下列範例顯示如何使用 cURL 以將溫度事件 *tevt* 新增至 ID 為 *5847d1df6522050001db0e1a* 的實體介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

下列範例顯示 POST 方法的回應：

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

下列範例顯示如何使用 cURL 以將溫度事件 *tempevt* 新增至 ID 為 *5847d1df6522050001db0e1b* 的實體介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

下列範例顯示 POST 方法的回應：

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
}
```

## 步驟 6：更新草稿裝置類型以連接草稿實體介面
{: #step9}

若要更新草稿裝置類型，請使用下列 API：

```
POST /draft/device/types/{typeId}/physicalinterface
```

其中，*typeId* 是裝置類型 ID。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文件。

在此情境下，會更新裝置類型 *EnvSensor1* 以連接至實體介面 *5847d1df6522050001db0e1a*，以及更新裝置類型 *EnvSensor2* 以連接至實體介面 *5847d1df6522050001db0e1b*。

下列範例顯示如何使用 cURL 來更新裝置類型 *EnvSensor1*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

下列範例顯示 POST 方法的回應：
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
新增實體介面及邏輯介面時，需要裝置 ID *EnvSensor1*。

下列範例顯示如何使用 cURL 來更新裝置類型 *EnvSensor2*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

下列範例顯示 POST 方法的回應：

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
新增實體介面及邏輯介面時，需要裝置 ID *EnvSensor2*。



## 步驟 7：建立草稿邏輯介面綱目檔
{: #step4}

下列範例顯示如何建立稱為 *envSensor.json* 的草稿邏輯介面綱目檔。

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

## 步驟 8：建立草稿邏輯介面綱目資源
{: #step5}

若要建立草稿邏輯介面綱目資源，請使用下列 API：

```
POST /draft/schemas
```

以下是必要參數：  

參數|	說明
------	|	-----
name	|	提供所建立邏輯介面綱目的名稱。
schemaFile	|	本端邏輯介面綱目 JSON 檔案的路徑。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文件。



下列範例顯示如何使用 cURL 來建立邏輯介面綱目：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=temperatureEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/envSensor.json"'
```

下列範例顯示 POST 方法的回應：

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
使用 POST 方法回應中所傳回的綱目 ID *5846ec826522050001db0e11*，以將邏輯介面綱目新增至邏輯介面。

## 步驟 9：建立用於參照草稿邏輯介面綱目的草稿邏輯介面
{: #step6}

若要建立草稿邏輯介面，請使用下列 API：

```
POST /draft/logicalinterfaces
```

以下是必要參數：  

參數|	說明
------	|	-----
name	|	提供所建立邏輯介面的名稱。
schemaId	|	針對邏輯介面綱目資源所建立的 ID。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 文件。

在此情境下，使用前一個回應中所傳回的綱目 ID *5846ec826522050001db0e11*，以將邏輯介面綱目新增至邏輯介面。

下列範例顯示如何使用 cURL 來建立邏輯介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "environment sensor interface", "schemaId" : "5846ec826522050001db0e11"}'
```

下列範例顯示 POST 方法的回應：

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
在此情境下，使用 POST 方法回應中所傳回的邏輯介面 ID *5846ed076522050001db0e12*，以將邏輯介面新增至裝置類型。您也可以使用此 ID，以將入埠裝置事件對映至邏輯介面所定義的內容。

## 步驟 10：將草稿邏輯介面新增至裝置類型
{: #step10}

若要將草稿邏輯介面新增至裝置類型，請使用下列 API：

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

其中，*typeId* 是裝置類型的名稱。 

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文件。  
**附註：**在此情境下，相同的邏輯介面 *5846ed076522050001db0e12* 是與 *EnvSensor1* 及 *EnvSensor2* 相關聯。

下列範例顯示如何使用 cURL 以將參照邏輯介面綱目 ID *5846ec826522050001db0e11* 的邏輯介面 *5846ed076522050001db0e12* 新增至裝置類型 *EnvSensor1*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

下列範例顯示 POST 方法的回應：

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

下列範例顯示如何使用 cURL 以將與邏輯綱目 ID *5846ec826522050001db0e11* 相關聯的邏輯介面 *5846ed076522050001db0e12* 新增至裝置類型 *EnvSensor2*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

下列範例顯示 POST 方法的回應：

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

## 步驟 11：定義對映，將入埠事件中的內容對映至邏輯介面中的內容
{: #step11}  
**重要事項：**如果您是使用 MQTT 用戶端應用程式，而且要訂閱接收裝置狀態更新的通知，則需要在定義對映時配置通知設定。透過此方式配置通知策略，即可在多種裝置類型之間重複使用邏輯介面，而且每一種裝置類型都配置成透過您選擇的方式來接收狀態變更通知。不論 MQTT 用戶端是否訂閱指定的主題字串，都可以定義通知策略。  
  
下列是可配置的通知設定：  

參數|	說明
------	|	-----
never	|	未傳送任何通知 - 此設定是預設值
on-every-event	|	不論是否變更裝置狀態，都會傳送每一個事件的通知
on-state-change	|	只有在事件導致裝置狀態變更時，才會傳送通知

**附註：**如果您選取通知設定 *never*，則應用程式絕不會收到裝置狀態變更的通知。因此，您可能要選擇 *on-every-event* 或 *on-state-change* 通知策略。  

若要對映事件，請使用下列 API：

```
POST /draft/device/types/{typeId}/mappings
```

以下是必要參數：  

參數|	說明
------	|	-----
logicalInterfaceId	|	針對邏輯介面所建立的 ID。
propertyMappings	|	有效的 JSON 結構，用於對映針對邏輯介面所定義的內容與裝置事件有效負載的內容。
您可以選擇性地設定 *notificationStrategy* 參數。  

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文件。



在此情境下，我們定義裝置類型 *EnvSensor1* 的對映，以將入埠事件 *tevt* 中的 **t** 內容對映至邏輯介面上的 **temperature** 內容。我們還會定義裝置類型 *EnvSensor2* 的對映，以將入埠事件 *tempevt* 中的 **temp** 內容對映至邏輯介面上的 **temperature** 內容。通知設定會設為 "on-state-change"。 

下列範例顯示如何使用 cURL 以將對映新增至裝置類型 *EnvSensor1*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**重要事項：**針對結構化 [JSON ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://json-schema.org/){:new_window} 事件有效負載，確定 $event.*property* 項目正確地符合內容的 JSON 結構。例如，如果有效負載為 `{"d":{"t":<temp>}}`，請使用 `$event.d.t`

指定用來建立邏輯介面及裝置類型 *EnvSensor1* 的 POST 方法回應中所傳回的邏輯介面 ID *5846ed076522050001db0e12*。

下列範例顯示 POST 方法的回應：

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt": {
      "temperature": "$event.t"
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
下列範例顯示如何使用 cURL 以將對映新增至裝置類型 *EnvSensor2*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

指定用來建立邏輯介面及裝置類型 *EnvSensor2* 的 POST 方法回應中所傳回的邏輯介面 ID *5846ed076522050001db0e12*。套用轉換，以將值從華氏度數測量變更為攝氏度數測量。


下列範例顯示 POST 方法的回應：

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

## 步驟 12：驗證及啟動配置
{: #step15}

驗證及啟動與每一種草稿裝置類型的裝置狀態更新相關的配置。此配置包括您的草稿綱目、事件類型、實體介面、邏輯介面及對映。

若要驗證及啟動草稿裝置類型配置，請使用下列 API：

```
PATCH /draft/device/types/{typeId}
```

其中，*typeId* 是裝置類型 ID。

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文件。

在此情境下，我們需要驗證及啟動兩種草稿裝置類型的配置。

下列範例顯示如何使用 cURL 來驗證及啟動草稿裝置類型 *EnvSensor1* 的配置：

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

下列範例顯示 PATCH 方法的回應：

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
下列範例顯示如何使用 cURL 來驗證及啟動草稿裝置類型 *EnvSensor2* 的配置：

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
下列範例顯示 PATCH 方法的回應：

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

## 步驟 13：發佈入埠裝置事件
{: #step12}

使用下列範例有效負載，將來自 *TemperatureSensor1* 的溫度事件發佈於主題 `iot-2/evt/tevt/fmt/json`：
```
{
  "t" : 34.5
}
```
使用下列範例有效負載，將來自 *TemperatureSensor2* 的溫度事件發佈於主題 `iot-2/evt/tempevt/fmt/json`：
```
{
  "temp" : 72.55
}
```

如需發佈來自裝置的入埠事件的相關資訊，請參閱[應用程式的 MQTT 連線功能](../applications/mqtt.html#publishing_device_events)。


## 步驟 14：擷取裝置的狀態
{: #step13}

您可以使用 HTTP REST API 或是訂閱主題來擷取裝置的狀態。

- 如果您具有 MQTT 用戶端應用程式，則可以訂閱下列主題字串：  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- 或者，您可以使用下列 HTTP REST API 來擷取最新的裝置狀態：  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

以下是必要參數：  

參數|	說明
------	|	-----
typeId	|	裝置類型 ID。
deviceId	|	裝置 ID。
logicalInterfaceId	|	針對邏輯介面所建立的 ID。

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文件。



下列範例顯示如何使用 cURL 藉由參照所建立邏輯介面的 ID 來擷取 *TemperatureSensor1* 的現行狀態：
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor1/devices/TemperatureSensor1/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

邏輯介面 ID *5846ed076522050001db0e12* 用於 GET 方法中。此 ID 是在用來建立邏輯介面的 POST 方法回應中所傳回。下列範例顯示 GET 方法的回應：
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
  }
}
```
下列範例顯示如何使用 cURL 藉由參照所建立邏輯介面的 ID 來擷取 *TemperatureSensor2* 的現行狀態：
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor2/devices/TemperatureSensor2/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

邏輯介面 ID *5846ed076522050001db0e12* 用於 GET 方法中。此 ID 是在用來建立邏輯介面的 POST 方法回應中所傳回。下列範例顯示 GET 方法的回應：
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
}
}
```
請注意，傳回的溫度讀數是攝氏度數，而非華氏度數。

您的應用程式可以處理此正規化資料，而且不需要配置即可瞭解或轉換不同的溫標。


