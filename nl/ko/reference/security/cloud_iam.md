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


# {{site.data.keyword.iot_short_notm}}용 Cloud IAM 인증 및 권한 부여(베타)
{: #cloud_iam}

{{site.data.keyword.iot_full}} API는 IAM(Identity and Access Management)을 사용한 사용자의 인증 및 권한 부여를 지원합니다. 

**중요:** {{site.data.keyword.iot_short_notm}}용 Cloud IAM 인증 및 권한 부여 기능은 제한된 베타 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

Cloud IAM은 IBM Cloud에 빌드되며, 자체 IBM 서비스를 구성하고 관리해야 하는 관리 및 개발자 사용자를 인증하고 권한 부여하는 데 사용됩니다. Watson IoT Platform UI에 액세스해야 하는 사용자는 IBM Cloud IAM으로 인증됩니다. Cloud IAM에 대한 ID의 소스는 등록된 IBM ID 사용자이거나 SAML을 지원하는 고객의 디렉터리 서비스일 수 있습니다.   

{{site.data.keyword.iot_short_notm}}에서는 App ID를 통한 사용자 인증도 지원합니다. App ID는 IBM Cloud에서 호스팅되는 애플리케이션에 액세스해야 하는 사용자를 인증하는 데 사용됩니다. App ID 사용자는 일반적으로 클라우드 서비스의 관리 또는 개발 활동을 수행하지 않습니다. 자세한 정보는 [Watson IoT Platform용 App ID 인증](app_id.html#app_id)을 참조하십시오. 

IAM을 통해 사용자는 IAM OAuth 토큰을 가져올 수 있으며 이를 사용하여 API 호출을 인증할 수 있습니다. 예를 들어, {{site.data.keyword.containershort_notm}} CLI가 설치된 사용자는 다음 명령을 사용할 수 있습니다. 

`bx iam oauth-tokens`

이 명령은 다음 형식으로 로그인한 사용자의 IAM 토큰을 리턴합니다. 

`IAM 토큰: 베어러 <some token>`

사용자는 다음 예제에 표시된 대로 IAM 토큰 엔드포인트를 사용하여 로그인하고 자체 IAM 토큰을 검색할 수도 있습니다.
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

이 API 예제는 IAM API 키를 사용하여 IAM 토큰을 요청하고 다음을 리턴합니다. `Bearer <token>`.

그리고 {{site.data.keyword.iot_short_notm}} API를 호출할 때 사용자는 권한 부여 헤더 `Authorization: Bearer <token>`를 자체 API 호출에서 제공할 수 있습니다. 

## IAM 토큰 생성
{: #iam_generate}

IBM Cloud를 인증하는 방법과 사용 중인 IBM Cloud ID의 유형에 따라 IAM 토큰의 작성을 자동화하는 방법을 선택할 수 있습니다. 

비-연합 ID를 사용하는 경우에는 IAM 토큰 작성을 위한 다음 옵션 중 하나를 선택할 수 있습니다. 
 - **IBM Cloud 사용자 이름 및 비밀번호:** 사용자는 토큰을 작성하고 IAM 액세스 토큰의 작성을 완전 자동화할 수 있습니다. 
 - **IBM Cloud API 키 생성:** 자신이 생성된 대상 IBM Cloud 계정에 의존하는 IBM Cloud API 키를 사용합니다. IBM Cloud API 키를 동일한 IAM 토큰의 다른 계정 ID와 결합할 수는 없습니다. IBM Cloud API 키의 기반이 되는 계정 이외의 계정으로 작성된 클러스터에 액세스하려면 계정에 로그인하여 새 API키를 생성해야 합니다. 

연합 ID를 사용하는 경우에는 IAM 토큰 작성을 위한 다음 옵션 중 하나를 선택할 수 있습니다. 
 - **IBM Cloud API 키 생성:** IBM Cloud API 키는 자신이 생성된 대상 IBM Cloud 계정에 의존합니다. IBM Cloud API 키를 동일한 IAM 토큰의 다른 계정 ID와 결합할 수는 없습니다. IBM Cloud API 키의 기반이 되는 계정 이외의 계정으로 작성된 클러스터에 액세스하려면 계정에 로그인하여 새 API키를 생성해야 합니다. 
 - **일회성 패스코드 사용:** 일회성 패스코드의 검색에서 웹 브라우저와 수동으로 상호작용해야 하므로, 일회성 패스코드를 사용하여 IBM Cloud를 인증하는 경우에는 IAM 토큰 작성을 완전히 자동화할 수 없습니다. IAM 토큰 작성을 완전히 자동화하려면 대신 IBM Cloud API 키를 작성해야 합니다. 

액세스 토큰을 작성할 때 요청에 포함된 본문 정보는 사용 중인 IBM Cloud 인증 방법에 따라 다양합니다. 다음과 같이 값을 바꾸십시오.
- `<my_username>` = IBM Cloud 사용자 이름입니다. 
- `<my_password>` = IBM Cloud 비밀번호입니다. 
- `<my_api_key>` = IBM Cloud API 키입니다. 
- `<my_passcode>` = IBM Cloud 일회성 패스코드입니다. `bx login --sso`를 실행하고 CLI 출력의 지시사항에 따라 웹 브라우저를 사용하여 일회성 패스코드를 검색하십시오. 

예:
`POST https://iam.<region>.bluemix.net/oidc/token`

IBM Cloud 지역을 지정하려면 API 엔드포인트에서 사용되는 지역 약어를 검토하십시오. 자세한 정보는 다음을 참조하십시오. [IBM Cloud 지역 API 엔드포인트 [외부 링크 아이콘](../../icons/launch-glyph.svg)(https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions)] 

입력 매개변수|값
---------------- | -----------
헤더| Content-Type:application/x-www-form-urlencoded<br>권한 부여: 기본 Yng6Yng=<br>참고: 사용자에게는 Yng6Yng=, 사용자 이름 bx 및 비밀번호 bx에 대한 URL 인코딩된 권한이 제공됩니다. 
IBM Cloud 사용자 이름 및 비밀번호의 본문|	grant_type: password<br>response_type: cloud_iam, uaa<br>username: `<my_username>`<br>password: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>참고: 값이 지정되지 않은 uaa_client_secret 키를 추가하십시오. 
IBM Cloud API 키의 본문|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>참고: 값이 지정되지 않은 uaa_client_secret 키를 추가하십시오. 
IBM Cloud 일회성 패스코드의 본문|	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>참고: 값이 지정되지 않은 uaa_client_secret 키를 추가하십시오. 

예제 API 출력: 

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
API 출력의 **access_token** 필드에서 IAM 토큰을 찾을 수 있습니다. 

다음 예제는 API 호출 중에 IAM 토큰을 사용하는 방법을 표시합니다. 

```
GET https://org.domain/api/v0002/bulk/devices
```

입력 매개변수|	값
----------------- | -----------
헤더|	Content-Type: application/json<br>권한 부여: 베어러 `<iam_token>`
