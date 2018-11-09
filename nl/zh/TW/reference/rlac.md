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

# 配置資源層次存取控制
{: #configure_RLAC}

資源層次存取控制可讓您控制使用者及 API 金鑰存取權來管理裝置。您可以使用資源群組來定義組織中每位使用者或每個 API 金鑰可管理的裝置。使用者及 API 金鑰可獲指派「角色對群組」配對，以定義他們只能執行已指定群組中裝置上已指定角色所涵蓋的作業。如需資源層次存取控制的相關資訊，請參閱[資源層次存取控制概觀](rlac_overview.html)及 [{{site.data.keyword.iot_short_notm}} 存取控制 API 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}。

## 配置資源層次存取控制 - 程序流程
{: #RLAC_process}

下列程序流程通常用於啟用及使用資源層次存取控制：
1. [建立組織](../iotplatform_overview.html#organizations)。
2. [建立使用者](../add_users.html#adding-new-users)及 [API 金鑰](../platform_authorization.html#api-key)。
3. [建立資源群組](rlac.html#create_delete_group)。
4. [為使用者及 API 金鑰指派角色對群組對映](rlac.html#assign_roletogroup)。
5. [將裝置新增至資源群組](rlac.html#add_device)。
6. [啟用資源層次存取控制](rlac.html#RLAC_enable)。

在各種裝置相關 API 中強制執行資源層次存取控制。如需受影響的 API 清單，請參閱[已強制執行資源層次存取控制的 API](rlac_overview.html#RLAC_enforced_APIs)。

## 建立及刪除資源群組
{: #create_delete_group}

您可以在不使用連接閘道的情況下建立及刪除資源群組。

若要建立資源群組，並傳回群組詳細資料，請使用下列 API：

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

刪除資源群組時，會從群組中移除該群組中的裝置，但不影響裝置本身。若要刪除資源群組，請使用下列 API：

    DELETE /api/v0002/groups/{groupUid}


## 將「角色對群組」配對指派給使用者或 API 金鑰
{: #assign_roletogroup}

需要將「角色對群組」配對指派給使用者或 API 金鑰，才能將使用者或 API 金鑰限制為裝置群組。沒有任何資源群組的使用者及 API 金鑰，可以無限制地管理其組織中的所有裝置。如果使用「角色對群組」配對，則 API 金鑰只能有一個角色。"rolesToGroups" 中所指定的角色必須符合 "roles" 中所指定的角色。

若要將「角色對群組」配對指派給使用者，請使用下列 API：

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



若要將「角色對群組」配對指派給 API 金鑰，請使用下列 API：

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## 將裝置新增至資源群組以及從中移除裝置
{: #add_device}

裝置必須是指派給使用者或 API 金鑰的資源群組成員，具備「角色對群組」配對的使用者或 API 金鑰才能管理裝置。將裝置新增至資源群組時，必須在要求的路徑中指定要新增裝置的群組，而且必須在要求內文中指定所新增的裝置。若要同時將多個裝置新增至資源群組，請使用下列 API：

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

從資源群組中移除裝置時，會從要求路徑中所指定的群組中移除要求內文中所指定的裝置。若要移除資源群組中的多個裝置，請使用下列 API：

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

如需要求綱目及回應的相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} 存取控制 API 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}。

## 啟用資源層次存取控制
{: #RLAC_enable}

啟用組織的資源層次存取控制，可將使用者及 API 金鑰限制為其指派的資源群組。若要使用資源層次存取控制，您必須使用下列 API 來啟用組織層次配置旗標：

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## 尋找資源群組
{: #find_group}

資源群組可以有關聯的搜尋標籤。使用下列 API，可以使用搜尋標籤來擷取資源群組的詳細資料：

    GET /api/v0002/groups

此 API 會傳回與所使用搜尋標籤相關聯的資源群組。如果未指定搜尋標籤，則會傳回所有資源群組。

若要尋找已指派給使用者之資源群組的唯一 ID，請使用下列 API：

    GET /api/v0002/authorization/users/{userUid}

若要尋找已指派給 API 金鑰之資源群組的唯一 ID，請使用下列 API：

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## 查詢資源群組
{: #query_group}

您可以使用各種參數來查詢資源群組，以傳回群組中所有裝置的完整內容、群組中所有裝置的唯一 ID，或是資源群組的內容。

若要傳回所指定資源群組中所有裝置的完整內容，請使用下列 API：

    GET /api/v0002/bulk/devices/{groupUid}

若只要傳回資源群組成員的唯一 ID，請使用下列 API：

    GET /api/v0002/bulk/devices/{groupUid}/ids

若要傳回資源群組的內容（包括路徑中所指定的名稱、說明、搜尋標籤及唯一 ID），請使用下列 API：

    GET /api/v0002/groups/{groupUid}

此 API 不會傳回資源群組成員清單。

## 更新群組內容
{: #update_group}

若要更新群組的內容，請使用下列 API：

    PUT /api/v0002/groups/{groupId}
