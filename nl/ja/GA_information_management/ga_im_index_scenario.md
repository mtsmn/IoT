---

copyright:
years: 2016, 2017
lastupdated: "2017-09-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# ステップバイステップ・ガイド: 共通インターフェースによってデバイスを処理する方法を詳細に示す例
{: #scenario}

以下の情報に基づいてシナリオを作成します。そのシナリオでは、2 つの温度センサーが {{site.data.keyword.iot_full}} にイベントをパブリッシュします。1 つは温度を摂氏で測定するセンサーです。もう 1 つは温度を華氏で測定するセンサーです。それぞれの読み取り値を 1 つの摂氏の読み取り値に対応付けます。それぞれのデバイスから新しい読み取り値がパブリッシュされると、デバイス状態に関連するプロパティーの値が変更されます。{: shortdesc}

## 前提条件

{{site.data.keyword.iot_short_notm}} 組織インスタンスとその組織の API キーまたはトークンが必要です。

## このタスクについて

このシナリオでは、2 つのデバイスを構成します。

1 つは *TemperatureSensor1* というデバイスです。このデバイスは、摂氏で測定した温度イベントをパブリッシュします。温度イベントのパブリッシュ先は `iot-2/evt/tevt/fmt/json` というトピックであり、ペイロードは以下のようになります。
```
{
  "t" : 34.5
}
```

**注:** イベント ID は *tevt* です。この ID は、このタイプの温度イベントを物理インターフェースに追加する時と、このタイプのインバウンド・イベントに関連するプロパティーを論理インターフェースのプロパティーに対応付けるマッピングを定義する時に必要になります。このシナリオでは、論理インターフェースで **temperature** というプロパティーを定義します。

もう 1 つは *TemperatureSensor2* というデバイスです。このデバイスは、華氏で測定した温度イベントをパブリッシュします。温度イベントのパブリッシュ先は `iot-2/evt/tempevt/fmt/json` というトピックであり、ペイロードは以下のようになります。
```
{
  "temp" : 72.55
}
```

**注:** イベント ID は *tempevt* です。この ID は、このタイプの温度イベントを物理インターフェースに追加する時と、このタイプのインバウンド・イベントに関連するプロパティーを論理インターフェースのプロパティーに対応付けるマッピングを定義する時に必要になります。このシナリオでは、論理インターフェースで **temperature** というプロパティーを定義します。

論理インターフェースも構成されます。この論理インターフェースでは、このタイプのデバイスの状態を以下の構造で記述します。
```
{
  "temperature" : <current temperature value in Celsius>
  }
```
この場合は、**temperature** に関連する値を処理するアプリケーションを構成します。**t** に関連する値を処理したり、**temp** に関連する値を摂氏に変換してから処理したりするアプリケーションを構成するわけではありません。

![{{site.data.keyword.iot_short_notm}} における温度センサー・デバイスとアプリケーションの間のマッピング。](images/Information)  

以下のサンプル･シナリオを使用して独自のインターフェース環境をセットアップします。

## 必要に応じて、デバイス・タイプとデバイスを追加します。
{: #step14}

このシナリオでは、2 つのデバイス・タイプと 2 つのデバイス・インスタンスを想定します。デバイス・インスタンス *TemperatureSensor1* は、デバイス・タイプ *EnvSensor1* に関連付けます。デバイス・インスタンス *TemperatureSensor2* は、デバイス・タイプ *EnvSensor2* に関連付けます。

REST API を使用してデバイス・タイプを追加する方法については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html) の資料を参照してください。

## 手順 1: ドラフト・イベント・スキーマ・ファイルを作成する
{: #step1}

このシナリオでは、2 つのドラフト・イベント・スキーマ・ファイルを作成して、それぞれのインバウンド温度イベントの構造を定義します。

*tEventSchema.json* というドラフト・スキーマ・ファイルを作成する例を以下に示します。このファイルでは、温度を摂氏で測定する温度センサーから送られてくるインバウンド・イベントの構造を定義します。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor1 tEvent Schema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
          "description" : "temperature in degrees Celsius",
      "type" : "number",
      "minimum" : -273.15,
      "default" : 0.0
    }
  },
      "required" : ["t"]
    }
  ```
**ヒント:** 1 つまたは複数のプロパティーを必須に指定するには、"required" パラメーターを使用します。{{site.data.keyword.iot_short_notm}} によってデバイス状態をデバイス・データで更新するには、デバイス・メッセージに必須プロパティーが含まれていなければなりません。必須プロパティーが含まれていないメッセージは、処理されません。   

イベント・タイプのイベント・スキーマ・リソースを作成する時には、*tEventSchema* というスキーマ・ファイル名を使用します。

*tempEventSchema.json* というドラフト・スキーマ・ファイルを作成する例を以下に示します。このファイルでは、温度を華氏で測定する温度センサーから送られてくるインバウンド・イベントの構造を定義します。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "EnvSensor2 tempEvent Schema",
  "description" : "defines the structure of a temperature event in degrees Fahrenheit",
  "properties" : {
    "temp" : {
      "description" : "temperature in degrees Fahrenheit",
      "type" : "number",
      "minimum" : -459.67,
      "default" : 0.0
    }
  },
  "required" : ["temp"]
}
  ```
