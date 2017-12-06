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


# 資源層次存取控制概觀（測試版）
{: #RLAC_overview}

資源層次存取控制可讓您控制使用者及 API 金鑰存取權來管理裝置。您可以使用資源群組來指定組織中每位使用者或每個 API 金鑰可管理的裝置。如需如何配置資源層次存取控制的相關資訊，請參閱[配置資源層次存取控制](rlac.html#configure_RLAC)。

**重要事項：**{{site.data.keyword.iot_full}} 資源層次存取控制特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## 資源層次存取控制概念 
{: #RLAC_concepts}

裝置群組是 {{site.data.keyword.iot_short_notm}} 資源層次存取控制的一部分。您可以使用裝置群組特性，來管理人員可根據其角色而存取的裝置數目。在實作裝置群組之前，請判定並記錄裝置群組的目的及範圍，以增加解決方案的有效性。 

您可以實作裝置群組作為 IoT 解決方案的一部分，讓使用者或應用程式存取裝置的子集，而不是存取與組織相關聯的所有裝置。若要增加裝置群組的有效性，請在有人必須存取許多裝置時使用裝置群組。

例如，雇用若干現場工程師及操作人員的公司想要在整個英國實作 IoT 解決方案。該公司會針對 69 個城市各配置一個裝置群組、針對每一個地理區域配置 9 個裝置群組，以及針對整個英國配置一個裝置群組。裝置會配置給這些群組，而群組會指派給 15 個現場工程師及操作人員，並且其中有些人員可以存取多個群組。只負責一或兩個裝置的使用者可以繼續使用 {{site.data.keyword.iot_short_notm}} 外部的行動應用程式及企業架構，但要補充整體 IoT 解決方案。

在 [Internet of Things 空間問題 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window} 中，可能會引發有關裝置群組的問題。

## 存取控制的資源群組限制

強制執行資源群組限制，以確保資源層次存取控制特性可以有效地運作。這些限制會限制指派給主旨的資源群組數目、群組內的裝置數目，以及資源可隸屬的群組數目。

下列是強制執行的限制：

 - 最多可以將 10 個資源群組指派給 API 金鑰、使用者或閘道。
 - 資源群組最多可以有 300 個資源。
 - 一個資源最多可以屬於 10 個群組。

## 資源層次存取控制術語

您可以使用下列各節，協助您瞭解資源層次存取控制特性中所使用的術語。 

### 主體
{: #subjects}

主體是要求平台存取的已鑑別實體，而這必須獲得授權。下列是有效的主體類型：

- *使用者*：一般使用者是使用應用程式的人員。成員是其帳戶沒有到期日的使用者，而訪客是其帳戶具有到期日的使用者。
- *裝置*：存取 {{site.data.keyword.iot_short_notm}} 平台並使用類型「裝置」的實體裝置。
- *閘道*：存取 {{site.data.keyword.iot_short_notm}} 平台並使用類型「閘道」的實體裝置。
- *應用程式*：使用 API 金鑰存取 {{site.data.keyword.iot_short_notm}} 平台以進行授權及存取控制的應用程式或服務。

### 資源
{: #resources}

資源是主體對其執行動作的實體。主體會要求資源的已授權平台存取權。裝置是唯一在資源層次存取控制測試版中支援的資源。

### 動作
{: #actions}

動作是主體所執行的作業，最常見的是「建立」、「讀取」、「更新」、「刪除」或「執行」。作業是用來將資源上的動作進行分組。

### 應用程式
{: #applications}

應用程式可以代表平台無法辨識的主體（例如控制家用電器的行動應用程式）執行動作，而且應用程式可以代表平台已知或無法辨識的實際或模擬裝置作業。應用程式也可以是不代表任何其他裝置類型作業的真正伺服器端應用程式。

提供鑑別，並根據 API 金鑰或記號來存取 {{site.data.keyword.iot_short_notm}} 平台。

### 作業
{: #operations}

作業包含一個以上的動作，而這些動作是利用使用者介面及程式化介面（例如 REST 服務及 MQTT 介面）以在特定資源類型上執行。它們供不同的主體（包括成員、應用程式及裝置）使用。作業是透過其「作業 ID (OID)」進行識別。

### 角色
{: #roles}

角色是作業集，用來提供使用作業的許可權。主體可以有多個角色，而角色是主體的屬性。

**角色格式**

0..n 角色的陣列

    { "roles": [ "PD_ADMIN_APP" ] }

**角色對群組格式**

0..n <string, list<string>> 配對的對映

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### 許可權
{: #permissions}

許可權容許主體對資源或資源群組執行作業。

### 資源群組
{: #resource_groups}

資源群組是作為資源的裝置集合。裝置可以新增至資源群組，而資源群組可以指派給角色對群組配對中的主體。這些關係容許主體對特定的裝置群組執行給定角色的作業。

### 使用者
{: #users}

使用者會使用隸屬組織的 ID 來登入 {{site.data.keyword.iot_short_notm}} 平台儀表板。使用者已獲指派角色，將呼叫特定 HTTP API 集的授權授與他們。如需使用者角色的相關資訊，請參閱[使用者角色存取層次](roles_access.html#levels-of-access-for-user-roles)。

### API 金鑰
{: #API_keys}

API 金鑰是用來呼叫 {{site.data.keyword.iot_short_notm}} 平台 HTTP API 的記號。API 金鑰已獲指派角色，將呼叫特定 HTTP API 集的授權授與他們。如需 API 金鑰角色的相關資訊，請參閱[應用程式角色存取層次](app_roles_access.html#levels-of-access-for-application-roles)。

## 已強制執行資源層次存取控制的 API
{: #RLAC_enforced_APIs}
已啟用資源層次存取控制時，只有在呼叫者可以存取指定的裝置時，本節所含的裝置相關 API 才能運作。

### 裝置 API（個別）

**列出某類型的裝置**

    GET /api/v0002/device/types/${typeId}

只會傳回屬於適當群組的裝置子集。

**取得裝置**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**取得裝置管理詳細資料**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**取得閘道所登錄的裝置**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

如果呼叫者無法存取閘道，則會傳回 404 錯誤。只會傳回屬於適當群組的裝置子集。裝置及閘道必須位於呼叫者的群組中。

**更新裝置**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**刪除裝置**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

### 裝置 API（大量）

**列出裝置**

    GET /api/v0002/bulk/devices

只會傳回屬於適當群組的裝置子集。

**刪除裝置**

    DELETE /api/v0002/bulk/devices/remove

只會刪除屬於適當群組的裝置子集。使用 success = true，仍然會傳回無法存取的裝置。

**更新裝置：**

    PUT /api/v0002/bulk/devices/update

### 裝置管理 API

**起始要求**

    POST /api/v0002/mgmt/requests

如果呼叫者無法存取任何裝置，則會失敗。

**刪除要求**

    DELETE /api/v0002/mgmt/requests/${requestId}

如果呼叫者無法存取任何裝置，則會失敗。

**檢視要求**

    GET /api/v0002/mgmt/requests/${requestId}

**列出要求**

    GET /api/v0002/mgmt/requests

**取得某個要求**

    GET /api/v0002/mgmt/requests/${requestId}

**取得要求裝置詳細資料**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**取得某個裝置的要求裝置詳細資料**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### 問題判斷 API

**裝置的連線日誌**

    GET /api/v0002/logs/connection

如果無法存取裝置，則會傳回空陣列。

**新增裝置錯誤碼**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**列出裝置錯誤碼**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**清除裝置錯誤碼**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**新增裝置診斷日誌**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**列出裝置診斷日誌**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**取得某個裝置診斷日誌**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**清除裝置診斷日誌**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**刪除某個裝置診斷日誌**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

###前次事件快取 API

**取得裝置的事件**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

如果呼叫者無法存取裝置，則會傳回 404 錯誤。

**取得裝置的事件**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

如果呼叫者無法存取裝置，則會傳回 404 錯誤。
