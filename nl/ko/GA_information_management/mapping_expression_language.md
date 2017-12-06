---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-09"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 맵핑 표현식 언어 이해
{: #mapping_expression}

맵핑 표현식 언어를 사용하여 데이터를 조작하고 결합할 수 있고 처리된 데이터에서 실행할 수 있는 모든 조회 결과를 형식화할 수 있습니다. 맵핑 표현식 언어는 [JSONata(![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘"))](http://docs.jsonata.org/index.html){:new_window}의 서브세트이며 [맵핑](ga_im_definitions.html#definitions_resources)을 정의할 때 사용될 수 있습니다. JSONata는 JSON 데이터에 대한 경량 조회 및 변환 언어입니다.

다음 정보는 사용 방법의 몇 가지 예와 함께 현재 지원되는 주요 연산자 및 함수를 표시합니다. 

## 지원되는 JSONata 연산자
{: #operators}

지원되는 JSONata 연산자의 서브세트는 다음과 같습니다. 

연산자 유형                   | 지원되는 연산자     | 참고   
------------- | ------------- | -------------
산술 | *+* - / * % | % 연산자는 나머지를 리턴합니다.
비교 | < <= > >= != = | 같음 연산자는 JSONata의 경우와 같이 =입니다.
부울 | *in*, *and* | 부울 상수는 *true* 또는 *false*입니다.
조건부 삼항 | ? | ? 연산자는 테스트 조건의 결과에 따라 두 대체 표현식 중 하나를 평가합니다. 연산자의 양식은 다음과 같습니다. *expression*  ? *value_if_true* : *value_if_false*
문자열| & | & 연산자는 단일 결과 문자열에 피연산자의 문자열 값을 결합합니다.
시퀀스 생성기 | .. | 증가하는 정수의 배열을 작성하며, 이 배열은 LHS의 숫자에서 시작해 RHS의 숫자로 끝납니다(예: [1..4] -> [1,2,3,4]). 피연산자는 정수로 평가해야 합니다. 시퀀스 생성기는 배열 생성자 [] 내에서만 사용 가능합니다.
기타| . | 점 연산자는 리터럴 키를 사용한 오브젝트 액세스에 사용됩니다(예: $event.object.hh). *
e:* 왼쪽에 있는 표현식은 이벤트($event), 상태($state) 또는 메타데이터($instance)에서 특정 특성으로 제한됩니다.

**참고:** 
- 표현식 그룹화 및 연산자 우선순위 변경에 소괄호 ( ) 사용
- 공백이 포함된 특성 이름을 묶으려면 작은따옴표 사용(예: $event.object.'a b') 

## 맵핑 표현식 언어 확장

맵핑 표현식 언어가 $event, $state 및 $instance 변수 도입을 통해 데이터 관리 기능과 함께 사용하도록 확장되었습니다. JSON은 표현식 평가 전에 이러한 변수에 바인드됩니다. 다음 표에는 표현식에 사용하도록 정의된 해당 변수의 개요가 제공됩니다.

변수| 예제 입력 JSON     | 표현식으로서의 예제 | 용도
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | 표현식에 사용할 인바운드 이벤트의 특성을 선택하십시오. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | 표현식에 사용할 디바이스 상태의 특성을 선택하십시오. 
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | 표현식에 사용할 디바이스 또는 디바이스 유형 속성을 선택하십시오. 

또한 이러한 변수 사용을 결합하는 표현식을 정의할 수 있습니다. 다음 예제에서 *temp_adjustment* 특성은 디바이스 메타데이터에 정의되고 이벤트 측정값을 보정하는 데 사용됩니다. 특성은 하나의 맵핑에 정의되지만 여러 디바이스에 적용될 수 있습니다. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## 지원되는 JSONata 함수
{: #functions}
지원되는 JSONata 함수의 서브세트는 다음과 같습니다. 

함수 유형                   |함수                   | 설명 | 예제
------------- | ------------- | ------------- 
문자열| $substring(string, start_index[, length]) | 문자열의 하위 문자열입니다(예: *$substring("Hello World", 3, 5) => "lo Wo"*). 
문자열| $string(arg) | 인수를 문자열 값으로 캐스트합니다(예: *$string(2) => "2"*).
숫자 | $number(arg) | 가능한 경우 인수를 숫자 값으로 캐스트합니다(예: *$number(2) => 2*).
숫자 | $sum(array) | 숫자 배열의 산술 합계를 리턴합니다(예: *([1,2,3]) = 6*).
숫자 | $average(array) | 숫자 배열의 평균값을 리턴합니다(예: *([1,2,3]) = 2.0*).
부울 | $exists(expression) | 표현식의 특성이 있는 경우 *true*를 리턴하고, 없는 경우 *false*를 리턴합니다.
배열 | $count(array) | 배열 매개변수의 항목 수를 리턴합니다(예: *([1,2,3,4]) = 4*). 배열 매개변수가 배열이 아니라 다른 JSON 유형의 값인 경우, 매개변수가 값이 포함된 싱글톤 배열로 처리되며 이 함수는 *1*을 리턴합니다.
배열 | $append(array1, array2) | *array1*의 값과 *array2*의 값이 차례로 있는 배열을 리턴합니다(예: *([1,2], [3,4]) = [1,2,3,4]*). 매개변수가 배열이 아닌 경우 값이 포함된 싱글톤 배열로 처리됩니다(예: *$append("Hello", "World") => ["Hello", "World"]*).

## 배열
지정된 순서로 값의 콜렉션을 입력하려면 JSON 배열을 사용하십시오. 배열은 배열의 각 값을 인덱스 또는 위치와 연관시킵니다. 배열의 개별 값을 처리하려면 배열의 필드 이름 다음에 대괄호를 사용하여 인덱스를 지정해야 합니다. 대괄호 안에 숫자 또는 숫자로 평가되는 표현식이 있는 경우 숫자가 선택할 값의 인덱스를 나타냅니다. 숫자 배열은 인덱스로 사용될 수 있습니다(예: *["a","b","c"][[1,2]] -> ["b", "c"]*). *[1,2]* 배열은 *["a","b","c"]* 배열에서 선택할 값을 식별하는 인덱스로 사용됩니다. 

인덱스는 제로 오프셋이므로 배열 *arr*의 첫 번째 값은 *arr[0]*입니다(예: *[1,2,3][0] -> 1*). 숫자가 정수가 아닌 경우 정수로 반내림됩니다(예: *[1,2,3][0.9] -> [1]*).

배열의 끝에서부터 계수하려면 음수 인덱스를 사용하십시오(예: *[1,2,3][-1] -> 3*). 

지정된 인덱스가 배열 크기를 초과하면 아무 것도 선택되지 않습니다.

## 출력 생성
배열 생성자 또는 오브젝트 생성자를 사용하여 출력에 처리된 데이터를 표시하는 방법을 지정할 수 있습니다.

### 지원되는 배열 생성자
대괄호 [] 쌍으로 리터럴 또는 표현식의 쉼표로 구분된 목록을 묶어 배열을 생성할 수 있습니다. 쉼표는 배열 생성자 내에서 여러 표현식을 구분하는 데 사용됩니다(예: *[1, 3-1] = [1,2]*).

### 지원되는 오브젝트 생성자
{: #constructors}
쉼표로 구분된 키 값 또는 쌍이 포함된 중괄호 {} 쌍을 사용하여 출력에서 JSON 오브젝트를 생성할 수 있으며,
각 키와 값은 콜론으로 구분됩니다(예: *{key1 : value1, key2:value2}* or *{"hello" : "world"}*). 오브젝트 키는 문자열이어야 합니다.  


## 예제: 배열을 사용하여 온도 데이터 처리 및 보고
다음 절에서는 [데이터 관리 시작하기](ga_im_example.html)에 예제를 빌드하여, 배열을 사용하여 온도 측정값의 슬라이드 창을 유지보수하고 해당 창 내에 포함된 측정값의 현재 합계 또는 평균을 계산하는 방법을 표시합니다. 

슬라이드 창은 도착 순서대로 데이터를 저장합니다. 삽입된 모든 데이터를 보관하는 대신 증분 방식으로 데이터를 제거하도록 슬라이드 창을 구성할 수 있습니다. 슬라이드 창이 채워진 다음에 데이터가 삽입되면 해당 창에서 가장 오래된 데이터 항목이 제거됩니다.

다음 예제는 개수 기반 제거 정책(크기 5)으로 구성된 슬라이드 창을 표시합니다.
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

다음 예제는 [단계별 안내서](ga_im_index_scenario.html#step4)의 7단계에 표시된 논리 인터페이스 스키마 파일 구성이 *tempReadings*라는 배열을 추가하여 수정될 수 있음을 표시합니다. 배열은 마지막 5개의 이벤트를 위해 디바이스에서 전송된 마지막 5개 값이 있는 창을 유지보수하는 데 사용됩니다. *tempReadings*에 값이 저장되지 않은 경우, 배열은 [0]으로 설정되고 다음에 수신된 측정값은 5개의 측정값을 수신할 때까지 늘어나는 배열에 추가됩니다. 5개의 측정값이 수신된 후 새로 측정값이 수신되면 창에서 가장 오래된 측정값이 제거됩니다. 

**중요 참고:** *tempReadings*를 "필수"로, 그리고 배열(기본값)로 설정해야 합니다. 

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
**참고:** *$state.tempReadings* 배열은 $average 및 $sum 함수에 사용되기 전에 다시 계산됩니다. 맵핑 표현식의 순서를 제어할 수 없으므로, *tempAverage* 또는 *tempSum* 표현식이 평가될 때 배열에 현재 값이 있는지 확인하려면 배열을 다시 계산해야 합니다.

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

