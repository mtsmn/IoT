---

copyright:
  years: 2018
lastupdated: "2018-05-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# ガイド 4: 多数のデバイスのシミュレート
最初のガイドでは、1 つ以上のコンベヤー・ベルトを手動でシミュレートするための基本的なデバイス・シミュレーターをセットアップしました。 このガイドでは、そのシミュレーションを拡張するために、多数の自己実行型のシミュレーターを環境に追加し、現実に近いマルチデバイス環境で分析機能とモニター機能をテストします。
{:shortdesc}

**重要:** このアプリケーションでは、512 MB のメモリーが必要です。この量は、デフォルトで割り振られる量を超えており、無料の試用版のアカウント ({{site.data.keyword.Bluemix}} の試用版アカウントや標準アカウントなど) で利用できる量も超えています。 サブスクライブ・アカウントや従量制課金アカウントの保有者であれば、メモリーの割り振り量を 512 MB に増やせます。 無料の試用版のアカウントの保有者は、サブスクライブ・アカウントや従量制課金アカウントにアップグレードする必要があります。 {{site.data.keyword.Bluemix_notm}} アカウント・タイプの詳細については、[アカウント・タイプ](/docs/pricing/index.html#pricing)を参照してください。

**注:** Bluemix は IBM Cloud になりました。詳しくは、[IBM Cloud ブログ](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

## 概説と目標
{: #overview}

このガイドでは、{{site.data.keyword.Bluemix_notm}} アプリケーションをセットアップして、複数のコンベヤー・ベルト・デバイスを使用する状況をシミュレートします。そのために、[Node-RED ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://nodered.org){: new_window} のバックエンドを使用します。

このアプリケーションには、以下の 3 つのフローがあります。   
1. 複数のデバイスを作成します。   
2. イベントの同時パブリッシュをシミュレートします。
3. デバイスを削除します。   

![デバイスの作成と削除](images/device_creation.png "デバイスの作成と削除")

![デバイス・イベントのシミュレーション](images/device_event_simulation.png "デバイス・イベントのシミュレーション")

このガイドの手順を実行することによって、以下の作業を行います。
- Cloud Foundry を使用して、Node-RED ベースの Webhook 対応デバイス・シミュレーター・アプリケーションをデプロイします。
- API 呼び出しを使用して、デバイスを作成/登録したり、デバイス・イベントをパブリッシュしたり、デバイスを削除したりします。

**重要:** 多数のデバイスから {{site.data.keyword.iot_short_notm}} にデバイス・データを同時に送信する状況をシミュレートすると、データ使用量が非常に多くなる可能性があります。 {{site.data.keyword.iot_short_notm}} の*「使用状況」*ダッシュボードを使用すれば、デバイスやアプリケーションのデータ使用量をモニターできます。 このメトリックは 2 時間間隔で最新表示されます。

## 前提条件
{: #prereqs}  
以下のアカウントおよびツールが必要です。

* [{{site.data.keyword.Bluemix_notm}} アカウント](https://console.ng.bluemix.net/registration/)    
 - 512 MB を超える RAM   
 - 1 GB を超えるディスク・クォータ  
 - 2 つの使用可能なサービス
* [Cloud Foundry コマンド・ライン・インターフェース (cf CLI) ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
cf CLI によって、{{site.data.keyword.Bluemix_notm}} アプリケーションをデプロイして管理します。
* オプション: [Git ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git-scm.com/downloads){: new_window}  
Git を使用してコード・サンプルをダウンロードする場合は、[GitHub.com アカウント ![外部リンク・アイコン](../../../icons/launch-glyph.svg " 外部リンク・アイコン")](https://github.com){: new_window} も必要です。 ただし、GitHub.com アカウントがなくても、コードを圧縮ファイルとしてダウンロードすることは可能です。

以下の手順の REST 呼び出しでは、cURL を使用することも、Mozilla の REST クライアント・アドオン・プラグインを使用することもできます。  
{: tip}

## 手順 1 - アプリケーションを {{site.data.keyword.Bluemix_notm}} にデプロイする
{: #step1}

以下の手順を実行し、アプリケーションを手動で作成してデプロイします。   

1. *Lesson4* サンプル・アプリケーションの GitHub リポジトリーを複製します。  
好みの git ツールを使用して以下のリポジトリーを複製します。  
https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
Git シェルでは、次のコマンドを使用します。
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
```
3. manifest.yml ファイルを編集して、ご使用の環境に合わせてアプリケーションを構成します。  
編集内容は以下のとおりです。
 - 既存の {{site.data.keyword.iot_short_notm}} サービスを使用する場合は、`lesson4-simulate-iotf-service` のすべてのインスタンスをそのサービス名に更新します。 例えば、ガイド 1 で作成した {{site.data.keyword.iot_short_notm}} サービスを使用する場合は、サービス名を `iotp-for-conveyor` にしてください。    
 - デバイス名とホストを設定します。   
applications セクションで、`name` と `host` の項目を固有の値 (`YOUR_NAME-lesson4-simulate` など) にしてください。   
**ヒント:** アプリケーションへのアクセス時に使用する経路 URL は、`host` 項目から作成されます。例えば、`https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net` のようになります。  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plan: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
1. コマンド・ラインから以下の cf api コマンドを実行して、API エンドポイントを設定します。   
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
2. ご使用の {{site.data.keyword.Bluemix_notm}} アカウントにログインします。
  ```
cf login
  ```
{{site.data.keyword.iot_short_notm}} とサンプル・アプリケーションのデプロイ先の組織とスペースを選択するためのプロンプトが表示されたら、それぞれの値を選択してください。
6. {{site.data.keyword.Bluemix_notm}} で必要なサービスを作成します。   
 1. 以下のコマンドを使用して、cloudantNoSQLDB Lite サービスを作成します。
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. 以下のコマンドを使用して、{{site.data.keyword.iot_short_notm}} サービスを作成します。
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**重要:** 既存の {{site.data.keyword.iot_short_notm}} サービスを使用していて、それに応じて manifest.yml ファイルを更新した場合は、既存のサービス名を使用していることを確認してください。 例えば、ガイド 1 で作成したサービスを使用する場合は、cf 呼び出しで `lesson4-simulate-iotf-service` の代わりに `iotp-for-conveyor` を使用する必要があります。
7. `cf push` コマンドを実行し、プロジェクトを作成して組織にプッシュします。  
サンプル・アプリケーションが {{site.data.keyword.Bluemix_notm}} にデプロイされます。  
デプロイメントが完了すると、アプリケーションが稼働していることを示すメッセージが表示されます。   
例:  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. アプリケーションのサービス資格情報を取得します。
 1. 以下の {{site.data.keyword.Bluemix_notm}} にログインします。  
 [https://bluemix.net ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://bluemix.net){: new_window}
 2. アプリケーションをデプロイしたアカウントとスペースを選択します。
 3. メニューから、**「アプリ」**を選択し、次に**「ダッシュボード」**を選択します。
 4. 「Cloud Foundry アプリ」で、デプロイしたアプリケーションの名前をクリックします。
 4. **「接続」**を選択します。
 5. アプリケーションで使用する iotf-service を見つけて、**「資格情報の表示」**をクリックします。  
 6. サービスの資格情報のページで、`apiKey` と `apiToken` を見つけます。   
アプリケーションの API を使用して、シミュレート対象デバイスを作成したり実行したりする時には、その API キーと認証トークンを資格情報として使用します。

## 手順 2 - Node-RED フローを保護する
{: #step2}

この手順は省略してもかまいませんが、Node-RED フローを保護しておくほうが得策です。

1. エディターを保護して、許可ユーザーだけがアクセスできるようにします。
2. Node-RED フローを保護するために、{{site.data.keyword.Bluemix_notm}} ダッシュボードに移動して、前にデプロイしたアプリケーションを選択します。
3. **「ランタイム」**をクリックし、**「環境変数」**をクリックします。
4. 以下のユーザー定義環境変数とそれぞれの値を追加します。
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. フローを保存します。

Node-RED フローを保護した場合は、REST API コマンドを使用して複数のデバイスを作成したり削除したりシミュレートしたりする時に、基本認証の資格情報として Node-RED のユーザー名とパスワードを指定しなければなりません。

## 手順 3 - デバイスを作成して接続する
{: #step3}

Node-RED のフロー・インターフェースやアプリケーションの REST API を使用して、以下の作業を実行できます。

### Node-RED を使用してデバイスを作成および接続する  
複数のデバイスを登録するには、次の手順を実行します。  
1. 以下の {{site.data.keyword.Bluemix_notm}} にログインします。  
[https://bluemix.net ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://bluemix.net){: new_window}
2. アプリケーションをデプロイしたアカウントとスペースを選択します。
3. メニューから、**「アプリ」**を選択し、次に**「ダッシュボード」**を選択します。
4. 「Cloud Foundry アプリ」で、デプロイしたアプリケーションの**経路** URL をクリックします。  
経路 URL (ROUTE_URL) は、manifest.yml ファイルで使用した `host` 項目から作成され、`HOST.mybluemix.net` という形式になります。  
例: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`  
Node-RED インターフェースが表示されます。
5. **「Go to your Node-RED flow editor」**をクリックします。
6. フロー・エディターで**「Device Type and Instance」**タブを選択します。
7. 5 つのデバイスを登録するために、**「Register 5 motorController devices」**というラベルの挿入ノードをクリックします。
8. デバイスが登録されたことを確認します。
 1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメニューから**「ボード」**を選択します。
 3. **「デバイス中心型の分析」**ボードを選択します。
 4. **「観察しているデバイス」**カードを見つけます。  
デバイス名が表示されます。

### REST API を使用してデバイスを作成および接続する  
複数のデバイスを登録するには、次の手順を実行します。  

1. URL `ROUTE_URL/rest/devices` に HTTP POST 要求を送信します。  
例: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - Content-Type と Accept を application/json に設定します。  
 - 以下の JSON ペイロードを使用します。  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
  各部の意味は以下のとおりです。  
    - numberDevices は、作成して登録するデバイスの数です。
    - オプション: typeId は、デバイス登録時に設定するデバイス・タイプです。 typeId を指定しなければ、デフォルトで iotp-for-conveyor になります。 **ヒント:** どんなデバイス・タイプ名でも入力できますが、このシリーズの他のガイドでは、デバイス・タイプが `iotp-for-conveyor` であるという前提で作業を進めます。 別のデバイス・タイプを使用する場合は、その名前に合わせて他のガイドの設定を変更してください。
    - オプション: authToken は、デバイス登録時に設定する許可トークンです。
    - オプション: chunkSize を指定しなければ、デフォルトで 500 に設定されます。 chunkSize は、numberDevices より少なく、またその因数でなければなりません。
    - deviceName は、作成したデバイスの deviceID のパターンです。
2. デバイスが登録されたことを確認します。
 1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメニューから**「ボード」**を選択します。
 3. **「デバイス中心型の分析」**ボードを選択します。
 4. **「観察しているデバイス」**カードを見つけます。  
デバイス名が表示されます。

## 手順 4 - デバイス・イベントをシミュレートする
{: #step4}

シミュレートするデバイスが {{site.data.keyword.iot_short_notm}} に登録されたので、シミュレーターを実行してデバイス・イベントの送信を開始できます。

### Node-RED を使用してデバイス・イベントをシミュレートする  
デバイス・イベントを送信するには、以下のようにします。  
1. Node-RED のフロー・エディターで**「Simulate multiple devices」**タブを選択します。
7. 5 つのデバイスをシミュレートするために、**「Simulate 5 devices」**というラベルの挿入ノードをクリックします。
8. デバイスからデータが送信されていることを確認します。
 1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメニューから**「ボード」**を選択します。
 3. **「デバイス中心型の分析」**ボードを選択します。
 4. **「観察しているデバイス」**カードを見つけます。  
 5. いずれかのデバイスを選択し、パブリッシュされたメッセージに相当する更新後のデバイス・データ・ポイントが**「デバイス・プロパティー」**カードに表示されていることを確認します。  


### Rest API を使用してデバイス・イベントをシミュレートする  
デバイス・イベントを送信するには、以下のようにします。

1. URL `ROUTE_URL/rest/runtest` に HTTP POST 要求を送信します。  
例: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - Content-Type と Accept を application/json に設定します。  
 - 以下の JSON ペイロードを使用します。   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
説明:
    - numberDevices は、データをシミュレートするデバイスの数です。
    - numberEvents は、シミュレートする各デバイスから送信されるイベントの数です。
    - timeInterval は、イベントの間隔 (ミリ秒) です。
    - deviceType は、シミュレートするデバイスを作成した時のデバイス・タイプです。
    - deviceName は、作成したデバイスの deviceID のパターンです。
8. デバイスからデータが送信されていることを確認します。
 1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメニューから**「ボード」**を選択します。
 3. **「デバイス中心型の分析」**ボードを選択します。
 4. **「観察しているデバイス」**カードを見つけます。  
 5. いずれかのデバイスを選択し、パブリッシュされたメッセージに相当する更新後のデバイス・データ・ポイントが**「デバイス・プロパティー」**カードに表示されていることを確認します。   

## 手順 5 - デバイスを削除する
{: #deleting}

### Node-RED  
デバイスを削除するには、以下のようにします。  
1. Node-RED のフロー・エディターで**「Device Type and Instance」**タブを選択します。
2. 5 つのデバイスを削除するために、**「Delete 5 devices」**というラベルの挿入ノードをクリックします。
3. デバイスが削除されたことを確認します。
 1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメニューから**「ボード」**を選択します。
 3. **「デバイス中心型の分析」**ボードを選択します。
 4. **「観察しているデバイス」**カードを見つけます。  
 5. デバイスがリストされなくなっていることを確認します。  


### Rest API  

1. URL `ROUTE_URL/rest/deleteDevices` に HTTP POST 要求を送信します。  
例: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - Content-Type と Accept を application/json に設定します。  
 - 以下の JSON ペイロードを使用します。      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName":"belt"  
}</code></pre>
2. デバイスが削除されたことを確認します。
 1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメニューから**「ボード」**を選択します。
 3. **「デバイス中心型の分析」**ボードを選択します。
 4. **「観察しているデバイス」**カードを見つけます。  
 5. デバイスがリストされなくなっていることを確認します。  


## 次の作業
{: @whats_next}  
興味のある別のトピックにジャンプします。
- [ガイド 2: リアルタイムの基本的なルールとアクションの使用](getting-started-iot-rules.html)  
コンベヤー・ベルトのセットアップ、{{site.data.keyword.iot_short_notm}} への接続、データの送信まで成功したので、ルールとアクションによってそのデータを活用します。
- [ガイド 3: デバイス・データのモニター](getting-started-iot-monitoring.html)  
1 つ以上のデバイスを接続してデバイス・データを活用するところまで完了したので、一連のデバイスのモニターを開始します。
- [{{site.data.keyword.iot_short_notm}} の詳細を確認してください。](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [{{site.data.keyword.iot_short_notm}} API の詳細を確認してください。](/docs/services/IoT/reference/api.html){:new_window}
