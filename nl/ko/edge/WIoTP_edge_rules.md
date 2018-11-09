---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} 에지 라우팅 규칙 구성(미리보기)
{: #edge_rules}

{{site.data.keyword.iot_full}} 에지 라우팅 규칙은 에지 노드 내에서 메시지가 전달되는 방법을 정의합니다. 

**중요:** {{site.data.keyword.iot_full}} 에지 기능은 제한된 미리보기 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

에지 노드에 대해 여러 라우팅 규칙을 정의할 수 있습니다. 규칙은 논리 디바이스나 애플리케이션이 에지 노드에 직접 공개하는 각 메시지마다 평가됩니다. 클라우드에서 에지 노드에 공개된 메시지는 항상 로컬로 공개되며 라우팅의 영향을 받지 않습니다. 

## 라우팅 규칙 형식
{: #edge_rules_format}

라우팅 규칙은 다음 형식의 JSON 오브젝트로 정의됩니다. 

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

라우팅 규칙은 에지 노드의 `/etc/wiotp-edge` 폴더에 저장된 `routing.json` 파일에 있는 JSON 오브젝트의 배열로서 지정됩니다. 

각 라우팅 규칙의 특성은 다음과 같이 정의됩니다. 

특성      |유형     |설명       
------------- | -----------| -----------
rule_id| 정수, 필수 |규칙의 ID입니다. 라우팅 규칙은 rule_id에 따라 오름차순으로 평가됩니다. rule_id가 동일한 규칙은 2개 이상 있을 수 없습니다. 
matching_filter | 문자열, 필수 | 공개된 메시지에 규칙을 적용해야 하는지 여부를 평가하는 데 사용됩니다. 이는 MQTT 구독의 형식이어야 합니다. MQTT 단일 레벨(+) 및 다중 레벨(#) 와일드카드가 지원됩니다. 이 필터는 메시지가 공개된 주제에 대해 평가됩니다. 이 특성은 {{site.data.keyword.iot_short_notm}} 애플리케이션 주제 형식으로 정의되며 `/type/deviceType/id/deviceId` 필드를 포함합니다. 예: `"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
Destination| 문자열, 필수 |메시지를 전달해야 하는지 여부를 정의합니다. `"CLOUD"`, `"IM"`(메시지를 정보 관리 컴포넌트에 전달함) 또는 `"LOCAL"`(에지 노드에서 로컬로 메시지를 전달함) 값 중에서 하나를 설정하십시오. 예: `"destination" : "CLOUD"`
continue_matching | 부울, 선택사항, 기본값 = true |규칙이 한 번 일치하는 경우 이 규칙을 사용하여 추가 메시지를 평가해야 하는지 여부를 지정합니다. 이 특성은 겹치는 라우팅 규칙이 존재할 때 라우팅을 보다 잘 제어할 수 있도록 합니다. 
forward_skip_levels |정수, 선택사항, 기본값 = 0) |메시지가 재공개될 때 제거되어야 할 원래 메시지 주제의 레벨 수를 지정합니다. 기본값은 0이며, 이는 원래 주제가 유지됨을 의미합니다. 예를 들어, `forward_skip_levels`가 `1`로 설정되어 있으며 메시지가 주제 `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json`에서 공개되는 경우에는 메시지가 주제 `iot-2/type/T1/id/D1/evt/E1/fmt/json`에서 재공개됩니다. 
store|부울, 선택사항, 기본값 = true) |이 규칙에 의해 라우팅되는 메시지에 저장 후 전달(store-and-forward) 기능을 적용해야 하는지 여부를 정의합니다. 이 특성은 대상 특성이 `"CLOUD"`로 설정된 경우에만 적용됩니다. 이 특성이 거짓(false)으로 설정되면 에지 노드와 클라우드의 연결이 끊어질 때 규칙과 일치하는 메시지가 저장되지 않습니다. 

## routing.json 파일의 예
{: #edge_rules_example}

다음 예제에서는 세 개의 규칙이 구성됩니다.
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

첫 번째 규칙은 `AlarmEvent` 유형의 모든 이벤트를 클라우드에 직접 라우팅합니다. 

두 번째 규칙은 `sendToCloud/iot-2`로 시작하는 주제에서 공개된 모든 이벤트를 클라우드에 직접 라우팅합니다. 메시지는 `iot-2`로 시작하는 주제에서 전달되며 `sendToCloud/` 접두부를 포함하지 않습니다. 

세 번째 규칙은 기타 모든 메시지를 로컬 MQTT 브로커로 라우팅하는 기본 규칙입니다. 그리고 이러한 메시지는 일치하는 구독의 모든 로컬 이용자에 의해 사용될 수 있습니다. 에지 노드의 설치에는 모두를 로컬로 라우팅하는 기본 규칙이 포함됩니다(`/etc/wiotp-edge/routing.json`에서). 

## 라우팅 규칙 편집
{: #edge_rules_edit}

`/etc/wiotp-edge/routing.json` 파일에서 직접 규칙을 편집할 수 있습니다. 파일 편집이 완료되면 변경사항을 저장한 후에 `docker ps`를 실행하여 에지-커넥터(edge-connector) 컨테이너의 ID를 판별하십시오. 해당 ID를 사용하여 Docker 컨테이너를 다시 시작하십시오. 

## 라우팅 규칙 문제점 해결
{: #edge_rules_ts}

`routing.json`의 변경 이후에도 에지-커넥터(edge-connector) 컨테이너가 계속 다시 시작되는 경우, 이는 라우팅 규칙 파일에 구문 오류가 포함되어 있음을 의미합니다. 오류 메시지를 보려면 로그 파일 `/var/wiotp-edge/trace/edgeConnector_trace.log`를 확인하십시오. 
