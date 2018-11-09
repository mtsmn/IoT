---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 閘道存取控制
{: #gateway-access-control}

閘道裝置是一種特殊化的裝置類別，而且可以代表其他裝置作業。閘道資源群組定義組織內每一個閘道可以代表作業的裝置。閘道角色可判定閘道向 Watson IoT Platform 登錄裝置的能力。
{: #shortdesc}

您可以使用資源群組，讓閘道代表組織的一部分裝置作業。閘道只能發佈或訂閱自己的訊息，以及代表其資源群組中的裝置來發佈或訂閱訊息。 

建立新的閘道時，也會建立預設資源群組，並將其指派給閘道。無法從閘道移除預設資源群組。沒有任何資源群組的現有閘道可以繼續代表組織中的所有裝置作業。

如需使用 API 來發佈閘道裝置事件的相關資訊，請參閱[閘道裝置的 HTTP API](gw_api.html)。

## 將角色指派給某閘道
{: #gw_roles}

若要將閘道指派給資源群組，您必須先將閘道指派給角色。依預設，新建立的閘道會獲指派*特許閘道* 角色，並指派給預設資源群組。具有特許角色的閘道可以自動登錄要新增至預設資源群組的裝置。

具有*標準閘道* 角色的閘道無法自動登錄裝置。 

閘道在獲指派資源群組之後，就只能代表該資源群組中的裝置及本身作業，即使其角色變更。

如需閘道角色的相關資訊，請參閱[閘道角色](../roles_index.html#gateway_roles)。

若要將角色指派給閘道，請使用下列 API，其中 *${clientID}* 是 URL 編碼 ClientID，格式為 *d:${orgId}:${typeId}:${deviceId}*（適用於裝置）或 *g:${orgId}:${typeId}:${deviceId}*（適用於閘道）：

```
PUT /authorization/devices/${clientID}/roles

Request Body:
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ]
}

Request Response: 200
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ],
    "rolesToGroups": {
        "PD_STANDARD_GW_DEVICE": ["gw_def_res_grp:abcdef:gatewayTypeId:gatewayDeviceId"]
    }
}
```

如需要求綱目的詳細資料，請參閱 [{{site.data.keyword.iot_full}} Limited Gateway API 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_authorization_devices_deviceId_roles){: new_window}。

## 將裝置新增至某資源群組以及從中移除裝置
{: #devices_groups}

裝置必須是指派給閘道的資源群組的成員，具有*標準閘道* 角色的閘道才能代表裝置作業。若要同時將多個裝置新增至資源群組，請使用下列 API：

```
 PUT /bulk/devices/{groupId}/add
```

要在其中新增裝置的群組必須指定於要求路徑中，而且要新增的裝置必須指定於要求主體中。如需要求綱目及回應的相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} Limited Gateway API 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_add){: new_window}。

若要移除資源群組中的多個裝置，請使用下列 API：

```
PUT /bulk/devices/{groupId}/remove
```

將會從要求路徑中所指定的群組中，移除要求主體所指定的裝置。如需要求綱目及回應的相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} Limited Gateway API 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_remove){: new_window}。

## 尋找資源群組
{: #finding_groups}

資源群組可以有關聯的搜尋標籤。使用下列 API，可以使用搜尋標籤來擷取資源群組的詳細資料：

```
GET /groups
```

此 API 會傳回與所使用搜尋標籤相關聯的資源群組。如果未指定搜尋標籤，則會傳回所有資源群組。<!-- For more information about the request schema, response, and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

使用下列 API 可以找到指派給閘道之資源群組的 ID，其中 *${clientID}* 是 URL 編碼 ClientID，格式為 *d:${orgId}:${typeId}:${deviceId}*（適用於裝置）或 *g:${orgId}:${typeId}:${deviceId}*（適用於閘道）：

```
GET /authorization/devices/${clientId}
```

此 API 會傳回指派給這個裝置之資源群組的唯一 ID。您可以在 [{{site.data.keyword.iot_short_notm}} Limited Gateway API 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_authorization_devices_deviceId){: new_window} 中找到此 API 的相關資訊。


## 查詢資源群組
{: #querying_groups}

您可以在各種參數內查詢資源群組，以傳回群組中所有裝置的完整內容、群組中所有裝置的唯一 ID，或是資源群組的內容。

若要傳回所指定資源群組中所有裝置的完整內容，請使用下列 API：

```
GET /bulk/devices/{groupId}
```

此 API 會傳回所指定資源群組的所有成員的完整內容清單。如需要求綱目、回應以及如何翻看結果的相關資訊，請參閱 [{{site.data.keyword.iot_short_notm}} Limited Gateway API 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_bulk_devices_groupId){: new_window}。

若只要傳回資源群組成員的唯一 ID，請使用下列 API：

```
GET /bulk/devices/{groupId}/ids
```

此 API 會傳回隸屬於資源群組的所有裝置的唯一 ID。<!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

若要傳回資源群組的內容，請使用下列 API：

```
GET /groups/{groupId}
```

此 API 會傳回路徑中所指定資源群組的內容（名稱、說明、搜尋標籤及唯一 ID），但不會傳回資源群組成員清單。<!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## 建立及刪除資源群組。
{: #crt_del_groups}

您可以在不使用連接閘道的情況下建立及刪除資源群組。若要建立資源群組，請使用下列 API：

```
POST /groups
```

此 API 會建立資源群組，並傳回群組詳細資料。<!-- For details on the request schema and the responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

若要刪除資源群組，請使用下列 API：

```
DELETE /groups/{groupId}
```

此 API 會刪除指定的資源群組。隸屬於群組的裝置將從群組中予以移除，但不會影響裝置本身。<!-- For more information, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## 更新群組內容
{: #update_groups}

  - /groups/{groupId}
    - PUT：更新所指定群組的內容。

## 擷取及更新裝置內容。
{: #fdevice_group_props}

有數種方法可以使用 API 來擷取裝置內容，而每一個 API 都會傳回不同的資訊。若要擷取 {{site.data.keyword.iot_short_notm}} 組織中所有現有裝置的裝置內容，請使用下列 API：

```
GET /authorization/devices

```

此 API 會傳回組織中所有現有裝置的內容，包括其存取控制相關內容（角色、狀態、到期日）。<!-- For more information on responses and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

若要擷取組織中單一裝置的裝置內容，請使用下列 API，其中 *${clientID}* 是 URL 編碼 ClientID，格式為 *d:${orgId}:${typeId}:${deviceId}*（適用於裝置）或 *g:${orgId}:${typeId}:${deviceId}*（適用於閘道）：

```
GET /authorization/devices/${clientId}
```

此 API 會傳回指定裝置的所有裝置內容。<!-- For more information, see the [{{site.data.keyword.iot_short_notm}} device model documentation](LINK TO DEVICE MODEL) and [API documentation](LINK TO CORRECT API). -->

若只要擷取特定裝置的存取控制資訊，請使用下列 API，其中 *${clientID}* 是 URL 編碼 ClientID，格式為 *d:${orgId}:${typeId}:${deviceId}*（適用於裝置）或 *g:${orgId}:${typeId}:${deviceId}*（適用於閘道）：

```
GET /authorization/devices/${clientId}/roles
```

此 API 會擷取指定裝置的存取控制相關資訊，而不傳回其他裝置內容。<!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

裝置內容可以使用兩種方式進行更新。您可以更新內容，而不變更存取控制內容，也可以直接更新存取控制內容。

若要更新裝置內容，而不影響存取控制內容，請使用下列 API，其中 *${clientID}* 是 URL 編碼 ClientID，格式為 *d:${orgId}:${typeId}:${deviceId}*（適用於裝置）或 *g:${orgId}:${typeId}:${deviceId}*（適用於閘道）：

```
PUT /authorization/devices/${clientId}
```

此 API 只會更新與存取控制無關的裝置的內容。<!-- For more information on request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

若只要更新已指定裝置的存取控制內容，請使用下列 API，其中 *${clientID}* 是 URL 編碼 ClientID，格式為 *d:${orgId}:${typeId}:${deviceId}*（適用於裝置）或 *g:${orgId}:${typeId}:${deviceId}*（適用於閘道）：

```
PUT /authorization/devices/${clientId}/withroles
```

此 API 只會更新指定裝置的存取控制內容。<!-- For more information on the request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->
