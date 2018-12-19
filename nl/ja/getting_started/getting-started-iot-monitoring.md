---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# ガイド 2: デバイス・データのモニター
1 つ以上のデバイスを接続できたので、デバイス・データのリアルタイム・モニターを開始します。
{:shortdesc}

## 概説と目標
{: #overview}  

このガイドでは、モニター・アプリケーションを {{site.data.keyword.Bluemix}} にデプロイして、デバイスから送られてくるデータを表示します。

ガイド 1 の場合と同じく、以下のパスのいずれかまたは両方を実行してください。
- パス A: [手順 1A - モニター Web アプリケーションをデプロイして接続する](#deploy_app)  
パス A を実行する場合は、既製のアプリケーションをデプロイして、組織内で実行するコンベヤー・ベルト・デバイスをモニターします。
- パス B: [手順 1B - ウィジェット・ライブラリーを使用してモニター用のユーザー・インターフェースを作成する](#widget-library)
パス B のほうが少し複雑です。こちらのパスでは、ウィジェット・ライブラリーの概要と基本的なユーザー・インターフェースの作成手順を見ていきます。

## 前提条件
{: #prereqs}  
始める前に、以下の要件を満たしていることを確認してください。

### ローカル環境
- [Node.js ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://nodejs.org){: new_window} バージョン 6.x 以上。
- パス A: [Angular CLI ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/angular/angular-cli){: new_window} バージョン 1.x 以上。  
- [Cloud Foundry CLI ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
cf CLI によって、アプリケーションとサービスを {{site.data.keyword.Bluemix_notm}} にデプロイします。 詳しくは、[Cloud Foundry CLI 資料![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.cloudfoundry.org/cf-cli/){: new_window}を参照してください。  
- オプション: [Git ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git-scm.com/downloads){: new_window}  
Git を使用してコード・サンプルをダウンロードする場合は、[GitHub.com アカウント ![外部リンク・アイコン](../../../icons/launch-glyph.svg " 外部リンク・アイコン")](https://github.com){: new_window} も必要です。 ただし、GitHub.com アカウントがなくても、コードを圧縮ファイルとしてダウンロードすることは可能です。

### その他の要件
さらに、接続したデバイスが必要です。そのデバイスでイベントを送信します。デバイス・タイプは `iot-conveyor-belt`、イベント名は `sensorData` です。メッセージ・ペイロードのプロパティーは以下のとおりです。
```
{
	"d": {
		"id": "belt1",
		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
デバイス・イベントとメッセージ形式の詳細については、[イベントのパブリッシュ](/docs/services/IoT/devices/mqtt.html#publishing_events)を参照してください。  
[ガイド 1 で {{site.data.keyword.iot_short_notm}} の概要とコンベヤー・ベルトのシミュレート](getting-started-iot-conveyor.html)の学習を完了していれば、すべての準備が整っています。  
{: tip}

## 手順 1A - モニター用の Web アプリケーションをデプロイして接続する
{: #deploy_app}

Plant Floor Monitoring サンプル・アプリケーションは、{{site.data.keyword.iot_full}} 組織に接続している iot-conveyor-belt タイプの全デバイスのリストとイベント・データのサブセット (RPM、最終更新日、デバイス ID など) を表示します。

このサンプル・アプリケーションは、[https://github.com/ibm-watson-iot/iot-nodejs ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window} にある Node.js クライアント・ライブラリーで作成されています。

![Node.js をベースにしたモニター・アプリケーション](images/app_monitor.png "Node.js をベースにしたモニター・アプリケーション")

この手順の一環として、次のことを行います。
- Cloud Foundry を使用して、GitHub ベースのサンプル・モニター Web アプリケーションをデプロイします。
- サンプル・アプリケーションで、API キーと認証トークンを使用して {{site.data.keyword.iot_short_notm}} に接続するための構成を行います。
- その Web アプリケーションを使用して、接続したコンベヤー・ベルト・デバイスをモニターします。  

### モニター用の Web アプリケーションをデプロイして接続するための詳しい手順
以下のステップでは、{{site.data.keyword.Bluemix_notm}} でのアプリの作成とデプロイを通してガイドします。 アプリケーションのローカル実行については、GitHub の README ファイルを参照してください。
1. Node.js の *Plant Floor Monitoring* サンプル・アプリケーションの GitHub リポジトリーを複製します。  
好みの git ツールを使用して以下のリポジトリーを複製します。  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular
Git シェルで、以下のコマンドを使用します。
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular
  ```
2. アプリケーションの API キーと認証トークンの組み合わせを作成します。  
組織に接続するための構成をアプリケーションで行う時に、その情報が必要になります。 デバイスの登録の詳細については、[アプリケーションの接続](/docs/services/IoT/platform_authorization.html)を参照してください。  
 1. {{site.data.keyword.iot_short_notm}} ダッシュボードを開きます。
 2. **「アプリ」**を選択します。
 3. **「API キーの生成」**をクリックします。
 4. 表示される API キーと認証トークンの値をコピーします。
 5. API の役割として**「視覚化アプリケーション」**を選択します。  
**ヒント:** アプリケーションにさらに多くの機能を追加する場合は、もっと高度な役割に昇格させることが必要になります。
 6. この API キーと認証トークンの組み合わせに関するわかりやすいコメントを追加します。
 7. **「生成」**をクリックします。
3. アプリケーションで、{{site.data.keyword.Bluemix_notm}} に接続するための構成を行います。
*guide-conveyor-ui-angular* リポジトリーのルートに移動し、`basicConfig.json` ファイルを作成して以下の内容を組み込んでください。
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
パラメーター値は、ご使用の {{site.data.keyword.Bluemix_notm}} 組織の対応値 (orgID や、作成した API キーや認証トークンの値) に置き換えてください。  
例:
```
 {   
  "org": "3v5whr",    
  "apiKey": "a-3v5whr-jhkmsghlqv",  
  "apiToken": "-q0MkPN2cNYB6+?ISk"  
}
```
4. cloudfoundry CLI を使用して、ご使用の {{site.data.keyword.Bluemix_notm}} アカウントにログインします。  
詳しくは、[Cloud Foundry CLI 資料![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.cloudfoundry.org/cf-cli/){: new_window}を参照してください。  
コマンド・ラインから、以下のコマンドを入力します。  
  ```
cf login
  ```
モニター・サンプル・アプリケーションのデプロイ先の組織とスペースを選択するためのプロンプトが表示されたら、それぞれの値を選択してください。
5. 必要に応じて、以下の cf api コマンドを実行して、API エンドポイントを設定します。   
`API-ENDPOINT` の値を該当地域の API エンドポイントに置き換えてください。
  ```
cf api API-ENDPOINT
  ```
例: `cf api https://api.ng.bluemix.net`
<table>
<tr>
<th>地域</th>
<th>API エンドポイント</th>
</tr>
<tr>
<td>米国南部</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>英国</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
<!--<tr>
<td>Germany</td>
<td>https://api.eu-de.bluemix.net</td>
</tr>-->
</table>
6. サンプル・アプリが置かれているディレクトリーに移動します。  
  ```
cd guide-conveyor-ui-angular
  ```
7. `npm install -g @angular/cli` を実行して Angular CLI をインストールします。
8. `npm install` を実行します。
9. `npm run push` を実行し、プロジェクトを作成して組織にプッシュします。  
サンプル Web アプリケーションが {{site.data.keyword.Bluemix_notm}} にデプロイされます。  
デプロイメントが完了すると、アプリケーションが稼働していることを示すメッセージが表示されます。   
例:  
  ```
requested state: started
instances: 1/1
usage: 64M x 1 instances
urls: iotmonitoringcontrol-undertided-eng.mybluemix.net
last uploaded: Tue May 16 19:01:13 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     state     since                    cpu    memory     disk        details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
10. Web アプリケーションを開きます。  
{{site.data.keyword.Bluemix_notm}} アプリケーションのダッシュボードから、**「Visit App URL」**をクリックして Web アプリケーションを開きます。  
**「経路」**をクリックし、アプリケーションの URL にアクセスして管理します。   
デフォルトの URL は、以下のような形式になります。  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## 手順 1B - ウィジェット・ライブラリーを使用してモニター用のユーザー・インターフェースを作成する
{: #widget-library}

ウィジェット・ライブラリーをベースにしたサンプル・アプリケーションには、モーター・スピード・ゲージ、加速度センサー・データ・ゲージ、モーター・スピード・ダイアグラムが含まれています。そのダイアグラムに、{{site.data.keyword.iot_short_notm}} 組織に接続している iot-conveyor-belt タイプの 1 つのデバイスのデータが表示されます。 サンプル・コードを実行すれば、{{site.data.keyword.iot_short_notm}} に接続したデバイスに対応したフロントエンド・アプリケーション全体を作成できます。

![ウィジェット・ライブラリーをベースにしたモニター・アプリケーション](images/app_monitor_b.png "ウィジェット・ライブラリーをベースにしたモニター・アプリケーション")

この手順の一環として、次のことを行います。
- Cloud Foundry を使用して、GitHub ベースのサンプル Web アプリケーションをデプロイします。
- サンプル・アプリケーションで、API キーと認証トークンを使用して {{site.data.keyword.iot_short_notm}} に接続するための構成を行います。
- デバイス・データをゲージやグラフとして表示する 3 つのユーザー・インターフェース・ウィジェットを構成します。
- その Web アプリケーションを使用して、接続したコンベヤー・ベルト・デバイスをモニターします。  

### ウィジェット・ライブラリーを使用してモニター用のユーザー・インターフェースを作成するための詳しい手順
以下のステップでは、{{site.data.keyword.Bluemix_notm}} でのアプリの作成とデプロイを通してガイドします。 アプリケーションのローカル実行については、GitHub の README ファイルを参照してください。
1. *Widget Library Monitoring* サンプル・アプリケーションの GitHub リポジトリーを複製します。  
好みの git ツールを使用して以下のリポジトリーを複製します。  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui
Git シェルでは、次のコマンドを使用します。
```
git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui
```
2. アプリケーションの従属関係をインストールします。  
*guide-conveyor-ui-html* リポジトリーのルートに移動して、以下のコマンドを実行します。
```
npm install
```
3. ユーザー・インターフェースを作成します。  
アプリケーション・ユーザー・インターフェースを作成するには、各ユーザー・インターフェース・コンポーネントに対応したウィジェットをアプリケーションの index.html ファイルに JavaScript コードとして追加する必要があります。  
各ウィジェットでは、以下の JavaScript パラメーターを使用します。  
`WIoTPWidget.CreateWIDGET_TYPE("ELEMENT_ID","EVENT_NAME", "DEVICE_TYPE", "DEVICE_ID", "PROPERTY" , {WIDGET_DEFAULT_OVERRIDE}, [WIDGET_SPECIFIC_SETTINGS])`

各パラメーターの説明を以下の表にまとめます。

| パラメーター | 説明 |    
| ----- | ---- |   
| WIDGET_TYPE | 作成するウィジェットのタイプ。 例: `Gauge` や `Chart` |
| ELEMENT_ID | ウィジェットのエレメント ID。この ID がアプリケーションに表示されます。 例: `RPM` |
| EVENT_NAME | 表示するプロパティーを組み込むデバイス・イベント名。 例: `sensorData` |
| DEVICE_TYPE | デバイス・タイプ。 例: `iot-conveyor-belt` |
| DEVICE_ID | 表示するデータを提供するデバイスの ID。 例: `belt1` |
| PROPERTY | 表示するデバイス・メッセージ・ペイロード・プロパティー。 例: `rpm` |
| WIDGET_DEFAULT_OVERRIDE | デフォルト設定をオーバーライドするウィジェット構成設定。|
| WIDGET_SPECIFIC_SETTINGS | ウィジェットの 1 つ以上の追加パラメーター (例を参照)。 |

各ウィジェット・タイプの詳細については、以下の例と [IoT ウィジェット GitHub リポジトリー ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/iot-widgets){: new_page} にある資料を参照してください。
 1. RPM ゲージを追加します。  
このゲージに、コンベヤー・ベルトの rpm 値が表示されます。rpm の最小値は 0、最大値は 5 です。
    1. `public/index.html` テンプレートを編集モードで開きます。  
    2. rpm ゲージのプレースホルダー (`) を見つけます。<!--- place holder for rpm gauge  -->`
    3. 以下の div エレメントを追加し、固有の ID を付けます。
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. JavaScript のプレースホルダー (`/// Add your scripts here`) を見つけます。
    4. rpm の JavaScript コードを追加します。  
例:  
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iot-conveyor-belt", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. 加速データ・ゲージを追加します。  
このゲージに、加速度センサーの読み取り値が表示されます。-1 から 1 までの値になります。
    1. 加速度センサー・ゲージのプレースホルダー (`) を見つけます。<!--- place holder for accelerometer gauge  -->`
    2. 以下の div エレメントを追加し、固有の ID を付けます。
 ```html
 <div id="aygauge" ></div>
 ```  
    3. JavaScript のプレースホルダー (`/// Add your scripts here`) を見つけます。
    4. 加速度センサーの JavaScript コードを追加します。  
例:   
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iot-conveyor-belt", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
                },
                show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. モーター・スピード・グラフを追加します。  
この折れ線グラフに、モーター・スピードが表示されます。
    1. モーター・スピード・ゲージのプレースホルダー (`) を見つけます。<!--- place holder for motor speed gauge  -->`
    2. 以下の div エレメントを追加し、固有の ID を付けます。
 ```html
 <div id="speedchart" ></div>
 ```  
    3. JavaScript のプレースホルダー (`/// Add your scripts here`) を見つけます。
    4. スピード・グラフの JavaScript コードを追加します。  
例:  
 ```javascript
 WIoTPWidget.CreateChart("speedchart ","sensorData", "iot-conveyor-belt", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. アプリケーションを {{site.data.keyword.Bluemix_notm}} にデプロイします。  
 1. {{site.data.keyword.iot_short_notm}} サービス名を使用して manifest.yml ファイルを更新します。  
例えば、[ガイド 1: コンベヤー・ベルト・デバイスの接続](getting-started-iot-monitoring.html)でデプロイした {{site.data.keyword.iot_short_notm}} サービスであれば、YOUR_PLATFORM_NAME は `iotp-for-conveyor` になります。
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. cloudfoundry CLI を使用して、ご使用の {{site.data.keyword.Bluemix_notm}} アカウントにログインします。  
 詳しくは、[Cloud Foundry CLI 資料![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.cloudfoundry.org/cf-cli/){: new_window}を参照してください。  
 コマンド・ラインから、以下のコマンドを入力します。  
   ```
 cf login
   ```
 モニター・サンプル・アプリケーションのデプロイ先の組織とスペースを選択するためのプロンプトが表示されたら、それぞれの値を選択してください。
 5. 必要に応じて、以下の cf api コマンドを実行して、API エンドポイントを設定します。   
 `API-ENDPOINT` の値を該当地域の API エンドポイントに置き換えてください。
   ```
 cf api API-ENDPOINT
   ```
 例: `cf api https://api.ng.bluemix.net`
 <table>
 <tr>
 <th>地域</th>
 <th>API エンドポイント</th>
 </tr>
 <tr>
 <td>米国南部</td>
 <td>https://api.ng.bluemix.net</td>
 </tr>
 <tr>
 <td>英国</td>
 <td>https://api.eu-gb.bluemix.net</td>
 </tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
 </table>
 6. サンプル・アプリが置かれているディレクトリーに移動します。  
   ```
 cd guide-conveyor-ui-html
   ```
 2. 以下の cf push コマンドを実行して、アプリケーションを {{site.data.keyword.Bluemix_notm}} にプッシュします。  
固有のアプリケーション名を使用してください。
```
cf push YOUR_APP_NAME
```
5. ウィジェット・ライブラリーをベースにした Web アプリケーションを開きます。  
{{site.data.keyword.Bluemix_notm}} アプリケーションのダッシュボードから、**「Visit App URL」**をクリックして Web アプリケーションを開きます。  
**「経路」**をクリックし、アプリケーションの URL にアクセスして管理します。   
デフォルトの URL は、以下のような形式になります。  
`https://YOUR_APP_NAME.mybluemix.net`

## 手順 2 - 接続したデバイスを表示する
{: #view_devices}

Web コンソールが稼働中の状態になっているので、接続したコンベヤー・ベルト・デバイスを表示できます。
1. Web コンソールの**「デバイス」**セクションのリストに対象のコンベヤー・ベルトがあって、正しい RPM 値と「実行中」の状態が表示されていることを確認します。
2. コンベヤー・ベルト・デバイスの RPM 値を変更し、モニター・アプリケーションで値が正しく更新されることを確認します。


## 次の作業
{: @whats_next}  
次のガイドに進むか、興味のある別のトピックにジャンプします。
- パス A: ニーズに合わせてモニター・アプリケーションを変更します。  
技術的な詳細については、以下を参照してください。
 - [https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular/blob/master/README.md ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular/blob/master/README.md){: new_window}
 - [Node.js クライアント・ライブラリー ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- パス B: ニーズに合わせてウィジェット・ライブラリー・アプリケーションを変更します。  
技術的な詳細については、以下を参照してください。
 - [https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui/blob/master/README.md ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui/blob/master/README.md){: new_window}
- [ガイド 3: 多数のデバイスのシミュレート](getting-started-iot-large-scale-simulation.html)  
基本的なシミュレーションを拡張するために、多数の自己実行型のシミュレーターを環境に追加します。
- [{{site.data.keyword.iot_short_notm}} の詳細を確認してください。](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [{{site.data.keyword.iot_short_notm}} API の詳細を確認してください。](/docs/services/IoT/reference/api.html){:new_window}