イベント・タイプのイベント・スキーマ・リソースを作成する時には、*tempEventSchema* というスキーマ・ファイル名を使用します。   

## 手順 2: 特定のイベント・タイプのドラフト・イベント・スキーマ・リソースを作成する
{: #step2}

イベント・スキーマ・リソースを作成するには、以下の API を使用します。

```
POST /draft/schemas
```

次のパラメーターが必要です。  

パラメーター|	説明

------	|	-----  
name
|	作成するイベント・スキーマの名前を入力します。
schemaFile
|	ローカルのイベント・スキーマ JSON ファイルへのパス。



詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) の資料を参照してください。

cURL を使用して *tEventSchema.json* というイベント・スキーマ・リソースを作成する例を以下に示します。

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "name" : "tEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "tEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e0d",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
イベント・タイプにイベント・スキーマを追加する時には、この POST メソッドへの応答で返されたスキーマ ID *5846cd7c6522050001db0e0d* が必要になります。

cURL を使用して *tempEventSchema.json* というイベント・スキーマ・リソースを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "schemaType" : "json-schema",
  "schemaFileName" : "tempEventSchema.json",
  "updated" : "2016-12-06T14:44:51Z",
  "name" : "tempEventSchema",
  "version" : "draft",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "created" : "2016-12-06T14:44:51Z",
  "id" : "5846cee36522050001db0e0e",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e/content"
  },
  "contentType" : "application/octet-stream",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
イベント・タイプにイベント・スキーマを追加する時には、この POST メソッドへの応答で返されたスキーマ ID *5846cee36522050001db0e0e* が必要になります。

## 手順 3: イベント・スキーマを参照するドラフト・イベント・タイプを作成する
{: #step3}

各イベント・タイプでは、イベント・スキーマ・リソースの作成時に使用した POST メソッドへの応答で返されたスキーマ ID を使用して、該当するイベント・スキーマを参照します。

ドラフト・イベント・タイプを作成するには、以下の API を使用します。

```
POST /draft/event/types
```

次のパラメーターが必要です。  

パラメーター|	説明
------	|	-----
name
|	作成するイベント・タイプの名前を入力します。

schemaId
|	イベント・スキーマ・リソース用に作成した ID。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types) の資料を参照してください。


cURL を使用して、摂氏で測定する温度イベントのイベント・タイプを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

イベント・タイプにイベント・スキーマを追加する時には、このスキーマ ID *5846cd7c6522050001db0e0d* を使用します。この ID は、イベント・スキーマ・リソース *tEventSchema.json* の作成時に使用した POST メソッドへの応答で返された ID です。

