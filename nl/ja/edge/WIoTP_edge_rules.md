---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} エッジのルーティング・ルールの構成 (プレビュー)
{: #edge_rules}

{{site.data.keyword.iot_full}} エッジのルーティング・ルールにより、エッジ・ノード内でのメッセージの転送方法が定義されます。

**重要:** {{site.data.keyword.iot_full}} エッジ機能は、限定されたプレビュー・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

エッジ・ノードには、複数のルーティング・ルールを定義できます。ルールは、ローカル・デバイスまたはアプリケーションからエッジ・ノードに直接パブリッシュされるメッセージごとに評価されます。クラウドからエッジ・ノードにパブリッシュされたメッセージは、常にローカルにパブリッシュされ、ルーティングによる影響は受けません。

## ルーティング・ルール形式
{: #edge_rules_format}

ルーティング・ルールは、以下の形式で JSON オブジェクトとして定義されます。

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

ルーティング・ルールは、エッジ・ノード上の `/etc/wiotp-edge` フォルダー内に保管されている `routing.json` ファイルに、JSON オブジェクトの配列として指定されます。

各ルーティング・ルールのプロパティーは、以下のように定義されます。

プロパティー      | タイプ     | 説明       
------------- | -----------| -----------
rule_id | 整数、必須 | ルールの ID。ルーティング・ルールは、rule_id の昇順で評価されます。複数のルールに同じ rule_id を使用することはできません。
matching_filter | ストリング、必須 | パブリッシュされたメッセージにルールを適用する必要があるかどうかを評価する場合に使用されます。MQTT サブスクリプションの形式にする必要があります。MQTT 単一レベル (+) および複数レベル (#) のワイルドカードがサポートされます。このフィルターは、メッセージがパブリッシュされるトピックに対して評価されます。このプロパティーは、{{site.data.keyword.iot_short_notm}} アプリケーション・トピック形式で定義され、`/type/deviceType/id/deviceId` フィールドを含みます。例えば、`"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"` となります。
Destination | ストリング、必須 | どこにメッセージを転送する必要があるかを定義します。`"CLOUD"`、`"IM"` (情報管理コンポーネントにメッセージを転送する場合)、または `"LOCAL"` (エッジ・ノード上でローカルにメッセージを転送する場合) のいずれかの値を設定します。例えば、`"destination" : "CLOUD"` となります。
continue_matching | ブール値、オプション、デフォルトは true | ルールが 1 回一致した場合に、このルールを使用して追加のメッセージを評価する必要があるかどうかを指定します。このプロパティーにより、ルーティング・ルールが重複している場合にルーティングをより適切に制御できます。
forward_skip_levels | 整数、オプション、デフォルトは 0 | メッセージのリパブリッシュ時に削除する必要がある、元のメッセージ・トピックのレベルの数を指定します。デフォルト値は 0 で、これは、元のトピックが維持されることを意味します。例えば、`forward_skip_levels` が `1` に設定されており、メッセージがトピック `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json` に対してパブリッシュされた場合、このメッセージはトピック `iot-2/type/T1/id/D1/evt/E1/fmt/json` に対してリパブリッシュされます。
store | ブール値、オプション、デフォルトは true | このルールによってルーティングされるメッセージに、ストア・アンド・フォワード機能を適用する必要があるかどうかを定義します。このプロパティーは、destination プロパティーが `"CLOUD"` に設定されている場合にのみ適用されます。このプロパティーが false に設定されている場合、エッジ・ノードがクラウドから切断されると、ルールと一致するメッセージは保管されません。

## routing.json ファイルの例
{: #edge_rules_example}

以下の例では、3 つのルールが構成されます。
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

最初のルールにより、タイプが `AlarmEvent` のすべてのイベントが、クラウドに直接ルーティングされます。

2 番目のルールにより、`sendToCloud/iot-2` で始まるトピックに対してパブリッシュされるすべてのイベントが、クラウドに直接ルーティングされます。`iot-2` で開始し、`sendToCloud/` 接頭辞を含まないトピックでは、メッセージは転送されます。

3 番目のルールはデフォルト・ルールで、これにより、その他すべてのメッセージがローカルの MQTT ブローカーにルーティングされます。その後、これらのメッセージは、サブスクリプションが一致するすべてのローカル・コンシューマーによって使用可能です。エッジ・ノードのインストールには、すべてをローカルにルーティングするためのデフォルト・ルールが `/etc/wiotp-edge/routing.json` に含まれます。

## ルーティング・ルールの編集
{: #edge_rules_edit}

ルールは、`/etc/wiotp-edge/routing.json` ファイルで直接編集できます。ファイルを編集した後、変更を保存してから `docker ps` を実行して、エッジ・コネクター・コンテナーの ID を判別します。その ID を使用して、Docker コンテナーを再始動します。

## ルーティング・ルールのトラブルシューティング
{: #edge_rules_ts}

`routing.json` を変更した後、エッジ・コネクター・コンテナーが引き続き再始動しようとする場合、ルーティング・ルール・ファイルに構文エラーが含まれることを意味します。ログ・ファイル `/var/wiotp-edge/trace/edgeConnector_trace.log` を調べて、エラー・メッセージを確認します。
