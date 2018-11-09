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

# 단계별 안내서 2 - 습도 디바이스에 대한 논리 인터페이스 구성의 추가 정보

다음 정보를 사용하여 2개의 습도 디바이스의 센서가 IBM Watson™ IoT Platform에 이벤트를 공개하는 시나리오를 작성할 수 있습니다.  습도 디바이스의 측정값은 하나의 습도 측정값으로 맵핑됩니다. 하나의 디바이스는 미팅 룸 1에 있으며, 다른 디바이스는 미팅 룸 2에 있습니다. 


## 이 태스크에 관한 정보

[단계별 안내서 1](../GA_information_management/ga_im_index_scenario.html)에서 사용한 조직에 대해 동일한 {{site.data.keyword.iot_short_notm}} 조직 인스턴스 및 API 키와 토큰을 사용하십시오.  

이 구성은 [단계별 안내서 2](im_index_scenario_thing.html) 문서에 설명된 튜토리얼에 대한 전제조건입니다. 

이 시나리오에서는 하나의 디바이스 유형과 두 개의 디바이스 인스턴스가 작성됩니다. 

디바이스 인스턴스를 *humiditySensor1* 및 *humiditySensor2*라고 합니다. 디바이스는 습도 이벤트를 공개합니다. 습도 이벤트에는 다음 예제 페이로드가 있습니다. 
```
{
  "hum" : 35
}
```
습도 이벤트는 주제 `iot-2/evt/humevt/fmt/json`에서 공개됩니다.  

**참고:** 이벤트 ID는 *humevt*입니다. 이 ID는 이러한 유형의 습도 이벤트를 실제 인터페이스에 추가할 때와 이 유형의 인바운드 이벤트와 연관된 특성을 논리 인터페이스의 특성에 맵핑하기 위한 맵핑을 정의할 때 필요합니다. 이 시나리오에서는 논리 인터페이스에 정의된 특성을 **humidity**라고 합니다. 이 논리 인터페이스는 다음 구조로 이 유형의 디바이스에 대한 상태를 표시합니다.
```
{
  "humidity" : <current humidity value in Celsius>
}
```

다음 예제 시나리오를 사용하여 자체 인터페이스 환경을 설정할 수 있습니다.

**중요 참고:** 기타 태스크를 완료하는 데 ID가 필요하므로 curl 응답에서 생성된 ID를 저장해야 합니다.


