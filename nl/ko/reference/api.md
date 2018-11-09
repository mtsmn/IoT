---

copyright:

years: 2017, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# API
{: #api_overview}

다양한 API를 {{site.data.keyword.iot_full}}에 연결된 디바이스, 게이트웨이 및 애플리케이션의 코드를 개발하는 데 사용할 수 있습니다.

HTTP API는 HTTP 기본 인증으로 보호됩니다. 대시보드를 사용하여 API 키를 생성하는 경우 키와 인증 토큰이 제공됩니다. API 키와 토큰에 대한 자세한 정보는 [API 키 연결](../platform_authorization.html#api-key)을 참조하십시오.


## HTTP API 정보
{: #api_about}

조직을 등록하면 6자의 조직 ID를 받게 되며 이 ID는 HTTP API 호출의 호스트 이름에 필요합니다. 다음 주소에서 조직의 기본 URL에 액세스할 수 있습니다.

https://<**orgId**>.internetofthings.ibmcloud.com/api/v0002

애플리케이션 API에 대한 요청을 인증하려면 사용자 이름을 API 키로 설정하고 비밀번호를 인증 토큰으로 설정하십시오.

Messaging API의 경우 다음 주소를 사용하십시오.

https://<**orgId**>.messaging.internetofthings.ibmcloud.com/api/v0002

## HTTP API
{: #api_http}

API                     |용도       
------------- | -------------
[조직 관리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/orgAdmin.html){: new_window} |조직을 구성하고(디바이스 작성 및 삭제 포함), 사용법 및 서비스 상태를 확인하고, 디바이스 연결 문제점을 진단합니다.
[보안 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window} |사용자의 인증 및 권한 부여, API 키 및 디바이스를 관리합니다. 
[정보 관리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html){: new_window} |디바이스 위치를 가져오고 업데이트할 뿐만 아니라 디바이스 이벤트 데이터에 액세스하고 해당 위치에 대한 날씨 정보를 가져옵니다. 
[데이터 관리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){: new_window}   |{{site.data.keyword.iot_short_notm}}에서 나가고 들어오는 데이터를 구성하고 통합합니다.
[디바이스 관리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/deviceMgmt.html){: new_window} |디바이스 관리 프로토콜을 사용하여 관리 디바이스와 상호작용합니다.
[메시징 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}   |HTTP를 사용하여 이벤트를 공개하고 명령을 전송합니다.
[위험 관리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/riskmgmt.html){: new_window}   |위험성 관리 정책 및 보고서를 관리합니다.

## 확장 HTTP API
{: #api_extension}

API                     |용도       
------------- | -------------
[AT&T 확장 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/ext-atnt.html){: new_window} |AT&T 디바이스를 관리합니다.
[Jasper 확장 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/ext-jasper.html){: new_window} |Jasper 디바이스를 관리합니다.
[Orange 확장 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/ext-orange.html){: new_window} |{{site.data.keyword.iot_short_notm}} 조직에 연결되어 있고 Orange SIM 카드가 설치된 디바이스에서 SIM 카드 데이터를 확인합니다.

## 베타 HTTP API
{: #api_beta}

API                     |용도       
------------- | -------------
[삭제된 디바이스 복원 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/restore-device-beta.html){: new_window}   |디바이스를 실수로 삭제한 경우 14일 이내에 복원할 수 있습니다.
