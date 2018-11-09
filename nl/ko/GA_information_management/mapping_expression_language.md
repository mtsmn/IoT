---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 맵핑 표현식 언어 이해
{: #mapping_expression}

맵핑 표현식 언어를 사용하여 데이터를 조작 및 결합하고 처리된 데이터에서 실행할 수 있는 조회의 결과를 형식화할 수 있습니다. 맵핑 표현식 언어는 [JSONata ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/index.html){:new_window}의 서브세트이며 [맵핑](ga_im_definitions.html#resources)을 정의하거나 [규칙](../information_management/im_rules.html)을 작성할 때 사용될 수 있습니다.  

JSONata는 JSON 데이터에 대한 경량 조회 및 변환 언어입니다. JSONata를 사용해 볼 수 있는 빠르고 편리하게 방법을 제공하는 [JSONata 실행기 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://try.jsonata.org/){:new_window} 도구를 사용할 수도 있습니다. 

다음 정보는 사용 방법의 몇 가지 예와 함께 현재 지원되는 주요 연산자 및 함수를 표시합니다. 

## JSONata 연산자
{: #operators}

다음 연산자를 제외한 모든 [JSONata 연산자 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/operators.html){:new_window}가 지원됩니다. 

- ~> (함수 연결)
- ^(…) (정렬 기준)
- := (변수 바인딩)

**참고:** 
- 표현식 그룹화 및 연산자 우선순위 변경에 소괄호 ( ) 사용
- 작은따옴표 또는 큰따옴표를 사용하여 문자열 및 특성 이름 묶기. 예: $event.object.'ab' 
- 백틱(`)을 사용하여 공백을 포함하여 특수 문자가 포함된 특성 이름 묶기. 예:  
```
{"x y":22}.`x y`
```
- 백틱(`)을 사용하여 숫자로 시작하는 특성 이름 묶기. 예: 
```
`7emperature`
```

## JSONata 함수
{: #functions}
다음의 JSONata 함수가 지원됩니다.  

 - 모든 [문자열 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/string-functions.html){:new_window} 함수
 - 모든 [숫자 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/numeric-functions.html){:new_window} 함수 	
 - 모든 [숫자 집계 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 함수 
 - 모든 [부울 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/boolean-functions.html){:new_window} 함수  
 - [$count 및 $append 배열 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/array-functions.html){:new_window} 함수 	

## 맵핑 표현식 언어 확장

맵핑 표현식 언어가 $event, $state 및 $instance 변수 도입을 통해 데이터 관리 기능과 함께 사용하도록 확장되었습니다. JSON은 표현식 평가 전에 이러한 변수에 바인드됩니다. 다음 표에는 표현식에 사용하도록 정의된 해당 변수의 개요가 제공됩니다.

변수                   |예제 입력 JSON     |표현식으로서의 예제    |용도   
------------- | ------------- | -------------
$event |*{"t": 34.5}*  |$event.t |표현식에 사용할 인바운드 이벤트의 특성을 선택하십시오. 
$state |*{"temperature": 34.5,"humidity": 78 }*  |$state.temperature |표현식에 사용할 디바이스 상태의 특성을 선택하십시오.
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  |$instance.metadata.temp_adjustment |표현식에 사용할 디바이스 또는 디바이스 유형 속성을 선택하십시오. 

다음 예제는 이벤트에 대한 입력으로 다음 오브젝트를 사용합니다.   
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
오브젝트는 다음 표현식을 사용하여 배열로 변환됩니다.

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
표현식의 결과로 다음 출력이 생성됩니다.
```
 [
    35,
    72,
    1024
  ]
```

이 예제를 되돌려서 입력의 배열을 오브젝트로 변환할 수 있습니다. 다음 예제는 이벤트에 대한 입력으로 다음 배열을 사용합니다. 
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
배열은 다음 표현식을 사용하여 오브젝트로 변환됩니다.
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
 표현식의 결과로 다음 출력이 생성됩니다.
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
또한 이러한 변수 사용을 결합하는 표현식을 정의할 수 있습니다. 다음 예제에서 *temp_adjustment* 특성은 디바이스 메타데이터에 정의되고 이벤트 측정값을 보정하는 데 사용됩니다. 특성은 하나의 맵핑에 정의되지만 여러 디바이스에 적용될 수 있습니다. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


점 연산자 "."은 리터럴 키의 오브젝트 액세스에 사용됩니다(예: $event.object.hh). 왼쪽에 있는 표현식은 이벤트($event), 상태($state) 또는 메타데이터($instance) 중 하나의 특정 특성으로 제한됩니다. 오른쪽의 표현식에는 액세스할 정보가 포함되어 있습니다.

점 연산자에 대한 자세한 정보는 JSONata 문서의 [연산자 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/operators.html){:new_window} 섹션을 참조하십시오.


## 언어 안내서

- 모든 [기본 선택사항 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/basic.html){:new_window}이 지원됩니다.
- 모든 [복합 선택사항 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/complex.html){:new_window}이 지원됩니다(와일드카드는 제외).
- 조건부 및 소괄호에 포함된 표현식이 [프로그램 구성 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.jsonata.org/programming.html){:new_window}의 일부로서 지원됩니다. 변수는 Watson IoT Platform 데이터 관리에 특정한 확장 맵핑 언어의 일부로서 $instance, $state 및 $event 변수의 사용을 통해 지원됩니다. JSONata에서 사용되는 "$" 및 "$$" 변수는 현재 지원되지 않습니다.

## 출력 생성
배열 생성자 또는 오브젝트 생성자를 사용하여 처리된 데이터가 출력에서 제시되는 방법을 지정할 수 있습니다.

### 지원되는 배열 생성자
대괄호 [] 쌍으로 쉼표로 구분된 리터럴 또는 표현식의 목록을 묶어서 배열을 생성할 수 있습니다. 쉼표는 시퀀스 생성을 포함하여 배열 생성자 내에서 여러 표현식을 구분하는 데 사용됩니다. 예: *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* 및 *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### 지원되는 오브젝트 생성자
{: #constructors}
쉼표로 구분된 키 값 또는 쌍이 포함된 중괄호 {} 쌍을 사용하여 출력에서 JSON 오브젝트를 생성할 수 있으며,
각각의 키와 값은 콜론으로 구분됩니다. 예: *{key1 : value1, key2:value2}* 또는 *{"hello" : "world"}*. 오브젝트 키는 문자열이어야 합니다.  


## 예제: 배열을 사용하여 온도 데이터 처리 및 보고
다음 절에서는 [REST API를 사용하여 데이터 관리 시작하기](ga_im_example.html)의 예제를 기반으로 배열을 사용하여 온도 측정값의 슬라이드 창을 유지보수하고 해당 창 내에 포함된 측정값의 현재 합계나 평균을 계산하는 방법을 보여줍니다.

슬라이드 창은 도착 순서대로 데이터를 저장합니다. 삽입된 모든 데이터를 보관하는 대신 증분 방식으로 데이터를 제거하도록 슬라이드 창을 구성할 수 있습니다. 슬라이드 창이 채워진 다음에 데이터가 삽입되면 해당 창에서 가장 오래된 데이터 항목이 제거됩니다.

다음 예제는 개수 기반 제거 정책(크기 5)으로 구성된 슬라이드 창을 표시합니다.
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

다음 예제는 [단계별 안내서 1](ga_im_index_scenario.html#step4)의 7단계에 표시된 논리 인터페이스 스키마 파일 구성을 *tempReadings*라고 하는 배열을 추가하여 수정하는 방법을 보여줍니다. 배열은 마지막 5개의 이벤트를 위해 디바이스에서 전송된 마지막 5개 값이 있는 창을 유지보수하는 데 사용됩니다. *tempReadings*에 저장된 값이 없는 경우, 배열은 [0]으로 설정되며 다음에 수신된 측정값은 5개의 측정값이 수신될 때까지 증가되는 배열에 추가됩니다. 5개의 측정값이 수신된 후 새로 측정값이 수신되면 창에서 가장 오래된 측정값이 제거됩니다. 

**중요 참고:** *tempReadings*를 "필수"로 설정하고, 기본적으로 배열로 설정해야 합니다.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature": {
          "description" : "latest temperature reading in degrees Celsius",
          "type" : "number",
          "minimum" : -273.15,
          "default" : 0.0
      },
      "tempAverage": {
          "type": "number"
      },
      "tempReadings": {
          "default": [],
          "items": {
              "type": "number"
          },
         "type": "array"
      },
      "tempSum": {
          "type": "number"
      }
  },
  "required": [
      "tempReadings"
  ],
  "type": "object"
}
```
다음 절에서는 현재 슬라이드 창 내에 포함된 측정값을 기준으로 한 모든 온도 측정값의 합계 및 평균 온도 측정값을 계산하도록 맵핑 리소스를 구성하는 방법의 예제를 표시합니다.


```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt": {
               "tempAverage": "$average($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))",
               "tempReadings": "$count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t)",
               "tempSum": "$sum($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))"
           }
       },
       "updated": "2017-10-13T10:05:40Z",
       "updatedBy": "a-8x7nmj-9iqt56kfil",
       "version": "active"
   }
]
```
**참고:** *$state.tempReadings* 배열은 $average 및 $sum 함수에서 사용되기 전에 다시 계산됩니다. 맵핑 표현식의 순서를 제어할 수 없으므로, *tempAverage* 또는 *tempSum* 표현식이 평가될 때 배열에 현재 값이 포함되어 있는지 확인하기 위해 배열의 재계산이 필요합니다.
다음 예제는 5개의 온도 측정값이 있는 슬라이드 창을 기준으로 한 온도 합계와 평균 온도를 표시합니다.
```
{
    "state": {
        "tempAverage": 18557.6,
        "tempReadings": [
            17591,
            10262,
            25621,
            16676,
            22638
        ],
        "tempSum": 92788
    },
    "timestamp": "2017-10-13T11:07:20Z",
    "updated": "2017-10-13T11:05:40Z"
}
```

## 맵핑 표현식과 입력 데이터 간의 불일치 처리

맵핑 표현식 중 하나에 공개된 이벤트에 지정되지 않은 입력 데이터에 대한 참조가 포함되어 있으면 상태 업데이트에 실패할 수 있습니다.

예를 들어, 다음 표현식을 구성할 수 있습니다.
```
temperature = $event.t 
humidity = $event.hum
```
여기서 *t*는 이벤트의 선택적 특성입니다.
선택적 *t* 특성이 공개된 디바이스 이벤트에 지정되지 않았으므로, 습도 데이터만 포함된 이벤트가 수신된 경우에는(예: `{"humidity":22}`) `temperature = $event.t` 표현식을 평가할 수 없습니다.

상태 업데이트가 실패합니다. 습도 상태 특성은 업데이트되지 않으며 오류 메시지가 디바이스에 대한 MQTT 오류 주제에 공개됩니다.
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
미지정 선택적 데이터로 인한 상태 업데이트 실패를 방지하려면, 3진 조건과 함께 $exists 함수를 사용하여 선택적 특성의 기본값을 지정할 수 있습니다. 
다음 예제는 *t* 특성에 대해 기본값 *0*을 정의합니다.

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

이 방식으로 선택적 특성의 기본값을 정의하면 *t* 특성이 공개된 디바이스 이벤트에 지정되지 않은 경우에도 표현식이 정상적으로 평가됩니다.

따라서 이벤트 `{"humidity":22}`가 수신되면 상태 업데이트가 정상적으로 완료되며 디바이스 상태가 `{"humidity":22, "temperature":0}`으로 설정됩니다.
