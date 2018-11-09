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

# 逐步手冊 2 的其他資訊 - 配置濕度裝置邏輯介面

使用下列資訊，以建立兩個濕度裝置上的感應器將事件發佈至 IBM Watson™ IoT Platform 的情境。濕度裝置的讀數會對映至單一濕度讀數。一個裝置位於會議室 1，另一個裝置則位於會議室 2。


## 關於此作業

使用相同的 {{site.data.keyword.iot_short_notm}} 組織實例，以及您在[逐步手冊 1](../GA_information_management/ga_im_index_scenario.html) 中使用且屬於該組織的 API 金鑰或記號。 

此配置是[逐步手冊 2](im_index_scenario_thing.html) 文件中所述指導教學的必要條件。

在此情境下，會建立一種裝置類型及兩個裝置實例。

裝置實例稱為 *humiditySensor1* 及 *humiditySensor2*。裝置會發佈濕度事件。濕度事件具有下列範例有效負載：
```
{
  "hum" : 35
}
```
濕度事件發佈於主題 `iot-2/evt/humevt/fmt/json`。 

**附註：**事件 ID 是 *humevt*。將這些類型的濕度事件新增至實體介面時，以及定義對映以將與這類型入埠事件相關聯的內容對映至邏輯介面中的內容時，需要此 ID。在此情境下，邏輯介面中所定義的內容稱為 **humidity**。此邏輯介面以下列結構呈現這類型裝置的狀態：
```
{
  "humidity" : <current humidity value in Celsius>
}
```

使用下列範例情境，以設定您自己的介面環境。

**重要注意事項：**您必須儲存 curl 回應中所產生的 ID，因為需要有 ID 才能完成其他作業。

