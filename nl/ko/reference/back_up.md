---

copyright:

years: 2017, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 데이터 백업
{: #back_up}

{{site.data.keyword.iot_full}}에 대한 데이터 백업 전략을 이해하려면 다음 정보를 사용하십시오.

## 백업되는 데이터 유형

다음 유형의 클라이언트 데이터는 현재 {{site.data.keyword.iot_short_notm}} 전략의 일부로 백업됩니다.

- 조직 정보
- 디바이스 정보
- API 키 및 토큰
- 사용자 정보
- 시작된 요청의 히스토리가 포함된, 디바이스 관리 요청의 모든 레코드 - 예: 요청의 현재 상태
- 사용자 정의 디바이스 관리 요청 번들의 정의

**참고:** 조직의 모든 데이터는 서비스 디프로비저닝 이후 14일 동안 보관됩니다. 조직을 복원하려면 14일 이내에 지원 팀에 문의하십시오. 

## 백업되지 않는 데이터 유형

다음 유형의 데이터는 {{site.data.keyword.iot_short_notm}}에 백업되지 않습니다.

- 디바이스 이벤트
- 임시 메시징 상태(예: 인플라이트 데이터)
- 디바이스 관리 요청의 일부로 송수신되는 MQTT 메시지
<!-- - Analytics rules and alert configuration -->

## 데이터의 백업 빈도 및 저장 위치

데이터는 24시간마다 한 번씩 백업됩니다.

데이터는 기본 {{site.data.keyword.iot_short_notm}} 서비스에서 오프사이트에 저장되어 지리적 중복성을 제공하고 중요한 인시던트가 발생하는 경우 서비스를 복원할 수 있습니다. 데이터 복원이 필요한 경우 클라이언트에게 연락합니다. 모든 데이터는 암호화되고 로컬 데이터 보호 요구사항을 준수합니다.

다음 오프사이트 위치는 현재 데이터 백업에 사용됩니다.

 위치                   |백업 위치                      
------------- | -------------
IBM Cloud 미국 남부(댈러스)|워싱턴
IBM Cloud 영국(런던) |프랑크푸르트
IBM Cloud 독일(프랑크푸르트) |런던
IBM Cloud Dedicated |{{site.data.keyword.iot_short_notm}} 데디케이티드 주문 시 고객 요청에 따라

**참고:** 향후 위치는 데이터 개인정보 보호법을 반영하기 위해 변경될 수 있습니다(예: EU 데이터 주권 규칙에 대한 브렉시트의 잠재적 영향).
