---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-01"
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

DSX には、複数のツールを使用して洞察を駆使できる、対話式でクラウド・ベースのコラボレーション環境が用意されています。 IBM DSX で利用できる Jupyter Notebook を使用して、{{site.data.keyword.iot_short_notm}} の履歴データをロードし、そのデータで視覚化を作成してデータ分析や異常の識別に役立てることができます。 履歴データからしきい値を導き出し、それらの値を使用して {{site.data.keyword.iot_short_notm}} のクラウド・ルールを作成できます。 ルールに関連付けられた IoT デバイスが構成済みのしきい値限度を超えた読み取り値をパブリッシュすると、クラウド・ルールはユーザーに対してアラートを出します。

{{site.data.keyword.iot_short_notm}} に送信されるデバイス・データは、{{site.data.keyword.cloudantfull}} NoSQL DB サービスを使用して収集し、{{site.data.keyword.Bluemix}} に保管することができます。 データを収集するには、まず {{site.data.keyword.iot_short_notm}} を {{site.data.keyword.cloudant_short_notm}} サービスに接続する必要があります。 デバイス・データは、構成されたバケット間隔に従って、{{site.data.keyword.cloudant_short_notm}} の日次、週次、または月次データベースに保管されます。


![DSX を使用したデータ分析の概要](images/DSX_overview.png)

このガイドの一環として、次のことを学習します。

 - Cloudant NoSQL DB をヒストリアン・サービスとして使用するようにプラットフォーム・データ・ストレージを構成する方法。
 - Weather Sensors シミュレーターを使用してプラットフォームが使用するデータを生成する方法。
 - データをエクスポートしてから DSX にインポートしてデータを分析する方法。
 - IoT データの視覚化、異常の検出、アラートの構成を行う方法。


## 前提条件

これらの手順を実行するには、[Cloudant NoSQL DB ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window} を使用した [{{site.data.keyword.iot_short_notm}} ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} へのアクセス権限、[Apache Spark ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} サービスへのアクセス権限、および [DSX アカウント ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window}へのアクセス権限が必要です。


