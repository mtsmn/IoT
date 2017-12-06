---

copyright:
  years: 2017
lastupdated: "2017-09-18"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Data Science Experience を使用したデータの分析
{: #DSX_integration}

{{site.data.keyword.iot_full}} を Data Science Experience (DSX) と併用することで、プラットフォームに接続されているデバイスから送信されるデータを視覚化して把握することができます。
{: shortdesc}

## 概要と目的

このガイドは、分析ツールとして DSX を使用して {{site.data.keyword.iot_short_notm}} デバイスのイベント・データを視覚化する一連のプロセスを段階的に記載しています。

DSX には、複数のツールを使用して洞察を駆使できる、対話式でクラウド・ベースのコラボレーション環境が用意されています。IBM DSX で利用できる Jupyter Notebook を使用して、{{site.data.keyword.iot_short_notm}} の履歴データをロードし、そのデータで視覚化を作成してデータ分析や異常の識別に役立てることができます。履歴データからしきい値を導き出し、それらの値を使用して {{site.data.keyword.iot_short_notm}} のクラウド・ルールを作成できます。ルールに関連付けられた IoT デバイスが構成済みのしきい値限度を超えた読み取り値をパブリッシュすると、クラウド・ルールはユーザーに対してアラートを出します。

{{site.data.keyword.iot_short_notm}} に送信されるデバイス・データは、{{site.data.keyword.cloudantfull}} NoSQL DB サービスを使用して収集し、{{site.data.keyword.Bluemix}} に保管することができます。データを収集するには、まず {{site.data.keyword.iot_short_notm}} を {{site.data.keyword.cloudant_short_notm}} サービスに接続する必要があります。デバイス・データは、構成されたバケット間隔に従って、{{site.data.keyword.cloudant_short_notm}} の日次、週次、または月次データベースに保管されます。


![DSX を使用したデータ分析の概要](images/DSX_overview.png)

このガイドの一環として、次のことを学習します。

 - Cloudant NoSQL DB をヒストリアン・サービスとして使用するようにプラットフォーム・データ・ストレージを構成する方法。
 - Weather Sensors シミュレーターを使用してプラットフォームが使用するデータを生成する方法。
 - データをエクスポートしてから DSX にインポートしてデータを分析する方法。
 - IoT データの視覚化、異常の検出、アラートの構成を行う方法。


## 前提条件

これらの手順を実行するには、[Cloudant NoSQL DB ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window} を使用した [{{site.data.keyword.iot_short_notm}} ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} へのアクセス権限、および [DSX アカウント ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://datascience.ibm.com/docs/content/getting-started/get-started.html){: new_window} へのアクセス権限が必要です。


## 手順 1: シミュレーターをセットアップする
{:#DSX_sensor_data}

意味のある分析を行うためには、意味のあるデータが必要です。実際のセンサー・データをシミュレートして、DSX で Watson IoT Platform デバイス・データを分析する方法を学ぶことができます。ここでは、以下の手順について説明します。
 - [**{{site.data.keyword.iot_short_notm}} の既存インスタンス**を使用したシミュレーターのセットアップ](#sim_existing_platorm)
 - [**{{site.data.keyword.iot_short_notm}} の新規インスタンス**を使用したシミュレーターのセットアップ](#sim_new_platform)


### {{site.data.keyword.iot_short_notm}} の既存インスタンスを使用した Weather Sensors シミュレーターのセットアップ
{: #sim_existing_platform}

Weather Sensors シミュレーターを使用して組織における実際のセンサー・データ・イベントをシミュレートするには、まずシミュレーターをセットアップする必要があります。以下の手順は、既に {{site.data.keyword.iot_short_notm}} のインスタンスが稼働中であることが前提になっています。

1. [シミュレーターの実行に必要な API キーとトークンを生成します。![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Weather Sensors シミュレーター Web アプリをデプロイし ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}、詳細な説明に従います。

   Weather Sensors について詳しくは、[Weather Sensors シミュレーターのガイド ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} を参照してください。
3. [手順 2. データベース・コネクターを構成する](#DSX_config_db)に進みます。


### {{site.data.keyword.iot_short_notm}} の新規インスタンスを使用した Weather Sensors シミュレーターのセットアップ
{: #sim_new_platform}

Weather Sensors シミュレーターを使用して組織における実際のセンサー・データ・イベントをシミュレートするには、まずシミュレーターをセットアップする必要があります。以下の手順には、シミュレーターとともに {{site.data.keyword.iot_short_notm}} インスタンスを作成するための説明も含まれています。

1. [Weather Sensors シミュレーター Web アプリと {{site.data.keyword.iot_short_notm}} のインスタンスをデプロイし ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window}、詳細な手順に従います。

   Weather Sensors について詳しくは、[Weather Sensors シミュレーターのガイド ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} を参照してください。
2. デプロイメントの完了を待ってから、Bluemix ダッシュボードにナビゲートします。
3. デプロイメント・プロセスで作成された {{site.data.keyword.iot_short_notm}} サービス「wiotp-for-weather-sensors-simulator」を起動します。
4. [手順 2. データベース・コネクターを構成する](#DSX_config_db)に進みます。


## 手順 2. データベース・コネクターを構成する
{: #DSX_config_db}

デバイス・データは、選択されたバケット間隔オプションに従って、Cloudant の日次、週次、または月次データベースに保管できます。すべてのデバイスから収集された、同じバケット間隔 (日、週、または月) に属するデータは、同じデータベースに保管されます。

バケット間隔の構成とは無関係に、コネクターによって 3 つのデータベースが自動的に作成されます。1 つのデータベースは現在のバケット間隔に対して作成され、1 つのデータベースは後続の間隔に対して作成され、もう 1 つは構成データベースに対して作成されます。間隔の終わりに到達すると、新しい間隔に対してデバイス・データがバケット・データベースに保管され、後続のバケットに対して新規データベースが作成されます。

{{site.data.keyword.cloudant_short_notm}} と DSX を併用するには、Cloudant NoSQL DB をヒストリアン・サービスとして使用するようにプラットフォーム・データ・ストレージを構成する必要があります。

1. {{site.data.keyword.cloudant_short_notm}} ダッシュボードで、ナビゲーション・バーの**「拡張」**をクリックします。
2. **「履歴データ・ストレージ」**で、**「セットアップ」**をクリックします。**「履歴データ・ストレージの構成」**セクションに、{{site.data.keyword.cloudant_short_notm}} と同じ Bluemix スペース内で使用できるすべての Cloudant NoSQL DB サービスがリストされます。
3. 接続する Cloudant NoSQL DB サービスを選択します。
4. 次の Cloudant NoSQL DB 構成オプションを指定します。
  - バケット間隔 = 日
  - タイム・ゾーン = UTC
  - データベース名 = デフォルト
5. **「完了」**をクリックして、Cloudant サービスへの接続の許可を確認します。確認ウィンドウにアクセスできるように、ブラウザーでポップアップを有効にしてください。Cloudant NoSQL DB が正常に構成されると、「履歴データ・ストレージ」の状況が構成済みに変わり、デバイス・データが {{site.data.keyword.cloudant_short_notm}} NoSQL DB に保管されます。
6. [手順 3: シミュレーターを実行する](#run_simulator)に進みます。


## 手順 3: シミュレーターを実行する
{: #run_simulator}

シミュレーターが、Haifa 領域にある 17 個の気象ステーションから実際の気象センサー・データを {{site.data.keyword.iot_short_notm}} 組織にパブリッシュします。

1. シミュレーターにナビゲートします。
2. {{site.data.keyword.iot_short_notm}} のバインド済みインスタンスとともにシミュレーターをデプロイした場合は、手順 3 に進んでください。スタンドアロン・バージョンのシミュレーターをデプロイした場合は、以下の詳細を入力してください。
   - Watson IoT Platform 組織
   - API キー
   - 認証トークン

3. **「シミュレーターの実行 (Run Simulator)」**をクリックします。データの生成には数分かかる場合があります。
4. シミュレーターの稼働中に Watson IoT Platform にアクセスし、デバイスが作成されたこと、およびこれらのデバイスにイベントが送信されることを確認します。
5. [手順 4. DSX をセットアップしてデータを視覚化する](#DSX_visualize_data)に進みます。


## 手順 4. DSX をセットアップしてデータを視覚化する
{: #DSX_visualize_data}

DSX をセットアップしてデータの視覚化を開始するには、以下のようにします。

1. データに対する洞察を得て、異常を検出するには、[事前構成 Jupyter Notebook をセットアップします](#setup_jupyter_notebook)。
2. [分析を実行します。](#run_analysis)
3. [センサー異常に関するアラートを構成します](#config_alerts)。


### 1. 事前構成 Jupyter Notebook のセットアップ
{: #setup_jupyter_notebook}

Jupyter Notebook は、実行可能コード、数式、グラフィック/視覚化 (matplotlib) 説明テキストが入った文書を作成して共有することができる Web アプリケーションです。

データに対する洞察を得て異常を検出するために事前構成 Jupyter Notebook をセットアップするには、以下のようにします。
1. サポート対象のブラウザーを使用して、Bluemix ID で [DSX ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://datascience.ibm.com/){: new_window} にログインします。
2. 「+」をクリックして**「プロジェクトの作成」**を選択し、新規プロジェクトを作成します。プロジェクトによってスペースが作成されます。ノートブックの収集と共有、データ・ソースへの接続、パイプラインの作成、データ・セットの追加はその場所で行います。
3. プロジェクト名を指定して、**「作成」**をクリックします。DSX アカウントのセットアップ中に、Spark サービスと Object Storage インスタンスが自動的に作成されます。別の方法として、Bluemix インターフェースを使用してそれらを作成してから、後の段階で DSX プロジェクトに関連付けることもできます。
4. **DSX** メニューでプロジェクトを選択し、![「データの検索と追加 (Find and Add Data)」アイコン](images/find_add_data_icon.png) をクリックして Cloudant 資格情報をインポートします。
5. **「接続」**タブをクリックします。
6. **「接続の作成」**をクリックして Cloudant 資格情報をインポートし、それをプロジェクト内のすべてのノートブックで使用可能にします。
7. 以下の情報を入力して、**「新規接続 (New Connections)」**画面を完成させます。
   - 接続名
   - **「サービス・カテゴリー (Service Category)」**を`「データ・サービス (Data Service)」`に設定します。
   - **「ターゲット・サービス・インスタンス (Target service instance)」**ドロップダウン・メニューから Cloudant サービスを選択します。
   - 現在日付に対応する Cloudant データベースを選択します。
8. **「作成」**をクリックします。
9. **「ノートブックの追加 (add notebooks)」**をクリックして、新規 Jupyter ノートブックを作成します。
10. **「URL から (From URL)」**を選択して既存のノートブックをロードしてから、ノートブックの記述名を指定し、以下の URL を入力してサンプル・ノートブックを開きます。
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```
11. **「ノートブックの作成 (Create Notebook)」**をクリックします。ノートブックが作成され、メタデータとコードが含まれていることを確認します。
12. 先頭が '!pip install --upgrade pixiedust, ,' であるセルを選択し、**「再生 (Play)」**をクリックしてコードを実行します。
13. インストールが完了したら、**「カーネルの再始動 (Restart Kernel)」**アイコンをクリックして Spark カーネルを再始動します。
14. カーネルが再始動するのを 10 秒ほど待ってから、コメント付きのセルをクリックして選択します。
15. 以下の手順を実行することにより、そのセルに Cloudant 資格情報をインポートします。

    1. ![「データの検索と追加 (Find and Add Data)」アイコン](images/find_add_data_icon.png) をクリックします。
    2. **「接続」**タブを選択します。
    3. **「コードに挿入 (Insert to code)」**をクリックします。
ご使用の Cloudant 資格情報で「credentials_1」という辞書が作成されます。辞書の名前が「credentials_1」と指定されていない場合は、ノートブックのコードの実行に必要な名前である「credentials_1」に名前変更します。
16. データベース名 (dbname) が表示されているセルに、データのソースである Cloudant データベースの名前 (例: `iotp_yourWIoTPorgId_default_Year-month-day`) を入力します。
17. ノートブックを保存します。ノートブックを実行する準備ができました。


### 2. 分析の実行
{: #run_analysis}

1.	Cloudant 資格情報が含まれているセルを選択します。
2.	**「再生 (Play)」**をクリックしてセル・コードを実行します。
3.	実行結果を確認して、各セルで使用されている python コードを分析します。
4.	それぞれのセルで手順 2 と 3 を繰り返します。**「ユーザー入力必須 (User input required)」**が指定されたセルの場合、実行前に、次のコード・セルで定義されている変数に対して新しい入力値を指定する必要があります。

**注:** 一部のセルはバックグラウンド Spark ジョブを実行するため、完了まで時間がかかる場合があります。セル内のコード実行が完了すると、アスタリスク `*` が数値に変わります (例えば、In `[*]` が In `[1]` に変わります)。この手順が完了したら、{{site.data.keyword.iot_short_notm}} でクラウド・ルールを作成して、異常の検出時に自動的にアラートを生成できます。


### 3. センサー異常に関するアラートの構成
{: #config_alerts}


{{site.data.keyword.iot_short_notm}} でクラウド・ルールを作成できます。パブリッシュされたイベントがノートブックから導き出されたしきい値を超えたときに異常が検出されると、これらのルールによってアラートを生成できます。

この例では、Nitrogen Dioxide (NO2) と、1 つの特定のデバイスの上限/下限しきい値を使用します。NO2 値が設定したしきい値を超えた場合に指定の E メール・アドレスに E メールが送信されるように、E メール・アクションを作成します。

クラウド・ルールを作成するには、次のようにします。

1. スキーマを作成します。
    1. {{site.data.keyword.iot_short_notm}} ダッシュボードの **「デバイス」**タブで、**「スキーマの管理」**タブを選択します。
    2. **「スキーマの追加」**をクリックします。
    3. スキーマの作成対象となる「DeviceType WS」を選択し、**「次へ」**をクリックします。
    4. **「プロパティーを追加」**をクリックして、接続済みデバイスからのデータ・ポイントを追加します。
    5. **「手動」**タブで、データ・タイプ・フィールドを`「浮動小数点」`、プロパティー・フィールドを`「NO2」`に設定します。
    6. **「OK」**をクリックします。
    7. **「完了」**をクリックします。

2. アクションを作成します。
    1. {{site.data.keyword.iot_short_notm}} ダッシュボードの**「ルール」**タブを選択します。
    2. **「アクション」**タブを選択します。
    3. **「+アクションの作成」**をクリックします。
    4. **「新規アクションの作成」**画面で、名前を入力し、アクション・タイプとして「E メールの送信」を選択します。
    5. **「次へ」**をクリックします。
    6. **「アクションの編集」**画面で、**「データの組み込み」**トグルをオンにします。
    7. **「完了」**をクリックします。
    8. **「ルール」**タブで、**「参照」**タブを選択します。
    9. **「+クラウド規則の作成」**をクリックします。
    10. **「新規クラウド規則の追加」**画面で、ルールの名前を入力し、**「適用対象」**フィールドでスキーマ名を選択します。この例では、スキーマ名は「WS」です。
    11. **「次へ」**をクリックします。

3. 条件を設定します。以下の手順で使用するしきい値は、NO2 グラフの横の、ノートブックで最後に実行されたコード・チャンクにあります。
    1. 「新規条件」をクリックして、最初の条件を以下のように設定します。
    - **「プロパティー」**フィールドに、`Nitrogen Dioxide` と入力します。
    - **「オペレーター」**フィールドで、より大記号アイコン `>` を選択します。
    - **「値」**フィールドに、上限しきい値を入力します。
    - **「OK」**をクリックします。
    2. 「OR」を選択してから、2 つ目の条件を追加します。
    - **「プロパティー」**フィールドに、`Nitrogen Dioxide` と入力します。
    - **「オペレーター」**フィールドで、より小記号アイコン `<` を選択します。
    - **「値」**フィールドに、下限しきい値を入力します。
    - **「OK」**をクリックします。これで、ルールを起動する条件を設定できました。
4. アクションを「E メールの送信」に設定し、**「OK」**をクリックしてルールをアクティブにします。デバイスがパブリッシュした Nitrogen Dioxide の読み取り値がいずれかのしきい値を超えた場合、E メール・アラートが生成されます。アラート E メールは、シミュレーターを実行して見ることができます。


## 次の作業

DSX について詳しくは、以下のリソースを参照してください。

 - [WIoTP クラウドのルール ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window}
 - [DSX コミュニティー・ノートブックとチュートリアル ![ 外部リンク・アイコン](../../../icons/launch-glyph.svg " 外部リンク・アイコン")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} (Jupyter ノートブックについて詳しくはリンクをたどります)。
 - [Watson IoT Platform クックブックの分析レシピ ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
