---

copyright:
  years: 2016, 2018
lastupdated: "2017-03-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# リアルタイムの IoT データ分析
{: #analytics}  

**重要:** IBM は、IoT デバイス・データに対するルールを新しい方法で定義できるベータ版の提供を開始しました。このベータ版は、{{site.data.keyword.iot_full}} のルールとアクションの使用方法を向上させるためのプログラム変更に含まれる一部です。

詳しくは、ブログ記事 [An alternative approach to defining Rules on IoT data ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window} を参照してください。

独自のルールの定義を開始するには、[組み込みルールの作成 (ベータ)](information_management/im_rules.html) 資料を参照してください。

## リアルタイムの IoT データ分析について

Watson {{site.data.keyword.iot_short_notm}} 分析を使用して、デバイスで生成された未加工のデータから、必要なリアルタイム分析情報を取得できます。  
{: shortdesc}

[ボードとカード](data_visualization.html)を使用すると、1 つ以上のデバイスから取得されたデータ・セットの値を示すグラフィックスを表示し、デバイス・データの概要を簡単に把握できます。

分析ルールを使用して、アクションをトリガーする条件を指定します。 例えば、1 つのルールを作成し、その中で、デバイスが除去された場合やデバイスの温度が急上昇した場合に、アラートをユーザーのデバイス上のダッシュボードに送信し、E メールを管理者に送信するように指定することもできます。

クラウドの {{site.data.keyword.iot_short_notm}} に直接接続されたデバイスに対するルールをトリガーする場合には[クラウド・ルール](cloud_analytics.html)を使用し、エッジ分析対応のゲートウェイに接続されたデバイスに対するルールをトリガーする場合には[エッジ・ルール](edge_analytics.html)を使用します。