この POST メソッドに対する応答の例を以下に示します。

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e0d",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d"
  },
  "name" : "tEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846d0fd6522050001db0e0f",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

物理インターフェースにイベント・タイプを追加する時には、この POST メソッドへの応答で返されたイベント・タイプ ID *5846d0fd6522050001db0e0f* を使用します。

cURL を使用して、華氏で測定する温度イベントのイベント・タイプを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
イベント・タイプにイベント・スキーマを追加する時には、このスキーマ ID *5846cee36522050001db0e0e* を使用します。この ID は、イベント・スキーマ・リソース *tempEventSchema.json* の作成時に使用した POST メソッドへの応答で返された ID です。この POST メソッドに対する応答の例を以下に示します。

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "schemaId" : "5846cee36522050001db0e0e",
  "created" : "2016-12-06T15:00:20Z",
  "id" : "5846d2846522050001db0e10",
  "updated" : "2016-12-06T15:00:20Z",
  "name" : "tempEvent",
  "version" : "draft",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e"
  },
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
物理インターフェースにイベント・タイプを追加する時には、この POST メソッドへの応答で返されたイベント・タイプ ID *5846d2846522050001db0e10* を使用します。
## 手順 4: ドラフト物理インターフェースを作成する
{: #step7}

ドラフト物理インターフェースを作成するには、以下の API を使用します。

```
POST /draft/physicalinterfaces
```

次のパラメーターが必要です。  

パラメーター|	説明
------	|	-----
name
|	作成する物理インターフェースの名前を入力します。





詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) の資料を参照してください。

このシナリオでは、イベント・タイプごとに 1 つずつ、合計で 2 つの物理インターフェースが必要です。

cURL を使用して最初の物理インターフェースを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Env sensor physical interface 1"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "Env sensor physical interface 1",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

摂氏で測定する温度イベントを物理インターフェースに追加する時に呼び出す POST メソッドの URL では、この応答で返された物理インターフェース ID *5847d1df6522050001db0e1a* を使用します。

cURL を使用して 2 番目の物理インターフェースを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Env sensor physical interface 2"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

華氏で測定する温度イベントを物理インターフェースに追加する時に呼び出す POST メソッドの URL では、この応答で返された物理インターフェース ID *5847d1df6522050001db0e1b* を使用します。   

## 手順 5: ドラフト物理インターフェースにイベント・タイプを追加する
{: #step8}

物理インターフェースにイベント・タイプを追加するには、以下の API を使用します。

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

次のパラメーターが必要です。  

パラメーター|	説明
------	|	-----
eventId	|	デバイスのイベント・ペイロードのイベント名を入力します。

eventTypeId	|	イベント・タイプ用に作成した ID。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces) の資料を参照してください。

このシナリオでは、以下のイベント・タイプを指定の物理インターフェースに追加します。
- 摂氏の温度イベント *tevt* を ID *5847d1df6522050001db0e1a* の物理インターフェースに追加します。その際に、トピックから送られてくる *eventId* と、イベント・スキーマ・リソースの作成時に返される *eventTypeId* を使用します。
- 華氏の温度イベント *tempevt* を ID *5847d1df6522050001db0e1b* の物理インターフェースに追加します。その際に、トピックから送られてくる *eventId* と、イベント・スキーマ・リソースの作成時に返される *eventTypeId* を使用します。


cURL を使用して、温度イベント *tevt* を ID *5847d1df6522050001db0e1a* の物理インターフェースに追加する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

