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

# 配置资源级别访问控制
{: #configure_RLAC}

资源级别的访问控制使您能够控制对管理设备的用户和 API 密钥访问权。您可以使用资源组来定义每个用户或 API 密钥可管理的组织中的设备。可以为用户和 API 密钥分配“角色到组”对，这可定义他们只能执行在指定组中的设备上指定的角色所涵盖的操作。有关资源级别访问控制的更多信息，请参阅[资源级别访问控制概述](rlac_overview.html)和[{{site.data.keyword.iot_short_notm}}访问控制 API 文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}。

## 配置资源级别访问控制 - 过程流
{: #RLAC_process}

以下过程流通常用于启用和使用资源级别访问控制：
1. [创建组织](../iotplatform_overview.html#organizations)。
2. [创建用户](../add_users.html#adding-new-users)和 [API 密钥](../platform_authorization.html#api-key)。
3. [创建资源组](rlac.html#create_delete_group)。
4. [为用户和 API 密钥指定角色到组的映射](rlac.html#assign_roletogroup)。
5. [将设备添加到资源组](rlac.html#add_device)。
6. [启用资源级别访问控制](rlac.html#RLAC_enable)。

资源级别访问控制在各种设备相关 API 中强制实施。有关受影响的 API 的列表，请参阅[强制实施资源级别访问控制的 API](rlac_overview.html#RLAC_enforced_APIs)。

## 创建和删除资源组
{: #create_delete_group}

资源组可以独立于连接网关进行创建和删除。

要创建资源组并返回组详细信息，请使用以下 API：

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

删除资源组时，组中的设备将从组中除去，但这些设备本身不会受到影响。要删除资源组，请使用以下 API：

    DELETE /api/v0002/groups/{groupUid}


## 将“角色到组”对分配给用户或 API 密钥
{: #assign_roletogroup}

需要将“角色到组”对分配给用户或 API 密钥，以将用户或 API 密钥限制为一组设备。没有任何资源组的用户和 API 密钥可以在不受限制的情况下管理组织中的所有设备。如果使用“角色到组”对，那么 API 密钥只能具有一个角色。在“rolesToGroups”中指定的角色必须与“roles”中指定的角色相匹配。

要将“角色到组”对分配给用户，请使用以下 API：

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



要将“角色到组”对分配给 API 密钥，请使用以下 API：

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## 向资源组添加设备以及从资源组中除去设备
{: #add_device}

设备必须是分配给用户或 API 密钥的资源组的成员，具有“角色到组”对的用户或 API 密钥才可以管理该设备。将设备添加到资源组时，必须在请求的路径中指定要将设备添加到的组，并且必须在请求的主体中指定添加的设备。要同时向某个资源组添加多个设备，请使用以下 API：

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

从资源组中除去设备时，将从请求的路径中指定的组中除去请求主体中指定的设备。要从资源组中除去多个设备，请使用以下 API：

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

有关请求模式和响应的更多信息，请参阅 [{{site.data.keyword.iot_short_notm}}访问控制 API 文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}。

## 启用资源级别访问控制
{: #RLAC_enable}

对组织启用资源级别访问控制将使用户和 API 密钥限制在其分配的资源组。要使用资源级别访问控制，必须使用以下 API 来启用组织级别的配置标志：

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## 查找资源组
{: #find_group}

资源组可以具有关联的搜索标签。搜索标签可用于通过以下 API 来检索资源组的详细信息：

    GET /api/v0002/groups

此 API 返回与所使用搜索标记相关联的资源组。如果未指定任何搜索标签，那么将返回所有资源组。

要查找分配给用户的资源组的唯一标识，请使用以下 API：

    GET /api/v0002/authorization/users/{userUid}

要查找分配给 API 密钥的资源组的唯一标识，请使用以下 API：

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## 查询资源组
{: #query_group}

可以通过各种参数查询资源组，以返回组中所有设备的完整属性、组中所有设备的唯一标识或资源组的属性。

要返回指定资源组中所有设备的完整属性，请使用以下 API：

    GET /api/v0002/bulk/devices/{groupUid}

要仅返回资源组中成员的唯一标识，请使用以下 API：

    GET /api/v0002/bulk/devices/{groupUid}/ids

要返回资源组的属性（包括路径中指定的名称、描述、搜索标记和唯一标识），请使用以下 API：

    GET /api/v0002/groups/{groupUid}

此 API 不会返回资源组的成员列表。

## 更新组属性
{: #update_group}

要更新组的属性，请使用以下 API：

    PUT /api/v0002/groups/{groupId}
