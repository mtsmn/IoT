---

copyright:
  years: 2018
lastupdated: "2018-07-16"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} 的 Cloud IAM 鑑別及授權（測試版）
{: #cloud_iam}

{{site.data.keyword.iot_full}} API 支援使用 Identity and Access Management (IAM) 來鑑別及授權使用者。

**重要事項：**「{{site.data.keyword.iot_short_notm}} 的 Cloud IAM 鑑別及授權」特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

Cloud IAM 內建於 IBM Cloud，並用於鑑別及授權需要配置與管理其 IBM 服務的管理和開發人員使用者。需要存取 Watson IoT Platform 使用者介面的使用者是使用 IBM Cloud IAM 進行鑑別。Cloud IAM 的身分來源可以是已登錄 IBM ID 使用者，也可以是支援 SAML 的客戶目錄服務。  

{{site.data.keyword.iot_short_notm}} 也支援透過 App ID 來鑑別使用者。App ID 用於鑑別需要存取 IBM Cloud 中所管理應用程式的使用者。App ID 使用者一般不會在雲端服務上執行管理或開發活動。如需相關資訊，請參閱 [Watson IoT Platform 的 App ID 鑑別](app_id.html#app_id)。

IAM 可讓使用者取得 IAM OAuth 記號，並使用它來鑑別 API 呼叫。例如，已安裝 {{site.data.keyword.containershort_notm}} CLI 的使用者可能會使用下列指令：

`bx iam oauth-tokens`

此指令會以下列格式傳回已登入使用者的 IAM 記號：

`IAM Token: Bearer <some token>`

使用者也可以使用 IAM 記號端點來登入並擷取其 IAM 記號，如下列範例所示：`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

此 API 範例使用 IAM API 金鑰來要求 IAM 記號，並傳回 `Bearer <token>`。

然後，當使用者呼叫 {{site.data.keyword.iot_short_notm}} API 時，可以在其 API 呼叫上提供授權標頭 `Authorization: Bearer <token>`。

## 產生 IAM 記號
{: #iam_generate}

您可以選擇如何自動建立 IAM 記號，視您如何使用 IBM Cloud 以及您使用的 IBM Cloud ID 類型進行鑑別而定。

如果您使用未聯合 ID，則可以選擇下列其中一個選項來建立 IAM 記號：
 - **IBM Cloud 使用者名稱及密碼：**您可以建立記號，以及完全自動建立 IAM 存取記號。
 - **產生 IBM Cloud API 金鑰：**使用 IBM Cloud API 金鑰，這與為其產生它們的 IBM Cloud 帳戶相依。您無法將 IBM Cloud API 金鑰與相同 IAM 記號中的不同帳戶 ID 結合。若要存取使用根據 IBM Cloud API 金鑰以外的帳戶所建立的叢集，您必須登入此帳戶才能產生新的 API 金鑰。

如果您使用聯合 ID，則可以選擇下列其中一個選項來建立 IAM 記號：
 - **產生 IBM Cloud API 金鑰：**IBM Cloud API 金鑰與為其產生它們的 IBM Cloud 帳戶相依。您無法將 IBM Cloud API 金鑰與相同 IAM 記號中的不同帳戶 ID 結合。若要存取使用根據 IBM Cloud API 金鑰以外的帳戶所建立的叢集，您必須登入此帳戶才能產生新的 API 金鑰。
 - **使用一次性密碼：**如果您使用一次性密碼來進行 IBM Cloud 鑑別，則無法完全自動建立您的 IAM 記號，因為擷取一次性密碼需要與 Web 瀏覽器進行手動互動。若要完全自動建立您的 IAM 記號，您必須改為建立 IBM Cloud API 金鑰。

當您建立存取記號時，根據使用的 IBM Cloud 鑑別方法，要求中所包括的內文資訊會不同。請取代這些值，如下所示：
- `<my_username>` = IBM Cloud 使用者名稱。
- `<my_password>` = IBM Cloud 密碼。
- `<my_api_key>` = IBM Cloud API 金鑰。
- `<my_passcode>` = IBM Cloud 一次性密碼。執行 `bx login --sso`，並遵循 CLI 輸出中的指示，利用 Web 瀏覽器來擷取一次性密碼。

範例：`POST https://iam.<region>.bluemix.net/oidc/token`

若要指定 IBM Cloud 地區，請檢閱在 API 端點中使用它們的地區縮寫。如需相關資訊，請參閱 [IBM Cloud 地區 API 端點 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions){: new_window}。

輸入參數      	  | 值
---------------- | -----------
標頭	| Content-Type:application/x-www-form-urlencoded<br>Authorization: Basic Yng6Yng=<br>附註：系統會將 Yng6Yng=、使用者名稱 bx 的 URL 編碼授權及密碼 bx 提供給您。
IBM Cloud 使用者名稱及密碼的內文 |	grant_type: password<br>response_type: cloud_iam, uaa<br>username: `<my_username>`<br>password: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>附註：新增未指定任何值的 uaa_client_secret 金鑰。
IBM Cloud API 金鑰的內文	|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>附註：新增未指定任何值的 uaa_client_secret 金鑰。
IBM Cloud 一次性密碼的內文	|	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>附註：新增未指定任何值的 uaa_client_secret 金鑰。

API 輸出範例：

```
{
"access_token": "<iam_token>",
"refresh_token": "<iam_refresh_token>",
"uaa_token": "<uaa_token>",
"uaa_refresh_token": "<uaa_refresh_token>",
"token_type": "Bearer",
"expires_in": 3600,
"expiration": 1493747503
}
```
您可以在 API 輸出的 **access_token** 欄位中找到 IAM 記號。

下列範例顯示如何在呼叫 API 時使用 IAM 記號。

```
GET https://org.domain/api/v0002/bulk/devices
```

輸入參數          |	值
----------------- | -----------
標頭	|	Content-Type: application/json<br>Authorization: bearer `<iam_token>`
