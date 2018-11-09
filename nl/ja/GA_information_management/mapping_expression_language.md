---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# マッピング式言語の解説
{: #mapping_expression}

マッピング式言語を使用すると、データを操作して結合したり、処理したデータに対して実行する照会の結果をフォーマット設定したりすることができます。 マッピング式言語は、[JSONata ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/index.html){:new_window} のサブセットで、[マッピング](ga_im_definitions.html#resources)を定義したり、[ルール](../information_management/im_rules.html)を作成したりする際に使用できます。 

JSONata は、JSON データに対して照会と変換を行うための軽量の言語です。 [JSONata Exerciser ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://try.jsonata.org/){:new_window} ツールを使用して、JSONata をすぐに便利な方法で試してみることもできます。

以下に、現在サポートされている重要な演算子と関数、およびその使用法の例をいくつか示します。 

## JSONata 演算子
{: #operators}

以下の演算子を除き、すべての [JSONata 演算子 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/operators.html){:new_window} がサポートされます。

- ~> (関数チェーン)
- ^(…) (順序付け)
- := (変数バインディング)

**注:** 
- 括弧 ( ) は、式のグループ化と演算子優先順位の変更に使用します
- 単一引用符または二重引用符は、ストリングおよびプロパティー名を囲む場合に使用します (例: $event.object.'ab')。 
- バックティックは、以下の例に示すように、スペースを含む特殊文字が使用されたプロパティー名を囲む場合に使用します。 
```
{"x y":22}.`x y`
```
- バックティックは、以下の例に示すように、数値で始まるプロパティー名を囲む場合に使用します。
```
`7emperature`
```

## JSONata 関数
{: #functions}
以下の JSONata 関数がサポートされます。 

 - すべての[ストリング ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/string-functions.html){:new_window} 関数
 - すべての[数値 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/numeric-functions.html){:new_window} 関数 	
 - すべての[数値集約 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 関数 
 - すべての[ブール ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/boolean-functions.html){:new_window} 関数  
 - [$count および $append の配列 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/array-functions.html){:new_window} 関数 	

## マッピング式言語の拡張

$event、$state、$instance の各変数の導入により、マッピング式言語が拡張され、データ管理フィーチャーで使用できるようになりました。 JSON は、式が評価される前にこれらの変数にバインドされます。 以下の表に、式で使用するために定義されたこれらの変数の概要を示します。

変数                   | JSON の入力例     | 式としての例    | 使用目的   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | 式で使用するインバウンド・イベントのプロパティーを選択します。 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | 式で使用するデバイスの状態に関するプロパティーを選択します。
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | 式で使用するデバイス属性またはデバイス・タイプ属性を選択します。 

以下の例では、次のオブジェクトがイベントへの入力として使用されます。  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
オブジェクトは、以下の式を使用して配列に変換されます。

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
この式により、以下の出力が生成されます。
```
 [
    35,
    72,
    1024
  ]
```

この例の逆を行い、配列を入力からオブジェクトに変換できます。以下の例では、次の配列がイベントへの入力として使用されます。
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
配列は、以下の式を使用してオブジェクトに変換されます。
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
 この式により、以下の出力が生成されます。
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
これらの変数の使用法を結合する式を定義することもできます。 次の例では、デバイス・メタデータに *temp_adjustment* プロパティーが定義され、これがイベント読み取り値の調整に使用されます。 このプロパティーは、1 つのマッピングに定義されていますが、多数のデバイスに適用できます。 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


ドット演算子「.」は、リテラル・キーでのオブジェクト・アクセスに使用されます (例: $event.object.hh)。左側の式は、特定のプロパティー (イベント ($event) または状態 ($state) またはメタデータ ($instance) のいずれか) に制限されます。 右側の式には、アクセスが必要な場合のある情報が含まれています。

ドット演算子について詳しくは、JSONata 資料の [Operators ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/operators.html){:new_window} セクションを参照してください。


## 言語ガイド

- すべての[基本選択 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/basic.html){:new_window} がサポートされます。
- ワイルドカードを除き、すべての[複合選択 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン ")](http://docs.jsonata.org/complex.html){:new_window} がサポートされます。
- 条件式および括弧で囲んだ式は、[プログラミング構成 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.jsonata.org/programming.html){:new_window} の一部としてサポートされます。変数は、$instance、$state、および $event の各変数を、Watson IoT Platform データ管理に固有な拡張マッピング言語の一部として使用することでサポートされます。JSONata で使用される "$" および "$$" の各変数は、現在サポートされていません。

## 出力の構成
配列コンストラクターまたはオブジェクト・コンストラクターを使用すると、出力に処理済みのデータを表示する方法を指定できます。

### サポートされる配列コンストラクター
配列を構成するには、リテラルまたは式のコンマ区切りリストを大括弧のペア ([]) で囲みます。シーケンス生成など、配列コンストラクター内で複数の式を区切るには、コンマを使用します (例: *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* や *[1..3, 7..9] -> [1,2,3,7,8,9]*)。

### サポートされるオブジェクト・コンストラクター
{: #constructors}
出力で JSON オブジェクトを構成するには、コンマ区切りのキー値のペアを入れた中括弧のペア {} を使用します。
各キーと値はコロンで区切ります (例: *{key1 : value1, key2:value2}* や *{"hello" : "world"}*)。オブジェクト・キーはストリングでなければなりません。  


## 例: 配列を使用した温度データの処理および報告
以下のセクションは、[REST API を使用したデータ管理の概説](ga_im_example.html) に記載された例に基づいて構成されています。ここでは、温度読み取り値を示すスライディング・ウィンドウを配列を使用して保守し、そのウィンドウ内に示された読み取り値の現在の合計や平均を計算する方法を示しています。

スライディング・ウィンドウには、データが到着順に保管されます。 スライディング・ウィンドウは、これまでに挿入されたデータをすべて保持するのではなく、段階的にデータを削除するように構成できます。 スライディング・ウィンドウがいっぱいになった後にさらに挿入すると、そのウィンドウ内の最も古いデータ項目が削除されます。

以下に、カウント・ベースの削除ポリシー (サイズは 5) によって構成されたスライディング・ウィンドウの例を示します。
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

以下の例は、*tempReadings* という配列を追加することによって論理インターフェース・スキーマ・ファイル構成を変更する方法を示しています。これは、[ステップバイステップ・ガイド 1](ga_im_index_scenario.html#step4) の手順 7 で示されている構成です。この配列は、最後の 5 つのイベントについて、デバイスから送信された最後の 5 つの値を示すウィンドウを保守するために使用します。 *tempReadings* に値が保管されていない場合、配列は [0] に設定され、次に受け取った読み取り値が配列の末尾に追加されます。この配列は 5 つの読み取り値を受け取るまで増えていきます。 5 つの読み取り値を受け取った後、新たに読み取り値を受け取ると、最も古い読み取り値がウィンドウから削除されます。 

**重要な注:** *tempReadings* を「required」に設定し、デフォルトで配列として設定する必要があります。 

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature" : {
          "description" : "latest temperature reading in degrees Celsius",
          "type" : "number",
          "minimum" : -273.15,
          "default" : 0.0
      },
      "tempAverage": {
          "type": "number"
      },
      "tempReadings": {
          "default": [],
          "items": {
              "type": "number"
          },
         "type": "array"
      },
      "tempSum": {
          "type": "number"
      }
  },
   "required" : [
      "tempReadings"
  ],
  "type": "object"
}
```
以下のセクションでは、現在のスライディング・ウィンドウに含まれている読み取り値に基づいて温度読み取り値の平均とすべての温度の読み取り値の合計を計算するための、マッピング・リソースを構成する方法の例を示します。


```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt": {
               "tempAverage": "$average($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))",
               "tempReadings": "$count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t)",
               "tempSum": "$sum($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))"
           }
       },
       "updated": "2017-10-13T10:05:40Z",
       "updatedBy": "a-8x7nmj-9iqt56kfil",
       "version": "active"
   }
]
```
**注:** *$state.tempReadings* 配列は、$average 関数と $sum 関数で使用される前に再計算されます。 *tempAverage* 式または *tempSum* 式を評価する際、マッピング式の順序は制御できないため、配列に現行値を含めるために配列の再計算が必要です。

以下に、5 つの温度読み取り値が含まれるスライディング・ウィンドウに基づく平均温度と合計温度の例を示します。
```
{
    "state": {
        "tempAverage": 18557.6,
        "tempReadings": [
            17591,
            10262,
            25621,
            16676,
            22638
        ],
        "tempSum": 92788
    },
    "timestamp": "2017-10-13T11:07:20Z",
    "updated": "2017-10-13T11:05:40Z"
}
```

## マッピング式と入力データの間の不一致の処理

状態の更新は、パブリッシュされたイベントに指定されていない入力データへの参照がいずれかのマッピング式に含まれていると失敗する場合があります。

例えば、以下の式を構成するとします。
```
temperature = $event.t 
humidity = $event.hum
```
ここで、*t* はイベントのオプション・プロパティーです。

湿度データのみを含むイベントが受信された場合 (例: `{"humidity":22}`)、式 `temperature = $event.t` は評価に失敗します。これは、パブリッシュされたデバイス・イベントにオプションの *t* プロパティーが指定されていないためです。

状態の更新は失敗します。湿度状態プロパティーは更新されず、エラー・メッセージがデバイスの MQTT エラー・トピックにパブリッシュされます。
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
オプション・データが指定されていないことが原因で状態の更新が失敗しないようにするには、$exists 関数を 3 項条件と組み合わせて使用し、オプション・プロパティーのデフォルト値を指定できます。以下の例では、*t* プロパティーにデフォルト値 *0* を定義します。

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

この方法でオプション・プロパティーにデフォルト値を定義することにより、パブリッシュされたデバイス・イベントに *t* プロパティーが指定されていない場合でも、式は正常に評価されます。

したがって、イベント `{"humidity":22}` が受信された場合、状態の更新は正常に実行され、デバイス状態は `{"humidity":22, "temperature":0}` に設定されます。
