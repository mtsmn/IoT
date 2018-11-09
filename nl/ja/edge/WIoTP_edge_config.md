---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} エッジの構成 (プレビュー)
{: #edge_configure}

**重要:** {{site.data.keyword.iot_full}} エッジ機能は、限定されたプレビュー・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

エッジ・ノードに接続されているデバイスからデータを受信できるようにするには、事前にゲートウェイを {{site.data.keyword.iot_short_notm}} に接続する必要があります。ゲートウェイを {{site.data.keyword.iot_short_notm}} に接続することには、ゲートウェイのデバイス・タイプを作成し、ゲートウェイを {{site.data.keyword.iot_short_notm}} に登録することが関係します。 その後、登録情報を使用して、ゲートウェイ・タイプを {{site.data.keyword.iot_short_notm}} に接続することができます。

## 構成フローの概要

1. {{site.data.keyword.iot_short_notm}} エッジ・プレビューのテスト環境内に、[組織を作成](#edge_org)します。
2. [{{site.data.keyword.iot_short_notm}} プレビューを有効に](#edge_enable)します。
3. [エッジを有効にしてゲートウェイ・タイプを作成](#edge_gw_type)し、[ゲートウェイ・デバイスを追加](#edge_gw)して、エッジ・ノード上で実行するエッジ・サービスを選択することにより、{{site.data.keyword.iot_short_notm}} でエッジ・ゲートウェイ・ノードを使用するように組織を構成します。必要に応じて、[カスタム・サービスを作成](WIoTP_edge_dev.html#create_service)できます。
4. [エッジ・サービスを構成](#config_service)します。
5. [デバイスにエッジをインストール](#edge_install_device)します。
6. [エッジ・サービスを管理およびモニター](#monitor_service)します。

## 組織の作成
{: #edge_org}

以下の手順を実行して、{{site.data.keyword.iot_short_notm}} エッジ・プレビューのテスト環境内に組織を作成します。
1. {{site.data.keyword.Bluemix_notm}} アカウントを登録し、{{site.data.keyword.iot_short_notm}} サービスのインスタンスを {{site.data.keyword.Bluemix_notm}} 組織内に作成しておきます。 {{site.data.keyword.iot_short_notm}} インスタンスは、[IBM Cloud サービス・カタログの {{site.data.keyword.iot_short_notm}} ページ ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window} から直接作成できます。  
2. 情報を入力して、IBM Cloud 組織をプロビジョンします。**重要:** {{site.data.keyword.iot_short_notm}} エッジ・プレビューを使用するには、IBM Cloud 組織を**「米国南部」**ロケーションにデプロイする必要があります。
3. **「作成」**をクリックします。
4. 開いた {{site.data.keyword.iot_short_notm}} インスタンスで、**「起動」**をクリックします。
新しい {{site.data.keyword.iot_short_notm}} 組織が作成されます。組織 ID は、URL の先頭およびダッシュボード内のユーザー名の下に表示されます。これで、組織に対して {{site.data.keyword.iot_short_notm}} エッジを有効にすることができます。

## {{site.data.keyword.iot_short_notm}} エッジ・プレビューの有効化
{: #edge_enable}

{{site.data.keyword.iot_short_notm}} エッジ・プレビューは、デフォルトでオフになっています。{{site.data.keyword.iot_short_notm}} エッジを有効にするには、以下のようにします。

1. 組織の {{site.data.keyword.iot_short_notm}} ダッシュボードから、**「設定」**を選択します。
2. **「試験機能」**セクションで、{{site.data.keyword.iot_short_notm}} エッジ・プレビューを含む試験機能をアクティブ化します。
3. ブラウザーをリフレッシュして、新しい**エッジ・サービス**機能とショートカットをダッシュボードのナビゲーション・メニューに表示します。**注:** **「エッジ・サービス (Edge Services)」**アイコンがダッシュボードのナビゲーション・メニューに表示されない場合は、場所として**「米国南部」**を選択していることを確認してください。

ローカルのブラウザー・ストレージにフラグが追加されます。

## エッジ機能を有効にしたゲートウェイ・タイプの作成
{: #edge_gw_type}

{{site.data.keyword.iot_short_notm}} に接続する各ゲートウェイには、ゲートウェイ・タイプを関連付ける必要があります。 ゲートウェイ・タイプとは、共通の特性を共有するデバイス・グループのことです。

1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメイン・ナビゲーション・メニューから、**「デバイス」**を選択します。
2. **「デバイス・タイプ」**タブで、**「デバイス・タイプの追加」**をクリックします。
3. **「タイプの追加」**ページの**「ID」**タブで、**「gateway」**タイプを選択し、`my_gateway_type` などのゲートウェイ・タイプ名およびそのゲートウェイ・タイプの説明を入力します。
**重要:** ゲートウェイ・タイプ名は 36 文字以下でなければなりません。以下の文字だけを含めることができます。
<ul>
 <li>英数字 (a-z、A-Z、0-9)</li>
 <li>ハイフン (-)</li>
 <li>下線 (&lowbar;)</li>
 <li>ピリオド (.)</li>
 </ul>
3. オプション: ゲートウェイ・タイプの属性やメタデータを入力します。
4. **「エッジ機能」**ボタンをオンにします。
5. ゲートウェイ・タイプのアーキテクチャー・タイプを選択します。アーキテクチャー・タイプはデバイスのアーキテクチャーと一致する必要があります。例えば、Odroid デバイスは ARM64 アーキテクチャー上で実行されます。
6. **「次へ」**をクリックします。
7. オプション: **「デバイス情報」**タブで、追加の詳細およびゲートウェイ・デバイス・タイプに関連するメタデータを入力します。
8. エッジ・デバイスに追加するエッジ・サービスを選択します。コア IoT サービスはすべてのデバイスに追加されますが、以下の手順を実行して他のサービスをカタログから追加できます。
 1. **「サービスの追加」**をクリックします。
 2. **「エッジ・サービスの追加 (Add Edge Services)」**ウィンドウで、追加するサービスの適切なバージョンを選択してから、**「完了」**をクリックします。
9. **「ゲートウェイ・タイプの追加 (Add Gateway Type)」**ウィンドウで、**「完了」**をクリックします。
続いて、[このゲートウェイ・タイプにゲートウェイ・デバイスを追加](#edge_gw)できます。

## ゲートウェイ・デバイスの追加
{: #edge_gw}

1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメイン・ナビゲーション・メニューから、**「デバイス」**を選択します。
2. **「デバイスの追加」**をクリックします。
3. **「ID」**タブで、デバイスを追加するゲートウェイ・デバイス・タイプを選択し、追加するゲートウェイ・デバイスの固有 ID を入力します。
デバイス ID は、{{site.data.keyword.iot_short_notm}} ダッシュボードでゲートウェイ・デバイスを識別するために使用され、ゲートウェイ・デバイスを {{site.data.keyword.iot_short_notm}} に接続するための必須パラメーターでもあります。
**重要:** デバイス ID は 36 文字以下でなければなりません。以下の文字だけを含めることができます。
 <ul>
 <li>英数字 (a-z、A-Z、0-9)</li>
 <li>ハイフン (-)</li>
 <li>下線 (&lowbar;)</li>
 <li>ピリオド (.)</li>
 </ul>
 **ヒント:** ネットワークに接続されたデバイスの場合は、デバイス MAC アドレス (区切り文字のコロンは付けない) などをデバイス ID として入力できます。
4. **「次へ」**をクリックします。
5. オプション: **「デバイス情報」**タブで、追加の詳細およびゲートウェイ・デバイスに関連するメタデータを入力します。
6. **「次へ」**をクリックします。
7. **「デバイスの追加」**ページの**「セキュリティー」**タブで、認証トークンを自動生成するか、またはこのデバイス用の独自の認証トークンを指定します。 自分でトークンを作成する場合は、長さを 8 文字から 36 文字にして、小文字と大文字、数字、記号 (ハイフン、下線、ピリオドのいずれか) を組み合わせる必要があります。 反復した文字シーケンスや、辞書に出てくる単語やユーザー名などの事前定義シーケンスをトークンに含めないでください。
9. **「次へ」**をクリックします。
10. **「完了」**をクリックします。
11. デバイス情報のページで、以下のデバイス情報をコピーして保存します。
 - 組織 ID (`tubo8x` など)
 - デバイス・タイプ (`my_gateway_type` など)
 - デバイス ID。 **ヒント:** ネットワーク接続のデバイスの場合は、区切り文字のコロンを使用しない MAC アドレスなどになることもあります。
 - 認証方式 (`token` など)
 - 認証トークン (`PtBVriRqIg4uh)_-Kl` など)
この情報を使用してゲートウェイを {{site.data.keyword.iot_short_notm}} に接続し、ゲートウェイに接続されているデバイスからのデータの受信を開始します。

## エッジ・サービスの構成
{: #config_service}

エッジ機能を有効にした新規デバイス・タイプを作成した後、エッジ・ノードにインストールするさまざまなサービスを選択できます。

1. {{site.data.keyword.iot_short_notm}} ダッシュボードのメイン・ナビゲーション・メニューで、**「デバイス」**を選択します。
2. **「デバイス・タイプ」**をクリックします。
3. 構成するエッジ・デバイス・タイプを選択します。
4. **「エッジ・サービス (Edge Services)」**タブで、鉛筆アイコンを選択してレコードを編集します。
6. **「エッジ・サービスの追加 (Add Edge Services)」**をクリックして、この組織用にアップロードされたサービスのリストを表示します。
7. **「エッジ・サービスの追加 (Add Edge Services)」**ウィンドウで、追加するサービスの適切なバージョンを選択してから、**「完了」**をクリックします。
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## エッジ・サービスの管理およびモニター
{: #monitor_service}

エッジ・ノードをインストールした後、{{site.data.keyword.iot_short_notm}} を使用して、エッジ・ノード上で実行中のサービスの状況をモニターできます。

1.	{{site.data.keyword.iot_short_notm}} ダッシュボードのメイン・ナビゲーション・メニューで、**「デバイス」**を選択します。
2.	エッジ・ノードに対応するデバイスを選択します。
3.	**「エッジ・サービス (Edge Services)」**をクリックして、エッジ・ノード上にインストールされているサービスの状況を確認します。

## 詳細情報の入手
{: #more_info}

{{site.data.keyword.iot_short_notm}} へのゲートウェイの接続について詳しくは、[ゲートウェイの MQTT 接続](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt)を参照してください。

{{site.data.keyword.iot_short_notm}} について詳しくは、[Watson IoT Platform の概説](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp)を参照してください。

{{site.data.keyword.iot_short_notm}} エッジについて詳しくは、以下のトピックを参照してください。
- [{{site.data.keyword.iot_short_notm}} エッジの概要](WIoTP_edge.html#edge_overview)
- エッジ・デバイスへの [{{site.data.keyword.iot_short_notm}} エッジのインストール](WIoTP_edge_install.html#edge_install_device)
- [{{site.data.keyword.iot_short_notm}} エッジのための開発](WIoTP_edge_dev.html#edge_dev)
