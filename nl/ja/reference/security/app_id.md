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

# Watson IoT Platform の App ID 認証 (ベータ)
{: #app_id}

App ID は、IBM Cloud でホストされているアプリケーションへのアクセス権が必要なユーザーを認証するために使用されます。これらのユーザーは、モバイルまたはサービスが提供する Web アプリケーションを介してサービスにアクセスします。
{: shortdesc}

**重要:** {{site.data.keyword.iot_short_notm}} の App ID の認証と許可機能は、限定されたベータ・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} では、Cloud IAM を使用したユーザーの認証もサポートされます。Cloud IAM は IBM Cloud に組み込まれており、IBM サービスを構成および管理する必要のある管理および開発者ユーザーを認証および許可するために使用されます。Cloud IAM について詳しくは、[Watson IoT Platform の Cloud IAM の認証と許可](cloud_iam.html#cloud_iam)を参照してください。

App ID ユーザーは、一般的にクラウド・サービスでの管理や開発のアクティビティーを実行することはなく、{{site.data.keyword.iot_short_notm}} Web ダッシュボードにログインできません。ダッシュボードにログインできるのは、Cloud IAM ユーザーのみです。

{{site.data.keyword.iot_short_notm}} API では IBM Cloud App ID サービスからのユーザーの認証がサポートされます。App ID サービスのインスタンスを使用するように {{site.data.keyword.iot_short_notm}} 組織を構成し、App ID ユーザーを組織に追加できます。アプリケーションは、これらのユーザーを App ID で認証します。その後、これを使用して {{site.data.keyword.iot_short_notm}} API を呼び出すことができます。IBM Cloud App ID サービスについて詳しくは、[IBM Cloud App ID Getting Started Tutorial![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/services/appid/index.html){: new_window} を参照してください。

現在の App ID のサポートは非メッセージング REST API 用で、これにはデバイス、ユーザー、およびその他の {{site.data.keyword.iot_short_notm}} 機能を構成および管理するための API が含まれます。App ID を使用して、メッセージをパブリッシュおよびサブスクライブすることはできません。

通常、非管理ユーザーの代わりに呼び出される REST API を認証するには、App ID を使用します。開発者は、管理アクセス権を持たない保守技術者など、登録されているユーザーの代わりに App ID トークンを使用して REST API を呼び出すアプリケーションを作成できます。単一の認証トークンではなく App ID を使用して非管理ユーザーを認証する利点は、{{site.data.keyword.iot_short_notm}} の監査証跡により、共有認証トークンの代わりに REST API を呼び出したユーザーが示されることです。App ID は、{{site.data.keyword.iot_short_notm}} コマンド・ライン・インターフェース (CLI) を使用する必要のある管理者以外の認証にも役立ちます。

**重要:** {{site.data.keyword.iot_short_notm}} REST API の呼び出しは、ブラウザーまたはモバイル・アプリから直接ではなく、ユーザー・アプリケーションを使用して行う必要があります。ユーザー・アプリケーションでは、追加のアクセス制御レベルを指定して、特定のユーザーがアクセスできるデバイス機能を決定したり、デバイスに送信される MQTT メッセージを検証したりできます。以下の図は、この呼び出しのフローを示しています。

![ユーザー・アプリケーションを使用した REST API の呼び出し](images/app_id_user_app.PNG)

App ID ユーザーは、Cloud IAM ユーザーが使用するのと同じプロセスを使用して {{site.data.keyword.iot_short_notm}} に登録する必要があり、両方ともダッシュボード・インターフェースに表示されます。

App ID ユーザーは通常、管理者ではないので、アクセスを許可するリソースとコマンドを考慮してから、{{site.data.keyword.iot_short_notm}} リソース・レベル・アクセス制御の設定を適切に設定することが重要です。ほとんどの場合、App ID ユーザーには、{{site.data.keyword.iot_short_notm}} 組織の定義済みのデフォルトの役割よりも限定的なカスタム役割が割り当てられます。役割について詳しくは、[ユーザー、アプリケーション、ゲートウェイの役割](../../roles_index.html#user-application-and-gateway-roles)を参照してください。

デフォルトでは、App ID ユーザーはすべてのデバイスにアクセスできます。App ID ユーザーが管理できるデバイスを制限されたリストのデバイスのみにする場合は、App ID ユーザーにリソース・グループを割り当てます。

**注:** {{site.data.keyword.iot_short_notm}} では、登録済みユーザーの総数が 1,000 に制限されています。

## App ID サービスのセットアップ
{: #set_up_app_id}

App ID サービスをセットアップする前に、IBM Cloud App ID サービスのインスタンスを追加し、ID プロバイダーを構成する必要があります。App ID のインスタンスがまだ存在していない場合は、[IBM Cloud サービス・カタログ ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog){: new_window} からいずれか 1 つをプロビジョンできます。

App ID サービスをセットアップしたら、サービス資格情報を取得して、App ID を {{site.data.keyword.iot_short_notm}} 組織のプロバイダーとして構成する必要があります。サービス資格情報は、App ID サービス UI で取得または作成できます。

1. IBM Cloud ダッシュボードで、App ID サービスを選択し、**「サービス資格情報」**をクリックします。
2. **「資格情報の表示」**をクリックして、資格情報にある `tenantId`、`clientId`、および `secret` を見つけます。これらの値は、後の手順で App ID を構成するために必要です。発行者は、`appid-oauth.ng.bluemix.net` など、`oauthServerUrl` のホスト名になります。

以下の図の例は、資格情報を取得する方法を示しています。

![App ID 資格情報の取得](images/app_id_credentials.PNG "App ID 資格情報を取得する例")


## {{site.data.keyword.iot_short_notm}} での App ID の構成
{: #config_app_id}

許可に App ID トークンを使用する前に、App ID を構成する必要があります。App ID 構成を作成するには、`POST /api/v0002/authentication/providers` API を使用します。`tenant_Id` および `issuer` は、[App ID サービスのセットアップ](#set_up_app_id)の手順 2 で取得されます。

Request Body:

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

Request Response: 200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## {{site.data.keyword.iot_short_notm}} へのユーザーの追加
{: #users_app_id}

ユーザーを追加し、その役割に基づいて許可を付与する必要があります。ユーザーを作成するには、`POST /api/v0002/authorization/users` API を使用し、以下の詳細を指定します。

 - `issuer` は、[App ID サービスのセットアップ](#set_up_app_id)の手順 2 で取得されます。
 - `uniqueSecurityName` には、App ID サービスの ID プロバイダーで構成されている `appIdConfigName`、`amr`、およびユーザーの E メールを入力します。
 - `appIdConfigName` は、「TestAppIdConfigName」など、[App ID の構成](##config_app_id)で使用される名前です。
 - `amr` は認証方式参照です。これは、App ID サービスで使用される ID プロバイダーです。App ID は、以下の ID プロバイダーをサポートします。

	 - クラウド・ディレクトリー
	 - SAML 2.0 フェデレーション
	 - Facebook
	 - Google

	 App ID サービスで使用される ID プロバイダーに基づいて、`amr` は `cloud_directory`、`saml`、`facebook`、または `google` になります。

以下の例では、クラウド・ディレクトリーが使用されています。

Request Body:

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

Request Response: 200

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

## APP ID トークンの生成
{: #token_app_id}

App ID を構成してユーザーを追加した後、アプリケーションでは、ユーザーの App ID トークンの生成を開始し、それらを使用してプラットフォーム API を呼び出すことができます。App ID の概説については、ビデオ [IBM Bluemix App ID![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window} を参照してください。

ユーザーは、以下の例を POST することによって、App ID エンドポイントを使用してログインし、App ID トークンを取得できます。

### App ID トークンを生成する HTTP の例

App ID トークンを生成するには以下の API を使用し、サービス資格情報を使用して IBM Cloud に対して認証します。

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

ここで、`tenantId`、`issuer`、`clientId`、および `secret` は、[App ID サービスのセットアップ](#set_up_app_id)の手順 2 から取得されます。

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

grant_type=password の要求本文:

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

grant_type= authorization_code の要求本文:

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
```

要求の応答:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### App ID トークンの Curl の例

以下のスニペットは、App ID トークンを生成する CURL の例を示しています。

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

ここで、`username` および `password` は、App ID でユーザーに対して定義されています。

Request Response:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### App ID トークンを生成する Swagger の例

Swagger を使用して App ID トークンを生成するには、[Swagger の例![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window} を使用します。

Swagger パラメーターにすべての詳細を入力し、メソッド `/oauth/v3/{tenantId}/token` の最後にある**「Try it out」**をクリックします。

トークン・エンドポイント応答には、アクセスおよび ID トークン、オプションのリフレッシュ・トークン、および有効期限情報が含まれます。

## App ID トークンの使用
{: #use_app_id}

App ID トークンが作成されたら、アプリケーションはそれを使用してプラットフォーム API を呼び出すことができます。App ID トークンは、[App ID トークンの生成](#token_app_id)で App ID トークンが生成された、応答内の `id_token` プロパティーです。App ID トークンの有効期限が切れると、アプリケーションは、プラットフォーム API の呼び出しを続行するために新規トークンを生成する必要があります。

以下の例は、API の呼び出し中に App ID トークンを使用する方法を示しています。

###	App ID トークンを使用する HTTP の例

`GET https://org.domain/api/v0002/bulk/devices`

入力パラメーター  |	値
----------------- | -----------
ヘッダー	|	Content-Type: application/json<br>Authorization: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### App ID トークンを使用する Curl の例

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
