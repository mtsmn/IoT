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

# 逐步手冊 2：如何透過共用介面使用物品的詳細範例（測試版）
{: #scenario}

此情境是根據前一個[逐步手冊 1：如何透過共用介面使用裝置的詳細範例](../GA_information_management/ga_im_index_scenario.html)所建置。

**重要事項：**資料管理的「{{site.data.keyword.iot_full}} 物品」特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

在此情境下，溫度及濕度裝置會發佈在兩間會議室（「會議室 1」及「會議室 2」）收集到的環境資料。針對每間會議室，溫度及濕度裝置資料會個別對映到兩個裝置類型邏輯介面。 

針對「會議室 1」，與 *TSensor* 裝置類型相關聯的溫度裝置資料會對映至 *Thermometer Interface* 邏輯介面，而與 *HumiditySensor* 裝置類型相關聯的濕度裝置資料會對映至 *Hygrometer Interface* 邏輯介面。 

針對「會議室 2」，與 TempSensor 裝置類型相關聯的溫度裝置資料會對映至 *Thermometer Interface* 邏輯介面，而與 *HumiditySensor* 裝置類型相關聯的濕度裝置資料會對映至 *Hygrometer Interface* 邏輯介面。 

接著會建立稱為 *RoomType* 的「物品」類型，以及兩個會議室「物品」實例：*meetingroom1* 及 *meetingroom2*。

此情境會設定包括溫度計及濕度計邏輯介面的組合，然後將正確的環境裝置對映至每個會議室實例（例如，*tSensor* 及 *humiditySensor1* 對映至 *meetingroom1*）。

## 必要條件
{: #pre_req}

繼續之前，請確定您已：
- 使用相同的 {{site.data.keyword.iot_full}} 組織實例，以及您在「逐步手冊 1」中使用且屬於該組織的 API 金鑰或記號。如需 API 金鑰及記號的相關資訊，請參閱[應用程式的 HTTP REST API](../applications/api.html#authentication) 文件的「鑑別」一節。
- 已配置兩個邏輯介面，一個適用於溫度裝置，一個則適用於濕度裝置。如需配置溫度裝置邏輯介面的相關資訊，請參閱[逐步手冊 1：如何透過共用介面使用裝置的詳細範例](../GA_information_management/ga_im_index_scenario.html#step4)文件。如需配置濕度裝置邏輯介面的相關資訊，請參閱[逐步手冊 2 的其他資訊 - 配置濕度裝置邏輯介面](im_hygrometer.html)。

## 關於此作業
{: #about}

在 {{site.data.keyword.iot_short_notm}} 中，「物品」可以包含數個裝置及「物品」。「物品」類型定義「物品」實例的組合方式。 

邏輯介面與「物品」類型相關聯。此關聯定義針對「物品」類型實例所產生之狀態的結構。對映是用來定義如何將所聚集裝置及「物品」的內容對映至「物品」狀態的內容。

邏輯介面用來移除應用程式的需求，以瞭解如何配置裝置或「物品」。例如，您可以使用單一裝置來測量會議室的溫度，也可以取得多個裝置的平均讀數來計算會議室的溫度。應用程式需要一個以上會議室狀態的資訊，而會議室的其中一個元件是 temperature 內容。計算應用程式所收到溫度值的方式無關。

在此情境下，兩個溫度裝置及兩個濕度裝置會將事件發佈至 {{site.data.keyword.iot_short_notm}}。一個溫度裝置及一個濕度裝置是在辦公室區塊的「會議室 1」中。另一個溫度及濕度裝置則是在「會議室 2」中。下圖說明「會議室 1」的配置：

![{{site.data.keyword.iot_short_notm}} 上溫度及濕度「物品」與應用程式之間的對映。](images/Information Management Thing example scenario.svg "在一個房間中的多部環境裝置，與 {{site.data.keyword.iot_short_notm}} 上的應用程式之間的對映")

稱為 *RoomType* 的「物品」類型用來定義會議室實例的組合方式。邏輯介面與 *RoomType* 相關聯，並定義入埠事件對映至同時顯示溫度及濕度的單一讀數。這個單一讀數是「物品」狀態。對映用來定義如何將溫度及濕度裝置的內容對映至此「物品」狀態。這些裝置發佈新的讀數時，與「物品」狀態相關聯的內容值就會變更。

下表顯示在我們的範例中使用的四個裝置、每個裝置所發佈的主題，以及每個裝置的範例有效負載。

裝置/類型|事件|事件有效負載/內容

------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**附註：**當您定義對映以將與該類型入埠事件相關聯的內容對映至邏輯介面中的內容時，需要事件 ID *tevt*、*tempevt*、*humevt*。在此情境下，邏輯介面中定義兩個內容：*temperature* 及 *humidity*。

也會配置邏輯介面。邏輯介面會以下列結構呈現「物品」狀態：
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

使用下列範例情境，以設定您自己的介面環境。 

**附註：**列出本手冊中所使用資源內容名稱、值及 ID 的表格，記載於[逐步手冊 1 及 2 文件中所參照的資源內容及 ID](im_id_reference.html) 中。

## 必要的話，新增裝置類型及裝置  
{: #add_device}  
在此情境下，使用三種裝置類型及四個裝置實例。裝置實例 *tSensor* 與裝置類型 *TSensor* 相關聯。裝置實例 *tempSensor* 與裝置類型 *TempSensor* 相關聯。裝置實例 *humiditySensor1* 及 *humiditySensor2* 與裝置類型 *HumiditySensor* 相關聯。

您可以使用 [{{site.data.keyword.iot_short_notm}} 儀表板 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://internetofthings.ibmcloud.com){: new_window} 或使用 REST API 來建立裝置類型及裝置。 

如需使用 {{site.data.keyword.iot_short_notm}} IoT Platform 儀表板來新增裝置類型及裝置的相關資訊，請參閱[使用 Web 介面開始使用資料管理](../GA_information_management/im_ui_flow.html)文件。

如需使用 REST API 來新增裝置類型及裝置的相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window} 文件。



## 步驟 1：建立「物品」類型綱目檔。  
{: #crt_composition_file}  
建立「物品」類型綱目檔，以參照溫度及濕度裝置類型的裝置邏輯介面 ID。  

下列範例顯示如何建立稱為 *roomTypeSchema* 的「物品」類型綱目檔。   
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
**提示：**使用 **required** 參數，以將一個以上的內容標示為必要。required 內容必須包含在 Watson IoT Platform 的裝置訊息中，才能使用裝置資料來更新裝置狀態。不會處理未包括 required 內容的訊息。   
**附註：**當您建立「物品」類型時，必須指定在建立「物品」類型綱目檔時所產生的綱目 ID。  

## 步驟 2：建立「物品」綱目資源。  
{: #crt_composition_resource}  

上傳前一個步驟中所產生的「物品」類型綱目檔，即可建立綱目資源。  
使用下列 API，以上傳「物品」類型綱目檔：  
```
POST /draft/schemas
```  
綱目定義檔會在多組件 POST (multipart/form-data) 內傳遞至 Watson IoT Platform。POST 的內文必須至少包含兩個組件：

  - 一個的名稱為 **schemaFile**，內含作為組件內文的實際檔案內容。
  - 一個的名稱為 **name**，所含的字串定義組件內文中的綱目資源名稱。


如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文件。  

下列範例顯示如何使用 cURL 來建立「物品」類型綱目資源：  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
下列範例顯示 POST 方法的回應：
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
建立「物品」類型時，需要 POST 方法回應中所傳回的綱目 ID *5a72ea48d60180002c4f5e58*。


## 步驟 3：建立「物品」類型  
{: #crt_thing_type}  

「物品」類型用來建立「物品」實例的模型。必須先在組織中建立「物品」類型，才能建立「物品」實例。在此情境中，建立一個「物品」類型。  
與「物品」類型相關聯的綱目定義聚集在一起以建立「物品」實例的物件類型。「物品」類型必須參照您在前一個步驟中所建立的「物品」類型綱目資源。  
使用下列 API，以建立「物品」類型：
```
POST /draft/thing/types
```
POST 方法內文中需要下列參數：  
<table>
<tr><th>參數</th><th>說明</th></tr>
<tr><td>id</td><td>提供所建立「物品」類型的 ID。</td></tr>
<tr><td>name</td><td>提供所建立「物品」類型的名稱。</td></tr>
<tr><td>schemaId</td><td>針對組合綱目資源所建立的 ID。</td></tr>
</table>

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文件。  

下列範例顯示如何使用 cURL 來建立稱為 *RoomType* 的「物品」類型。
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

回應： 
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

## 步驟 4：建立邏輯介面綱目檔  
{: #crt_ai_schema_file}
在邏輯介面中，您可以定義儲存為「物品」狀態之資料的結構。在此情境中，建立可定義溫度及濕度內容的邏輯介面。參照建立邏輯介面資源時所產生的邏輯介面 ID，以將邏輯介面與「物品」類型 *RoomType* 相關聯。  
下列範例顯示如何建立稱為 *roomSchema* 的邏輯介面綱目檔。

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
## 步驟 5：建立邏輯介面綱目資源。  
{: #crt_ai_schema_resource}  
使用下列 API，以上傳前一個步驟中所建立的邏輯介面綱目檔，來建立「物品」類型的邏輯介面綱目資源：  
```
POST /draft/schemas
```  

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 文件。  

下列範例顯示如何使用 cURL 來建立邏輯介面綱目：
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
下列範例顯示 POST 方法的回應：
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
使用 POST 方法回應中所傳回的綱目 ID *5a4b9847d60180002efce645*，以將邏輯介面綱目資源新增至「物品」類型的邏輯介面。  


## 步驟 6：建立「物品」類型的邏輯介面。  
{: #crt_thing_ai}  
邏輯介面必須參照您在前一個步驟中所建立的邏輯介面綱目資源。  
若要建立邏輯介面，請使用下列 API：  
```
POST draft/logicalinterfaces
```  
您可以選擇性地指定有意義的邏輯介面別名。在用來擷取「物品」狀態的 API 呼叫或主題字串訂閱中，可以參照別名，而不使用自動產生的邏輯介面 ID。

**附註：**別名的長度必須是 1 - 36 個字元，且可以包括英數、連字號、句點、底線字元。別名不能是 24 個字元的十六進位字串。

在此情境下，使用前一個回應中所傳回的綱目 ID *5a4b9847d60180002efce645*，以將邏輯介面綱目新增至邏輯介面。

下列範例顯示如何使用 cURL 來建立別名為 *IRoom* 的邏輯介面：
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
下列範例顯示 POST 方法的回應：
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
在此情境下，使用 POST 方法回應中所傳回的邏輯介面 ID *5a4b9847d60180002efce645*，以將邏輯介面新增至裝置類型。您也可以使用此 ID，以將入埠裝置事件對映至邏輯介面所定義的內容。



如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 文件。  


## 步驟 7：將邏輯介面新增至「物品」類型。  
{: #add_thing_ai}  
若要將邏輯介面新增至「物品」類型，請使用下列 API：  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文件。  
在此情境下，邏輯介面與「物品」類型 *RoomType* 相關聯。

下列範例顯示如何使用 cURL 以將「物品」邏輯介面 *IRoom* 新增至「物品」類型 *RoomType*：  
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

下列範例顯示 POST 方法的回應：
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

## 步驟 8：定義對映
{: #define_Thing_type_mappings}

定義「物品」類型的對映，以說明如何將所聚集裝置或「物品」之狀態的內容對映至「物品」類型邏輯介面上所定義的內容。

若要對映事件，請使用下列 API：  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

其中 *thingtypeId* 是在建立「物品」類型時，POST 要求回應中所傳回的 ID。 

POST 要求內文中需要下列參數：  
<table>
<tr>
<th>	參數</th><th>	說明</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	有效的 JSON 結構，可對映針對邏輯介面所定義的內容與裝置事件有效負載的內容。</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	有效負載內文中需要邏輯介面 ID。</td>
</tr>
</table>  

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文件。

下列範例顯示如何使用 cURL 以將對映新增至「物品」類型 *RoomType*：

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
*thermometer* 裝置定義於 [roomTypeSchema](#crt_composition_file) 中。*$event.temperature* 內容定義於 ID 為 *5846ed076522050001db0e12* 且別名為 *IThermometer* 的邏輯介面綱目中。  
*hygrometer* 裝置定義於 [roomTypeSchema](#crt_composition_file) 中。*$event.humidity* 內容定義於 ID 為 *5846cd7c6522050001db0e24* 且別名為 *IHygrometer* 的邏輯介面綱目中。


## 步驟 9：驗證及啟動配置
{: #activate}

驗證及啟動與每種「物品」類型的「物品」狀態更新相關的配置。此配置包括綱目、邏輯介面及對映。 

若要驗證及啟動「物品」類型配置，請使用下列 API：
```
PATCH /draft/thing/types/{thingTypeId}
```
其中，*thingTypeId* 是「物品」類型 ID。 

下列範例顯示如何使用 cURL 來驗證及啟動與「物品」類型 *RoomType* 相關聯的配置：
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

下列範例顯示 PATCH 方法的回應：
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

## 步驟 10：建立「物品」類型的實例
{: #create_Thing_instances}

「物品」是「物品」類型的實例。「物品」可讓您將裝置或「物品」的一個以上實例聚集至單一物件。

若要建立「物品」，請使用下列 API：

```
POST /thing/types/{thingTypeId}/things
```
其中 *thingtypeId* 是在建立「物品」類型時，POST 要求回應中所傳回的 ID。 

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things) 文件。

在此情境下，我們需要建立「物品」類型為 *RoomType* 的兩個「物品」實例。其中一個「物品」實例稱為 *meetingroom1*，另一個「物品」實例則稱為 *meetingroom2*。

下列範例顯示如何使用 cURL 來建立稱為 *meetingroom1* 的「物品」實例。*meetingroom1*「物品」實例與 *tSensor* 及 *humiditySensor1* 裝置實例相關聯。

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

所建立的「物品」ID 用於呼叫以將溫度事件新增至「物品」邏輯介面的 POST 方法 URL 中。

下列範例顯示如何使用 cURL 來建立稱為 *meetingroom2* 的「物品」實例。*meetingroom2* 與 *tempSensor* 及 *humiditySensor2* 裝置實例相關聯。

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

## 步驟 11：驗證對映的裝置事件已發佈至邏輯介面  
{: #publish_event}  
針對聚集至「物品」*meetingroom1* 的裝置，發佈下列事件：  
 - 將來自 *tSensor* 的溫度事件發佈於主題 `iot-2/evt/tevt/fmt/json`  
 - 將來自 *humiditySensor1* 的濕度事件發佈於主題 `iot-2/evt/humevt/fmt/json`  
 
針對聚集至「物品」*meetingroom2* 的裝置，發佈下列事件：  
 - 將來自 *tempSensor* 的溫度事件發佈於主題 `iot-2/evt/tempevt/fmt/json`  
 - 將來自 *humiditySensor2* 的濕度事件發佈於主題 `iot-2/evt/humevt/fmt/json`  
 
如需發佈來自裝置的入埠事件的相關資訊，請參閱[應用程式的 MQTT 連線功能](../applications/mqtt.html#publishing_device_events)。  

## 步驟 12：確認「物品」的狀態已變更。  
{: #verify_Thing_state}  

您可以使用 HTTP REST API 或是訂閱主題來擷取「物品」的狀態。

如果您具有 MQTT 用戶端應用程式，則可以訂閱下列主題字串：
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

或者，您也可以使用下列 HTTP REST API 來擷取最新的「物品」狀態：

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

以下是必要參數：  
<table>
<tr>
<th>參數</th><th>	說明</th>
</tr>
<tr>
<td>thingTypeId	</td><td>「物品」類型 ID。</td>
</tr>
<tr>
<td>thingId	</td><td>	「物品」ID。</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>針對邏輯介面所建立的 ID。</td>
</tr>
</table>  

如需詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 文件。  


## 後續步驟

建立規則，以用來在 {{site.data.keyword.iot_short_notm}} 接收到的事件導致裝置或「物品」狀態變更時起始動作。如需建立規則的相關資訊，請參閱[建立內嵌規則（測試版）](im_rules.html)。
