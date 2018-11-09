---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# ガイド 1: コンベヤー・ベルト・デバイスの接続  
{{site.data.keyword.Bluemix_notm}} 上の {{site.data.keyword.iot_full}} にモニター・データを送信する IoT デバイスを使用して基本的なコンベヤー・ベルトを作成します。
{:shortdesc}

## 概説と目標
{: #overview}  
この一連のステップバイステップ・ガイドでは、デバイスを {{site.data.keyword.iot_short_notm}} に接続してデバイス・データをモニターしたり操作したりするプロセスを学びます。これがその最初のガイドです。 各ガイドが前のガイドに基づいており、各ガイドのサンプル・コードが次のガイドの入力データになっています。 それぞれの用途に合わせてサンプル・コードを変更することによって、こうしたガイドをスタンドアロンで使用することも可能です。

この最初のガイドでは、接続したコンベヤー・ベルトをセットアップし、そのコンベヤー・ベルトを使用して IoT データを {{site.data.keyword.iot_short_notm}} に送信します。
スキル・レベルに応じて、コンベヤー・ベルトをセットアップするために以下のパスのいずれかまたは両方を実行してください。
- パス A  
このパスでは、{{site.data.keyword.Bluemix_notm}} にコンベヤー・ベルト・シミュレーター・アプリケーションをインストールして、すぐに作業を開始します。 このアプリケーションは、{{site.data.keyword.iot_short_notm}} に自動的にデバイスを登録し、定形式のデータをプラットフォームに自動的に送信します。 このパスの手順は、[手順 2A - GitHub からシミュレーター・サンプル・アプリケーションを使用する](#deploy_app)にあります。  
- パス B  
このパスを実行するには、高度な技術が必要です。追加のハードウェアや Python のプログラミング・スキルも必要で、デバイスを {{site.data.keyword.iot_short_notm}} に手動で登録しなければなりません。 このパスの手順は、[手順 2B - Raspberry Pi と電気モーターを使用して物理コンベヤー・ベルトを作成する](#raspberry)にあります。

このガイドの一環として、次のことを行います。
- Cloud Foundry CLI によって、{{site.data.keyword.iot_short_notm}} 組織を作成してデプロイします。
- サンプル・コンベヤー・ベルト・デバイスを作成してデプロイします。
- シミュレートするコンベヤー・ベルト・デバイスを {{site.data.keyword.iot_short_notm}} に接続します。

別の IoT デバイスを使用して {{site.data.keyword.iot_short_notm}} の作業を開始する場合は、[概説チュートリアル](/docs/services/IoT/getting-started.html)を参照してください。
{: tip}

## 前提条件
{: #prereqs}

以下のアカウントおよびツールが必要です。
* [{{site.data.keyword.Bluemix_notm}} アカウント ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.ng.bluemix.net/registration/){: new_window}
* [Cloud Foundry コマンド・ライン・インターフェース (cf CLI) ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
cf CLI によって、アプリケーションとサービスを {{site.data.keyword.Bluemix_notm}} にデプロイします。 詳しくは、[Cloud Foundry CLI 資料![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.cloudfoundry.org/cf-cli/){: new_window}を参照してください。
* オプション: [Git ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git-scm.com/downloads){: new_window}  
Git を使用してコード・サンプルをダウンロードする場合は、[GitHub.com アカウント ![外部リンク・アイコン](../../../icons/launch-glyph.svg " 外部リンク・アイコン")](https://github.com){: new_window} も必要です。 ただし、GitHub.com アカウントがなくても、コードを圧縮ファイルとしてダウンロードすることは可能です。
* オプション: *コンベヤー・ベルト* サンプル Web アプリケーションを実行して加速度センサー・データを送信するための携帯電話。

## 手順 1 - {{site.data.keyword.iot_short_notm}} をデプロイする  
{: #deploy_watson_iot_platform_service}
以下の手順では、`iotp-for-conveyor` という名前の {{site.data.keyword.iot_short_notm}} サービスのインスタンスを {{site.data.keyword.Bluemix_notm}} 環境にデプロイします。 すでに別のサービス・インスタンスを実行している場合は、この手順をスキップして、そのインスタンスを使ってガイドの作業を進めてもかまいません。 ただし、ガイドの作業を進めるにあたり、正しいサービス名と {{site.data.keyword.Bluemix_notm}} スペースを使用していることを確認してください。
{: tip}
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
3. {{site.data.keyword.iot_short_notm}} サービスを {{site.data.keyword.Bluemix_notm}} にデプロイします。  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
   ```
YOUR_IOT_PLATFORM_NAME の部分では *iotp-for-conveyor* を使用してください。  
例: `cf create-service iotf-service iotf-service-free iotp-for-conveyor`
3. サンプル・コンベヤー・ベルト・デバイスを作成します。  
 - パス A: [手順 2A - GitHub からシミュレーター・サンプル・アプリケーションを使用する](#deploy_app)  
 - パス B: [手順 2B - Raspberry Pi と電気モーターを使用して物理コンベヤー・ベルトを作成する](#raspberry)  

## 手順 2A - サンプル・コンベヤー・ベルト Web アプリケーションをデプロイする  
{: #deploy_app}

このサンプル・アプリケーションによって、{{site.data.keyword.Bluemix_notm}} に接続した業務用のコンベヤー・ベルトをシミュレートします。 ベルトを開始/停止したり、ベルトのスピードを調整したりできます。 ベルトに関するあらゆる変更が MQTT メッセージの形で {{site.data.keyword.Bluemix_notm}} に送信され、アプリケーションに表示されます。 デフォルトのダッシュボード・カードでベルトの動作をモニターできます。
このサンプル・アプリケーションは、[https://github.com/ibm-watson-iot/iot-nodejs ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window} にある Node.js クライアント・ライブラリーで作成されています。

![コンベヤー・ベルト・アプリケーション](images/app_conveyor_belt.png "コンベヤー・ベルト・アプリケーション")

1. 好みの git ツールを使用して以下のリポジトリーを複製します。  
https://github.com/ibm-watson-iot/guide-conveyor-simulator
Git シェルで、以下のコマンドを使用します。
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. コマンド・ラインで、サンプル・アプリケーションの置かれているディレクトリーに移動します。
  ```bash
cd guide-conveyor-simulator
  ```
3. *guide-conveyor-simulator* ディレクトリーから、アプリケーションを {{site.data.keyword.Bluemix_notm}} にプッシュし、新しい名前を設定します。以下の cf push コマンドで *YOUR_APP_NAME* をその名前に置き換えてください。 ここでは `--no-start` オプションを使用します。アプリケーションを {{site.data.keyword.iot_short_notm}} にバインドしてから、次の段階でアプリケーションを開始するからです。
**注:** アプリケーションのデプロイには数分の時間がかかることもあります。
   ```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. guide-conveyor-simulator ディレクトリーで、アプリケーションを {{site.data.keyword.iot_short_notm}} のインスタンスにバインドします。指定した名前を使用してください。
  ```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
アプリケーションのバインドの詳細については、[アプリケーションの接続](/docs/services/IoT/platform_authorization.html#bluemix-binding) を参照してください。
2. アプリケーションを開始して、バインディングを有効にします。
  ```bash
cf start YOUR_APP_NAME
  ```
この段階で、サンプル Web アプリケーションが {{site.data.keyword.Bluemix_notm}} にデプロイされます。
デプロイメントが完了すると、アプリケーションが稼働していることを示すメッセージが表示されます。   
例:
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
start command:     ./vendor/initial_startup.rb

     state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
アプリケーションのデプロイメントの状況とアプリケーションの URL の両方を確認するために、以下のコマンドを実行します。
  ```bash
cf apps
  ```
cf logs YOUR_APP_NAME --recent コマンドを使用して、デプロイメント・プロセスで発生したエラーのトラブルシューティングを行います。
{: tip}
1. ブラウザーでアプリケーションにアクセスします。  
https://YOUR_APP_NAME.mybluemix.net という URL を開きます。    
例: https://conveyorbelt.mybluemix.net/
2. デバイスのデバイス ID とトークンを入力します。  
指定したデバイス ID とトークンに基づいて、サンプル・アプリケーションが iot-conveyor-belt タイプのデバイスを自動的に登録します。 デバイスの登録の詳細については、[デバイスの接続](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1) を参照してください。
4. [手順 3 - {{site.data.keyword.iot_short_notm}} で生データを表示する](#see_live_data) に進みます。

## 手順 2B - Raspberry Pi をベースにしたコンベヤーベルトを作成する
{: #raspberry}

Raspberry Pi ソリューションは、[https://github.com/ibm-watson-iot/iot-python ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/iot-python){: new_window} にある Python クライアント・ライブラリーで作成されています。

### 回路図

![回路図](images/raspi_circuit.png)

### 必要な接続

Raspberry Pi と L298N デュアル H ブリッジ・モーター・ドライバー・ボードとの間に以下の接続が必要です。 接続:

- Raspberry Pi のピン 2 と L298N ボードの +5v
- Raspberry Pi のピン 6 と L298N ボードの GND
- Raspberry Pi のピン 13 と L298N ボードの IN1
- Raspberry Pi のピン 15 と L298N ボードの IN2
- バッテリーの +9v と L298N ボードの +12v
- バッテリーの -9v と L298N ボードの GND
- モーターの +ve ノードと L298N ボードの OUT1
- モーターの -ve ノードと L298N ボードの OUT2

### ハードウェア要件

1. [Raspberry Pi(2/3) ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.raspberrypi.org/){: new_window} と Raspbian Jessie (8.x)
2. [L298N デュアル H ブリッジ・モーター・ドライバー・ボード ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. 9V DC モーター
4. 9V バッテリー

### Raspberry Pi のソフトウェア要件
- Python 2.7.9 以上。Raspbian Jessie (8.x) をインストールする必要があります。
- [Watson IoT Python ライブラリー ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- オプション: Git。  
Raspberry Pi に git をインストールしていない場合は、以下のコマンドでインストールできます。  
```bash
$ sudo apt-get install git
```

### 詳しい手順

1. Raspberry Pi で端末または SSH を開きます。
2. 好みの git ツールを使用して以下のリポジトリーを Raspberry Pi に複製します。
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
Git シェルで以下のコマンドを使用します。
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. デバイスを {{site.data.keyword.iot_short_notm}} に登録します。  
デバイスの登録の詳細については、[デバイスの接続](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1) を参照してください。
	 1. {{site.data.keyword.Bluemix_notm}} コンソールの{{site.data.keyword.iot_short_notm}} サービス詳細ページで「起動」をクリックします。
	     新しいブラウザー・タブで以下の URL の {{site.data.keyword.iot_short_notm}} Web コンソールが開きます。
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     ここで、ORG_ID は [{{site.data.keyword.iot_short_notm}} 組織](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}の 6 文字の固有 ID です。
	 2. 「概要」ダッシュボードのメニュー・ペインから**「デバイス」**を選択し、**「デバイスの追加」**をクリックします。
	 3. 追加するデバイスのデバイス・タイプを作成します。
	     1. **「デバイス・タイプの作成」**をクリックします。
	     2. デバイス・タイプ名 `iotp-for-conveyor` とデバイス・タイプの説明を入力します。  
	**ヒント:** どんなデバイス・タイプ名でも入力できますが、このシリーズの他のガイドでは、デバイスで使用されるデバイス・タイプが `iotp-for-conveyor` であるという前提で作業を進めます。別のデバイス・タイプを使用する場合は、その名前に合わせて他のガイドの設定を変更してください。
	     3. オプション: デバイス・タイプの属性やメタデータを入力します。
	 4. **「次へ」**をクリックして、選択したデバイス・タイプのデバイスを追加するプロセスを開始します。
	 5. `conveyor_belt` などのデバイス ID を入力します。
	 5. **「次へ」**をクリックすると、プロセスが完了します。
	 6. 認証トークンを指定するか、自動生成されたトークンを受け入れます。
	 7. 要約情報が正しいことを確認してから、**「追加」**をクリックして接続を追加します。
	 8. デバイス情報のページで、以下の詳細をコピーして保存します。
	     * 組織 ID
	     * デバイス・タイプ
	     * デバイス ID
	     * 認証方式
	     * 認証トークン
	      {{site.data.keyword.iot_short_notm}} に接続するようにデバイスを構成するには、組織 ID、デバイス・タイプ、デバイス ID、および認証トークンの値が必要になります。
4. 複製したリポジトリーの *guide-conveyor-rasp-pi* ルートに移動します。
5. `sudo chmod +x setup.sh` コマンドを使用して、シェル・スクリプトを実行可能ファイルにします。
6. *setup.sh* ファイルを実行します。デバイス情報ページからコピーした詳細情報の入力を求めるプロンプトが表示されたら、その情報を入力します。
```bash  
./setup.sh
```  

7. deviceClient プログラムを実行します。  
このプログラムを実行すると、Raspberry Pi によってモーターが開始され、パラメーター設定に基づいて最大 2 分間作動します。
```bash
python deviceClient.py -t 2
```

モーターの作動中に、このプログラムからイベント・タイプ `sensorData` のイベントがパブリッシュされます。サンプル・ペイロード構造は以下のとおりです。  
```
{
"d": {
  "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. [手順 3 - {{site.data.keyword.iot_short_notm}} で生データを表示する](#see_live_data)に進みます。

## 手順 3 - {{site.data.keyword.iot_short_notm}} で生データを表示する
{: #see_live_data}
1. デバイスが {{site.data.keyword.iot_short_notm}} に登録されていることを確認します。
 1. {{site.data.keyword.Bluemix_notm}} ダッシュボード (https://bluemix.net) にログインします。
 2. [サービスのリスト ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://bluemix.net/dashboard/services){: new_window} から、*iotp-for-conveyor* {{site.data.keyword.iot_short_notm}} サービスをクリックします。
 3. *「起動」*をクリックして、{{site.data.keyword.iot_short_notm}} ダッシュボードを新しいブラウザー・タブで開きます。  
URL にブックマークを付けておくと、後からアクセスしやすくなります。   
例: `https://*iot-org-id*.internetofthings.ibmcloud.com`
 4. メニューから**「デバイス」**を選択し、新しいデバイスが表示されていることを確認します。
2. センサー・データをプラットフォームに送信します。   
センサーの読み取り値が変わると、デバイスがデータを {{site.data.keyword.iot_short_notm}} に送信します。 コンベヤー・ベルトを停止/開始したりスピードを変更したりして、このデータ送信をシミュレートできます。   
**パス A:** モバイル・ブラウザーでアプリケーションにアクセスしている場合は、コンベヤー・ベルトの加速度センサー・データを起動するために、スマートフォンを振ってみてください。
{: tip}

## 次の作業
{: @whats_next}  
次のガイドに進むか、興味のある別のトピックにジャンプします。
- パス A: ニーズに合わせてコンベヤー・ベルト・アプリケーションを変更します。  
技術的な詳細については、以下を参照してください。
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Node.js クライアント・ライブラリー ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **注:** Bluemix は IBM Cloud になりました。詳しくは、[IBM Cloud ブログ](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。
 
- パス B: ニーズに合わせて Raspberry Pi のセットアップを変更します。  
技術的な詳細については、以下を参照してください。
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}


**注:** Bluemix は IBM Cloud になりました。詳しくは、[IBM Cloud ブログ](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン") を参照してください。

- [ガイド 2: デバイス・データのモニター](getting-started-iot-monitoring.html)  
1 つ以上のデバイスを接続してデバイス・データを活用するところまで完了したので、一連のデバイスのモニターを開始します。
- [ガイド 3: 多数のデバイスのシミュレート](getting-started-iot-large-scale-simulation.html)  
パス A のコンベヤー・ベルト・サンプル・アプリケーションを使用すれば、1 つか少数のコンベヤー・ベルト・デバイスを手動でシミュレートできます。 このガイドでは、多数のデバイスをシミュレートするための環境をセットアップします。
- [{{site.data.keyword.iot_short_notm}} の詳細を確認してください。](/docs/services/IoT/iotplatform_overview.html)
- [{{site.data.keyword.iot_short_notm}} API の詳細を確認してください。](/docs/services/IoT/reference/api.html)
