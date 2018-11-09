---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iot_short_notm}} の概説
{: #gettingstartedtemplate}

{{site.data.keyword.iot_full}} for {{site.data.keyword.Bluemix_notm}} には、ゲートウェイ・デバイス、デバイス管理機能、強力なアプリケーション・アクセス機能など、便利なツールキットが用意されています。 {{site.data.keyword.iot_short_notm}} を使用すれば、接続先デバイスのデータを収集し、組織のリアルタイム・データに基づく分析を実行できます。
{:shortdesc}

## 始めに
{: #byb}

デバイスを接続してデータを利用する前に、{{site.data.keyword.Bluemix_notm}} アカウントを登録し、{{site.data.keyword.iot_short_notm}} サービスのインスタンスを {{site.data.keyword.Bluemix_notm}} 組織内に作成しておきます。 {{site.data.keyword.iot_short_notm}} インスタンスは、[IBM Cloud サービス・カタログの {{site.data.keyword.iot_short_notm}} ページ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window} から直接作成できます。  

{{site.data.keyword.Bluemix_notm}} のアカウントを登録し、領域やその他のアカウント管理設定を構成する方法について詳しくは、[IBM Cloud への登録](https://console.bluemix.net/docs/account/adminpublic.html#signing-up-for-ibm-cloud)を参照してください。


{{site.data.keyword.iot_short_notm}} インスタンスをダッシュボードからセットアップして構成することができます。 ダッシュボードを開くには、{{site.data.keyword.Bluemix_notm}} の {{site.data.keyword.iot_short_notm}} サービス・インスタンスに移動し、**「起動」**をクリックします。

## このタスクについて

以下の手順では、{{site.data.keyword.iot_short_notm}} サービスを使用した作業をすぐに始める方法を説明します。

さらに詳しい一連の概説ガイドとサンプル・アプリケーションもあります。{{site.data.keyword.iot_short_notm}} を使用して、実稼働環境ですぐに利用できるエンドツーエンド IoT プロトタイプ・システムを開発する基本的な手順を段階的に取り上げた資料です。 {{site.data.keyword.iot_short_notm}} の操作を始めたばかりの開発者であれば、[概説ガイド](https://console.bluemix.net/docs/services/IoT/getting_started/getting-started-iot-overview.html#getting-started)のセクションにあるステップバイステップ・プロセスを使用してください。

## 手順 1: デバイスを接続する
{: #up_and_running}

そのサービスを稼働させるために、各自の状況に応じて以下のオプションを調べてください。

|  |   サービスがデプロイされている | サービスがデプロイされていない
 | -------------| ------------- | -------------
  |**接続するデバイスがある** | [デバイスを {{site.data.keyword.iot_short_notm}}](iotplatform_task.html#iotplatform_task) に接続します。| [Play with {{site.data.keyword.iot_short_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play){:new_window} で、デバイスの接続を試すことができます。
  |**接続するデバイスがない** | [Node-RED デバイス・シミュレーターを作成して接続します](nodereddevice_sample.html){:new_window}。 または、[スマートフォンを接続します ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play/device/smartphone){:new_window}。 | [Watson IoT Platform の概説](https://console.bluemix.net/docs/starters/IoT-starter/iot500.html)から始めます。
  
特定のデバイス・タイプを {{site.data.keyword.iot_short_notm}} に接続する方法について詳しくは、[developerWorks recipes ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){:new_window} を参照してください。  

デバイス接続の開発者資料については、以下のリンク先をご覧ください。
- [デバイスの MQTT 接続](devices/mqtt.html)。
- [ゲートウェイの MQTT 接続](gateways/mqtt.html)。

<!--
## Step 2: Analyze your device data
{: #analyzing_data}
Start exploring the real-time data that the devices are sending to {{site.data.keyword.iot_short_notm}}.
{{site.data.keyword.iot_short_notm}} includes the following analytics tools:  
- [Boards and cards](data_visualization.html) to visualize your real-time device data.
- [Rules and actions](analytics.html) that are triggered by real-time device data.
For a quick getting started example, see the [Using Rules and Actions with IBM Watson IoT Platform Cloud Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/using-rules-and-actions-with-ibm-watson-iot-platform-cloud-analytics/){:new_window} developerWorks recipe.
-->

## 手順 2: デバイス・データをコンシュームするアプリケーションを作成します
{: #develop_applications}

デバイス・データをコンシュームする独自のアプリケーションを作成して接続します。

詳しくは、以下のトピックを参照してください。   
- [アプリケーション開発者資料](applications/api.html)と [{{site.data.keyword.iot_short_notm}} API 資料](reference/api.html)を調べてください。
- [{{site.data.keyword.iot_short_notm}} クライアント・ライブラリー](iot_platform_client_lib.html)を調べてください。デバイスやアプリケーションを統合して接続するコードを作成したり開発したりするためのツールやファイルが用意されています。
- {{site.data.keyword.iot_short_notm}} に [{{site.data.keyword.cloudantfull}} サービスを接続して](cloudant_connector.html)、デバイスの履歴データを格納してください。
- 新しい [組み込みルール (ベータ)](information_management/im_rules.html) 機能を使用して、独自のルールを作成します。
