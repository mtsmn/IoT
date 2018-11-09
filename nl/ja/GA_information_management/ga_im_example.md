---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# REST API を使用したデータ管理の概説
{: #im_example}

以下の手順を使用して、{{site.data.keyword.iot_full}} データ管理コンポーネントのデバイス・ツイン機能とアセット・ツイン機能の使用を開始するのに必要なリソースを構成します。

API の詳細については、[{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} の資料を参照してください。


## ワークフローの概要
{: #workflow}

以下の手順を使用して、デバイス・ツイン機能またはアセット・ツイン機能を使用したデバイス・データまたはモノ・データのマッピングを開始するのに必要なリソースを構成します。

**ヒント:** 各手順の詳細については、サンプル・シナリオを参照するか、サンプル・シナリオ内の各手順への直接リンクを使用してください。 [ステップバイステップ・ガイド 1](ga_im_index_scenario.html#scenario) では、種類の異なる温度計デバイスに対応したデバイス・タイプ論理インターフェースを作成するための手順について説明しています。[ステップバイステップ・ガイド 2](../information_management/im_index_scenario_thing.html#scenario) では、部屋タイプの 1 つのモノにまとめられた 2 つの異なる環境デバイス・タイプからのデータをアプリケーションでコンシュームできるようにする論理インターフェースを作成する方法について説明し、シナリオを具体化しています。

作成する論理インターフェースをデバイス・タイプとモノ・タイプのどちらに関連付けるかに応じて、論理インターフェースの作成と取り込みのプロセスは多少異なります。

### 始めに
デバイス・タイプまたはモノ・タイプに関連付ける論理インターフェースを作成するには、イベントと状態プロパティーを送信する[ 1 つ以上のデバイスが {{site.data.keyword.iot_short_notm}} に登録](ga_im_index_scenario.html#step14)されていなければなりません。  


### 手順

1. 	着信状態プロパティーを定義します。  
最初に、論理インターフェースからアプリケーションに提供する着信状態プロパティーを定義します。  
作成する論理インターフェースに応じて、以下のどちらかを行います。
<dl>
<dt>デバイス・タイプ: 物理インターフェースを作成します。</dt>
<dd>
<ol>
<li>[イベント・スキーマ・ファイルを作成します](ga_im_index_scenario.html#step1)。 イベント・スキーマ・ファイルは、インバウンド・イベントの構造と形式を定義したローカルの .JSON ファイルです。
<li>[使用するイベント・タイプのためのイベント・スキーマ・リソースを作成します](ga_im_index_scenario.html#step2)。 イベント・スキーマ・リソースは、{{site.data.keyword.iot_short_notm}} で使用されるプログラムの構成要素です。
<li>[このイベント・スキーマを参照するイベント・タイプを作成します](ga_im_index_scenario.html#step3)。 {{site.data.keyword.iot_short_notm}} はそのイベント・タイプを使用して、1 つ以上のイベント・スキーマ・リソースを物理インターフェースにマップします。
<li>[物理インターフェースを作成します](ga_im_index_scenario.html#step7)。
<li>[物理インターフェースにイベント・タイプを追加します](ga_im_index_scenario.html#step8)。
<li>[デバイス・タイプに物理インターフェースを追加します](ga_im_index_scenario.html#step9)。
</ol>
</dd>
<dt>モノ・タイプ: モノ・タイプを定義します。</dt>
<dd>
<ol>
<li>[モノ・タイプのスキーマ・ファイルを作成します](../information_management/im_index_scenario_thing.html#crt_composition_file)。  
モノのタイプのスキーマ・ファイルは、既存の論理インターフェースを指してモノ・タイプの構成を定義するローカルの .JSON ファイルです。
<li>[モノのタイプのスキーマ・リソースを作成します](../information_management/im_index_scenario_thing.html#crt_composition_resource)。  
ローカルの .JSON ファイルを {{site.data.keyword.iot_short_notm}} にアップロードします。
<li>[モノ・タイプを作成します](../information_management/im_index_scenario_thing.html#crt_thing_type)。 モノ・タイプは、モノのクラスを表すという点でデバイス・タイプと同じ役割を果たします。
</ol>
</dd>
</dl>
4. 	論理インターフェースを作成します。
 1. 	[デバイス・タイプ](ga_im_index_scenario.html#step4)または[モノ・タイプ](../information_management/im_index_scenario_thing.html#crt_ai_schema_file)のための論理インターフェース・スキーマ・ファイルを作成します。  
論理インターフェース・スキーマ・ファイルは、アプリケーションに提供するデバイス状態を定義した、ローカルの .JSON ファイルです。
 2. 	[デバイス・タイプ](ga_im_index_scenario.html#step5)または[モノ・タイプ](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource)のための論理インターフェース・スキーマ・リソースを作成します。
 3.	[デバイス・タイプ](ga_im_index_scenario.html#step6)または[モノ・タイプ](../information_management/im_index_scenario_thing.html#crt_thing_ai)のための論理インターフェースを作成します。
 4.	[デバイス・タイプ](ga_im_index_scenario.html#step10)または[モノ・タイプ](../information_management/im_index_scenario_thing.html#add_thing_ai)にその論理インターフェースを追加します。
5. 	[デバイス・タイプ](ga_im_index_scenario.html#step11)または[モノ・タイプ](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings)のマッピングを定義します。   
マッピングを使用して、インバウンド・プロパティーを論理インターフェースのプロパティーにマップします。  
6. 	ドラフト・[デバイス・タイプ](ga_im_index_scenario.html#step15)または[モノ・タイプ](../information_management/im_index_scenario_thing.html#activate)に関連付けられた構成を検証してアクティブ化します。
7. 	[デバイス](ga_im_index_scenario.html#step13)または[モノ](../information_management/im_index_scenario_thing.html##verify_Thing_state)の状態を取得します。  
REST 呼び出しを使用するか、またはトピック・ストリングをサブスクライブすることにより、更新されたデバイス・データがサブスクリプションで示されること、または更新されたデバイス・データが返されることを確認します。  



