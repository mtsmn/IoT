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

# Watson IoT Platform 的 App ID 认证 (Beta)
{: #app_id}

App ID 用于对需要访问在 IBM Cloud 中托管的应用程序的用户进行认证。这些用户通过服务提供的移动或 Web 应用程序来访问服务。
{: shortdesc}

**重要信息：**{{site.data.keyword.iot_short_notm}} 的 App ID 认证和授权功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} 还支持通过 Cloud IAM 对用户进行认证。Cloud IAM 内置于 IBM Cloud 中，用于对需要配置和管理其 IBM 服务的管理用户和开发者用户进行认证和授权。有关 Cloud IAM 的更多信息，请参阅 [Watson IoT Platform 的 Cloud IAM 认证和授权](cloud_iam.html#cloud_iam)。

App ID 用户通常并不在云服务上执行管理或开发活动，也无法登录到 {{site.data.keyword.iot_short_notm}} Web 仪表板。只有 Cloud IAM 用户才能登录到该仪表板。

{{site.data.keyword.iot_short_notm}} API 支持通过 IBM Cloud App ID 服务对用户进行认证。您可以将 {{site.data.keyword.iot_short_notm}} 组织配置为使用 App ID 服务的实例，然后将 App ID 用户添加到组织。应用程序会使用 App ID 对这些用户进行认证，然后可以使用 App ID 来调用 {{site.data.keyword.iot_short_notm}} API。有关 IBM Cloud App ID 服务的更多信息，请参阅 [IBM Cloud App ID 入门教程 ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}。

当前 App ID 支持仅适用于非消息传递 REST API，包括用于配置和管理设备、用户和其他 {{site.data.keyword.iot_short_notm}} 功能的 API。App ID 不能用于发布和预订消息。

通常，您可使用 App ID 来对代表非管理用户调用的 REST API 进行认证。开发者可以创建应用程序，以使用 App ID 令牌代表注册用户（例如，没有管理访问权的维护技术人员）来调用 REST API。使用 App ID 对非管理用户进行认证而不使用单个认证令牌的优点是，{{site.data.keyword.iot_short_notm}} 中的审计跟踪会显示调用了 REST API 的用户，而不是显示共享认证令牌。要对需要使用 {{site.data.keyword.iot_short_notm}} 命令行界面 (CLI) 的非管理员进行认证，App ID 也非常有用。

**重要信息：**调用 {{site.data.keyword.iot_short_notm}} REST API 应该通过用户应用程序，而不是直接从浏览器或移动应用程序来执行。用户应用程序可以提供其他级别的访问控制，以确定允许特定用户访问的设备功能，以及验证发送到设备的 MQTT 消息。下图显示了此调用流程：

![使用用户应用程序进行的 REST API 调用](images/app_id_user_app.PNG)

App ID 用户必须使用 Cloud IAM 用户所用的相同流程在 {{site.data.keyword.iot_short_notm}} 中进行注册，并且这两种用户都会显示在仪表板界面中。

由于 App ID 用户通常不是管理员，因此务必考虑应该允许这些用户访问的资源和命令，然后相应地设置 {{site.data.keyword.iot_short_notm}} 资源级别访问控制设置。在大多数情况下，向 App ID 用户分配的定制角色相比定义的 {{site.data.keyword.iot_short_notm}} 组织缺省角色，受到的限制更多。有关角色的更多信息，请参阅[用户、应用程序和网关角色](../../roles_index.html#user-application-and-gateway-roles)。

缺省情况下，App ID 用户可以访问所有设备。如果 App ID 用户应该只能管理受限的设备列表，请为 App ID 用户分配资源组。

**注：**{{site.data.keyword.iot_short_notm}} 的注册用户总数限制为 1,000。

## 设置 App ID 服务
{: #set_up_app_id}

设置 App ID 服务之前，必须添加 IBM Cloud App ID 服务的实例，并配置身份提供者。如果您还没有 App ID 实例，那么可以从 [IBM 云服务目录 ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog){: new_window} 供应 App ID 实例。

设置 App ID 服务后，必须获取服务凭证以将 App ID 配置为 {{site.data.keyword.iot_short_notm}} 组织的提供者。您可以在 App ID 服务 UI 中获取或创建服务凭证。

1. 在 IBM Cloud“仪表板”中，选择您的 App ID 服务，然后单击**服务凭证**。
2. 单击**查看凭证**，以在凭证中找到 `tenantId`、`clientId` 和 `secret`。在稍后的步骤中配置 App ID 时需要这些值。颁发者将是 `oauthServerUrl` 中的主机名，例如 `appid-oauth.ng.bluemix.net`。

下图显示了有关如何检索凭证的示例：

![检索 App ID 凭证](images/app_id_credentials.PNG "检索 App ID 凭证的示例")


## 在 {{site.data.keyword.iot_short_notm}} 中配置 App ID
{: #config_app_id}

必须先配置 App ID，然后才能将 App ID 令牌用于授权。要创建 App ID 配置，请使用 `POST /api/v0002/authentication/providers` API，其中 `tenant_Id` 和 `issuer` 可从[设置 App ID 服务](#set_up_app_id)的步骤 2 中获取：

请求主体：

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

请求响应：200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## 向 {{site.data.keyword.iot_short_notm}} 添加用户
{: #users_app_id}

必须添加用户并根据其角色对其授予权限。要创建用户，请使用 `POST /api/v0002/authorization/users` API，详细信息如下：

 - `issuer` 可从[设置 App ID 服务](#set_up_app_id)的步骤 2 中获取。
 - `uniqueSecurityName` 使用在 App ID 服务的身份提供者中配置的 `appIdConfigName`、`amr` 和用户电子邮件的详细信息进行填写。
 - `appIdConfigName` 是[配置 App ID](##config_app_id) 中使用的名称，例如“TestAppIdConfigName”。
 - `amr` 是认证方法引用，这是 App ID 服务中使用的身份提供者。App ID 支持以下身份提供者：

	 - Cloud Directory
	 - SAML 2.0 Federation
	 - Facebook
	 - Google

	 根据在 App ID 服务中使用的身份提供者，`amr` 将为 `cloud_directory`、`saml`、`facebook` 或 `google`。

在以下示例中，使用的是 Cloud Directory：

请求主体：

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

请求响应：200

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

## 生成 App ID 令牌
{: #token_app_id}

配置 App ID 并添加用户后，应用程序可以开始为用户生成 App ID 令牌，然后使用这些令牌来调用平台 API。有关 App ID 入门的更多信息，请参阅以下视频：[IBM Bluemix App ID ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}。

用户可以使用 App ID 端点登录，然后通过发布以下示例来检索其 App ID 令牌。

### 生成 App ID 令牌的 HTTP 示例

要生成 App ID 令牌，请使用以下 API 并通过服务凭证向 IBM Cloud 认证：

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

其中，`tenantId`、`issuer`、`clientId` 和 `secret` 可从[设置 App ID 服务](#set_up_app_id)的步骤 2 中获取。

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

grant_type=password 的请求主体：

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

grant_type= authorization_code 的请求主体：

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

请求响应：

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### 生成 App ID 令牌的 Curl 示例

以下片段显示了生成 App ID 令牌的 CURL 示例：

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

其中，`username` 和 `password` 已在 App ID 中为用户定义。

请求响应：

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### 生成 App ID 令牌的 Swagger 示例

要使用 Swagger 生成 App ID 令牌，请使用 [Swagger 示例 ![外部链接图标](../../../../icons/launch-glyph.svg "外部链接图标")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}。

填写 Swagger 参数的所有详细信息，然后单击 `/oauth/v3/{tenantId}/token` 方法末尾的**试用**。

令牌端点响应包含访问和标识令牌、可选的刷新令牌以及到期时间信息。

## 使用 App ID 令牌
{: #use_app_id}

创建 App ID 令牌后，应用程序可以开始使用该令牌来调用平台 API。App ID 令牌是在[生成 App ID 令牌](#token_app_id)中生成该令牌的响应中的 `id_token` 属性。App ID 令牌到期后，应用程序必须生成新的令牌才能继续调用平台 API。

以下示例显示了如何在调用 API 时使用 App ID 令牌。

###	使用 App ID 令牌的 HTTP 示例

`GET https://org.domain/api/v0002/bulk/devices`

输入参数|	值
----------------- | -----------
头|	Content-Type: application/json<br>Authorization: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### 使用 App ID 令牌的 Curl 示例

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
