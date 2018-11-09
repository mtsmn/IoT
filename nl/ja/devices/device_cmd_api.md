---

copyright:
 years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# デバイス用の HTTP Messaging API
{: #api}


## デバイス用の HTTP Messaging API 資料へのアクセス
{: #rest_messaging_api}

{{site.data.keyword.iot_short_notm}} HTTP Messaging API 資料にアクセスするには、[{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}を参照してください。


## クライアント接続
{: #client_connections}

クライアント・セキュリティーの詳細と、クライアントを {{site.data.keyword.iot_short_notm}} のデバイスに接続する方法については、[{{site.data.keyword.iot_short_notm}} へのアプリケーション、デバイス、ゲートウェイの接続](../reference/security/connect_devices_apps_gw.html)を参照してください。

## イベントのパブリッシュ 
{: #event_publication}

MQTT メッセージング・プロトコルの使用に加えて、HTTP REST API コマンドを使用して、HTTP を介してイベントを {{site.data.keyword.iot_short_notm}} にパブリッシュするようにデバイスを構成することもできます。

{{site.data.keyword.iot_short_notm}} に接続されているデバイスから ``POST`` 要求を送信するには、以下のいずれかの URL を使用します。

### イベントをパブリッシュするための非セキュアな POST 要求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### イベントをパブリッシュするためのセキュアな POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**注:** デフォルト SSL ポートのポート 443 も、セキュアな HTTP API 呼び出し用に指定できます。

### 認証

すべての要求には許可ヘッダーを組み込む必要があります。 基本認証が、唯一サポートされる方法です。 デバイスが {{site.data.keyword.iot_short_notm}} HTTP REST API を使用して HTTP 要求を行う場合、以下の資格情報が必要です。

|資格情報|必要な入力|
|:---|:---|
|ユーザー名|`use-token-auth`
|パスワード| 自動生成の認証トークン、またはデバイス登録時に手動で指定した認証トークン。


### Content-Type 要求ヘッダー

コンテンツが JSON でない場合、`Content-Type` 要求ヘッダーを要求に含める必要があります。 以下の表に、サポート対象タイプがどのように {{site.data.keyword.iot_short_notm}} 内部フォーマットにマップされるかを示します。

|Content-Type ヘッダー|{{site.data.keyword.iot_short_notm}} での形式|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"


## コマンドの受信
{: #receive_commands}

MQTT メッセージング・プロトコルの使用に加えて、HTTP Messaging API コマンドを使用することで、HTTP を介してコマンドを {{site.data.keyword.iot_short_notm}} から受信するようにデバイスを構成することもできます。 デバイスはそれ自体に対するコマンドを受信することができます。

{{site.data.keyword.iot_short_notm}} に接続されているデバイスから ``POST`` 要求を送信するには、以下のいずれかの URL を使用します。

### コマンドを受信するための非セキュアな POST 要求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### コマンドを受信するためのセキュアな POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**注:** デフォルト SSL ポートのポート 443 も、セキュアな HTTP API 呼び出し用に指定できます。

オプションで、HTTP 要求の body にパラメーター *waitTimeSecs* を含めて、コマンドを待機する最大秒数を表す整数を指定することができます。
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**重要事項:**
- *waitTimeSecs* の値は、0 から 3600 までの範囲の整数にする必要があります。 デフォルト値は 0 です。
- すべてのコマンドを受信するには、{command} コンポーネントで、「あらゆる」を意味するワイルドカード文字 (+) を使用します。 このワイルドカード文字を使用すると、コマンド識別子が応答ヘッダー・フィールド *X-commandId* に含められます。
- HTTP 応答の状況コードが 200 である場合は、コマンド・データが応答の body に含まれています。 応答ヘッダー・フィールド *Content-Type* を確認すると、コンテンツ・タイプが見つかります。
- HTTP 応答の状況コードが 204 である場合、コマンド・データはありません。


デバイスの開発については、[{{site.data.keyword.iot_short_notm}} でのデバイスの開発](../devices/device_dev_index.html)を参照してください。
