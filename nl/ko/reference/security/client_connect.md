---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 클라이언트 연결 모니터링(베타)
{: #connect_status}

{{site.data.keyword.iot_full}}을 사용하여 클라이언트의 연결 상태를 조회하고 모니터할 수 있습니다. 클라이언트가 연결되어 있는지 여부를 실시간으로 알 수 있으며 연결 문제를 해결할 수 있습니다.
{:shortdesc}

예를 들어, 클라이언트가 마지막으로 활성화된 시점과 연결이 끊어진 이유를 파악할 수 있습니다. 또는 문제를 해결할 수 있도록 이전에 연결되지 않은 모든 클라이언트를 볼 수도 있습니다. 

**중요:** 클라이언트 연결 모니터링 기능은 제한된 베타 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

## 클라이언트 연결 상태 조회
{: #query_status}

클라이언트 연결 상태 API를 사용하면 {{site.data.keyword.iot_short_notm}}에 연결된 클라이언트의 연결 상태를 검색하고 조회할 수 있습니다. 

연결 상태를 조회할 때 다음 정보를 볼 수 있습니다.

 - 연결 상태(연결됨 또는 연결 끊김)
 - 마지막 연결 시간(ISO 8601 형식)
 - 마지막 연결 끊김 시간(ISO 8601 형식)
 - 마지막 활동 이벤트(예: MQTT 공개 또는 MQTT 구독)
 - 마지막 활동 시간(ISO 8601 형식)
 - 클라이언트의 연결이 끊어진 경우 연결 끊김을 유발한 엔티티(클라이언트, 서버 또는 네트워크)
 - 클라이언트의 연결이 끊어진 경우 연결 끊김에 대한 MQTT 이유 코드 

**예제**

단일 클라이언트의 클라이언트 연결 상태 검색:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

모든 클라이언트의 연결 상태 검색:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

연결된 모든 클라이언트 검색:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

지난 2일 내에 활성 상태인 모든 클라이언트 검색:

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**참고:** 클라이언트가 연결하는 시간과 API의 연결 상태가 올바른 상태를 반영하는 시간 사이에는 약간의 지연이 있을 수 있습니다. 

사용 가능한 모든 조회 필터를 포함하여 API에 대한 자세한 정보는 [클라이언트 연결 상태 API Swagger 문서 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window}를 참조하십시오. 

## 클라이언트 연결 상태 모니터링
{: #monitor_status}

 모든 클라이언트에는 사용자가 클라이언트에 대한 활성 연결 상태 업데이트 수신을 구독할 수 있도록 하는 모니터 주제가 있습니다. 자세한 정보는 [디바이스 상태 메시지 구독](../../applications/mqtt.html#subscribe_device_commands)을 참조하십시오. 

## 클라이언트 연결 문제점 해결

연결 로그 API를 사용하면 디바이스에 대한 연결 로그 이벤트 목록을 검색하여 연결 문제를 진단할 수 있습니다. 항목은 연결 성공, 연결 시도 실패, 의도적 연결 끊기 및 서버가 실행한 연결 끊기를 기록합니다. 

자세한 정보는 [연결 로그 API Swagger 문서 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window}를 참조하십시오. 
