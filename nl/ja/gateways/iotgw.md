---

copyright:
years: 2016, 2018
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IoT ゲートウェイ・パッケージ 
{: #gw_package}

{{site.data.keyword.iot_full}} IoT ゲートウェイ・パッケージを使用すると、{{site.data.keyword.iot_short_notm}} に登録されたゲートウェイに関連付けられたリソース・グループに属する各デバイスの代わりに、そのゲートウェイがプラットフォームにイベントを送信できるようになります。 
{:shortdesc}


## 概説

IoT ゲートウェイ・パッケージには、以下のエンティティーが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | パッケージ | org、domain、gatewayTypeId、gatewayId、gatewayToken  | {{site.data.keyword.iot_short_notm}} ゲートウェイを操作する |
| `/watson-iot/iot-gateway/publishEvent` | アクション | org、domain、gatewayTypeId、gatewayId、gatewayToken、eventType、typeId、deviceId、payload | 関連するデバイスの代わりに登録済みゲートウェイからイベントを {{site.data.keyword.iot_short_notm}} に送信する   |

## IoT ゲートウェイ・パッケージ・バインディングの作成
IoT ゲートウェイ・パッケージ・バインディングを作成するには、以下のパラメーターを指定する必要があります。

| パラメーター |  説明 |
| --- | ---  |
| org | 組織 ID |
| gatewayTypeId | 登録済みゲートウェイのゲートウェイ・タイプ ID |
| gatewayId | 登録済みゲートウェイのゲートウェイ ID |
| gatewayToken | 登録済みゲートウェイの許可トークン |


パッケージ・バインディングを作成するには、次の手順を実行します。  
1. [IBM Cloud コンソールにログインします ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/)。
2. IBM Cloud で [Internet of Things Platform サービス](https://console.bluemix.net/docs/services/IoT/index.html)を作成し、[`API キー`と `API トークン`をメモします](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications)。 この情報は、ゲートウェイ・タイプの作成とゲートウェイの登録に必要です。
3. ご使用の Watson IoT 組織に[ゲートウェイ・タイプを作成し](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html) (例えば、*myGWType*)、[ゲートウェイのインスタンスを登録します](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html) (例えば、*myGWId*)。 登録されたゲートウェイの*ゲートウェイ・トークン*をメモします。
4. 以下のサンプル・コマンドを使用して、IoT ゲートウェイ・パッケージでパッケージ・バインディングを作成します。
   ```
   bx wsk package bind /watson-iot/iot-gateway myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. 以下のコマンドを使用して、パッケージ・バインディングが存在することを確認します。  
   ```
   bx wsk package list
   ```

## デバイス・イベントのパブリッシュ

`/watson-iot/iot-gateway/publishEvent` アクションは、関連付けられたデバイスの代わりに登録済み {{site.data.keyword.iot_short_notm}} ゲートウェイからイベントをパブリッシュします。 次の表では、イベントをパブリッシュする際に使用されるパラメーターについて説明します。  

パラメーター |  説明 | 例
------------- | ------------- | -------------
org | 登録済みゲートウェイが属する組織の ID。  | `-p org "uguhsp"`
domain | オプションです。 登録済みゲートウェイが属するドメイン。 デフォルトでは、`messaging.internetofthings.ibmcloud.com` を指し示します。 | `-p domain "messaging.internetofthings.ibmcloud.com"`
gatewayTypeId | 登録済みゲートウェイのゲートウェイ・タイプ ID。 | `-p gatewayTypeId "myGatewayType"`
gatewayId | 登録済みゲートウェイのゲートウェイ ID。 ID は、組織内の所定のゲートウェイ・タイプの間で固有でなければなりません。 | `-p gatewayId "00aabbccde03"`
gatewayToken | Watson IoT Platform に接続するために登録済みゲートウェイによって使用されるトークン (パスワード)。  | `-p gatewayToken "ZZZ"`
eventType | 登録済みゲートウェイが、それに関連付けられたデバイスの代わりにイベントをパブリッシュするときの対象イベント・タイプ。 | `-p eventType "evt"`
typeId | 登録済みゲートウェイに関連付けられたデバイスのデバイス・タイプ。 登録済みゲートウェイは、関連付けられたデバイスの代わりにイベントをパブリッシュします。 | `-p typeId "myDeviceType"`
deviceId | 登録済みゲートウェイに関連付けられたデバイスのデバイス ID。 登録済みゲートウェイは、関連付けられたデバイスの代わりにイベントをパブリッシュします。 デバイス ID は、組織内の所定のゲートウェイ・タイプの間で固有でなければなりません。 | `-p deviceId "00aabbccde03_0001"`
payload | 登録済みゲートウェイがデバイスの代わりにパブリッシュするペイロード。 | `-p payload "{'d':{'temp':38}}"`


以下の例は、*iotgw* パッケージからイベントをパブリッシュする方法を示しています。

作成したパッケージ・バインディングで *publishEvent* アクションを使用することにより、デバイス・イベントをパブリッシュします。 `/myNamespace/myGateway` をご使用のパッケージ名に置き換える必要があります。

 ``` 
 bx wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## リポジトリーの処理

`installCatalog.sh` を使用してパッケージをデプロイするには、次の手順を実行します
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

ここで、*AUTH* は許可キー、*APIHOST* は IBM Cloud Functions ホスト名、および *WSK_CLI* は IBM Cloud Functions CLI バイナリーの場所です。