cURL を使用して、温度イベント *tempevt* を ID *5847d1df6522050001db0e1b* の物理インターフェースに追加する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
}
```

## 手順 6: ドラフト物理インターフェースを接続するためにドラフト・デバイス・タイプを更新する
{: #step9}

ドラフト・デバイス・タイプを更新するには、以下の API を使用します。

```
POST /draft/device/types/{typeId}/physicalinterface
```

ここで *typeId* はデバイス・タイプ ID です。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) の資料を参照してください。

このシナリオでは、デバイス・タイプ *EnvSensor1* を更新して物理インターフェース *5847d1df6522050001db0e1a* に接続し、デバイス・タイプ *EnvSensor2* を更新して物理インターフェース *5847d1df6522050001db0e1b* に接続します。

cURL を使用してデバイス・タイプ *EnvSensor1* を更新する例を以下に示します。

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

この POST メソッドに対する応答の例を以下に示します。
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "Env sensor physical interface 1",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
物理インターフェースと論理インターフェースを追加する時に、デバイス ID *EnvSensor1* が必要になります。

cURL を使用してデバイス・タイプ *EnvSensor2* を更新する例を以下に示します。

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "Env sensor physical interface 2",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
物理インターフェースと論理インターフェースを追加する時に、デバイス ID *EnvSensor2* が必要になります。



## 手順 7: ドラフト論理インターフェース・スキーマ・ファイルを作成する
{: #step4}

*envSensor.json* というドラフト論理インターフェース・スキーマ・ファイルを作成する例を以下に示します。

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Environment Sensor Schema",
    "description" : "Schema to represent a canonical environment sensor device",
    "properties" : {
        "temperature" : {
           "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## 手順 8: ドラフト論理インターフェース・スキーマ・リソースを作成する
{: #step5}

ドラフト論理インターフェース・スキーマ・リソースを作成するには、以下の API を使用します。

```
POST /draft/schemas
```

次のパラメーターが必要です。  

パラメーター|	説明
------	|	-----
name
|	作成する論理インターフェース・スキーマの名前を指定します。
schemaFile
|	ローカルの論理インターフェース・スキーマ JSON ファイルへのパス。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) の資料を参照してください。

cURL を使用して論理インターフェース・スキーマを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=temperatureEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/envSensor.json"'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "temperatureEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "envSensor.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
論理インターフェースに論理インターフェース・スキーマを追加する際には、POST メソッドへの応答で返されたスキーマ ID *5846ec826522050001db0e11* を使用します。

## 手順 9: ドラフト論理インターフェース・スキーマを参照するドラフト論理インターフェースを作成する
{: #step6}

ドラフト論理インターフェースを作成するには、以下の API を使用します。

```
POST /draft/logicalinterfaces
```

次のパラメーターが必要です。  

パラメーター|	説明
------	|	-----
name
|	作成する論理インターフェースの名前を指定します。
schemaId
|	論理インターフェース・スキーマ・リソース用に作成した ID。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) の資料を参照してください。

このシナリオでは、論理インターフェースに論理インターフェース・スキーマを追加する際に、前の応答で返されたスキーマ ID *5846ec826522050001db0e11* を使用します。

cURL を使用して論理インターフェースを作成する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "environment sensor interface", "schemaId" : "5846ec826522050001db0e11"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "schemaId" : "5846ec826522050001db0e11",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846ed076522050001db0e12",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "environment sensor interface",
  "version" : "draft"
}
```
このシナリオでは、デバイス・タイプに論理インターフェースを追加する際に、POST メソッドへの応答で返された論理インターフェース ID *5846ed076522050001db0e12* を使用します。インバウンド・デバイス・イベントを論理インターフェースで定義するプロパティーに対応付ける時にも、この ID を使用します。

## 手順 10: デバイス・タイプにドラフト論理インターフェースを追加する
{: #step10}

デバイス・タイプにドラフト論理インターフェースを追加するには、以下の API を使用します。

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

ここで、*typeId* はデバイス・タイプの名前です。 

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) の資料を参照してください。  
**注:** このシナリオでは、同じ論理インターフェース *5846ed076522050001db0e12* を *EnvSensor1* および *EnvSensor2* に関連付けます。

