---

copyright:
  years: 2016, 2018
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Node-RED デバイス・シミュレーターの作成と接続
Node-RED を使用して、デバイス・シミュレーターを作成し、シミュレートしたデバイス・データを {{site.data.keyword.iot_full}} 組織に送信します。  
{:shortdesc}

## 概説
Node-RED は、ハードウェア・デバイス、API、オンライン・サービスを斬新な方法で接続するためのツールです。 詳しくは、[Node-RED ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://nodered.org/){: new_window} Web サイトを参照してください。  

Node-RED インスタンスは、独自の環境で実行することも、{{site.data.keyword.Bluemix_notm}} アプリケーションとして使用することもできます。以下の手順に、{{site.data.keyword.Bluemix_notm}} に関する説明も含まれています。

Node-RED デバイス・シミュレーターを作成して接続し、MQTT デバイス・メッセージを {{site.data.keyword.iot_short_notm}} に送信します。 以下の例で使用するデバイス・シミュレーターでは、貨物輸送コンテナーのデータを {{site.data.keyword.iot_short_notm}} などの MQTT ブローカーに送信する処理をシミュレートします。

## 手順

1. Node-RED デバイス・シミュレーターを作成します。以下の手順を実行してください。   
    1. {{site.data.keyword.Bluemix_notm}} (https://console.ng.bluemix.net) にログインします。
    2. **「カタログ」**タブを選択します。
    3. サービス・カタログの「ボイラープレート」セクションを見つけて、**「Internet of Things Platform Starter」**をクリックします。 **ヒント:** [こちら![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window} をクリックすると、「Internet of Things Platform Starter」ページに直接移動できます。
    4. 「Internet of Things Platform Starter」ページで、Node-RED のデプロイ先のスペースを選択し、「アプリの作成」の選択項目を確認し、**「作成」**をクリックして Node-RED を Bluemix 組織に追加します。 以下に例を示します。
    <ul>
     <li> スペース: dev
     <li> アプリケーション名: myDevice
     <li> ホスト名: myDevice  
    </ul>  
その他のオプションは、デフォルト値のままにしておきます。 アプリケーションがデプロイされると、「Node-RED でのコーディングの開始 (Start coding with Node-RED)」ページが表示されます。
**注:** ステージング・プロセスには数分かかることがあります。  

2. 「経路」リンクをクリックして Node-RED を開くか、「Visit App URL」から `http://myDevice.mybluemix.net` などにアクセスします。  
3. 以下の手順を実行して、**Node-RED エディターを保護します**。
    1. **「次へ」**をクリックします。
    2. ユーザー名とパスワードを追加します。  
    **注:** パスワードの複雑さに関する規則に準拠することが必要です。そうでない場合は、**「次へ」**ボタンがグレー表示のままになります。  
    3. オプションです。 **「全ユーザーにエディターの表示を許可し、変更は許可しない (Allow anyone to view the editor, but not make any changes)」**または**「非推奨: 全ユーザーにエディターの表示と変更を許可する (Not recommended: Allow anyone to access the editor and make changes)」**にチェック・マークを付けます。
    4. **「完了」**をクリックして、構成を完了します。
4. **「Go to your Node-RED flow editor」**をクリックします。
5. 作成したユーザー名とパスワードを入力します。  
デバイス・シミュレーター・フローがフロー・エディターで有効になります。 このフローでは、場所、温度の測定値、湿度データを {{site.data.keyword.iot_short_notm}} に送信する**サーモスタット**の処理をシミュレートします。  
6. デバイスが接続していることを確認するために、**「Send to IBM IoT Platform」**ノードにドットと接続メッセージが表示されているかどうかを確かめてください。 その確認は、**「Temperature Monitor」**サブフローの**「IBM IoT App In」**ノードでも有効です。  
7. MQTT デバイス・メッセージを送受信します。以下の手順を実行してください。  
    1. **「Send Data」**挿入ノードをクリックして、{{site.data.keyword.iot_short_notm}} へのデータ送信を起動します。
       **注:** **「Debug output payload」**デバッグ・ノードをアクティブ化すれば、どんなデータが送信されているかを確認できます。**「Device payload」**機能ノードには、ペイロードを作成したコードが表示されます。 
    2. **「Send Data」**をクリックすると、MQTT メッセージが {{site.data.keyword.iot_short_notm}} に送信され、**「IBM IoT App In」**ノードがそのメッセージを受け取ります。 **「Temperature Monitor」**サブフローで、温度が定義済みの制限値の範囲内になっているかどうかが確認され、デバッグ・タブにメッセージが表示されます。 
       **注:** **「temp thresh」**スイッチ・ノードをクリックすれば、しきい値を確認したり変更したりできます。
    3. デバッグ・タブを確認します。 **「Temperature (17) within safe limits」**などのメッセージが表示されます。
    
これでサンプル IoT デバイスが {{site.data.keyword.iot_short_notm}} に接続され、デバイス・データが表示されるようになりました。