## 필요하면 디바이스 유형 및 디바이스 추가
{: #step14}

이 시나리오에서는 하나의 디바이스 유형과 두 개의 디바이스 인스턴스를 가정합니다. 디바이스 인스턴스 *humiditySensor1* 및 *humiditySensor2*는 디바이스 유형 *HumiditySensor*와 연관되어 있습니다.  

[{{site.data.keyword.iot_short_notm}} 대시보드 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://internetofthings.ibmcloud.com){: new_window}를 사용하거나 REST API를 사용하여 디바이스 유형 및 디바이스를 작성할 수 있습니다. {{site.data.keyword.iot_short_notm}} IoT Platform 대시보드를 사용한 디바이스 유형 및 디바이스 추가에 대한 자세한 정보는 [웹 인터페이스를 사용하여 데이터 관리 시작하기](../GA_information_management/im_ui_flow.html) 문서를 참조하십시오. 

다음 예제는 REST API를 사용하여 *HumiditySensor*라고 하는 디바이스 유형을 작성하는 방법을 표시합니다. 

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**중요 참고:** 디바이스 유형 및 디바이스를 작성할 때 메타데이터를 추가할 수 있습니다. 이 시나리오에서는 다음 메타데이터가 디바이스 유형 *HumiditySensor*에 추가됩니다.
```
{
    "maxHumidityThreshold": 70
}
```
이 메타데이터는 디바이스 상태의 *humidity* 특성이 70% 값을 초과하도록 하는 습도 이벤트를 *humiditySensor1* 또는 *humiditySensor2*에서 {{site.data.keyword.iot_short_notm}}이 수신할 때 트리거되는 [규칙을 작성](im_rules.html)할 때 사용될 수 있습니다.  

## 디바이스 등록

다음 예제는 *humiditySensor1*이라고 하는 디바이스 인스턴스를 등록하는 방법을 표시합니다. 
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
다음 예제는 *humiditySensor2*라고 하는 디바이스 인스턴스를 등록하는 방법을 표시합니다.
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**참고:** 디바이스가 Watson IoT Platform HTTP REST API를 통해 HTTP 요청을 작성하는 경우에는 사용자 이름과 비밀번호가 필요합니다. 비밀번호는 디바이스가 등록될 때 자동으로 생성되거나 수동으로 지정된 인증 토큰의 값입니다. MQTT 클라이언트를 사용하는 경우, IoT 주제 문자열을 구독하여 디바이스 또는 사물 상태를 검색하려면 토큰이 필요하므로 디바이스의 인증 토큰을 기록해 두어야 합니다. 

## 1단계: 이벤트 스키마 파일 작성
{: #step1}

이 시나리오의 경우에는 인바운드 습도 이벤트의 구조를 정의하는 이벤트 스키마 파일을 작성하십시오. 

다음 예제는 *humEventSchema.json*이라고 하는 스키마 파일을 작성하는 방법을 표시합니다. 이 파일은 습도 디바이스에서 인바운드 이벤트의 구조를 정의합니다.

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
**팁:** **필수** 매개변수를 사용하여 하나 이상의 특성을 필수로 표시할 수 있습니다. {{site.data.keyword.iot_short_notm}}이 디바이스 데이터로 디바이스 상태를 업데이트할 수 있도록 필수 특성이 디바이스 메시지에 포함되어야 합니다. 필수 특성이 포함되지 않은 메시지는 처리되지 않습니다.   


## 2단계: 이벤트 유형에 대한 이벤트 스키마 리소스 작성
{: #step2}

이벤트 스키마 리소스를 작성하려면 다음 API를 사용하십시오.

```
POST /draft/schemas
```

스키마 정의 파일은 다중 파트 POST(multipart/form-data) 내에서 Watson IoT Platform으로 전달됩니다. POST의 본문에는 최소한 두 개의 파트가 포함되어 있어야 합니다. 

- 하나의 이름은 **schemaFile**이며, 여기에는 파트의 본문으로서 파일의 실제 컨텐츠가 포함되어 있습니다. 
- 하나의 이름은 **name**이며, 여기에는 파트의 본문에서 스키마 리소스의 이름을 정의하는 문자열이 포함되어 있습니다. 

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) 문서를 참조하십시오.

다음 예제는 cURL을 사용하여 이벤트 스키마 리소스 *humEventSchema.json*을 작성하는 방법을 표시합니다. 

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**참고:** 예제 권한 부여 값 `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=`은 다음 정보로 구성되어 있습니다.
`{API Key}:{authorization token}` (이는 다시 Base64 인코딩됨) 

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

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
POST 메소드에 대한 응답으로 리턴된 스키마 ID *5846cd7c6522050001db0e20*는 이벤트 유형에 이벤트 스키마를 추가할 때 필요합니다. 



## 3단계: 이벤트 스키마를 참조하는 이벤트 유형 작성
{: #step3}

각각의 이벤트 유형은 이벤트 스키마 리소스의 작성에 사용된 POST 메소드에 대한 응답으로 리턴된 스키마 ID를 사용하여 이전 예제에서 작성된 관련 이벤트 스키마를 참조합니다.

이벤트 유형을 작성하려면 다음 API를 사용하십시오.

```
POST /draft/event/types
```
드래프트 이벤트 유형은 인바운드 MQTT 이벤트의 구조를 정의하는 스키마 정의를 참조해야 합니다. 


세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) 문서를 참조하십시오.


다음 예제는 cURL을 사용하여 습도 이벤트에 대한 이벤트 유형을 작성합니다. 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

스키마 ID *5846cd7c6522050001db0e20*은 이벤트 유형에 이벤트 스키마를 추가하는 데 사용됩니다. 이 ID는 이벤트 스키마 리소스 *humEventSchema.json*을 작성하는 데 사용된 POST 메소드에 대한 응답으로 리턴되었습니다. 

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

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

POST 메소드에 대한 응답으로 리턴된 이벤트 유형 ID *5846cd7c6522050001db0e21*는 실제 인터페이스에 이벤트 유형을 추가하는 데 사용됩니다. 


## 4단계: 실제 인터페이스 작성
{: #step7}

실제 인터페이스를 작성하려면 다음 API를 사용하십시오.

```
POST /draft/physicalinterfaces
```

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 문서를 참조하십시오.

이 시나리오에서는 이전에 작성된 이벤트 유형에 대해 하나의 실제 인터페이스가 필요합니다. 

다음 예제는 cURL을 사용하여 실제 인터페이스를 작성하는 방법을 표시합니다. 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

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

응답에서 리턴된 실제 인터페이스 ID *5846cd7c6522050001db0e22*는 섭씨 온도로 측정된 온도 이벤트를 실제 인터페이스에 추가하기 위해 호출되는 POST 메소드의 URL에서 사용됩니다. 


## 5단계: 실제 인터페이스에 이벤트 유형 추가
{: #step8}

실제 인터페이스에 이벤트 유형을 추가하려면 다음 API를 사용하십시오.

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) 문서를 참조하십시오.

이 시나리오에서는 다음 이벤트 유형이 지정된 실제 인터페이스에 추가됩니다. 
- 습도 이벤트 *humevt*는 주제의 *eventId* 및 이벤트 스키마 리소스 작성의 *eventTypeId*를 사용하여 ID *5846cd7c6522050001db0e22*의 실제 인터페이스에 추가됩니다. 

다음 예제는 cURL을 사용하여 습도 이벤트 *humevt*를 ID *5846cd7c6522050001db0e22*의 실제 인터페이스에 추가하는 방법을 표시합니다. 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## 6단계: 실제 인터페이스에 연결하도록 디바이스 유형 업데이트
{: #step9}

디바이스 유형를 업데이트하려면 다음 API를 사용하십시오.

```
POST /draft/device/types/{typeId}/physicalinterface
```

여기서 *typeId*는 디바이스 유형 ID입니다.


세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) 문서를 참조하십시오.

이 시나리오에서 디바이스 유형 *HumiditySensor*는 실제 인터페이스 *5846cd7c6522050001db0e22*에 연결하기 위해 업데이트됩니다. 

다음 예제는 cURL을 사용하여 디바이스 유형 *HumiditySensor*를 업데이트하는 방법을 표시합니다. 

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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
디바이스 ID *HumiditySensor*는 실제 인터페이스와 논리 인터페이스를 추가할 때 필요합니다. 
  


## 7단계: 논리 인터페이스 스키마 파일 작성
{: #step4}

다음 예제는 *hygrometer.json*이라고 하는 논리 인터페이스 스키마 파일을 작성하는 방법을 표시합니다. 

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

## 8단계: 논리 인터페이스 스키마 리소스 작성
{: #step5}
다음 예제는 cURL을 사용하여 논리 인터페이스를 작성하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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

## 9단계: 논리 인터페이스 스키마를 참조하는 논리 인터페이스 작성
{: #step6}

논리 인터페이스를 작성하려면 다음 API를 사용하십시오.

```
POST /draft/logicalinterfaces
```

선택적으로 논리 인터페이스의 의미 있는 별명 이름을 지정할 수 있습니다. 별명은 자동 생성된 논리 인터페이스 ID를 사용하는 대신에 디바이스의 상태를 검색하는 데 사용되는 API 호출 또는 주제 문자열 구독에서 참조될 수 있습니다. 

**참고:** 별명 이름의 길이는 1-36자여야 하며 영숫자, 하이픈, 마침표, 밑줄 문자를 포함할 수 있습니다. 별명 이름은 24자의 16진 문자열일 수 없습니다. 

세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) 문서를 참조하십시오.

이 시나리오에서는 이전 응답에서 리턴된 스키마 ID *5846cd7c6522050001db0e23*를 사용하여 논리 인터페이스 스키마를 논리 인터페이스에 추가합니다. 

다음 예제는 cURL을 사용하여 논리 인터페이스를 작성하는 방법을 표시합니다.

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.

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
이 시나리오에서는 POST 메소드에 대한 응답으로 리턴된 논리 인터페이스 ID *5846cd7c6522050001db0e24*를 사용하여 논리 인터페이스를 디바이스 유형에 추가합니다. 또한 이 ID를 사용하여 논리 인터페이스에서 정의한 특성에 인바운드 디바이스 이벤트를 맵핑합니다. 논리 인터페이스 별명 *IHygrometer*를 사용하면 HTTP REST API를 사용하거나 주제 문자열을 구독하여 디바이스의 상태를 검색할 수 있습니다. 


## 10단계: 디바이스 유형에 논리 인터페이스 추가

다음 예제는 cURL을 사용하여 논리 인터페이스 스키마 ID 5846cd7c6522050001db0e23을 참조하는 논리 인터페이스 5846cd7c6522050001db0e24를 디바이스 유형 HumiditySensor에 추가하는 방법을 표시합니다. 
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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

## 11단계: 디바이스 유형에 맵핑 추가

    
다음 예제는 cURL을 사용하여 디바이스 유형 *HumiditySensor*에 대한 맵핑을 추가하는 방법을 표시합니다. 

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

다음 예제는 POST 메소드에 대한 응답을 표시합니다.
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

## 12단계: 디바이스 유형에 대한 구성 활성화


    
다음 예제는 cURL을 사용하여 디바이스 유형 *HumiditySensor*에 대한 구성을 유효성 검증하고 활성화하는 방법을 표시합니다. 
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
다음 예제는 PATCH 메소드에 대한 응답을 표시합니다.
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
