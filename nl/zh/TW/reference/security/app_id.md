---

copyright:
years: 2018
lastupdated: "2018-08-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Watson IoT Platform 的 App ID 鑑別（測試版）
{: #app_id}

App ID 用於鑑別需要存取 IBM Cloud 中所管理應用程式的使用者。這些使用者透過服務所提供的行動或 Web 應用程式來存取服務。
{: shortdesc}

**重要事項：**「{{site.data.keyword.iot_short_notm}} 的 App ID 鑑別及授權」特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} 也支援透過 Cloud IAM 來鑑別使用者。Cloud IAM 內建於 IBM Cloud，並用於鑑別及授權需要配置與管理其 IBM 服務的管理和開發人員使用者。如需 Cloud IAM 的相關資訊，請參閱 [Watson IoT Platform 的 Cloud IAM 鑑別及授權](cloud_iam.html#cloud_iam)。

App ID 使用者一般不會在雲端服務上執行管理或開發活動，而且無法登入 {{site.data.keyword.iot_short_notm}} Web 儀表板。只有 Cloud IAM 使用者才能登入儀表板。

{{site.data.keyword.iot_short_notm}} API 支援從 IBM Cloud App ID 服務鑑別使用者。您可以將 {{site.data.keyword.iot_short_notm}} 組織配置為使用 App ID 服務的實例，然後將您的 App ID 使用者新增至您的組織。您的應用程式會使用 App ID 來鑑別這些使用者，而且接著會使用此 App ID 來呼叫 {{site.data.keyword.iot_short_notm}} API。如需 IBM Cloud App ID 服務的相關資訊，請參閱 [IBM Cloud App ID 入門指導教學 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}。

現行 App ID 支援適用於非傳訊 REST API，包括用來配置及管理裝置、使用者及其他 {{site.data.keyword.iot_short_notm}} 功能的 API。App ID 無法用來發佈及訂閱訊息。

一般而言，您使用 App ID 來鑑別代表非管理使用者所呼叫的 REST API。開發人員可以建立應用程式，以代表沒有管理存取權的已登錄使用者（例如維護技師）使用 App ID 記號來呼叫 REST API。使用 App ID 鑑別非管理使用者而非使用單一鑑別記號的優點，在於 {{site.data.keyword.iot_short_notm}} 中的審核追蹤會顯示已呼叫 REST API 的使用者，而不是共用鑑別記號。App ID 也適用於鑑別需要使用 {{site.data.keyword.iot_short_notm}} 指令行介面 (CLI) 的非管理者。

**重要事項：**應該使用使用者應用程式而且不是直接從瀏覽器或行動應用程式來呼叫 {{site.data.keyword.iot_short_notm}} REST API。使用者應用程式可以提供額外層次的存取控制，以判定容許特定使用者存取的裝置功能，以及驗證傳送至裝置的 MQTT 訊息。下圖顯示此呼叫流程：

![使用使用者應用程式的 REST API 呼叫](images/app_id_user_app.PNG)

App ID 使用者必須使用 Cloud IAM 使用者所使用的相同處理程序登錄於 {{site.data.keyword.iot_short_notm}} 中，並且同時出現在儀表板介面中。

因為 App ID 使用者通常不是管理者，所以請務必考慮應該容許他們存取的資源及指令，然後適當地設定 {{site.data.keyword.iot_short_notm}} 資源層次存取控制設定。在大部分情況下，App ID 使用者會獲指派一個比 {{site.data.keyword.iot_short_notm}} 組織的定義預設角色還要嚴格的自訂角色。如需角色的相關資訊，請參閱[使用者、應用程式及閘道角色](../../roles_index.html#user-application-and-gateway-roles)。

依預設，App ID 使用者可以存取所有裝置。如果 App ID 使用者只能管理受限制的裝置清單，請為 App ID 使用者指派資源群組。

**附註：**{{site.data.keyword.iot_short_notm}} 會將已登錄使用者總數限制為 1,000。

## 設定 App ID 服務
{: #set_up_app_id}

設定 App ID 服務之前，您必須新增 IBM Cloud App ID 服務的實例，並配置身分提供者。如果您還沒有 App ID 實例，則可以從 [IBM Cloud 服務型錄 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog){: new_window} 進行佈建。

設定 App ID 服務之後，您必須取得服務認證，才能將 App ID 配置為 {{site.data.keyword.iot_short_notm}} 組織的提供者。您可以在 App ID 服務使用者介面中，取得或建立服務認證。

1. 在 IBM Cloud 儀表板中，選取 App ID 服務，然後按一下**服務認證**。
2. 按一下**檢視認證**，以找到認證中的 `tenantId`、`clientId` 及 `secret`。在後續步驟中配置 App ID 時需要這些值。發證者將是 `oauthServerUrl` 的主機名稱（例如 `appid-oauth.ng.bluemix.net`）。

下圖顯示如何擷取認證的範例：

![擷取 App ID 認證](images/app_id_credentials.PNG "擷取 App ID 認證的範例")


## 在 {{site.data.keyword.iot_short_notm}} 中配置 App ID
{: #config_app_id}

您必須先配置 App ID，才能使用 App ID 記號進行授權。若要建立 App ID 配置，請使用 `POST /api/v0002/authentication/providers` API，其中 `tenant_Id` 及 `issuer` 取自[設定 App ID 服務](#set_up_app_id)中的步驟 2：

要求內文：

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

要求回應：200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## 將使用者新增至 {{site.data.keyword.iot_short_notm}}
{: #users_app_id}

您必須根據使用者的角色，來新增使用者並且授與授權。若要建立使用者，請使用 `POST /api/v0002/authorization/users` API，詳細資料如下：

 - `issuer` 取自[設定 App ID 服務](#set_up_app_id)中的步驟 2。
 - `uniqueSecurityName` 會填入 App ID 服務身分提供者中所配置的 `appIdConfigName`、`amr` 及使用者電子郵件的詳細資料。
 - `appIdConfigName` 是[配置 App ID](##config_app_id) 中所使用的名稱（例如 "TestAppIdConfigName"）。
 - `amr` 是鑑別方法參照，其為 App ID 服務中所使用的身分提供者。App ID 支援下列身分提供者：

	 - Cloud Directory
	 - SAML 2.0 Federation
	 - Facebook
	 - Google

	 根據 App ID 服務中所使用的身分提供者，`amr` 會是 `cloud_directory`、`saml`、`facebook` 或 `google`。

在下列範例中，將會使用 Cloud Directory：

要求內文：

```
{
    "uniqueSecurityName": "TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
    "PD_ADMIN_USER"
  ]
}
```

要求回應：200

```
{
    "uniqueSecurityName": “TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
        "PD_ADMIN_USER"
    ]
}
```

## 產生 App ID 記號
{: #token_app_id}

在您已配置 App ID 並新增使用者之後，您的應用程式可以開始產生使用者的 App ID 記號，並使用它們來呼叫平台 API。如需開始使用 App ID 的相關資訊，請參閱下列視訊：[IBM Bluemix App ID ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}。

使用者可以使用 App ID 端點來登入及擷取其 App ID 記號，方法是張貼下列範例。

### 產生 App ID 記號的 HTTP 範例

若要產生 App ID 記號，請使用下列 API，以透過服務認證向 IBM Cloud 鑑別：

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

其中 `tenantId`、`issuer`、`clientId` 及 `secret` 取自[設定 App ID 服務](#set_up_app_id)中的步驟 2。

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

grant_type=password 的要求內文：

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

grant_type= authorization_code 的要求內文：

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

要求回應：

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### 產生 App ID 記號的 Curl 範例

下列 Snippet 顯示產生 App ID 記號的 CURL 範例：

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

其中 `username` 及 `password` 是針對 App ID 中的使用者所定義。

要求回應：

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### 產生 App ID 記號的 Swagger 範例

若要使用 Swagger 產生 App ID 記號，請使用 [Swagger 範例 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}。

完成 Swagger 參數中的所有詳細資料，然後按一下 `/oauth/v3/{tenantId}/token` 方法尾端的**試用**。

記號端點回應包含存取權和 ID 記號、選用的重新整理記號，以及有效期限資訊。

## 使用 App ID 記號
{: #use_app_id}

建立 App ID 記號之後，您的應用程式就可以開始使用它來呼叫平台 API。App ID 記號是在[產生 App ID 記號](#token_app_id)中產生它之回應中的 `id_token` 內容。App ID 記號到期時，應用程式必須產生新的記號，才能繼續呼叫平台 API。

下列各範例顯示如何在呼叫 API 時使用 App ID 記號。

###	使用 App ID 記號的 HTTP 範例

`GET https://org.domain/api/v0002/bulk/devices`

輸入參數          |	值
----------------- | -----------
標頭	|	Content-Type：application/json<br>Authorization：bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### 使用 App ID 記號的 Curl 範例

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
