---

copyright:
  years: 2017
lastupdated: "2017-10-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 리소스 레벨 액세스 제어 개요(베타)
{: #RLAC_overview}

리소스 레벨 액세스 제어를 통해 디바이스 관리를 위한 사용자 및 API 키 액세스를 제어할 수 있습니다. 리소스 그룹을 사용하여 각 사용자 또는 API 키가 관리할 수 있는 조직의 디바이스를 지정할 수 있습니다. 리소스 레벨 액세스 제어 구성 방법에 대한 정보는 [리소스 레벨 액세스 제어 구성](rlac.html#configure_RLAC)을 참조하십시오.

**중요:** {{site.data.keyword.iot_full}} 리소스 레벨 액세스 제어 기능은 제한된 베타 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

## 리소스 레벨 액세스 제어 개념 
{: #RLAC_concepts}

디바이스 그룹은 {{site.data.keyword.iot_short_notm}}의 리소스 레벨 액세스 제어의 일부입니다. 디바이스 그룹 기능을 사용하여 직원이 해당 역할에 따라 액세스할 수 있는 여러 디바이스를 관리할 수 있습니다. 디바이스 그룹을 구현하기 전에 디바이스 그룹의 인텐트와 범위를 판별하고 기록하여 솔루션의 효율성을 높이십시오. 

IoT 솔루션의 일부로 디바이스 그룹을 구현하여 사용자 또는 애플리케이션이 조직과 연관된 모든 디바이스가 아닌 다른 디바이스의 서브세트에 액세스하도록 할 수 있습니다. 디바이스 그룹의 효율성을 높이기 위해 하나의 개인에게 많은 디바이스에 대한 액세스 권한이 있어야 하는 경우 디바이스 그룹을 사용하십시오.

예를 들어, 많은 현장 엔지니어 및 운영 요원을 고용하는 회사가 영국 전체에서 IoT 솔루션을 구현하려고 할 수 있습니다. 회사는 69개 도시 각각에 대한 하나의 디바이스 그룹, 지리적 지역 각각에 대한 9개의 디바이스 그룹 및 영국 전체에 대한 하나의 디바이스 그룹을 구성합니다. 디바이스는 이러한 그룹에 할당되며, 그룹은 15명의 현장 엔지니어 및 운영 요원에게 지정되고 이 중 일부는 둘 이상의 그룹에 대한 액세스 권한을 갖습니다. 하나 또는 두 디바이스를 담당하는 사용자는 {{site.data.keyword.iot_short_notm}}의 외부에 있지만 전체 IoT 솔루션을 보완하는 모바일 애플리케이션 및 엔터프라이즈 아키텍처를 계속 사용할 수 있습니다.

[Internet of Things 영역의 질문(![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘"))](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window}에서 디바이스 그룹에 대해 질문할 수 있습니다.

## 액세스 제어에 대한 리소스 그룹 한계

리소스 그룹 한계는 리소스 레벨 액세스 제어 기능이 효율적으로 작동되도록 적용됩니다. 한계는 하나의 주체에 지정된 리소스 그룹의 수, 그룹 내 디바이스의 수 및 리소스가 속하는 그룹의 수를 제한합니다.

다음 한계가 적용됩니다.

 - 최대 10개의 리소스 그룹은 API 키, 사용자 또는 게이트웨이에 지정할 수 있습니다.
 - 리소스 그룹에는 최대 300개의 리소스가 있을 수 있습니다.
 - 리소스는 최대 10개의 그룹에 속할 수 있습니다.

## 리소스 레벨 액세스 제어 용어

다음 섹션을 사용하여 리소스 레벨 액세스 제어 기능에 사용되는 용어를 이해할 수 있습니다. 

### 주체
{: #subjects}

주체는 플랫폼 액세스를 요청하는 인증된 엔티티이며, 권한을 부여받아야 합니다. 다음 유형의 주체가 유효합니다.

- *사용자*: 애플리케이션을 사용하는 일반 사용자입니다. 구성원은 계정에 만기 날짜가 없는 사용자이고 게스트는 계정에 만기 날짜가 있는 사용자입니다.
- *디바이스*: {{site.data.keyword.iot_short_notm}} 플랫폼에 액세스하고 디바이스 유형을 사용하는 실제 디바이스입니다.
- *게이트웨이*: {{site.data.keyword.iot_short_notm}} 플랫폼에 액세스하고 게이트웨이 유형을 사용하는 실제 디바이스입니다.
- *애플리케이션*: 권한 부여 및 액세스 제어를 위해 API 키를 사용하여 {{site.data.keyword.iot_short_notm}} 플랫폼에 액세스하는 애플리케이션 또는 서비스입니다.

### 리소스
{: #resources}

리소스는 주체가 조치를 수행하는 엔티티입니다. 주체는 리소스에 대한 권한 부여된 플랫폼 액세스를 요청합니다. 디바이스는 리소스 레벨 액세스 제어 베타 릴리스에서 지원되는 유일한 리소스입니다.

### 조치
{: #actions}

조치는 주체가 수행하는 것이며, 대부분 작성, 읽기, 업데이트, 삭제 또는 실행입니다. 오퍼레이션은 리소스에서 조치를 그룹화하는 데 사용됩니다.

### 애플리케이션
{: #applications}

애플리케이션은 플랫폼에 알려지지 않은 주체(예: 가전 제품을 제어하는 모바일 애플리케이션) 대신 작동되며, 플랫폼에 알려져 있거나 알려지지 않은 실제 디바이스 또는 시뮬레이션된 디바이스 대신 작동될 수 있습니다. 또한 다른 종류의 디바이스 대신 작동되지 않는 실제 서버 측 애플리케이션일 수도 있습니다.

인증이 제공되며 {{site.data.keyword.iot_short_notm}} 플랫폼에 대한 액세스는 API 키 또는 토큰을 기반으로 제공됩니다.

### 오퍼레이션
{: #operations}

오퍼레이션에는 사용자 인터페이스 및 프로그래밍 방식의 인터페이스(예: REST 서비스 및 MQTT 인터페이스)를 사용하여 특정 리소스 유형에서 실행되는 하나 이상의 조치가 있습니다. 이러한 조치는 구성원, 애플리케이션 및 디바이스를 포함한 여러 주체가 사용합니다. 오퍼레이션은 오퍼레이션 ID(OID)로 식별됩니다.

### 역할
{: #roles}

역할은 오퍼레이션을 사용하는 권한을 제공하는 데 사용되는 오퍼레이션 세트입니다. 주체는 여러 역할을 가질 수 있으며 역할은 주체의 속성입니다.

**역할 형식**

0..n 역할의 배열

    { "roles": [ "PD_ADMIN_APP" ] }

**역할 대 그룹 형식**

0..n <string, list<string>> 쌍의 맵

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### 권한
{: #permissions}

권한은 주체가 리소스 또는 리소스 그룹에서 오퍼레이션을 실행할 수 있게 합니다.

### 리소스 그룹
{: #resource_groups}

리소스 그룹은 리소스로서의 디바이스 콜렉션입니다. 디바이스를 리소스 그룹에 추가할 수 있으며 리소스 그룹을 역할 대 그룹 쌍의 주체에 지정할 수 있습니다. 이러한 관계를 사용하면 주체가 특정 디바이스 그룹에서 지정된 역할에 대한 오퍼레이션을 수행할 수 있습니다.

### 사용자
{: #users}

사용자는 조직의 구성원인 ID를 사용하여 {{site.data.keyword.iot_short_notm}} 플랫폼 대시보드에 로그인합니다. 사용자는 HTTP API의 특정 세트를 호출할 수 있도록 권한을 부여하는 지정된 역할입니다. 사용자 역할에 대한 자세한 정보는 [사용자 역할의 액세스 레벨](roles_access.html#levels-of-access-for-user-roles)을 참조하십시오.

### API 키
{: #API_keys}

API 키는 {{site.data.keyword.iot_short_notm}} 플랫폼 HTTP API를 호출하는 데 사용되는 토큰입니다. API 키는 HTTP API의 특정 세트를 호출할 수 있도록 권한을 부여하는 지정된 역할입니다. API 키 역할에 대한 자세한 정보는 [애플리케이션 역할의 액세스 레벨](app_roles_access.html#levels-of-access-for-application-roles)을 참조하십시오.

## 리소스 레벨 액세스 제어가 적용되는 API
{: #RLAC_enforced_APIs}
리소스 레벨 액세스 제어가 사용되면 이 섹션에 포함된 디바이스 관련 API는 호출자가 지정된 디바이스에 액세스할 수 있는 경우에만 작동됩니다.

### 디바이스 API(개별)

**유형의 디바이스 나열**

    GET /api/v0002/device/types/${typeId}

적절한 그룹에 속한 디바이스의 서브세트만 리턴됩니다.

**디바이스 가져오기**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스 관리 세부사항 가져오기**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**게이트웨이가 등록한 디바이스 가져오기**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

호출자가 게이트웨이에 액세스할 수 없는 경우 404 오류를 리턴합니다. 적절한 그룹에 속한 디바이스의 서브세트만 리턴됩니다. 디바이스 및 게이트웨이는 호출자 그룹에 있어야 합니다.

**디바이스 업데이트**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스 삭제**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

### 디바이스 API(벌크)

**디바이스 나열**

    GET /api/v0002/bulk/devices

적절한 그룹에 속한 디바이스의 서브세트만 리턴됩니다.

**디바이스 삭제**

    DELETE /api/v0002/bulk/devices/remove

적절한 그룹에 속한 디바이스의 서브세트만 삭제됩니다. 액세스할 수 없는 디바이스는 여전히 성공 = true와 함께 리턴됩니다.

**디바이스 업데이트:**

    PUT /api/v0002/bulk/devices/update

### 디바이스 관리 API

**요청 시작**

    POST /api/v0002/mgmt/requests

호출자가 디바이스에 액세스할 수 없는 경우 실패합니다.

**요청 삭제**

    DELETE /api/v0002/mgmt/requests/${requestId}

호출자가 디바이스에 액세스할 수 없는 경우 실패합니다.

**요청 보기**

    GET /api/v0002/mgmt/requests/${requestId}

**요청 나열**

    GET /api/v0002/mgmt/requests

**하나의 요청 가져오기**

    GET /api/v0002/mgmt/requests/${requestId}

**요청 디바이스 세부사항 가져오기**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**하나의 디바이스에 대한 요청 디바이스 세부사항 가져오기**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### 문제점 판별 API

**디바이스에 대한 연결 로그**

    GET /api/v0002/logs/connection

디바이스가 액세스 불가능한 경우 빈 배열을 리턴합니다.

**디바이스 오류 코드 추가**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스 오류 코드 나열**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스 오류 코드 지우기**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스 진단 로그 추가**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스 진단 로그 나열**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**하나의 디바이스 진단 로그 가져오기**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스 진단 로그 지우기**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**하나의 디바이스 진단 로그 삭제**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

###마지막 이벤트 캐시 API

**디바이스에 대한 이벤트 가져오기**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.

**디바이스에 대한 이벤트 가져오기**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

호출자가 디바이스에 액세스할 수 없는 경우 404 오류를 리턴합니다.
