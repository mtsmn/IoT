---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 리소스 레벨 액세스 제어 구성
{: #configure_RLAC}

리소스 레벨 액세스 제어를 통해 디바이스 관리를 위한 사용자 및 API 키 액세스를 제어할 수 있습니다. 리소스 그룹을 사용하여 각 사용자 또는 API 키가 관리할 수 있는 조직의 디바이스를 정의할 수 있습니다. 사용자 및 API키에 "역할 대 그룹" 쌍을 지정할 수 있으며, 이를 통해 지정된 그룹에 있는 디바이스에서 지정된 역할이 다루는 오퍼레이션만 수행하도록 정의할 수 있습니다. 리소스 레벨 액세스 제어에 대한 자세한 정보는 [리소스 레벨 액세스 제어 개요](rlac_overview.html) 및 [{{site.data.keyword.iot_short_notm}} 액세스 제어 API 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}를 참조하십시오. 

## 리소스 레벨 액세스 제어 구성 - 프로세스 플로우
{: #RLAC_process}

다음 프로세스 플로우는 일반적으로 리소스 레벨 액세스 제어 사용을 위해 사용됩니다.
1. [조직 작성](../iotplatform_overview.html#organizations).
2. [API 키](../platform_authorization.html#api-key) 및 [사용자 작성](../add_users.html#adding-new-users).
3. [리소스 그룹 작성](rlac.html#create_delete_group).
4. [사용자 및 API 키에 대한 역할 대 그룹 맵핑 지정](rlac.html#assign_roletogroup).
5. [리소스 그룹에 디바이스 추가](rlac.html#add_device).
6. [리소스 레벨 액세스 제어 사용](rlac.html#RLAC_enable).

리소스 레벨 액세스 제어는 다양한 디바이스 관련 API에 적용됩니다. 적용된 API 목록은 [리소스 레벨 액세스 제어가 적용되는 API](rlac_overview.html#RLAC_enforced_APIs)를 참조하십시오.

## 리소스 그룹 작성 및 삭제
{: #create_delete_group}

리소스 그룹은 연결 중인 게이트웨이와 무관하게 작성되고 삭제될 수 있습니다.

리소스 그룹을 작성하고 그룹 세부사항을 리턴하려면 다음 API를 사용하십시오.

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

리소스 그룹을 삭제하면 그룹에 있는 디바이스가 그룹에서 제거되지만 디바이스 자체에는 별다른 영향이 없습니다. 리소스 그룹을 삭제하려면 다음 API를 사용하십시오.

    DELETE /api/v0002/groups/{groupUid}


## 사용자 또는 API 키에 "역할 대 그룹" 쌍 지정
{: #assign_roletogroup}

사용자 또는 API 키를 디바이스 그룹으로 제한하려면 사용자 또는 API 키에 "역할 대 그룹" 쌍을 지정해야 합니다. 리소스 그룹이 없는 사용자와 API 키는 제한 없이 조직의 모든 디바이스를 관리할 수 있습니다. "역할 대 그룹" 쌍이 사용되는 경우 API 키에 하나의 역할만 있을 수 있습니다. "rolesToGroups"에 지정된 역할은 "roles"에 지정된 역할과 일치해야 합니다.

사용자에게 "역할 대 그룹" 쌍을 지정하려면 다음 API를 사용하십시오.

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



API 키에 "역할 대 그룹" 쌍을 지정하려면 다음 API를 사용하십시오.

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## 리소스 그룹에 디바이스 추가 및 이 그룹에서 디바이스 제거
{: #add_device}

"역할 대 그룹" 쌍이 있는 사용자 또는 API 키가 디바이스를 관리하려면 디바이스가 사용자 또는 API 키에 지정된 리소스 그룹의 멤버여야 합니다. 리소스 그룹에 디바이스를 추가하는 경우, 디바이스를 추가하는 그룹을 요청 경로에 지정해야 하며 추가되는 디바이스는 요청 본문에 지정해야 합니다. 동시에 리소스 그룹에 여러 디바이스를 추가하려면 다음 API를 사용하십시오.

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

리소스 그룹에서 디바이스를 제거하면 요청의 본문에 지정된 디바이스가 요청의 경로에 지정된 그룹에서 제거됩니다. 리소스 그룹에서 여러 디바이스를 제거하려면 다음 API를 사용하십시오.

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

요청 스키마 및 응답에 대한 자세한 정보는 [{{site.data.keyword.iot_short_notm}} 액세스 제어 API 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}를 참조하십시오.

## 리소스 레벨 액세스 제어 사용
{: #RLAC_enable}

조직에 대한 리소스 레벨 액세스 제어를 사용하면 사용자 및 API 키를 지정된 리소스 그룹으로 제한할 수 있습니다. 리소스 레벨 액세스 제어를 사용하려면 다음 API를 사용하여 조직 레벨 구성 플래그를 사용으로 설정해야 합니다.

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## 리소스 그룹 찾기
{: #find_group}

리소스 그룹에는 연관된 검색 태그가 있을 수 있습니다. 검색 태그를 사용하면 다음 API를 사용하여 리소스 그룹의 세부사항을 검색할 수 있습니다.

    GET /api/v0002/groups

이 API는 사용된 검색 태그와 연관된 리소스 그룹을 리턴합니다. 검색 태그가 지정되지 않으면 모든 리소스 그룹이 리턴됩니다. 

사용자에게 지정된 리소스 그룹의 고유 ID을 찾으려면 다음 API를 사용하십시오.

    GET /api/v0002/authorization/users/{userUid}

API 키에 지정된 리소스 그룹의 고유 ID를 찾으려면 다음 API를 사용하십시오.

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## 리소스 그룹 조회
{: #query_group}

다양한 매개변수으로 리소스 그룹을 조회하여 그룹에 있는 모든 디바이스의 전체 특성, 그룹에 있는 모든 디바이스의 고유 ID 또는 리소스 그룹의 특성을 리턴할 수 있습니다.

지정된 리소스 그룹에 있는 모든 디바이스의 전체 특성을 리턴하려면 다음 API를 사용하십시오.

    GET /api/v0002/bulk/devices/{groupUid}

리소스 그룹의 멤버에 대한 고유 ID만 리턴하려면 다음 API를 사용하십시오.

    GET /api/v0002/bulk/devices/{groupUid}/ids

이름, 설명, 검색 태그 및 경로에 지정된 고유 ID를 포함한 리소스 그룹의 특성을 리턴하려면 다음 API를 사용하십시오.

    GET /api/v0002/groups/{groupUid}

이 API는 리소스 그룹의 멤버 목록을 리턴하지 않습니다.

## 그룹 특성 업데이트
{: #update_group}

그룹의 특성을 업데이트하려면 다음 API를 사용하십시오.

    PUT /api/v0002/groups/{groupId}
