---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ステップバイステップ・ガイド 2 の追加情報 - 湿度デバイスの論理インターフェースの構成

以下の情報を使用して、2 つの湿度デバイスのセンサーが IBM Watson™ IoT Platform にイベントをパブリッシュするシナリオを作成します。湿度デバイスからの読み取り値は、単一の湿度読み取り値にマップされます。デバイスの 1 つは会議室 1 にあり、もう 1 つのデバイスは会議室 2 にあります。


## このタスクについて

[ステップバイステップ・ガイド 1](../GA_information_management/ga_im_index_scenario.html) で使用したものと同じ {{site.data.keyword.iot_short_notm}} 組織インスタンスとその組織の API キーまたはトークンを使用します。 

この構成は、[ステップバイステップ・ガイド 2](im_index_scenario_thing.html) の資料で説明されているチュートリアルの前提条件です。

このシナリオでは、1 つのデバイス・タイプと 2 つのデバイス・インスタンスが作成されます。

デバイス・インスタンスは、*humiditySensor1* と *humiditySensor2* という名前です。デバイスは湿度イベントをパブリッシュします。湿度イベントのペイロードは以下の例のとおりです。
```
{
  "hum" : 35
}
```
湿度イベントは、トピック `iot-2/evt/humevt/fmt/json` でパブリッシュされます。 

**注:** イベント ID は *humevt* です。 この ID は、これらのタイプの湿度イベントを物理インターフェースに追加する場合と、このタイプのインバウンド・イベントに関連するプロパティーを論理インターフェースのプロパティーに対応付けるマッピングを定義する場合に必要になります。このシナリオでは、論理インターフェースで **humidity** というプロパティーを定義します。 この論理インターフェースでは、このタイプのデバイスの状態を以下の構造で記述します。
```
{
  "humidity" : <current humidity value in Celsius>
}
```

以下のサンプル･シナリオを使用して独自のインターフェース環境をセットアップします。

**重要事項:** curl 応答で生成される ID はその他のタスクの完了に必要であるため、この ID を保存しておく必要があります。