## 手順 1: シミュレーターをセットアップする
{:#DSX_sensor_data}

意味のある分析を行うためには、意味のあるデータが必要です。 実際のセンサー・データをシミュレートして、DSX で Watson IoT Platform デバイス・データを分析する方法を学ぶことができます。 ここでは、以下の手順について説明します。
 - [**{{site.data.keyword.iot_short_notm}} の既存インスタンス**を使用したシミュレーターのセットアップ](#sim_existing_platorm)
 - [**{{site.data.keyword.iot_short_notm}} の新規インスタンス**を使用したシミュレーターのセットアップ](#sim_new_platform)


### {{site.data.keyword.iot_short_notm}} の既存インスタンスを使用した Weather Sensors シミュレーターのセットアップ
{: #sim_existing_platform}

Weather Sensors シミュレーターを使用して組織における実際のセンサー・データ・イベントをシミュレートするには、まずシミュレーターをセットアップする必要があります。 以下の手順は、既に {{site.data.keyword.iot_short_notm}} のインスタンスが稼働中であることが前提になっています。

1. [シミュレーターの実行に必要な API キーとトークンを生成します。![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Weather Sensors シミュレーター Web アプリをデプロイし ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}、詳細な説明に従います。

   Weather Sensors について詳しくは、[Weather Sensors シミュレーターのガイド ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} を参照してください。
3. [手順 2. データベース・コネクターを構成する](#DSX_config_db)に進みます。


### {{site.data.keyword.iot_short_notm}} の新規インスタンスを使用した Weather Sensors シミュレーターのセットアップ
{: #sim_new_platform}

Weather Sensors シミュレーターを使用して組織における実際のセンサー・データ・イベントをシミュレートするには、まずシミュレーターをセットアップする必要があります。 以下の手順には、シミュレーターとともに {{site.data.keyword.iot_short_notm}} インスタンスを作成するための説明も含まれています。

1. [Weather Sensors シミュレーター Web アプリと {{site.data.keyword.iot_short_notm}} のインスタンスをデプロイし ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window}、詳細な手順に従います。

   Weather Sensors について詳しくは、[Weather Sensors シミュレーターのガイド ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} を参照してください。
2. デプロイメントの完了を待ってから、IBM Cloud ダッシュボードにナビゲートします。
3. デプロイメント・プロセスで作成された {{site.data.keyword.iot_short_notm}} サービス「wiotp-for-weather-sensors-simulator」を起動します。
4. [手順 2. データベース・コネクターを構成する](#DSX_config_db)に進みます。


## 手順 2. データベース・コネクターを構成する
{: #DSX_config_db}

デバイス・データは、選択されたバケット間隔オプションに従って、Cloudant の日次、週次、または月次データベースに保管できます。 すべてのデバイスから収集された、同じバケット間隔 (日、週、または月) に属するデータは、同じデータベースに保管されます。

バケット間隔の構成とは無関係に、コネクターによって 3 つのデータベースが自動的に作成されます。 1 つのデータベースは現在のバケット間隔に対して作成され、1 つのデータベースは後続の間隔に対して作成され、もう 1 つは構成データベースに対して作成されます。 間隔の終わりに到達すると、新しい間隔に対してデバイス・データがバケット・データベースに保管され、後続のバケットに対して新規データベースが作成されます。

{{site.data.keyword.cloudant_short_notm}} と DSX を併用するには、Cloudant NoSQL DB をヒストリアン・サービスとして使用するようにプラットフォーム・データ・ストレージを構成する必要があります。

1. {{site.data.keyword.cloudant_short_notm}} ダッシュボードで、ナビゲーション・バーの**「拡張」**をクリックします。
2. **「履歴データ・ストレージ」**で、**「セットアップ」**をクリックします。 **「履歴データ・ストレージの構成」**セクションに、{{site.data.keyword.cloudant_short_notm}} と同じ IBM Cloud スペース内で使用できるすべての Cloudant NoSQL DB サービスがリストされます。
3. 接続する Cloudant NoSQL DB サービスを選択します。
4. 次の Cloudant NoSQL DB 構成オプションを指定します。
  - バケット間隔 = 日
  - タイム・ゾーン = UTC
  - データベース名 = デフォルト
5. **「完了」**をクリックして、Cloudant サービスへの接続の許可を確認します。 確認ウィンドウにアクセスできるように、ブラウザーでポップアップを有効にしてください。 Cloudant NoSQL DB が正常に構成されると、「履歴データ・ストレージ」の状況が構成済みに変わり、デバイス・データが {{site.data.keyword.cloudant_short_notm}} NoSQL DB に保管されます。
6. [手順 3: シミュレーターを実行する](#run_simulator)に進みます。


## 手順 3: シミュレーターを実行する
{: #run_simulator}

シミュレーターが、Haifa 領域にある 17 個の気象ステーションから実際の気象センサー・データを {{site.data.keyword.iot_short_notm}} 組織にパブリッシュします。

1. シミュレーターにナビゲートします。
2. {{site.data.keyword.iot_short_notm}} のバインド済みインスタンスとともにシミュレーターをデプロイした場合は、手順 3 に進んでください。スタンドアロン・バージョンのシミュレーターをデプロイした場合は、以下の詳細を入力してください。
   - Watson IoT Platform 組織
   - API キー
   - 認証トークン

3. **「シミュレーターの実行 (Run Simulator)」**をクリックします。 データの生成には数分かかる場合があります。
4. シミュレーターの稼働中に Watson IoT Platform にアクセスし、デバイスが作成されたこと、およびこれらのデバイスにイベントが送信されることを確認します。
5. [手順 4. DSX をセットアップしてデータを視覚化する](#DSX_visualize_data)に進みます。


## 手順 4. DSX をセットアップしてデータを視覚化する
{: #DSX_visualize_data}

DSX をセットアップしてデータの視覚化を開始するには、以下のようにします。

1. データに対する洞察を得て、異常を検出するには、[事前構成 Jupyter Notebook をセットアップします](#setup_jupyter_notebook)。
2. [分析を実行します。](#run_analysis)
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. 事前構成 Jupyter Notebook のセットアップ
{: #setup_jupyter_notebook}

Jupyter Notebook は、実行可能コード、数式、グラフィック/視覚化 (matplotlib) 説明テキストが入った文書を作成して共有することができる Web アプリケーションです。

データに対する洞察を得て異常を検出するために事前構成 Jupyter Notebook をセットアップするには、以下のようにします。
1. サポート対象のブラウザーを使用して、IBM Cloud ID で [DSX ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://datascience.ibm.com/){: new_window} にログインします。
2. **「+ 新規プロジェクト (+ New project)」**をクリックして新規プロジェクトを作成し、**「Jupyter Notebooks」**タイルを選択します。プロジェクトによってスペースが作成されます。ノートブックの収集と共有、データ・ソースへの接続、パイプラインの作成、データ・セットの追加はその場所で行います。
3. **「+ プロジェクトに追加 (+ Add to project)」**ドロップダウンをクリックし、**「接続」**を選択します。サービスのリストで**「Cloudant」**を選択してから、{{site.data.keyword.cloudant_short_notm}} サービス・ページの「サービス資格情報」タブで確認した Cloudant URL、ユーザー名、パスワードを入力し、表示されるフィールドにこれらを入力します。接続の名前を指定し、**「作成」**をクリックします。
4. ダッシュボードの「データ・アセット (Data assets)」セクションに接続があることを確認します。
5. **「+ 新規ノートブック (+ New notebook)」**をクリックし、新規 Jupyter ノートブックを作成します。
6. 「サービス」の下の「ランタイムの選択 (Select runtime)」ドロップダウンから、**spark-iw** を選択します。これが存在しない場合は、Apache Spark サービスが正しくプロビジョンされていないことを意味します。IBM Cloud ダッシュボードでこれが存在するかどうかを確認し、存在しない場合は、サービス・ページにアクセスしてプロビジョンします。
7. 「言語」に**「Python 2」**、「Spark バージョン (Spark version)」に**「2.1」**を設定します。
8. **「URL から (From URL)」**を選択して既存のノートブックをロードしてから、ノートブックの記述名を指定し、以下の URL を入力してサンプル・ノートブックを開きます。
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. **「ノートブックの作成 (Create Notebook)」**をクリックします。 ノートブックが作成され、メタデータとコードが含まれていることを確認します。
10. 先頭が '!pip install --upgrade pixiedust, ,' であるセルを選択し、**「実行 (Run)」**をクリックしてコードを実行します。
11. インストールが完了したら、**「カーネルの再始動 (Restart Kernel)」**アイコンをクリックするか、「カーネル (Kernel)」メニューから**「再始動」**を選択して Spark カーネルを再始動します。
12. カーネルが再始動するのを 10 秒ほど待ってから、コメント付きの空のコード・セルをクリックして選択します。
13. 以下の手順を実行することにより、そのセルに Cloudant 資格情報をインポートします。

    1. ![データの検索と追加 (Find and add data)](images/find_add_data_icon.png) をクリックします。
    2. **「接続」**タブを選択します。
    3. **「コードに挿入 (Insert to code)」**をクリックします。
ご使用の Cloudant 資格情報で「credentials_1」という辞書が作成されます。 辞書の名前が「credentials_1」と指定されていない場合は、ノートブックのコードの実行に必要な名前である「credentials_1」に名前変更します。
14. データベース名 (dbname) が表示されているセルに、データのソースである Cloudant データベースの名前 (例: `iotp_yourWIoTPorgId_default_Year-month-day`) を入力します。
15. ノートブックを保存します。 ノートブックを実行する準備ができました。


### 2. 分析の実行
{: #run_analysis}

1.	Cloudant 資格情報が含まれているセルを選択します。
2.	**「再生 (Play)」**をクリックしてセル・コードを実行します。
3.	実行結果を確認して、各セルで使用されている python コードを分析します。
4.	それぞれのセルで手順 2 と 3 を繰り返します。 **「ユーザー入力必須 (User input required)」**が指定されたセルの場合、実行前に、次のコード・セルで定義されている変数に対して新しい入力値を指定する必要があります。

**注:** 一部のセルはバックグラウンド Spark ジョブを実行するため、完了まで時間がかかる場合があります。 セル内のコード実行が完了すると、アスタリスク `*` が数値に変わります (例えば、In `[*]` が In `[1]` に変わります)。 この手順が完了したら、{{site.data.keyword.iot_short_notm}} でクラウド・ルールを作成して、異常の検出時に自動的にアラートを生成できます。


<!-- ### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails. -->


## 次の作業

DSX について詳しくは、以下のリソースを参照してください。

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [DSX コミュニティー・ノートブックとチュートリアル ![ 外部リンク・アイコン](../../../icons/launch-glyph.svg " 外部リンク・アイコン")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} (Jupyter ノートブックについて詳しくはリンクをたどります)。
 - [Watson IoT Platform クックブックの分析レシピ ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
