---

copyright:
 years: 2015, 2017
lastupdated: "2017-10-04"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ゲートウェイ・デバイス用の HTTP Messaging API
{: #api}

## ゲートウェイ・デバイス用の HTTP Messaging API 資料へのアクセス
{: #rest_messaging_api}

{{site.data.keyword.iot_short_notm}} HTTP Messaging API 資料にアクセスし、ゲートウェイ・デバイスからのイベントの送信に関する詳細情報を見つけるには、[{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window} を参照してください。


## クライアント接続
{: #client_connections}

クライアント・セキュリティーの詳細と、クライアントを {{site.data.keyword.iot_short_notm}} のゲートウェイ・デバイスに接続する方法については、[{{site.data.keyword.iot_short_notm}} へのアプリケーション、デバイス、ゲートウェイの接続](../reference/security/connect_devices_apps_gw.html)を参照してください。


## イベントのパブリッシュ
{: #event_publication}

MQTT メッセージング・プロトコルの使用に加えて、HTTP Messaging API コマンドを使用することで、HTTP を介してイベントを {{site.data.keyword.iot_short_notm}} にパブリッシュするようにゲートウェイ・デバイスを構成することもできます。

{{site.data.keyword.iot_short_notm}} に接続されているデバイスから ``POST`` 要求を送信するには、以下のいずれかの URL を使用します。

### 非セキュアな POST 要求
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### セキュアな POST 要求
<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**重要事項:**
- HTTP メッセージングを使用して送信できるのは、ゲートウェイ・デバイス・イベントのみです。他のゲートウェイ・デバイス管理機能と制御機能の要求を送信するには、MQTT メッセージング・プロトコルを使用します。
- ポート 443 (デフォルトの SSL ポート) は、セキュアな HTTP API 呼び出しにも指定できます。
- ゲートウェイに*標準ゲートウェイ*役割が割り当てられていない場合、そのゲートウェイは組織内の任意のデバイスのためにイベントをパブリッシュできます。ゲートウェイに接続されているデバイスが未登録の場合、ゲートウェイはデバイスを自動的に登録します。
- デバイス許可レベルを検査する場合は、*標準ゲートウェイ*役割を割り当ててください。

ゲートウェイとリソース・グループの役割について詳しくは、[ゲートウェイ・アクセス制御 (ベータ)](../gateways/gateway-access-control.html) を参照してください。

### 認証

すべての要求には許可ヘッダーを組み込む必要があります。基本認証が、唯一サポートされる方法です。デバイスが {{site.data.keyword.iot_short_notm}} HTTP REST API を使用して HTTP 要求を行う場合、以下の資格情報が必要です。

|資格情報|必要な入力|
|:---|:---|
|ユーザー名| `g/{orgId}/{gwType}/{gwDevId}` または `g-{orgId}-{gwType}-{gwDevId}`
|パスワード| 自動生成の認証トークン、またはゲートウェイ・デバイス登録時に手動で指定した認証トークン。

各部の意味は以下のとおりです。

<dl>
<dt>orgId</dt>  
<dd>組織名。ホスト・ヘッダーに指定されている名前と一致する必要があります。</dd>

<p></p>
<dt>gwType</dt>  
<dd>ゲートウェイのタイプ。</dd>
<dd>ユーザー名でハイフン「-」文字を区切り文字として使用した場合は、この値にハイフン文字を含めることはできません。</dd>
<p></p>
<dt>gwDevId</dt>  
<dd>ゲートウェイのデバイス ID。</dd>
</dl>


### Content-Type 要求ヘッダー

コンテンツが JSON でない場合、`Content-Type` 要求ヘッダーを要求に含める必要があります。以下の表に、サポート対象タイプがどのように {{site.data.keyword.iot_short_notm}} 内部フォーマットにマップされるかを示します。

|Content-Type ヘッダー|{{site.data.keyword.iot_short_notm}} での形式 |
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml| "xml"
|application/octet-stream|"bin"

### サービス品質

　HTTP REST メッセージングは、MQTT サービス品質における「最高 1 回」送信サービス・レベル 0 と同じように、非永続メッセージ送信を行いますが、HTTP 応答を送信する前に、要求が正しいことと、サーバーに送信可能であることを検証します。HTTP 状況コード 200 を含む応答によって、メッセージがサーバーに届いたことを確認できます。「最高 1 回」の MQTT サービス品質レベルまたは HTTP の同等機能を使用してイベント・メッセージを送信する場合、デバイスまたはアプリケーションが送信を保証するための再試行ロジックを実装している必要があります。

{{site.data.keyword.iot_short_notm}} の MQTT プロトコルおよびサービス品質レベルについて詳しくは、[MQTT メッセージング](../reference/mqtt/index.html)を参照してください。

API を使用したゲートウェイ・デバイスの管理について詳しくは、[ゲートウェイ・デバイス用の HTTP REST API](../gateways/gw_api.html) を参照してください。

## コマンドの受信
{: #receive_commands}

MQTT メッセージング・プロトコルの使用に加えて、HTTP Messaging API コマンドを使用することで、HTTP を介してコマンドを {{site.data.keyword.iot_short_notm}} から受信するようにゲートウェイ・デバイスを構成することもできます。ゲートウェイ・デバイスは、関連するリソース・グループ内のデバイスに対するコマンドを受信することができます。ゲートウェイ・リソース・グループについて詳しくは、[ゲートウェイ・アクセス制御 (ベータ)](../gateways/gateway-access-control.html) を参照してください。

{{site.data.keyword.iot_short_notm}} に接続されているゲートウェイから ``POST`` 要求を送信するには、以下のいずれかの URL を使用します。

### 非セキュアな POST 要求
<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### セキュアな POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**注:** デフォルト SSL ポートのポート 443 も、セキュアな HTTP API 呼び出し用に指定できます。

オプションで、HTTP 要求の body にパラメーター *waitTimeSecs* を含めて、コマンドを待機する最大秒数を表す整数を指定することができます。
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**重要事項:**
- *waitTimeSecs* の値は、0 から 3600 までの範囲の整数にする必要があります。デフォルト値は 0 です。
- すべてのデバイス・タイプに対するコマンドを受信するには、`typeId` コンポーネントで、「あらゆる」を意味するワイルドカード文字 (+) を使用します。このワイルドカード文字を使用すると、デバイス・タイプが応答ヘッダー・フィールド *X-deviceType* に含められます。
- すべてのデバイスに対するコマンドを受信するには、`deviceId` コンポーネントで、「あらゆる」を意味するワイルドカード文字 (+) を使用します。このワイルドカード文字を使用すると、デバイス識別子が応答ヘッダー・フィールド *X-deviceId* に含められます。
- すべてのコマンドを受信するには、`command` コンポーネントで、「あらゆる」を意味するワイルドカード文字 (+) を使用します。このワイルドカード文字を使用すると、コマンド識別子が応答ヘッダー・フィールド *X-commandId* に含められます。
- HTTP 応答の状況コードが 200 である場合は、コマンド・データが応答の body に含まれています。応答ヘッダー・フィールド *Content-Type* を確認すると、コンテンツ・タイプが見つかります。
- HTTP 応答の状況コードが 204 である場合、コマンド・データはありません。