## 必要に応じて、デバイス・タイプとデバイスを追加します。
{: #step14}

このシナリオでは、1 つのデバイス・タイプと 2 つのデバイス・インスタンスを想定します。 デバイス・インスタンス *humiditySensor1* と *humiditySensor2* はデバイス・タイプ *HumiditySensor* と関連付けられます。 

[{{site.data.keyword.iot_short_notm}} ダッシュボード ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://internetofthings.ibmcloud.com){: new_window} を使用して、または REST API を使用して、デバイス・タイプおよびデバイスを作成できます。{{site.data.keyword.iot_short_notm}} IoT Platform ダッシュボードを使用してデバイス・タイプおよびデバイスを追加する方法について詳しくは、[Web インターフェースを使用したデータ管理の概説](../GA_information_management/im_ui_flow.html)の資料を参照してください。

以下の例は、REST API を使用して *HumiditySensor* というデバイス・タイプを作成する方法を示しています。

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**重要事項:** デバイス・タイプおよびデバイスの作成時にメタデータを追加できます。このシナリオでは、デバイス・タイプ *HumiditySensor* に以下のメタデータを追加します。
```
{
    "maxHumidityThreshold": 70
}
```
このメタデータは、{{site.data.keyword.iot_short_notm}} が、デバイス状態の *humidity* プロパティー値が 70% を超える原因となる湿度イベントを *humiditySensor1* または *humiditySensor2* から受信するとトリガーされる[ルールを作成する](im_rules.html)場合に使用できます。 

## デバイスの登録

以下の例は、*humiditySensor1* というデバイス・インスタンスを登録する方法を示しています。
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
以下の例は、*humiditySensor2* というデバイス・インスタンスを登録する方法を示しています。
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**注:** デバイスが Watson IoT Platform HTTP REST API を介して HTTP 要求を行う場合、ユーザー名とパスワードが必要です。パスワードは、デバイス登録時に自動生成された、または手動で指定した認証トークンの値です。MQTT クライアントを使用している場合は、デバイスの認証トークンをメモしておく必要があります。このトークンは、IoT トピック・ストリングをサブスクライブすることによってデバイスまたはモノの状態を取得する際に必要になります。

## 手順 1: イベント・スキーマ・ファイルを作成する
{: #step1}

このシナリオでは、イベント・スキーマ・ファイルを作成して、インバウンド湿度イベントの構造を定義します。

*humEventSchema.json* というスキーマ・ファイルを作成する例を以下に示します。 このファイルでは、湿度デバイスから送られてくるインバウンド・イベントの構造を定義します。

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "defines the structure of a humidity event",
    "properties" : {
        "humidity" : {
            "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
      "humidity"
    ]
}
```
**ヒント:** 1 つまたは複数のプロパティーを必須に指定するには、**required** パラメーターを使用します。 {{site.data.keyword.iot_short_notm}} によってデバイス状態をデバイス・データで更新するには、デバイス・メッセージに必須プロパティーが含まれていなければなりません。 必須プロパティーが含まれていないメッセージは、処理されません。   


## 手順 2: イベント・タイプのイベント・スキーマ・リソースを作成する
{: #step2}

イベント・スキーマ・リソースを作成するには、以下の API を使用します。

```
POST /draft/schemas
```

スキーマ定義ファイルは、マルチパート POST (multipart/form-data) 内で Watson IoT Platform に渡されます。POST の本体には、少なくとも 2 つのパーツが含まれている必要があります。

- 1 つは、パーツの本体としてのファイルの実際の内容を含む **schemaFile** という名前のパーツ。
- もう 1 つは、パーツの本体にあるスキーマ・リソースの名前を定義するストリングを含む **name** という名前のパーツ。

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) の資料を参照してください。

cURL を使用して *humEventSchema.json* というイベント・スキーマ・リソースを作成する例を以下に示します。

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**注:** サンプル許可値 `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` は、
`{API Key}:{authorization token}` という情報で構成されており、Base64 でエンコードされます。

この POST メソッドに対する応答の例を以下に示します。

```
{
  "name" : "humEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "humEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e20",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
イベント・タイプにイベント・スキーマを追加する時には、この POST メソッドへの応答で返されたスキーマ ID *5846cd7c6522050001db0e20* が必要になります。



## 手順 3: イベント・スキーマを参照するイベント・タイプを作成する
{: #step3}

各イベント・タイプでは、イベント・スキーマ・リソースの作成時に使用した POST メソッドへの応答で返されたスキーマ ID を使用して、該当するイベント・スキーマを参照します。

イベント・タイプを作成するには、以下の API を使用します。

```
POST /draft/event/types
```
ドラフト・イベント・タイプは、インバウンド MQTT イベントの構造を定義するスキーマ定義を参照する必要があります。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) の資料を参照してください。


cURL を使用して、湿度イベントのイベント・タイプを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

イベント・タイプにイベント・スキーマを追加する時には、このスキーマ ID *5846cd7c6522050001db0e20* を使用します。 この ID は、イベント・スキーマ・リソース *humEventSchema.json* の作成時に使用した POST メソッドへの応答で返された ID です。

この POST メソッドに対する応答の例を以下に示します。

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e20",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20"
  },
  "name" : "humEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e21",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

物理インターフェースにイベント・タイプを追加する時には、この POST メソッドへの応答で返されたイベント・タイプ ID *5846cd7c6522050001db0e21* を使用します。


## 手順 4: 物理インターフェースを作成する
{: #step7}

物理インターフェースを作成するには、以下の API を使用します。

```
POST /draft/physicalinterfaces
```

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) の資料を参照してください。

このシナリオでは、前に作成したイベント・タイプに対して 1 つの物理インターフェースが必要です。

cURL を使用して物理インターフェースを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

摂氏で測定する温度イベントを物理インターフェースに追加する時に呼び出す POST メソッドの URL では、この応答で返された物理インターフェース ID *5846cd7c6522050001db0e22* を使用します。


## 手順 5: 物理インターフェースにイベント・タイプを追加する
{: #step8}

物理インターフェースにイベント・タイプを追加するには、以下の API を使用します。

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) の資料を参照してください。

このシナリオでは、以下のイベント・タイプを指定の物理インターフェースに追加します。
- 湿度イベント *humevt* を ID *5846cd7c6522050001db0e22* の物理インターフェースに追加します。その際に、トピックから送られてくる *eventId* と、イベント・スキーマ・リソースの作成時に返される *eventTypeId* を使用します。

cURL を使用して、湿度イベント *humevt* を ID *5846cd7c6522050001db0e22* の物理インターフェースに追加する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## 手順 6: 物理インターフェースを接続するためにデバイス・タイプを更新する
{: #step9}

デバイス・タイプを更新するには、以下の API を使用します。

```
POST /draft/device/types/{typeId}/physicalinterface
```

ここで *typeId* はデバイス・タイプ ID です。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) の資料を参照してください。

このシナリオでは、デバイス・タイプ *HumiditySensor* が更新されて、物理インターフェース *5846cd7c6522050001db0e22* に接続されます。

cURL を使用してデバイス・タイプ *HumiditySensor* を更新する例を以下に示します。

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

この POST メソッドに対する応答の例を以下に示します。
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
物理インターフェースと論理インターフェースを追加する時に、デバイス ID *HumiditySensor* が必要になります。
  


## 手順 7: 論理インターフェース・スキーマ・ファイルを作成する
{: #step4}

*hygrometer.json* という論理インターフェース・スキーマ・ファイルを作成する例を以下に示します。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema that defines the canonical interface for a hygrometer",
    "properties" : {
        "humidity" : {
            "description" : "percentage humidity",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## 手順 8: 論理インターフェース・スキーマ・リソースを作成する
{: #step5}
cURL を使用して論理インターフェースを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
この POST メソッドに対する応答の例を以下に示します。
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometer",
  "version" : "draft"
}
```

## 手順 9: 論理インターフェース・スキーマを参照する論理インターフェースを作成する
{: #step6}

論理インターフェースを作成するには、以下の API を使用します。

```
POST /draft/logicalinterfaces
```

オプションで、論理インターフェースに対して意味のある別名を指定することができます。別名は、自動生成される論理インターフェース ID を使用する代わりに、デバイスの状態を取得するために使用する API 呼び出しやトピック・ストリング・サブスクリプション内で参照できます。

**注:** 別名は、1 から 36 文字までの長さでなければならず、英数字、ハイフン、ピリオド、下線を含めることができます。別名を 24 文字の 16 進ストリングにすることはできません。 

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) の資料を参照してください。

このシナリオでは、論理インターフェースに論理インターフェース・スキーマを追加する際に、前の応答で返されたスキーマ ID *5846cd7c6522050001db0e23* を使用します。

cURL を使用して論理インターフェースを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometermometer",
  "version" : "draft"
}
```
このシナリオでは、デバイス・タイプに論理インターフェースを追加する際に、POST メソッドへの応答で返された論理インターフェース ID *5846cd7c6522050001db0e24* を使用します。 インバウンド・デバイス・イベントを論理インターフェースで定義するプロパティーに対応付ける時にも、この ID を使用します。 論理インターフェース別名 *IHygrometer* を使用して、HTTP REST API を使用するかトピック・ストリングをサブスクライブすることによって、デバイスの状態を取得できます。


## 手順 10: デバイス・タイプに論理インターフェースを追加する

cURL を使用して、論理インターフェース・スキーマ ID 5846cd7c6522050001db0e23 を参照する論理インターフェース 5846cd7c6522050001db0e24 をデバイス・タイプ HumiditySensor に追加する例を以下に示します。
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
この POST メソッドに対する応答の例を以下に示します。
```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Hygrometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846cd7c6522050001db0e24",
  "schemaId" : "5846cd7c6522050001db0e23"
}
```

## 手順 11: デバイス・タイプにマッピングを追加する

    
cURL を使用してデバイス・タイプ *HumiditySensor* にマッピングを追加する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

この POST メソッドに対する応答の例を以下に示します。
```
{
  "logicalInterfaceId": "5846cd7c6522050001db0e24",
  "propertyMappings": {
    "humevt": {
      "humidity": "$event.hum"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## 手順 12: デバイス・タイプの構成をアクティブ化する


    
cURL を使用して、デバイス・タイプ *HumiditySensor* の構成を検証してアクティブ化する例を以下に示します。
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
この PATCH メソッドに対する応答の例を以下に示します。
```
{
 "message": "CUDRS0520I: State update configuration for device type 'HumiditySensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["HumiditySensor"]
  },
"failures": []
}
```