cURL を使用して、論理インターフェース・スキーマ ID *5846ec826522050001db0e11* を参照する論理インターフェース *5846ed076522050001db0e12* をデバイス・タイプ *EnvSensor1* に追加する例を以下に示します。

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "environment sensor interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

cURL を使用して、論理スキーマ ID *5846ec826522050001db0e11* に関連する論理インターフェース *5846ed076522050001db0e12* をデバイス・タイプ *EnvSensor2* に追加する例を以下に示します。

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

この POST メソッドに対する応答の例を以下に示します。

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "environment sensor interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

## 手順 11: インバウンド・イベントのプロパティーを論理インターフェースのプロパティーにマップするためのマッピングを定義する
{: #step11}  
**重要:** MQTT クライアント・アプリケーションを使用しており、デバイス状態の更新に関する通知を受信するためにサブスクライブする場合は、マッピングを定義するときに通知設定を構成する必要があります。この方法で通知方式を構成すると、選択された方法で状態変更通知を受信するように各デバイス・タイプが構成されて、複数のデバイス・タイプ間で論理インターフェースを再使用することができます。指定されたトピック・ストリングを MQTT クライアントがサブスクライブするかどうかにかかわりなく、通知方式を定義できます。  
  
以下の通知設定を構成できます。  

パラメーター|	説明
------	|	-----
never	|	通知は送信されません。この設定がデフォルトです
on-every-event	|	デバイス状態が変更されたかどうかにかかわらず、イベントごとに通知が送信されます
on-state-change	|	イベントの結果としてデバイス状態が変更された場合にのみ、通知が送信されます

**注:** 通知設定 *never* を選択した場合、アプリケーションはデバイス状態の変更に関する通知をいっさい受信しません。そのため、必要に応じて通知方式 *on-every-event* または *on-state-change* を選択できます。  

イベントをマップするには、以下の API を使用します。

```
POST /draft/device/types/{typeId}/mappings
```

次のパラメーターが必要です。  

パラメーター|	説明
------	|	-----
logicalInterfaceId	|	論理インターフェース用に作成した ID。
propertyMappings
|	論理インターフェースに定義されたプロパティーとデバイス・イベント・ペイロードのプロパティーをマップする、有効な JSON 構造。
オプションで *notificationStrategy* パラメーターを設定できます。  

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) の資料を参照してください。

このシナリオでは、デバイス・タイプ *EnvSensor1* のマッピングを定義して、インバウンド・イベント *tevt* のプロパティー **t** を論理インターフェースのプロパティー **temperature** に対応付けます。また、デバイス・タイプ *EnvSensor2* のマッピングを定義して、インバウンド・イベント *tempevt* のプロパティー **temp** を論理インターフェースのプロパティー **temperature** に対応付けます。通知設定は「on-state-change」に設定されます。 