## 必要的話，新增「裝置類型」及「裝置」
{: #step14}

在此情境下，假設有一種裝置類型及兩個裝置實例。裝置實例 *humiditySensor1* 及 *humiditySensor2* 與裝置類型 *HumiditySensor* 相關聯。 

您可以使用 [{{site.data.keyword.iot_short_notm}} 儀表板 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://internetofthings.ibmcloud.com){: new_window} 或使用 REST API 來建立裝置類型及裝置。如需使用 {{site.data.keyword.iot_short_notm}} IoT Platform 儀表板來新增裝置類型及裝置的相關資訊，請參閱[使用 Web 介面開始使用資料管理](../GA_information_management/im_ui_flow.html)文件。

下列範例顯示如何使用 REST API 來建立稱為 *HumiditySensor* 的裝置類型：

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**重要注意事項：**您可以在建立裝置類型及裝置時新增 meta 資料。在此情境下，下列 meta 資料會新增至裝置類型 *HumiditySensor*：
```
{
    "maxHumidityThreshold": 70
}
```
建立在下列時機觸發的[規則](im_rules.html)時，會使用此 meta 資料：{{site.data.keyword.iot_short_notm}} 從 *humiditySensor1* 或 *humiditySensor2* 接收到導致裝置狀態的 *humidity* 內容超過 70% 之值的濕度事件時。 

## 登錄裝置

下列範例顯示如何登錄稱為 *humiditySensor1* 的裝置實例：
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
下列範例顯示如何登錄稱為 *humiditySensor2* 的裝置實例：
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**附註：**裝置透過 Watson IoT Platform HTTP REST API 提出 HTTP 要求時，需要使用者名稱及密碼。密碼是登錄裝置時自動產生或手動指定的鑑別記號值。如果您使用 MQTT 用戶端，則必須記下裝置的鑑別記號，因為您需要有此記號才能藉由訂閱 IoT 主題字串來擷取裝置或「物品」狀態。

## 步驟 1：建立事件綱目檔
{: #step1}

在此情境中，建立一個事件綱目檔，以定義入埠濕度事件的結構。

下列範例顯示如何建立稱為 *humEventSchema.json* 的綱目檔。此檔案定義濕度裝置的入埠事件結構：

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
**提示：**使用 **required** 參數，以將一個以上的內容標示為必要。必要內容必須內含在 {{site.data.keyword.iot_short_notm}} 的裝置訊息中，才能使用裝置資料來更新裝置狀態。不會處理未包括 required 內容的訊息。   


## 步驟 2：建立事件類型的事件綱目資源
{: #step2}

若要建立事件綱目資源，請使用下列 API：

```
POST /draft/schemas
```

綱目定義檔會在多組件 POST (multipart/form-data) 內傳遞至 Watson IoT Platform。POST 的內文必須至少包含兩個組件：

- 一個的名稱為 **schemaFile**，內含作為組件內文的實際檔案內容。
- 一個的名稱為 **name**，所含的字串定義組件內文中的綱目資源名稱。

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文件。

下列範例顯示如何使用 cURL 來建立事件綱目資源 *humEventSchema.json*：

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**附註：**範例授權值 `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` 包含下列資訊：`{API Key}:{authorization token}`，其之後將以 Base64 編碼。

下列範例顯示 POST 方法的回應：

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
將事件綱目新增至事件類型時，需要 POST 方法回應中所傳回的綱目 ID *5846cd7c6522050001db0e20*。



## 步驟 3：建立參照事件綱目的事件類型
{: #step3}

每一種事件類型都會參照前一個範例中所建立的相關事件綱目，方法是使用用來建立事件綱目資源的 POST 方法回應中所傳回的綱目 ID。

若要建立事件類型，請使用下列 API：

```
POST /draft/event/types
```
草稿事件類型必須參照綱目定義，以定義入埠 MQTT 事件的結構。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) 文件。


下列範例顯示如何使用 cURL 來建立濕度事件的事件類型：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

綱目 ID *5846cd7c6522050001db0e20* 用來將事件綱目新增至事件類型。此 ID 是在用來建立事件綱目資源 *humEventSchema.json* 的 POST 方法回應中所傳回。

下列範例顯示 POST 方法的回應：

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

POST 方法回應中所傳回的事件類型 ID *5846cd7c6522050001db0e21* 是用來將事件類型新增至實體介面。


## 步驟 4：建立實體介面
{: #step7}

若要建立實體介面，請使用下列 API：

```
POST /draft/physicalinterfaces
```

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文件。

在此情境下，我們需要先前建立之事件類型的一個實體介面。

下列範例顯示如何使用 cURL 來建立實體介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

下列範例顯示 POST 方法的回應：

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

回應中所傳回的實體介面 ID *5846cd7c6522050001db0e22* 是用於呼叫以將以攝氏度數測量的溫度事件新增至實體介面的 POST 方法 URL 中。


## 步驟 5：將事件類型新增至實體介面
{: #step8}

若要將事件類型新增至實體介面，請使用下列 API：

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 文件。

在此情境下，下列事件類型會新增至指定的實體介面：
- 濕度事件 *humevt* 會新增至 ID 為 *5846cd7c6522050001db0e22* 的實體介面，方法是使用主題中的 *eventId* 以及建立事件綱目資源的 *eventTypeId*。

下列範例顯示如何使用 cURL 以將濕度事件 *humevt* 新增至 ID 為 *5846cd7c6522050001db0e22* 的實體介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

下列範例顯示 POST 方法的回應：

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## 步驟 6：更新裝置類型以連接實體介面
{: #step9}

若要更新裝置類型，請使用下列 API：

```
POST /draft/device/types/{typeId}/physicalinterface
```

其中，*typeId* 是裝置類型 ID。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 文件。

在此情境下，會更新裝置類型 *HumiditySensor* 以連接至實體介面 *5846cd7c6522050001db0e22*。

下列範例顯示如何使用 cURL 來更新裝置類型 *HumiditySensor*：

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

下列範例顯示 POST 方法的回應：
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
新增實體介面及邏輯介面時，需要裝置 ID *HumiditySensor*。
  


## 步驟 7：建立邏輯介面綱目檔
{: #step4}

下列範例顯示如何建立稱為 *hygrometer.json* 的邏輯介面綱目檔。

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

## 步驟 8：建立邏輯介面綱目資源
{: #step5}
下列範例顯示如何使用 cURL 來建立邏輯介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
下列範例顯示 POST 方法的回應：
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

## 步驟 9：建立用於參照邏輯介面綱目的邏輯介面
{: #step6}

若要建立邏輯介面，請使用下列 API：

```
POST /draft/logicalinterfaces
```

您可以選擇性地指定有意義的邏輯介面別名。在用來擷取裝置狀態的 API 呼叫或主題字串訂閱中，可以參照別名，而不使用自動產生的邏輯介面 ID。

**附註：**別名的長度必須是 1 - 36 個字元，且可以包括英數、連字號、句點、底線字元。別名不能是 24 個字元的十六進位字串。 

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 文件。

在此情境下，使用前一個回應中所傳回的綱目 ID *5846cd7c6522050001db0e23*，以將邏輯介面綱目新增至邏輯介面。

下列範例顯示如何使用 cURL 來建立邏輯介面：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

下列範例顯示 POST 方法的回應：

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
在此情境下，使用 POST 方法回應中所傳回的邏輯介面 ID *5846cd7c6522050001db0e24*，以將邏輯介面新增至裝置類型。您也可以使用此 ID，以將入埠裝置事件對映至邏輯介面所定義的內容。

您可以使用邏輯介面別名 *IHygrometer* 來擷取裝置的狀態，方法是使用 HTTP REST API，或訂閱主題字串。


## 步驟 10：將邏輯介面新增至裝置類型

下列範例顯示如何使用 cURL 以將參照邏輯介面綱目 ID 5846cd7c6522050001db0e23 的邏輯介面 5846cd7c6522050001db0e24 新增至裝置類型 HumiditySensor：
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
下列範例顯示 POST 方法的回應：
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

## 步驟 11：將對映新增至裝置類型

    
下列範例顯示如何使用 cURL 以將對映新增至裝置類型 *HumiditySensor*：

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

下列範例顯示 POST 方法的回應：
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

## 步驟 12：啟動裝置類型的配置


    
下列範例顯示如何使用 cURL 來驗證及啟動裝置類型 *HumiditySensor* 的配置：
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
下列範例顯示 PATCH 方法的回應：
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
