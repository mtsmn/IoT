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

# ゲートウェイ・デバイス用の HTTP API
{: #api_link}

{{site.data.keyword.iot_full}} で使用できる HTTP API には 2 つのタイプがあります。HTTP REST API は、
ゲートウェイ・デバイスを作成、更新、リスト、および削除するために使用できます。HTTP Messaging API は、ゲートウェイ・デバイスからクラウドにイベント情報を送信したり、クラウド内のアプリケーションからコマンドを受け入れたりするために使用できます。 


## クライアント接続
{: #client_connections}

クライアント・セキュリティーの詳細と、クライアントを {{site.data.keyword.iot_short_notm}} のゲートウェイ・デバイスに接続する方法については、[{{site.data.keyword.iot_short_notm}} へのアプリケーション、デバイス、ゲートウェイの接続](../reference/security/connect_devices_apps_gw.html)を参照してください。

**注:** HTTP ポート 1883 はデフォルトでは無効になっています。デフォルト設定の変更について詳しくは、[セキュリティー・ポリシーの構成](../reference/security/set_up_policies.html#set_up_policies.md)を参照してください。


## 認証

すべての要求には許可ヘッダーを組み込む必要があります。 基本認証が、唯一サポートされる方法です。 デバイスが {{site.data.keyword.iot_short_notm}} HTTP REST API を使用して HTTP 要求を行う場合、以下の資格情報が必要です。

|資格情報|必要な入力|
|:---|:---|
|ユーザー名| `g/{orgId}/{gwType}/{gwDevId}` または `g-{orgId}-{gwType}-{gwDevId}`
|パスワード| 自動生成の認証トークン、またはゲートウェイ・デバイス登録時に手動で指定した認証トークン。

各部の意味は、次のとおりです。

<dl>
<dt>orgId</dt>  
<dd>組織名。ホスト・ヘッダーに指定されている名前と一致する必要があります。</dd>

<p></p>
<dt>gwType</dt>  
<dd>ゲートウェイのタイプ。</dd> 
<dd>ユーザー名でハイフン「-」文字を区切り文字として使用した場合は、この値にハイフン文字を含めることはできません。 </dd>
<p></p>
<dt>gwDevId</dt>  
<dd>ゲートウェイのデバイス ID。 </dd>
</dl>


## Content-Type 要求ヘッダー

`Content-Type` 要求ヘッダーを要求に含める必要があります。 以下の表に、サポート対象タイプがどのように {{site.data.keyword.iot_short_notm}} 内部フォーマットにマップされるかを示します。

|Content-Type ヘッダー|{{site.data.keyword.iot_short_notm}} での形式|
|:---|:---|
|text/plain|"text"
|application/json| "json"
|application/xml | "xml"
|application/octet-stream|"bin"

## サービス品質

HTTP REST メッセージングは、MQTT サービス品質における「最高 1 回」送信サービス・レベル 0 と同じように、非永続メッセージ送信を行いますが、HTTP 応答を送信する前に、要求が正しいことと、サーバーに送信可能であることを検証します。 HTTP 状況コード 200 を含む応答によって、メッセージがサーバーに届いたことを確認できます。 「最高 1 回」の MQTT サービス品質レベルまたは HTTP の同等機能を使用してイベント・メッセージを送信する場合、デバイスまたはアプリケーションが送信を保証するための再試行ロジックを実装している必要があります。

{{site.data.keyword.iot_short_notm}} の MQTT プロトコルおよびサービス品質レベルについて詳しくは、[MQTT メッセージング](../reference/mqtt/index.html)を参照してください。

HTTP Messaging API を使用して、ゲートウェイ・デバイスからクラウドにイベントをパブリッシュしたり、クラウド内のアプリケーションからコマンドを受け取ったりすることができます。

## イベントのパブリッシュ
{: #event_publication}

MQTT メッセージング・プロトコルの使用に加えて、HTTP Messaging API コマンドを使用することで、HTTP を介してイベントを {{site.data.keyword.iot_short_notm}} にパブリッシュするようにゲートウェイ・デバイスを構成することもできます。

{{site.data.keyword.iot_short_notm}} に接続されているデバイスから ``POST`` 要求を送信するには、以下のいずれかの URL を使用します。

### イベントをパブリッシュするための非セキュアな POST 要求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### イベントをパブリッシュするためのセキュアな POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**重要事項:**
- ポート 443 (デフォルトの SSL ポート) は、セキュアな HTTP API 呼び出しにも指定できます。
- ゲートウェイに*標準ゲートウェイ*役割が割り当てられていない場合、そのゲートウェイは組織内の任意のデバイスのためにイベントをパブリッシュできます。 ゲートウェイに接続されているデバイスが未登録の場合、ゲートウェイはデバイスを自動的に登録します。
- デバイス許可レベルを検査する場合は、*標準ゲートウェイ*役割を割り当ててください。

ゲートウェイとリソース・グループの役割について詳しくは、[ゲートウェイ・アクセス制御](../gateways/gateway-access-control.html) を参照してください。

## コマンドの受信
{: #receive_commands}

MQTT メッセージング・プロトコルの使用に加えて、HTTP messaging API コマンドを使用することで、HTTP を介してコマンドを {{site.data.keyword.iot_short_notm}} から受信するようにゲートウェイ・デバイスを構成することもできます。 ゲートウェイ・デバイスは、関連するリソース・グループ内のデバイスに対するコマンドを受信することができます。 ゲートウェイ・リソース・グループについて詳しくは、[ゲートウェイ・アクセス制御](../gateways/gateway-access-control.html) を参照してください。

{{site.data.keyword.iot_short_notm}} に接続されているゲートウェイから ``POST`` 要求を送信するには、以下のいずれかの URL を使用します。

### コマンドを受信するための非セキュアな POST 要求

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### コマンドを受信するためのセキュアな POST 要求

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**注:** デフォルト SSL ポートのポート 443 も、セキュアな HTTP API 呼び出し用に指定できます。

オプションで、HTTP 要求の body にパラメーター *waitTimeSecs* を含めて、コマンドを待機する最大秒数を表す整数を指定することができます。
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**重要事項:**
- *waitTimeSecs* の値は、0 から 3600 までの範囲の整数にする必要があります。 デフォルト値は 0 です。
- すべてのデバイス・タイプに対するコマンドを受信するには、`typeId` コンポーネントで、「あらゆる」を意味するワイルドカード文字 (+) を使用します。 このワイルドカード文字を使用すると、デバイス・タイプが応答ヘッダー・フィールド *X-deviceType* に含められます。
- すべてのデバイスに対するコマンドを受信するには、`deviceId` コンポーネントで、「あらゆる」を意味するワイルドカード文字 (+) を使用します。 このワイルドカード文字を使用すると、デバイス識別子が応答ヘッダー・フィールド *X-deviceId* に含められます。
- すべてのコマンドを受信するには、`command` コンポーネントで、「あらゆる」を意味するワイルドカード文字 (+) を使用します。 このワイルドカード文字を使用すると、コマンド識別子が応答ヘッダー・フィールド *X-commandId* に含められます。
- HTTP 応答の状況コードが 200 である場合は、コマンド・データが応答の body に含まれています。 応答ヘッダー・フィールド *Content-Type* を確認すると、コンテンツ・タイプが見つかります。
- HTTP 応答の状況コードが 204 である場合、コマンド・データはありません。

{{site.data.keyword.iot_short_notm}} HTTP REST API にアクセスするには、[{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}を参照してください。

{{site.data.keyword.iot_short_notm}} HTTP Messaging API にアクセスするには、[{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}を参照してください。

## 最新イベント・キャッシュ
{: #last-event-cache}

最新イベント・キャッシュを使用して、接続したデバイスから {{site.data.keyword.iot_short_notm}} に送信された最新のイベントに関する情報を保管できます。[API ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window} を使用することで、デバイスの特定のイベント ID について記録された値、または特定のデバイスにより報告されたイベント ID ごとに最後に記録された値を取得できます。 

### 最新イベント・キャッシュの構成

最新イベント・キャッシュ機能は、デフォルトでは無効になっていますが、以下の方法でこの機能を有効にすることができます。 

-	API を使用して *enabled* パラメーターを **true** に設定します。
-	{{site.data.keyword.iot_short_notm}} ダッシュボードの*「設定」*ページで、**「LEC の有効化 (Enable LEC)」**を**「オン (On)」**に設定します。

キャッシュにイベント・データを保管する時間を指定するには、存続時間 (TTL) 値を使用します。特定のイベントの最新イベント・データは、デフォルトで 7 日間保管されます。この期間は、API を使用して **ttlDays** パラメーターを更新するか、{{site.data.keyword.iot_short_notm}} ダッシュボードの*「設定」*ページで**「イベント・データ TTL (Event Data TTL)」**フィールドの値を選択することによって変更できます。

ライト・プランの場合、最小で 1 日、最大で 7 日間の情報を保管できます。その他のプランの場合、最小で 1 日、最大で 45 日間の情報を保管できます。

以下の情報を使用して、API により最新イベント・キャッシュ設定を構成できます。

組織の最新イベント・キャッシュの現在の構成を取得するには、以下の API を使用します。

```
    GET /api/v0002/config/lec
```   

   
以下の例は、GET メソッドへの成功の応答を示します。
```
{
    "enabled": false,
    "ttlDays": 7
}
```
組織内の個人またはアプリケーションは、このエンドポイントへのアクセスを許可されます。 

{{site.data.keyword.iot_short_notm}} 管理者が組織に対して最新イベント・キャッシュを無効にした場合、GET メソッドへの応答で以下の HTTP 403 FORBIDDEN 状況コードが返されます。

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: この組織に対して最新イベント・キャッシュが無効になっています。詳しくは、サポート・チームに連絡してください。 (Last event cache is disabled for this organization. For more information, contact your support team.)"
}
```

最新イベント・キャッシュが有効になっていないと、GET メソッドへの応答で HTTP 404 状況コードが返されます。組織に対して最新イベント・キャッシュ機能を有効にするには、以下の API を使用します。

```
    PUT /api/v0002/config/lec
```

使用するサンプル・ペイロードは以下のとおりです。 
```
{
    "enabled": true,
    "ttlDays": 5
}
```
各部の意味は、次のとおりです。 

- **enabled** は必須で、最新イベント・キャッシュが組織に対して有効かどうかを指定します。値はブール値である必要があります。 

- **ttlDays** はオプションで、組織の最新イベント・キャッシュ内にイベントを保持しておく日数を指定します。値は 1 から 45 の間 (1 と 45 を含む) の整数である必要があります。
   
 
管理者、オペレーター・ユーザー、またはオペレーター・アプリケーションのみがこのエンドポイントへのアクセスを許可されます。パラメーターまたは JSON が無効の場合、PUT メソッドへの応答で HTTP 401 BAD REQUEST 状況コードが返されます。 

**注:** 構成変更が完了するまでに最大で 30 秒ほどかかります。

### 最新イベント・キャッシュからの情報の取得

API を使用して、デバイスによって送信された最新のイベントを取得することができます。 デバイスの物理的位置や使用状況に関係なくデバイスの状況を取得できます。 特定のデバイスで 1 つのイベント ID について最後に記録された値を取得することも、特定のデバイスで報告された各イベント ID について最後に記録された値を取得することもできます。 過去 45 日以内に発生したものであれば、特定のイベントに関するデバイスの最新イベント・データを取得できます。

特定のイベント ID について最新の値を要求するには、以下のような API 要求を使用します。この要求では、「power」というイベント ID について最後に記録された値が返されます。

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

以下のような JSON 形式で応答が返されます。

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**注:** API 応答は JSON 形式ですが、イベント・ペイロードは任意の形式で記述できます。 最終イベント・キャッシュ API から返されるペイロードは base64 でエンコードされます。

1 つのデバイスで報告された各イベント ID について最新の値を要求するには、以下のような API 要求を使用します。

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

応答には、そのデバイスから送信されたすべてのイベント ID が組み込まれます。 以下の例では、「power」と「temperature」というイベントの値が返されます。

```
[
    {
        "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

{{site.data.keyword.iot_short_notm}} 最新イベント・キャッシュ API にアクセスするには、[{{site.data.keyword.iot_short_notm}} HTTP Information Management API ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window} を参照してください。