cURL を使用してデバイス・タイプ *EnvSensor1* にマッピングを追加する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**重要:** 構造化された [JSON ![外部リンクのアイコン](../../../icons/launch-glyph.svg "外部リンクのアイコン")](http://json-schema.org/){:new_window} イベント・ペイロードを使用する場合は、$event.*property* 項目をプロパティーの JSON 構造と正確に一致させてください。例えば、ペイロードが `{"d":{"t":<temp>}}` の場合は、`$event.d.t` を使用します

論理インターフェースの作成時に使用した POST メソッドへの応答で返された論理インターフェース ID *5846ed076522050001db0e12* と、デバイス・タイプ *EnvSensor1* を指定します。

この POST メソッドに対する応答の例を以下に示します。

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt" : {
               "temperature" : "$event.t"
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
cURL を使用してデバイス・タイプ *EnvSensor2* にマッピングを追加する例を以下に示します。

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

論理インターフェースの作成時に使用した POST メソッドへの応答で返された論理インターフェース ID *5846ed076522050001db0e12* と、デバイス・タイプ *EnvSensor2* を指定します。
変換を適用して、華氏の測定値を摂氏の測定値に変更します。


この POST メソッドに対する応答の例を以下に示します。

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
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

## 手順 12: 構成を検証してアクティブ化する
{: #step15}

各ドラフト・デバイス・タイプのデバイス状態の更新に関連した構成を検証してアクティブ化します。この構成には、ドラフト・スキーマ、イベント・タイプ、物理インターフェース、論理インターフェース、マッピングが含まれています。

ドラフト・デバイス・タイプ構成を検証してアクティブ化するには、以下の API を使用します。

```
PATCH /draft/device/types/{typeId}
```

ここで *typeId* はデバイス・タイプ ID です。

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) の資料を参照してください。

このシナリオでは、2 つのドラフト・デバイス・タイプの構成を検証してアクティブ化する必要があります。

cURL を使用して、ドラフト・デバイス・タイプ *EnvSensor1* の構成を検証してアクティブ化する例を以下に示します。

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor1 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

この PATCH メソッドに対する応答の例を以下に示します。

```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor1' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor1"]
  },
"failures": []
}
```
cURL を使用して、ドラフト・デバイス・タイプ *EnvSensor2* の構成を検証してアクティブ化する例を以下に示します。

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/EnvSensor2 \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
この PATCH メソッドに対する応答の例を以下に示します。```
{
 "message": "CUDRS0520I: State update configuration for device type 'EnvSensor2' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["EnvSensor2"]
  },
"failures": []
}
```

## 手順 13: インバウンド・デバイス・イベントをパブリッシュする
{: #step12}

以下のサンプル・ペイロードを使用して、*TemperatureSensor1* の温度イベントをトピック `iot-2/evt/tevt/fmt/json` にパブリッシュします。
```
{
  "t" : 34.5
}
```
以下のサンプル・ペイロードを使用して、*TemperatureSensor2* の温度イベントをトピック `iot-2/evt/tempevt/fmt/json` にパブリッシュします。
```
{
  "temp" : 72.55
}
```

デバイスからインバウンド・イベントをパブリッシュする方法については、[アプリケーションの MQTT 接続](../applications/mqtt.html#publishing_device_events)を参照してください。


## 手順 14: デバイスの状態を取得する
{: #step13}

HTTP REST API を使用するか、またはトピックをサブスクライブすることで、デバイスの状態を取得できます。

- MQTT クライアント・アプリケーションを使用している場合、以下のトピック・ストリングをサブスクライブできます。  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- あるいは、以下の HTTP REST API を使用して、最新のデバイス状態を取得できます。  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

次のパラメーターが必要です。  

パラメーター|	説明
------	|	-----
typeId
|	デバイス・タイプ ID。
deviceId	|	デバイス ID。
logicalInterfaceId	|	論理インターフェース用に作成した ID。

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types) の資料を参照してください。

cURL を使用して *TemperatureSensor1* の現在の状態を取得する例を以下に示します。その際に、作成した論理インターフェースの ID を参照します。
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor1/devices/TemperatureSensor1/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

この GET メソッドでは、論理インターフェース ID *5846ed076522050001db0e12* を使用しています。この ID は、論理インターフェースの作成時に使用した POST メソッドへの応答で返された ID です。
この GET メソッドに対する応答の例を以下に示します。
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
}
}
```
cURL を使用して *TemperatureSensor2* の現在の状態を取得する例を以下に示します。その際に、作成した論理インターフェースの ID を参照します。
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/EnvSensor2/devices/TemperatureSensor2/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

この GET メソッドでは、論理インターフェース ID *5846ed076522050001db0e12* を使用しています。この ID は、論理インターフェースの作成時に使用した POST メソッドへの応答で返された ID です。
この GET メソッドに対する応答の例を以下に示します。
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
}
}
```
返された読み取り値は、華氏ではなく摂氏の温度です。

アプリケーションでは、この正規化データを処理できます。構成で温度のそれぞれの単位を取得したり変換したりする必要はありません。


