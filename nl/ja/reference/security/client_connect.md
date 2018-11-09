---

copyright:
  years: 2019
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クライアント接続のモニター (ベータ)
{: #connect_status}

{{site.data.keyword.iot_full}} では、クライアントの接続状況を照会およびモニターすることができます。クライアントが接続されているかどうかをリアルタイムで知ることができ、接続の問題をトラブルシューティングできます。
{:shortdesc}

例えば、クライアントが最後にアクティブだった時間や切断された理由を表示したり、問題に対応できるように過去 1 日に接続されなかったすべてのクライアントを表示したりすることができます。

**重要:** クライアント接続のモニター機能は、限定されたベータ・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## クライアント接続状況の照会
{: #query_status}

クライアント接続状態 API を使用すると、{{site.data.keyword.iot_short_notm}} に接続されたすべてのクライアントの接続状況を取得および照会できます。

接続状況を照会すると、以下の情報が表示されます。

 - 接続状況 (接続または切断のいずれか)
 - 最後の接続の時刻 (ISO 8601 形式)
 - 最後の切断の時刻 (ISO 8601 形式)
 - 最後のアクティビティー・イベント (MQTT パブリッシュや MQTT サブスクライブなど)
 - 最後のアクティビティーの時刻 (ISO 8601 形式)
 - クライアントが切断された場合、切断の原因となったエンティティー (クライアント、サーバー、またはネットワーク)
 - 切断の MQTT 理由コード (クライアントが切断されている場合)

**例**

単一クライアントのクライアント接続状況を取得するには、以下のようにします。

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates/{clientId}`

すべてのクライアントの接続状況を取得するには、以下のようにします。

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates`

接続されているすべてのクライアントを取得するには、以下のようにします。

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?connectionStatus=connected`

過去 2 日以内にアクティブであったすべてのクライアントを取得するには、以下のようにします。

`GET {orgId}.internetofthings.ibmcloud.com/api/v0002/clientconnectionstates?lastConnectedAfter={currentTime-2D}`

**注:** クライアントが接続する時刻と、API からの接続状況が正しい状況を反映する時刻の間に、若干の遅延がある可能性があります。

使用可能なすべての照会フィルターを含め、API について詳しくは、[Client Connection State API の Swagger 資料![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/clientstate-beta.html#!/Client_Connection_State/get_clientconnectionstates_clientId){: new_window} を参照してください。

## クライアント接続状況のモニター
{: #monitor_status}

 すべてのクライアントには、クライアントのライブ接続状況更新を受け取るためにサブスクライブすることができるモニター・トピックがあります。詳しくは、[デバイス状況メッセージのサブスクライブ](../../applications/mqtt.html#subscribe_device_commands)を参照してください。

## クライアント接続の問題のトラブルシューティング

接続ログ API を使用すると、デバイスの接続ログ・イベントのリストを取得して、接続の問題を診断できます。これらのエントリーには、成功した接続、失敗した接続試行、意図的な切断、サーバーから開始された切断が記録されています。

詳しくは、[Connection Logs API の Swagger 資料![外部リンク・アイコン](../../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html?cm_mc_uid=08862496634215124007223&cm_mc_sid_50200000=36272221529958773076#!/Device_Problem_Determination/get_logs_connection){: new_window} を参照してください。
