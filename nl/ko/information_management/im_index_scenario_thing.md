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

# 단계별 안내서 2: 공통 인터페이스를 통한 사물 관련 작업 방법에 대한 자세한 예제(베타)
{: #scenario}

이 시나리오는 이전의 [단계별 안내서 1: 공통 인터페이스를 통한 디바이스 관련 작업 방법에 대한 자세한 예제](../GA_information_management/ga_im_index_scenario.html)를 기반으로 합니다. 

**중요:** 데이터 관리를 위한 {{site.data.keyword.iot_full}} 사물 기능은 제한된 베타 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

이 시나리오에서 온도 및 습도 디바이스는 두 개의 미팅 룸(미팅 룸 1과 미팅 룸 2)에서 수집된 환경 데이터를 공개합니다. 각 미팅 룸에 대해 온도 및 습도 디바이스 데이터는 두 개의 디바이스 유형 논리 인터페이스에 개별적으로 맵핑됩니다. 

미팅 룸 1의 경우, *TSensor* 디바이스 유형과 연관된 온도 디바이스 데이터는 논리 인터페이스 *온도계 인터페이스*에 맵핑되고 디바이스 유형 *HumiditySensor*와 연관된 습도 디바이스 데이터는 논리 인터페이스 *습도계 인터페이스*에 맵핑됩니다.  

미팅 룸 2의 경우, TempSensor 디바이스 유형과 연관된 온도 디바이스 데이터는 *온도계 인터페이스* 논리 인터페이스에 맵핑되고 디바이스 유형 *HumiditySensor*와 연관된 습도 디바이스 데이터는 *습도계 인터페이스* 논리 인터페이스에 맵핑됩니다.  

그리고 두 개의 룸 사물 인스턴스 *meetingroom1* 및 *meetingroom2*와 함께 *RoomType*이라고 하는 사물 유형이 작성됩니다. 

이 시나리오에서는 온도계 및 습도계 논리 인터페이스가 포함된 구성을 설정한 후에 올바른 환경 디바이스를 각각의 룸 인스턴스에 맵핑합니다(예: *tSensor* 및 *humiditySensor1*이 *meetingroom1*에 맵핑됨). 

## 전제조건
{: #pre_req}

계속하기 전에 다음을 확인하십시오.
- 단계별 안내서 1에서 사용한 해당 조직에 대해 동일한 {{site.data.keyword.iot_full}} 조직 인스턴스 및 API 키와 토큰을 사용하십시오. API 키와 토큰에 대한 자세한 정보는 [애플리케이션용 HTTP REST API](../applications/api.html#authentication) 문서의 인증 섹션을 참조하십시오. 
- 두 개의 논리 인터페이스(온도 디바이스용 하나와 습도 디바이스용 하나)가 구성되어 있습니다. 온도 디바이스의 논리 인터페이스 구성에 대한 정보는 [단계별 안내서 1: 공통 인터페이스를 통한 디바이스 관련 작업 방법에 대한 자세한 예제](../GA_information_management/ga_im_index_scenario.html#step4) 문서를 참조하십시오. 습도 디바이스의 논리 인터페이스 구성에 대한 정보는 [단계별 안내서 2 - 습도 디바이스에 대한 논리 인터페이스 구성의 추가 정보](im_hygrometer.html)를 참조하십시오. 

## 이 태스크에 관한 정보
{: #about}

{{site.data.keyword.iot_short_notm}}에서 하나의 사물은 다수의 디바이스와 사물로 구성될 수 있습니다. 사물 유형은 사물의 인스턴스가 작성되는 방법을 정의합니다.  

논리 인터페이스는 사물 유형과 연관되어 있습니다. 이 연관은 사물 유형 인스턴스에 대해 생성된 상태의 구조를 정의합니다. 맵핑을 사용하면 집계된 디바이스와 사물의 특성이 사물 상태의 특성에 맵핑되는 방법을 정의할 수 있습니다. 

논리 인터페이스를 사용하면 애플리케이션이 디바이스 또는 사물이 구성된 방법을 이해하고 있어야 할 필요가 없습니다. 예를 들어, 하나의 디바이스를 사용하여 룸의 온도를 측정하거나 여러 디바이스의 평균 측정값을 취합하여 룸 온도를 계산할 수 있습니다. 애플리케이션은 룸의 상태에 대한 정보를 요구하며, 이 가운데 하나의 컴포넌트는 온도 특성입니다. 애플리케이션이 수신한 온도 값을 계산하는 방법은 중요하지 않습니다.

이 시나리오에서는 두 개의 온도 디바이스와 두 개의 습도 디바이스가 이벤트를 {{site.data.keyword.iot_short_notm}}에 공개합니다. 하나의 온도 디바이스와 하나의 습도 디바이스는 사무실 블록의 미팅 룸 1에 있습니다. 다른 온도 및 습도 디바이스는 미팅 룸 2에 있습니다. 다음 다이어그램은 미팅 룸 1에 대한 구성을 보여줍니다. 

![ {{site.data.keyword.iot_short_notm}}에서 온도 및 습도 사물 및 애플리케이션 간의 맵핑.](images/Information)

*RoomType*이라고 하는 사물 유형은 룸의 인스턴스가 작성되는 방법을 정의하는 데 사용됩니다. 논리 인터페이스는 *RoomType*과 연관되어 있으며, 온도와 습도를 모두 보여주는 단일 측정값으로 인바운드 이벤트가 맵핑됨을 정의합니다. 이 단일 측정값은 사물 상태입니다. 맵핑을 사용하면 온도 및 습도 디바이스의 특성이 이 사물 상태에 맵핑되는 방법을 정의할 수 있습니다. 새 측정값이 이러한 디바이스에 의해 공개되면 사물 상태와 연관된 특성의 값이 변경됩니다. 

다음 표에서는 예제에서 사용되는 4개의 디바이스, 각 디바이스가 공개되는 주제, 그리고 각 디바이스의 예제 페이로드를 보여줍니다. 

디바이스/유형 |이벤트 |이벤트 페이로드/특성
------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**참고:** 이벤트 ID *tevt*, *tempevt*, *humevt*는 논리 인터페이스의 특성에 대해 해당 유형의 인바운드 이벤트와 연관된 특성을 작성하기 위한 맵핑을 정의할 때 필요합니다. 이 시나리오에서는 두 개의 특성 *temperature* 및 *humidity*가 논리 인터페이스에서 정의됩니다. 

논리 인터페이스도 구성됩니다. 논리 인터페이스는 다음과 같은 구조로 사물 상태를 표시합니다. 
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

다음 예제 시나리오를 사용하여 자체 인터페이스 환경을 설정할 수 있습니다. 

**참고:** 이 안내서에서 사용되는 리소스 특성 이름, 값 및 ID를 나열하는 표는 [단계별 안내서 1 및 2 문서에서 참조되는 리소스 특성 및 ID](im_id_reference.html)에 설명되어 있습니다. 

## 필요하면 디바이스 유형 및 디바이스 추가  
{: #add_device}  
이 시나리오에서는 세 개의 디바이스 유형과 네 개의 디바이스 인스턴스가 사용됩니다. 디바이스 인스턴스 *tSensor*는 디바이스 유형 *TSensor*와 연관되어 있습니다. 디바이스 인스턴스 *tempSensor*는 디바이스 유형 *TempSensor*와 연관되어 있습니다. 디바이스 인스턴스 *humiditySensor1* 및 *humiditySensor2*는 디바이스 유형 *HumiditySensor*와 연관되어 있습니다. 

[{{site.data.keyword.iot_short_notm}} 대시보드 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://internetofthings.ibmcloud.com){: new_window}를 사용하거나 REST API를 사용하여 디바이스 유형 및 디바이스를 작성할 수 있습니다.  

{{site.data.keyword.iot_short_notm}} IoT Platform 대시보드를 사용한 디바이스 유형 및 디바이스 추가에 대한 자세한 정보는 [웹 인터페이스를 사용하여 데이터 관리 시작하기](../GA_information_management/im_ui_flow.html) 문서를 참조하십시오. 

REST API를 사용한 디바이스 유형 및 디바이스의 추가에 대한 정보는 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window} 문서를 참조하십시오. 



## 1단계: 사물 유형 스키마 파일 작성   
{: #crt_composition_file}  
온도 및 습도 디바이스 유형에 대해 디바이스 논리 인터페이스 ID를 참조하는 사물 유형 스키마 파일을 작성하십시오.   

다음 예제는 *roomTypeSchema*라고 하는 사물 유형 스키마 파일을 작성하는 방법을 표시합니다.     
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
**팁:** **필수** 매개변수를 사용하여 하나 이상의 특성을 필수로 표시할 수 있습니다. Watson IoT Platform이 디바이스 데이터로 디바이스 상태를 업데이트할 수 있도록 필수 특성은 디바이스 메시지에 포함되어야 합니다. 필수 특성이 포함되지 않은 메시지는 처리되지 않습니다.   
**참고:** 사물 유형 스키마 파일을 작성할 때 생성된 스키마 ID는 사물 유형을 작성할 때 지정되어야 합니다.
  

## 2단계: 사물 스키마 리소스 작성   
{: #crt_composition_resource}  

이전 단계에서 생성된 사물 유형 스키마 파일을 업로드하여 스키마 리소스를 작성하십시오.   
다음 API를 사용하여 사물 유형 스키마 파일을 업로드하십시오.   
```
POST /draft/schemas
```  
스키마 정의 파일은 다중 파트 POST(multipart/form-data) 내에서 Watson IoT Platform으로 전달됩니다. POST의 본문에는 최소한 두 개의 파트가 포함되어 있어야 합니다. 

  - 하나의 이름은 **schemaFile**이며, 여기에는 파트의 본문으로서 파일의 실제 컨텐츠가 포함되어 있습니다. 
  - 하나의 이름은 **name**이며, 여기에는 파트의 본문에서 스키마 리소스의 이름을 정의하는 문자열이 포함되어 있습니다. 


세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 문서를 참조하십시오.  

다음 예제는 cURL을 사용하여 사물 유형 스키마 리소스를 작성하는 방법을 표시합니다.   
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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
POST 메소드에 대한 응답으로 리턴된 스키마 ID *5a72ea48d60180002c4f5e58*은 사물 유형을 작성할 때 필요합니다. 


## 3단계: 사물 유형 작성   
{: #crt_thing_type}  

사물 유형은 사물 인스턴스를 모델링하는 데 사용됩니다. 사물 인스턴스를 작성하려면 우선 사물 유형이 조직에서 작성되어야 합니다. 이 시나리오의 경우에는 하나의 사물 유형을 작성합니다.   
사물 유형과 연관된 스키마는 사물의 인스턴스를 작성하기 위해 함께 집계되는 오브젝트의 유형을 정의합니다. 사물 유형은 이전 단계에서 작성된 사물 유형 스키마 리소스를 참조해야 합니다.   
다음 API를 사용하여 사물 유형을 작성하십시오. 
```
POST /draft/thing/types
```
다음 매개변수는 POST 메소드의 본문에서 필요합니다.   
<table>
<tr><th>매개변수</th><th>설명</th></tr>
<tr><td>id</td><td>작성 중인 사물 유형의 ID를 제공합니다. </td></tr>
<tr><td>name</td><td>작성 중인 사물 유형의 이름을 제공합니다. </td></tr>
<tr><td>schemaId</td><td>구성 스키마 리소스에 대해 작성된 ID입니다. </td></tr>
</table>

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 문서를 참조하십시오.  

다음 예제는 cURL을 사용하여 *RoomType*이라고 하는 사물 유형을 작성하는 방법을 표시합니다. 
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

응답: 
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

## 4단계: 논리 인터페이스 스키마 파일 작성  
{: #crt_ai_schema_file}
논리 인터페이스에서 사용자는 사물 상태로서 저장된 데이터의 구조를 정의할 수 있습니다. 이 시나리오의 경우에는 온도 및 습도 특성을 정의하는 논리 인터페이스를 작성합니다. 논리 인터페이스 리소스를 작성할 때 생성된 논리 인터페이스 ID를 참조하여 논리 인터페이스를 사물 유형 *RoomType*과 연관시키십시오.   
다음 예제는 *roomSchema*라고 하는 논리 인터페이스 스키마 파일을 작성하는 방법을 표시합니다. 

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
## 5단계: 논리 인터페이스 스키마 리소스 작성   
{: #crt_ai_schema_resource}  
다음 API를 사용하여 사물 유형의 논리 인터페이스 스키마 리소스를 작성하는 이전 단계에서 작성된 논리 인터페이스 스키마 파일을 업로드하십시오.   
```
POST /draft/schemas
```  

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 문서를 참조하십시오.  

다음 예제는 cURL을 사용하여 논리 인터페이스 스키마를 작성하는 방법을 표시합니다.
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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
POST 메소드에 대한 응답으로 리턴된 스키마 ID *5a4b9847d60180002efce645*를 사용하여 논리 인터페이스 스키마 리소스를 사물 유형의 논리 인터페이스에 추가할 수 있습니다.   


## 6단계: 사물 유형의 논리 인터페이스 작성   
{: #crt_thing_ai}  
논리 인터페이스는 이전 단계에서 작성된 논리 인터페이스 스키마 리소스를 참조해야 합니다.   
논리 인터페이스를 작성하려면 다음 API를 사용하십시오.  
```
POST draft/logicalinterfaces
```  
선택적으로 논리 인터페이스의 의미 있는 별명 이름을 지정할 수 있습니다. 별명은 자동 생성된 논리 인터페이스 ID를 사용하는 대신에 사물의 상태를 검색하는 데 사용되는 API 호출 또는 주제 문자열 구독에서 참조될 수 있습니다. 

**참고:** 별명 이름의 길이는 1-36자여야 하며 영숫자, 하이픈, 마침표, 밑줄 문자를 포함할 수 있습니다. 별명 이름은 24자의 16진 문자열일 수 없습니다.

이 시나리오에서는 이전 응답에서 리턴된 스키마 ID *5a4b9847d60180002efce645*를 사용하여 논리 인터페이스 스키마를 논리 인터페이스에 추가합니다. 

다음 예제는 cURL을 사용하여 별명 *IRoom*의 논리 인터페이스를 작성하는 방법을 표시합니다. 
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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
이 시나리오에서는 POST 메소드에 대한 응답으로 리턴된 논리 인터페이스 ID *5a4b9847d60180002efce645*를 사용하여 논리 인터페이스를 디바이스 유형에 추가합니다. 또한 이 ID를 사용하여 논리 인터페이스에서 정의한 특성에 인바운드 디바이스 이벤트를 맵핑합니다.

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 문서를 참조하십시오.  


## 7단계: 논리 인터페이스를 사물 유형에 추가   
{: #add_thing_ai}  
사물 유형에 논리 인터페이스를 추가하려면 다음 API를 사용하십시오.  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 문서를 참조하십시오.  
이 시나리오에서는 논리 인터페이스가 사물 유형 *RoomType*과 연관되어 있습니다. 

다음 예제는 cURL을 사용하여 사물 논리 인터페이스 *IRoom*를 사물 유형 *RoomType*에 추가하는 방법을 표시합니다.   
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

다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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

## 8단계: 맵핑 정의
{: #define_Thing_type_mappings}

집계된 디바이스 또는 사물의 상태의 특성을 사물 유형 논리 인터페이스에서 정의된 특성으로 맵핑하는 방법을 기술하는 사물 유형에 대한 맵핑을 정의하십시오. 

이벤트를 맵핑하려면 다음 API를 사용하십시오.  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

여기서 *thingtypeId*는 사물 유형이 작성될 때 POST 요청에 대한 응답으로 리턴되는 ID입니다.  

다음 매개변수는 POST 요청의 본문에서 필요합니다.   
<table>
<tr>
<th>	매개변수	</th><th>	설명	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	디바이스 이벤트 페이로드의 특성으로 논리 인터페이스에 대해 정의된 특성을 맵핑하는 유효한 JSON 구조입니다. </td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	논리 인터페이스 ID가 페이로드의 본문에서 필요합니다.	</td>
</tr>
</table>  

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 문서를 참조하십시오.

다음 예제는 cURL을 사용하여 사물 유형 *RoomType*에 대한 맵핑을 추가하는 방법을 표시합니다. 

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
*온도계* 디바이스는 [roomTypeSchema](#crt_composition_file)에 정의되어 있습니다. *$event.temperature* 특성은 ID *5846ed076522050001db0e12* 및 별명 *IThermometer*의 논리 인터페이스 스키마에 정의되어 있습니다.   
*습도계* 디바이스는 [roomTypeSchema](#crt_composition_file)에 정의되어 있습니다. *$event.humidity* 특성은 ID *5846cd7c6522050001db0e24* 및 별명 *IHygrometer*의 논리 인터페이스 스키마에 정의되어 있습니다. 


## 9단계: 구성의 유효성 검증 및 활성화
{: #activate}

각 사물 유형의 사물 상태 업데이트와 관련된 구성을 유효성 검증하고 활성화하십시오. 이 구성에는 스키마, 논리 인터페이스 및 맵핑이 포함됩니다.  

사물 유형 구성을 유효성 검증하고 활성화하려면 다음 API를 사용하십시오.
```
PATCH /draft/thing/types/{thingTypeId}
```
여기서 *thingTypeId*는 사물 유형 ID입니다.  

다음 예제는 cURL을 사용하여 사물 유형 *RoomType*과 연관된 구성을 유효성 검증하고 활성화하는 방법을 표시합니다. 
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

다음 예제는 PATCH 메소드에 대한 응답을 표시합니다.
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

## 10단계: 사물 유형의 인스턴스 작성 
{: #create_Thing_instances}

사물은 사물 유형의 인스턴스입니다. 사물을 사용하면 디바이스 또는 사물의 하나 이상의 인스턴스를 단일 오브젝트에 함께 집계할 수 있습니다. 

사물을 작성하려면 다음 API를 사용하십시오. 

```
POST /thing/types/{thingTypeId}/things
```
여기서 *thingtypeId*는 사물 유형이 작성될 때 POST 요청에 대한 응답으로 리턴되는 ID입니다.  

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things) 문서를 참조하십시오.

이 시나리오에서는 사물 유형 *RoomType*의 사물 인스턴스를 두 개 작성해야 합니다. 하나의 사물 인스턴스를 *meetingroom1*이라고 하고, 또 다른 사물 인스턴스를 *meetingroom2*라고 합니다. 

다음 예제는 cURL을 사용하여 *meetingroom1*이라고 하는 사물 인스턴스를 작성하는 방법을 표시합니다. *meetingroom1* 사물 인스턴스는 *tSensor* 및 *humiditySensor1* 디바이스 인스턴스와 연관되어 있습니다. 

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

작성된 사물 ID는 온도 이벤트를 사물 논리 인터페이스에 추가하기 위해 호출되는 POST 메소드의 URL에서 사용됩니다. 

다음 예제는 cURL을 사용하여 *meetingroom2*라고 하는 사물 인스턴스를 작성하는 방법을 표시합니다. *meetingroom2*는 *tempSensor* 및 *humiditySensor2* 디바이스 인스턴스와 연관되어 있습니다. 

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

## 11단계: 맵핑된 디바이스 이벤트가 논리 인터페이스에 공개되는지 확인  
{: #publish_event}  
사물 *meetingroom1*에 집계된 디바이스에 대해 다음 이벤트를 공개하십시오.   
 - `iot-2/evt/tevt/fmt/json` 주제에서 *tSensor*의 온도 이벤트   
 - `iot-2/evt/humevt/fmt/json` 주제에서 *humiditySensor1*의 습도 이벤트   
 
사물 *meetingroom2*에 집계된 디바이스에 대해 다음 이벤트를 공개하십시오.   
 - `iot-2/evt/tempevt/fmt/json` 주제에서 *tempSensor*의 온도 이벤트  
 - `iot-2/evt/humevt/fmt/json` 주제에서 *humiditySensor2*의 습도 이벤트  
 
디바이스에서 인바운드 이벤트의 공개에 대한 정보는 [애플리케이션용 MQTT 연결](../applications/mqtt.html#publishing_device_events)을 참조하십시오.  

## 12단계: 사물의 상태가 변경되었는지 확인.   
{: #verify_Thing_state}  

HTTP REST API를 사용하거나 주제를 구독하여 사물의 상태를 검색할 수 있습니다.

MQTT 클라이언트 애플리케이션이 있는 경우 다음 주제 문자열을 구독할 수 있습니다.
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

또는 다음 HTTP REST API를 사용하여 최신 사물 상태를 검색할 수 있습니다.

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

필수 매개변수는 다음과 같습니다.  
<table>
<tr>
<th>매개변수	</th><th>	설명</th>
</tr>
<tr>
<td>thingTypeId	</td><td>사물 유형 ID입니다.</td>
</tr>
<tr>
<td>thingId	</td><td>	사물 ID입니다.</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>논리 인터페이스에 대해 작성된 ID입니다.</td>
</tr>
</table>  

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) 문서를 참조하십시오.  


## 다음 단계

{{site.data.keyword.iot_short_notm}}에서 수신한 이벤트로 인해 디바이스 또는 사물 상태가 변경될 때 조치를 시작하기 위해 사용할 수 있는 규칙을 작성하십시오. 규칙 작성에 대한 정보는 [임베디드 규칙 작성(베타)](im_rules.html)을 참조하십시오. 
