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

# Watson IoT Platform용 App ID 인증(베타)
{: #app_id}

App ID는 IBM Cloud에서 호스팅되는 애플리케이션에 액세스해야 하는 사용자를 인증하는 데 사용됩니다. 이러한 사용자는 서비스에서 제공하는 모바일 또는 웹 애플리케이션을 통해 서비스에 액세스합니다.
{: shortdesc}

**중요:** {{site.data.keyword.iot_short_notm}}용 App ID 인증 및 권한 부여 기능은 제한된 베타 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

{{site.data.keyword.iot_short_notm}}에서는 Cloud IAM을 통한 사용자 인증도 지원합니다. Cloud IAM은 IBM Cloud에 빌드되며, 자체 IBM 서비스를 구성하고 관리해야 하는 관리 및 개발자 사용자를 인증하고 권한 부여하는 데 사용됩니다. 클라우드 IAM에 대한 자세한 정보는 [Watson IoT Platform용 Cloud IAM 인증 및 권한 부여](cloud_iam.html#cloud_iam)를 참조하십시오. 

App ID 사용자는 일반적으로 클라우드 서비스의 관리 또는 개발 활동을 수행하지 않으며, {{site.data.keyword.iot_short_notm}} 웹 대시보드에 로그인할 수 없습니다. 클라우드 IAM 사용자만 대시보드에 로그인할 수 있습니다.

{{site.data.keyword.iot_short_notm}} API는 IBM Cloud App ID 서비스에서 사용자의 인증을 지원합니다. App ID 서비스의 인스턴스를 사용하도록 {{site.data.keyword.iot_short_notm}} 조직을 구성한 후에 App ID 사용자를 조직에 추가할 수 있습니다. 애플리케이션은 App ID로 이러한 사용자를 인증하며, 이 App ID는 다시 {{site.data.keyword.iot_short_notm}} API를 호출하는 데 사용될 수 있습니다. IBM Cloud App ID 서비스에 대한 자세한 정보는 [IBM Cloud App ID 시작하기 튜토리얼 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}을 참조하십시오. 

현재 App ID는 디바이스, 사용자의 구성과 관리 및 기타 {{site.data.keyword.iot_short_notm}} 기능을 위한 API를 포함하여 비-메시징 REST API용으로 지원됩니다. App ID를 사용하여 메시지를 공개하고 이를 구독할 수는 없습니다. 

일반적으로는 App ID를 사용하여 비-관리자를 대신하여 호출되는 REST API를 인증할 수 있습니다. 개발자는 App ID 토큰을 사용하여 관리 액세스 권한이 없는 등록된 사용자(예: 유지보수 기술자)를 대신하여 REST API를 호출하는 애플리케이션을 작성할 수 있습니다. 단일 인증 토큰을 사용하는 대신 App ID를 사용하여 비-관리자를 인증하면 {{site.data.keyword.iot_short_notm}}에서 감사 추적이 공유 인증 토큰 대신 REST API를 호출한 사용자를 표시한다는 장점이 있습니다. App ID는 {{site.data.keyword.iot_short_notm}} 명령행 인터페이스(CLI)를 사용해야 하는 비-관리자의 인증에도 유용합니다. 

**중요:** {{site.data.keyword.iot_short_notm}} REST API의 호출은 사용자 애플리케이션을 사용하여 수행되어야 하며, 브라우저 또는 모바일 앱에서 직접 수행되지 않아야 합니다. 사용자 애플리케이션은 디바이스로 보낸 MQTT 메시지의 유효성 검증은 물론 특정 사용자가 액세스할 수 있는 디바이스 기능을 판별하기 위한 추가 레벨의 액세스 제어를 제공할 수 있습니다. 다음 이미지는 이 호출 플로우를 표시합니다.

![사용자 애플리케이션을 사용한 REST API 호출](images/app_id_user_app.PNG)

App ID 사용자는 클라우드 IAM 사용자가 사용하는 동일한 프로세스를 사용하여 {{site.data.keyword.iot_short_notm}}에서 등록되어야 하며, 이는 둘 다 대시보드 인터페이스에 나타납니다. 

App ID 사용자가 일반적으로 관리자가 아니므로, 이 사용자의 액세스를 허용해야 하는 리소스와 명령을 고려한 후에 {{site.data.keyword.iot_short_notm}} 리소스 레벨 액세스 제어 설정을 알맞게 설정하는 것이 중요합니다. 대부분의 경우, App ID 사용자에게는 {{site.data.keyword.iot_short_notm}} 조직의 정의된 기본 역할보다 더 제한적인 사용자 정의 역할이 지정됩니다. 사용자 역할에 대한 자세한 정보는 [사용자, 애플리케이션 및 게이트웨이 역할](../../roles_index.html#user-application-and-gateway-roles)을 참조하십시오. 

기본적으로 App ID 사용자는 모든 디바이스에 액세스할 수 있습니다. App ID 사용자가 제한된 디바이스 목록 관리만 수행할 수 있어야 하는 경우에는 리소스 그룹에 App ID 사용자를 지정하십시오. 

**참고:** {{site.data.keyword.iot_short_notm}}에서는 등록된 사용자의 총 수를 1,000명으로 제한합니다.

## App ID 서비스 설정
{: #set_up_app_id}

App ID 서비스를 설정하기 전에, IBM Cloud App ID 서비스의 인스턴스를 추가하고 ID 제공자를 구성해야 합니다. App ID의 인스턴스가 아직 없는 경우에는 [IBM Cloud 서비스 카탈로그 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/catalog){: new_window}에서 이를 프로비저닝할 수 있습니다. 

App ID 서비스를 설정한 후에는 {{site.data.keyword.iot_short_notm}} 조직의 제공자로서 App ID를 구성하기 위한 서비스 인증 정보를 가져와야 합니다. App ID 서비스 UI에서 서비스 인증 정보를 가져오거나 이를 작성할 수 있습니다. 

1. IBM Cloud 대시보드에서 App ID 서비스를 선택하고 **서비스 인증 정보**를 클릭하십시오. 
2. **인증 정보 보기**를 클릭하여 인증 정보의 `tenantId`, `clientId` 및 `secret`을 찾으십시오. 이러한 값은 나중 단계에서 App ID를 구성하는 데 필요합니다. 발행자는 `oauthServerUrl`의 호스트 이름입니다(예: `appid-oauth.ng.bluemix.net`).

다음 이미지는 인증 정보를 검색하는 방법의 예를 보여줍니다. 

![App ID 인증 정보 검색](images/app_id_credentials.PNG "App ID 인증 정보 검색의 예")


## {{site.data.keyword.iot_short_notm}}에서 App ID 구성
{: #config_app_id}

권한 부여에 App ID 토큰을 사용하려면 우선 App ID를 구성해야 합니다. App ID 구성을 작성하려면 `POST /api/v0002/authentication/providers` API를 사용하십시오. 여기서 `tenant_Id` 및 `issuer`는 [App ID 서비스 설정](#set_up_app_id)의 2단계에서 가져온 것입니다. 

요청 본문:

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

요청 응답: 200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## {{site.data.keyword.iot_short_notm}}에 사용자 추가
{: #users_app_id}

사용자를 추가하고 해당 역할에 따라 권한을 부여해야 합니다. 사용자를 작성하려면 다음 세부사항으로 `POST /api/v0002/authorization/users` API를 사용하십시오. 

 - `issuer`는 [App ID 서비스 설정](#set_up_app_id)의 2단계에서 가져온 것입니다. 
 - `uniqueSecurityName`은 App ID 서비스의 ID 제공자에서 구성된 사용자의 이메일 및 `appIdConfigName`, `amr`의 세부사항으로 채워집니다. 
 - `appIdConfigName`은 [App ID 구성](##config_app_id)에서 사용되는 이름입니다(예: “TestAppIdConfigName”).
 - `amr`은 인증 방법 참조이며, 이는 App ID 서비스에서 사용되는 ID 제공자입니다. App ID는 다음의 ID 제공자를 지원합니다.

	 - Cloud Directory
	 - SAML 2.0 Federation
	 - Facebook
	 - Google

	 App ID 서비스에서 사용되는 ID 제공자를 기반으로 하여 `amr`은 `cloud_directory`, `saml`, `facebook` 또는 `google`일 수 있습니다. 

다음 예제에서는 클라우드 디렉토리가 사용됩니다. 

요청 본문:

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

요청 응답: 200

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

## APP ID 토큰 생성
{: #token_app_id}

App ID를 구성하고 사용자를 추가한 후에, 애플리케이션은 사용자에 대한 App ID 토큰 생성을 시작하고 이를 사용하여 플랫폼 API를 호출할 수 있습니다. App ID 시작하기에 대한 자세한 정보는 [IBM Bluemix App ID ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window} 동영상을 참조하십시오. 

사용자는 App ID 엔드포인트를 사용하여 로그인하고 다음 예제를 게시하여 App ID 토큰을 검색할 수 있습니다. 

### App ID 토큰 생성의 HTTP 예제

App ID 토큰을 생성하려면 서비스 인증 정보로 IBM Cloud를 인증하여 다음 API를 사용하십시오. 

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

여기서 `tenantId`, `issuer`, `clientId` 및 `secret`은 [App ID 서비스 설정](#set_up_app_id)의 2단계에서 가져온 것입니다. 

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

grant_type=password의 요청 본문:

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

grant_type= authorization_code의 요청 본문:

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

요청 응답:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### App ID 토큰 생성의 Curl 예제

다음 스니펫은 App ID 토큰 생성의 CURL 예제를 표시합니다. 

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

여기서 `username` 및 `password`는 App ID의 사용자에 대해 정의되어 있습니다. 

요청 응답:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### App ID 토큰 생성의 Swagger 예제

Swagger를 사용하여 App ID 토큰을 생성하려면 [Swagger 예제 ![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}를 사용하십시오. 

Swagger 매개변수의 모든 세부사항을 완료하고 `/oauth/v3/{tenantId}/token` 메소드의 끝에서 **시도해 보기**를 클릭하십시오. 

토큰 엔드포인트 응답에는 액세스 및 ID 토큰, 선택적 새로 고치기 토큰 및 만기 정보가 포함됩니다. 

## App ID 토큰 사용
{: #use_app_id}

App ID 토큰이 작성된 후에 애플리케이션은 이를 사용하여 플랫폼 API 호출을 시작할 수 있습니다. App ID 토큰은 [App ID 토큰 생성](#token_app_id)에서 생성된 응답의 `id_token` 특성입니다. App ID 토큰이 만료되면 애플리케이션은 플랫폼 API 호출을 계속하기 위해 새 토큰을 생성해야 합니다.

다음 예제에서는 API 호출 중에 App ID 토큰을 사용하는 방법을 표시합니다. 

###	App ID 토큰 사용의 HTTP 예제

`GET https://org.domain/api/v0002/bulk/devices`

입력 매개변수|	값
----------------- | -----------
헤더|	Content-Type: application/json<br>권한 부여: 베어러 eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### App ID 토큰 사용의 Curl 예제

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
