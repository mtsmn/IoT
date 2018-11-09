---

copyright:
years: 2016, 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ステップバイステップ・ガイド 2: 共通インターフェースによってモノを処理する方法を詳細に示す例 (ベータ)
{: #scenario}

このシナリオは、前のシナリオの[ステップバイステップ・ガイド 1: 共通インターフェースによってデバイスを処理する方法を詳細に示す例](../GA_information_management/ga_im_index_scenario.html)に基づいて作成されています。

**重要:** データ管理用の {{site.data.keyword.iot_full}} モノ機能は、限定されたベータ・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

このシナリオでは、温度デバイスと湿度デバイスにより、2 つの会議室 (会議室 1 と会議室 2) で収集された環境データがパブリッシュされます。会議室ごとに、温度と湿度のデバイス・データが 2 つのデバイス・タイプ論理インターフェースに個別にマップされます。 

会議室 1 の場合、*TSensor* デバイス・タイプに関連付けられている温度デバイス・データが論理インターフェース *Thermometer Interface* にマップされ、デバイス・タイプ *HumiditySensor* に関連付けられている湿度デバイス・データが論理インターフェース *Hygrometer Interface* にマップされます。 

会議室 2 の場合、TempSensor デバイス・タイプに関連付けられている温度デバイス・データが *Thermometer Interface* 論理インターフェースにマップされ、デバイス・タイプ *HumiditySensor* に関連付けられている湿度デバイス・データが *Hygrometer Interface* 論理インターフェースにマップされます。 

次に *RoomType* というモノ・タイプを作成し、*meetingroom1* と *meetingroom2* という 2 つの部屋のモノ・インスタンスを作成します。

このシナリオでは、温度計論理インターフェースと湿度計論理インターフェースを含む構成をセットアップしてから、正しい環境デバイスを各部屋のインスタンスにマップします。例えば、*tSensor* と *humiditySensor1* を *meetingroom1* にマップします。

## 前提条件
{: #pre_req}

先に進む前に、以下のことを確認してください。
- ステップバイステップ・ガイド 1 で使用したものと同じ {{site.data.keyword.iot_full}} 組織インスタンスとその組織の API キーまたはトークンを使用していること。API キーやトークンについて詳しくは、[アプリケーションの HTTP REST API](../applications/api.html#authentication) 資料の『認証』セクションを参照してください。
- 温度デバイスと湿度デバイスごとに 1 つずつ、2 つの論理インターフェースが構成されていること。温度デバイスの論理インターフェースの構成については、[ステップバイステップ・ガイド 1: 共通インターフェースによってデバイスを処理する方法を詳細に示す例](../GA_information_management/ga_im_index_scenario.html#step4)の資料を参照してください。湿度デバイスの論理インターフェースの構成については、[ステップバイステップ・ガイド 2 の追加情報 - 湿度デバイスの論理インターフェースの構成](im_hygrometer.html)を参照してください。

## このタスクについて
{: #about}

{{site.data.keyword.iot_short_notm}} では、1 つのモノを複数のデバイスとモノで構成することができます。 モノ・タイプは、モノのインスタンスの構成を定義するものです｡ 

論理インターフェースは、モノ・タイプに関連付けられます。この関連付けによって、モノ・タイプのインスタンスに対して生成される状態の構造が定義されます。 マッピングを使用して、集約されたデバイスやモノのプロパティーをモノの状態のプロパティーにマップする方法を定義します。

論理インターフェースを使用すると、アプリケーションでデバイスやモノの構成を認識する必要がなくなります。 例えば、1 つのデバイスを使用して部屋の温度を測定したり、複数のデバイスの読み取り値の平均を取って部屋の温度を計算したりするとします。 アプリケーションには、1 つまたは複数の部屋の状態に関する情報が必要です。その情報の一要素が温度のプロパティーです。 アプリケーションで受け取る温度値がどのように計算されたかは問題ではありません。

このシナリオでは、2 つの温度デバイスと 2 つの湿度デバイスがイベントを {{site.data.keyword.iot_short_notm}} にパブリッシュします。 事業所ビルの会議室 1 に、温度デバイスが 1 つと湿度デバイスが 1 つあります。 会議室 2 に、もう 1 つの温度と湿度のデバイスがあります。以下の図は、会議室 1 の構成を示しています。

![{{site.data.keyword.iot_short_notm}} における温度および湿度のモノとアプリケーション間のマッピング。](images/Information)

*RoomType* というモノ・タイプを使用して、部屋のインスタンスの構成を定義します。 論理インターフェースに、この *RoomType* を関連付け、温度と湿度の両方を示す単一の読み取り値にインバウンド・イベントがマップされるように定義します。 この単一の読み取り値が、このモノの状態です。 マッピングを使用して、温度デバイスや湿度デバイスのプロパティーをこのモノの状態にマップする方法を定義します。 それぞれのデバイスから新しい読み取り値がパブリッシュされると、モノの状態に関連付けられているプロパティーの値が変更されます。

以下の表は、この例で使用される 4 つのデバイス、各デバイスがパブリッシュするトピック、および各デバイスのサンプル・ペイロードを示しています。

デバイス/タイプ | イベント | イベント・ペイロード/プロパティー
------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**注:** イベント ID の *tevt*、*tempevt*、*humevt* は、そのタイプのインバウンド・イベントに関連するプロパティーを、論理インターフェースのプロパティーに対応付けるマッピングを定義する場合に必要です。このシナリオでは、論理インターフェースで *temperature* と *humidity* という 2 つのプロパティーが定義されます。

論理インターフェースも構成されます。 論理インターフェースは、モノの状態を以下の構造で表します。
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

以下のサンプル･シナリオを使用して独自のインターフェース環境をセットアップします。 

**注:** このガイドで使用されるリソース・プロパティーの名前、値、および ID をリストしている表については、[ステップバイステップ・ガイド 1 および 2 参照用資料にあるリソース・プロパティーおよび ID](im_id_reference.html) を参照してください。

## 必要に応じてデバイス・タイプとデバイスを追加する  
{: #add_device}  
このシナリオでは、3 つのデバイス・タイプと 4 つのデバイス・インスタンスを使用します。 デバイス・インスタンス *tSensor* は、デバイス・タイプ *TSensor* に関連付けます。 デバイス・インスタンス *tempSensor* は、デバイス・タイプ *TempSensor* に関連付けます。 デバイス・インスタンス *humiditySensor1* と *humiditySensor2* はデバイス・タイプ *HumiditySensor* と関連付けられます。

[{{site.data.keyword.iot_short_notm}} ダッシュボード ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://internetofthings.ibmcloud.com){: new_window} を使用して、または REST API を使用して、デバイス・タイプおよびデバイスを作成できます。 

{{site.data.keyword.iot_short_notm}} IoT Platform ダッシュボードを使用してデバイス・タイプおよびデバイスを追加する方法について詳しくは、[Web インターフェースを使用したデータ管理の概説](../GA_information_management/im_ui_flow.html)の資料を参照してください。

REST API を使用してデバイス・タイプおよびデバイスを追加する方法について詳しくは、[{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window} の資料を参照してください。



## 手順 1: モノのタイプのスキーマ・ファイルを作成する  
{: #crt_composition_file}  
温度と湿度のデバイス・タイプのデバイス論理インターフェース ID を参照するモノのタイプのスキーマ・ファイルを作成します。  

*roomTypeSchema* というモノのタイプのスキーマ・ファイルを作成する例を以下に示します。   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON Schema that defines the structure of the Room Thing Type.",
   "properties" : {
        "thermometer": {
            "type": "object",
            "description": "The thermometer device",
            "$logicalInterfaceRef": "IThermometer"
        },
        "hygrometer": {
            "type": "object",
            "description": "The hygrometer device",
            "$logicalInterfaceRef": "IHygrometer"
        }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```
**ヒント:** 1 つまたは複数のプロパティーを必須に指定するには、**required** パラメーターを使用します。 デバイスの状態をデバイス・データで更新するには、必須プロパティーが Watson IoT Platform のデバイス・メッセージに含まれている必要があります。 必須プロパティーが含まれていないメッセージは、処理されません。   
**注:** モノのタイプを作成するときには、モノのタイプのスキーマ・ファイルを作成したときに生成されたスキーマ ID を指定しなければなりません。  

## 手順 2: モノのスキーマ・リソースを作成する  
{: #crt_composition_resource}  

前の手順で生成されたモノのタイプのスキーマ・ファイルをアップロードして、スキーマ・リソースを作成します。  
以下の API を使用して、モノのタイプのスキーマ・ファイルをアップロードします。  
```
POST /draft/schemas
```  
スキーマ定義ファイルは、マルチパート POST (multipart/form-data) 内で Watson IoT Platform に渡されます。POST の本体には、少なくとも 2 つのパーツが含まれている必要があります。

  - 1 つは、パーツの本体としてのファイルの実際の内容を含む **schemaFile** という名前のパーツ。
  - もう 1 つは、パーツの本体にあるスキーマ・リソースの名前を定義するストリングを含む **name** という名前のパーツ。


詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) の資料を参照してください。  

cURL を使用してモノのタイプのスキーマ・リソースを作成する例を以下に示します。  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
この POST メソッドに対する応答の例を以下に示します。
```
{
  "name" : "roomTypeSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "roomType.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5a72ea48d60180002c4f5e58",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
モノのタイプを作成する時には、この POST メソッドへの応答で返されたスキーマ ID *5a72ea48d60180002c4f5e58* が必要になります。


## 手順 3: モノのタイプを作成する  
{: #crt_thing_type}  

モノのタイプを使用して、モノのインスタンスをモデル化します。 モノのインスタンスを作成するには、その前にモノのタイプを組織内に作成しておく必要があります。 このシナリオの場合は、モノのタイプを 1 つ作成します。  
モノのタイプに関連付けるスキーマによって、モノのインスタンスを構成するためにまとめられるオブジェクトのタイプが定義されます。 モノのタイプは、前の手順で作成したモノのタイプのスキーマ・リソースを参照する必要があります。  
モノのタイプを作成するには、以下の API を使用します。
```
POST /draft/thing/types
```
POST メソッドの本体には、以下のパラメーターが必要です。  
<table>
<tr><th>パラメーター</th><th>説明</th></tr>
<tr><td>id</td><td>作成するモノのタイプの ID を指定します。</td></tr>
<tr><td>name</td><td>作成するモノのタイプの名前を指定します。</td></tr>
<tr><td>schemaId</td><td>構成スキーマ・リソース用に作成した ID。</td></tr>
</table>

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) の資料を参照してください。  

以下の例は、cURL を使用して *RoomType* というモノ・タイプを作成する方法を示しています。
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

応答: 
```
{
 "name": "RoomType",
 "description": "Room Thing Type",
 "id": "RoomType",
 "schemaId": "5a72ea48d60180002c4f5e58",
 "metadata": {},
  "refs": {
    "schema": "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58",
    "mappings": "/api/v0002/draft/thing/types/RoomType/mappings",
    "logicalInterfaces": "/api/v0002/draft/thing/types/RoomType/logicalinterfaces"
   },
 "version": "draft",
 "created": "2018-02-01T10:22:43Z",
 "createdBy": "ANOther",
 "updated": "2018-02-01T10:22:43Z",
 "updatedBy": "ANOther"
}
```

## 手順 4: 論理インターフェース・スキーマ・ファイルを作成する  
{: #crt_ai_schema_file}
論理インターフェースでは、モノの状態として格納するデータの構造を定義できます。 このシナリオの場合、温度と湿度のプロパティーを定義する論理インターフェースを作成します。 論理インターフェース・リソースの作成時に生成された論理インターフェース ID を参照して、論理インターフェースをモノのタイプ *RoomType* と関連付けます。  
*roomSchema* という論理インターフェース・スキーマ・ファイルを作成する例を以下に示します。

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON Schema that defines the canonical room state structure",
    "properties" : {
        "temperature" : {
            "description" : "Temperature in degrees celsius",
           "type" : "number",
           "minimum" : -273.15,
           "default" : 0.0
        },
       "humidity" : {
            "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```  
## 手順 5: 論理インターフェース・スキーマ・リソースを作成する  
{: #crt_ai_schema_resource}  
以下の API を使用して、前の手順で作成した論理インターフェース・スキーマ・ファイルをアップロードします。これにより、モノのタイプのための論理インターフェース・スキーマ・リソースが作成されます。  
```
POST /draft/schemas
```  

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas) の資料を参照してください。  

cURL を使用して論理インターフェース・スキーマを作成する例を以下に示します。
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
この POST メソッドに対する応答の例を以下に示します。
```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "roomSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "room.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5a4b9847d60180002efce645/content"
  },
  "id" : "5a4b9847d60180002efce645"
}
```
モノ・タイプの論理インターフェースに論理インターフェース・スキーマ・リソースを追加するには、POST メソッドへの応答で返されたスキーマ ID *5a4b9847d60180002efce645* を使用します。  


## 手順 6: モノのタイプのための論理インターフェースを作成する  
{: #crt_thing_ai}  
論理インターフェースで、前の手順で作成した論理インターフェース・スキーマ・リソースを参照する必要があります。  
論理インターフェースを作成するには、以下の API を使用します。  
```
POST draft/logicalinterfaces
```  
オプションで、論理インターフェースに対して意味のある別名を指定することができます。別名は、自動生成される論理インターフェース ID を使用する代わりに、モノの状態を取得するために使用する API 呼び出しやトピック・ストリング・サブスクリプション内で参照できます。

**注:** 別名は、1 から 36 文字までの長さでなければならず、英数字、ハイフン、ピリオド、下線を含めることができます。別名を 24 文字の 16 進ストリングにすることはできません。

このシナリオでは、論理インターフェースに論理インターフェース・スキーマを追加する際に、前の応答で返されたスキーマ ID *5a4b9847d60180002efce645* を使用します。

以下の例は、cURL を使用して別名 *IRoom* を持つ論理インターフェースを作成する方法を示しています。
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
この POST メソッドに対する応答の例を以下に示します。
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58"
  },
  "schemaId" : "5a72ea48d60180002c4f5e58",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5a4b9847d60180002efce645",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "IRoom",
  "alias" : "IRoom",
  "version" : "draft"
}
```
このシナリオでは、デバイス・タイプに論理インターフェースを追加する際に、POST メソッドへの応答で返された論理インターフェース ID *5a4b9847d60180002efce645* を使用します。 インバウンド・デバイス・イベントを論理インターフェースで定義するプロパティーに対応付ける時にも、この ID を使用します。

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces) の資料を参照してください。  


## 手順 7: モノのタイプに論理インターフェースを追加する  
{: #add_thing_ai}  
モノ・タイプに論理インターフェースを追加するには、以下の API を使用します。  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) の資料を参照してください。  
このシナリオでは、論理インターフェースにモノのタイプ *RoomType* を関連付けます。

以下の例は、cURL を使用してモノの論理インターフェース *IRoom* をモノ・タイプ *RoomType* に追加する方法を示しています。  
```
{   
  "id": "5a4b9847d60180002efce645"
}
```
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id": "5a4b9847d60180002efce645"}'
```

この POST メソッドに対する応答の例を以下に示します。
```
{
 "name": "Room Logical Interface",
 "description": "This is a Room logical interface",
 "id": "5a4b9847d60180002efce645",
 "schemaId": "5a4b9817d60180002efce644",
 "refs": {
   "schema": "/api/v0002/draft/schemas/5a4b9817d60180002efce644"
 },
 "version": "draft",
 "created": "2018-01-02T14:33:43Z",
 "createdBy": "ANOther",
 "updated": "2018-01-02T14:33:43Z",
 "updatedBy": "ANOther"
}
```

## 手順 8: マッピングを定義する
{: #define_Thing_type_mappings}

モノのタイプのマッピングを定義します。これにより、集約されたデバイスまたはモノの状態のプロパティーを、モノのタイプの論理インターフェースで定義されているプロパティーにマップする方法を記述します。

イベントをマップするには、以下の API を使用します。  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

ここで、*thingtypeId* は、モノ・タイプの作成時に POST 要求への応答で返される ID です。 

POST 要求の本体には、以下のパラメーターが必要です。  
<table>
<tr>
<th>	パラメーター	</th><th>	説明	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	論理インターフェースに定義されたプロパティーとデバイス・イベント・ペイロードのプロパティーをマップする、有効な JSON 構造。	</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	ペイロードの本体には論理インターフェース ID が必要です。</td>
</tr>
</table>  

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) の資料を参照してください。

cURL を使用してモノのタイプ *RoomType* にマッピングを追加する例を以下に示します。

```
{
  "logicalInterfaceId": "5a4b9847d60180002efce645",
  "notificationStrategy": "on-state-change",
  "propertyMappings": {
       "thermometer": {
         "temperature": "$event.temperature"
       },
       "hygrometer": {
         "humidity": "$event.humidity"
       }
   },
}
```  
*thermometer* デバイスは [roomTypeSchema](#crt_composition_file) で定義されています。*$event.temperature* プロパティーは、ID *5846ed076522050001db0e12* および別名 *IThermometer* を持つ論理インターフェース・スキーマで定義されています。  
*hygrometer* デバイスは [roomTypeSchema](#crt_composition_file) で定義されています。*$event.humidity* プロパティーは、ID *5846cd7c6522050001db0e24* および別名 *IHygrometer* を持つ論理インターフェース・スキーマで定義されています。


## 手順 9: 構成を検証してアクティブ化する
{: #activate}

各モノ・タイプのモノの状態の更新に関連した構成を検証してアクティブ化します。 この構成には、スキーマ、論理インターフェース、マッピングが含まれています。 

モノ・タイプ構成を検証してアクティブ化するには、以下の API を使用します。
```
PATCH /draft/thing/types/{thingTypeId}
```
ここで *thingTypeId* はモノ・タイプ ID です。 

以下の例は、cURL を使用してモノ・タイプ *RoomType* に関連付けられている構成を検証してアクティブ化する方法を示しています。
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

この PATCH メソッドに対する応答の例を以下に示します。
```
{
 "message": "CUDIM0300I: State update configuration for Thing Type 'Room Thing Type' has been successfully submitted for  activation.",
"details": {
   "id": "CUDIM0300I",
   "properties": [
     "Thing Type",
     "Room Thing Type"
   ]
 },
"failures": []
}
```

## 手順 10: モノのタイプのインスタンスを作成する
{: #create_Thing_instances}

「モノ」とは、「モノのタイプ」のインスタンスです。 モノを使用すると、デバイスやモノの 1 つ以上のインスタンスを集約して 1 つのオブジェクトにすることができます。

モノを作成するには、以下の API を使用します。

```
POST /thing/types/{thingTypeId}/things
```
ここで、*thingtypeId* は、モノ・タイプの作成時に POST 要求への応答で返される ID です。 

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things) の資料を参照してください。

このシナリオでは、モノ・タイプが *RoomType* のモノ・インスタンスを 2 つ作成する必要があります。 一方のモノ・インスタンスを *meetingroom1* と呼び、もう一方のモノ・インスタンスを *meetingroom2* と呼びます。

以下の例は、cURL を使用して *meetingroom1* というモノ・インスタンスを作成する方法を示しています。モノ・インスタンス *meetingroom1* は、デバイス・インスタンスの *tSensor* と *humiditySensor1* に関連付けられます。

```
thingId = "meetingroom1"
 meetingroom1AggregatedObjects = {
   "thermometer": {
     "type": "device",
     "typeId": "TSensor",
     "id": "tSensor"
   },
   "hygrometer": {
     "type": "device",
     "typeId": "HumiditySensor",
     "id": "humiditySensor1"
   }
 }
``` 

作成されたモノの ID を、モノの論理インターフェースに温度イベントを追加するときに呼び出す POST メソッドの URL で使用します。

以下の例は、cURL を使用して *meetingroom2* というモノ・インスタンスを作成する方法を示しています。*meetingroom2* は、デバイス・インスタンスの *tempSensor* と *humiditySensor2* に関連付けられます。

```
thingId = "meetingroom2"
   meetingroom2AggregatedObjects = {
    "thermometer": {
      "type": "device",
      "typeId": "TempSensor",
      "id": "tempSensor"
    },
    "hygrometer": {
      "type": "device",
      "typeId": "HumiditySensor",
      "id": "humiditySensor2"
    }
  }

``` 

## 手順 11: マップしたデバイス・イベントが論理インターフェースにパブリッシュされることを確認する  
{: #publish_event}  
モノ *meetingroom1* に集約されるデバイスに関する以下のイベントをパブリッシュします。  
 - *tSensor* の温度イベントをトピック `iot-2/evt/tevt/fmt/json` に  
 - *humiditySensor1* の湿度イベントをトピック `iot-2/evt/humevt/fmt/json` に  
 
モノ *meetingroom2* に集約されるデバイスに関する以下のイベントをパブリッシュします。  
 - *tempSensor* の温度イベントをトピック `iot-2/evt/tempevt/fmt/json` に  
 - *humiditySensor2* の湿度イベントをトピック `iot-2/evt/humevt/fmt/json` に  
 
デバイスからインバウンド・イベントをパブリッシュする方法については、[アプリケーションの MQTT 接続](../applications/mqtt.html#publishing_device_events)を参照してください。  

## 手順 12: モノの状態が変更されたことを確認する  
{: #verify_Thing_state}  

HTTP REST API を使用するか、またはトピックをサブスクライブすることで、モノの状態を取得できます。

MQTT クライアント・アプリケーションを使用している場合、以下のトピック・ストリングをサブスクライブできます。
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

あるいは、以下の HTTP REST API を使用して、最新のモノの状態を取得できます。

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

次のパラメーターが必要です。  
<table>
<tr>
<th>パラメーター	</th><th>	説明</th>
</tr>
<tr>
<td>thingTypeId	</td><td>モノ・タイプ ID。</td>
</tr>
<tr>
<td>thingId	</td><td>	モノの ID。</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>論理インターフェース用に作成した ID。</td>
</tr>
</table>  

詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types) の資料を参照してください。  


## 次の手順

{{site.data.keyword.iot_short_notm}} が受信するイベントが原因でデバイスまたはモノの状態が変更される場合にアクションを開始するために使用できるルールを作成します。ルールの作成について詳しくは、[組み込みルールの作成 (ベータ)](im_rules.html) を参照してください。
