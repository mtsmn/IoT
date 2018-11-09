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


# {{site.data.keyword.iot_short_notm}} の Cloud IAM の認証と許可 (ベータ)
{: #cloud_iam}

{{site.data.keyword.iot_full}} API は、Identity and Access Management (IAM) を使用したユーザーの認証および許可をサポートします。

**重要:** {{site.data.keyword.iot_short_notm}} の Cloud IAM の認証と許可機能は、限定されたベータ・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

Cloud IAM は IBM Cloud に組み込まれており、IBM サービスを構成および管理する必要のある管理および開発者ユーザーを認証および許可するために使用されます。Watson IoT Platform UI へのアクセスが必要なユーザーは、IBM Cloud IAM を使用して認証されます。Cloud IAM の ID のソースは、登録済みの IBM ID ユーザーにすることも、SAML をサポートする顧客のディレクトリー・サービスにすることもできます。  

{{site.data.keyword.iot_short_notm}} では、App ID を使用したユーザーの認証もサポートされます。App ID は、IBM Cloud でホストされているアプリケーションへのアクセス権が必要なユーザーを認証するために使用されます。App ID ユーザーは、一般的にクラウド・サービスでの管理や開発のアクティビティーを実行することはありません。詳しくは、[Watson IoT Platform の App ID 認証](app_id.html#app_id)を参照してください。

IAM を使用すると、ユーザーは IAM OAuth トークンを取得し、それを使用して API 呼び出しを認証できます。例えば、{{site.data.keyword.containershort_notm}} CLI をインストールしているユーザーは、以下のコマンドを使用する可能性があります。

`bx iam oauth-tokens`

このコマンドは、ログイン・ユーザーの IAM トークンを以下の形式で返します。

`IAM Token: Bearer <some token>`

ユーザーは、次の例に示すように、IAM トークン・エンドポイントを使用してログインし、IAM トークンを取得することもできます。
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

この API の例では、IAM API キーを使用して IAM トークンを要求し、`Bearer <token>` を返します。

次に、ユーザーが {{site.data.keyword.iot_short_notm}} API を呼び出すと、API 呼び出しで許可ヘッダー `Authorization: Bearer <token>` を提供できます。

## IAM トークンの生成
{: #iam_generate}

IBM Cloud での認証方法および使用する IBM Cloud ID のタイプに応じて、IAM トークンの作成を自動化する方法を選択できます。

非フェデレーテッド ID を使用する場合は、以下のいずれかのオプションを選択して、IAM トークンを作成できます。
 - **IBM Cloud ユーザー名およびパスワード:** トークンを作成し、IAM アクセス・トークンの作成を完全に自動化できます。
 - **IBM Cloud API キーの生成:** 生成対象の IBM Cloud アカウントに依存する IBM Cloud API キーを使用します。IBM Cloud API キーと別のアカウント ID を同一の IAM トークンで結合することはできません。 IBM Cloud API キーの基となっているアカウント以外のアカウントを使用して作成されたクラスターにアクセスするには、そのアカウントにログインして新しい API キーを生成する必要があります。

フェデレーテッド ID を使用する場合は、以下のいずれかのオプションを選択して、IAM トークンを作成できます。
 - **IBM Cloud API キーの生成:** IBM Cloud API キーは、生成対象の IBM Cloud アカウントに依存します。IBM Cloud API キーと別のアカウント ID を同一の IAM トークンで結合することはできません。 IBM Cloud API キーの基となっているアカウント以外のアカウントを使用して作成されたクラスターにアクセスするには、そのアカウントにログインして新しい API キーを生成する必要があります。
 - **ワンタイム・パスコードの使用: **ワンタイム・パスコードを使用して IBM Cloud で認証する場合、IAM トークンの作成を完全に自動化することはできません。ワンタイム・パスコードを取得するには、Web ブラウザーとの手動対話が必要になるためです。 IAM トークンの作成を完全に自動化するには、代わりに IBM Cloud API キーを作成する必要があります。

アクセス・トークンを作成すると、要求に含まれる本文情報は、使用する IBM Cloud 認証方式によって異なります。以下のように値を置き換えます。
- `<my_username>` = IBM Cloud ユーザー名。
- `<my_password>` = IBM Cloud のパスワード。
- `<my_api_key>` = IBM Cloud API キー。
- `<my_passcode>` = IBM Cloud ワンタイム・パスコード。 `bx login --sso` を実行し、CLI 出力の説明に従って、Web ブラウザーを使用してワンタイム・パスコードを取得します。

例: `POST https://iam.<region>.bluemix.net/oidc/token`

IBM Cloud 地域を指定するには、API エンドポイントで使用される地域の略語を確認してください。 詳しくは、[IBM Cloud region API endpoints [外部リンク・アイコン](../../icons/launch-glyph.svg)(https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions)] を参照してください。

入力パラメーター | 値
---------------- | -----------
ヘッダー | Content-Type:application/x-www-form-urlencoded<br>Authorization: Basic Yng6Yng=<br>注: Yng6Yng= は、ユーザー名 bx とパスワード bx の URL エンコードされた許可です。
IBM Cloud ユーザー名とパスワードの本文	|	grant_type: password<br>response_type: cloud_iam, uaa<br>username: `<my_username>`<br>password: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>注: uaa_client_secret キーは値を指定せずに追加します。
IBM Cloud API キーの本文	|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>注: uaa_client_secret キーは値を指定せずに追加します。
IBM Cloud ワンタイム・パスコードの本文	|	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>注: uaa_client_secret キーは値を指定せずに追加します。

API 出力例:

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
IAM トークンは、API 出力の **access_token** フィールドで確認できます。

以下の例は、API の呼び出し中に IAM トークンを使用する方法を示しています。

```
GET https://org.domain/api/v0002/bulk/devices
```

入力パラメーター  |	値
----------------- | -----------
ヘッダー|	Content-Type: application/json<br>Authorization: bearer `<iam_token>`
