---

copyright:
years: 2018
lastupdated: "2018-06-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# グループの管理 (ベータ)
{: #groups_overview}

管理者の役割を持つユーザーは、{{site.data.keyword.iot_full}} グループを使用して、メンバーおよび API キーに特定のデバイスに対するアクセス権限を付与することができます。グループを作成してそのグループにデバイスを追加したら、そのグループにメンバーと API キーを追加し、それらにグループ内の役割を割り当てます。 役割とグループの組み合わせによって、ユーザーと API キーがアクセスできるデバイス、およびそれらがデバイスに対して実行できるアクションが決まります。
{: shortdesc}

{{site.data.keyword.iot_short_notm}} ダッシュボード・ユーザー・インターフェースを使用するか {{site.data.keyword.iot_short_notm}} アクセス制御 API を使用して、グループを管理できます。

役割の管理方法について詳しくは、[ユーザー役割の管理](managing_user_roles.html#managing-user-roles)を参照してください。

**重要:** {{site.data.keyword.iot_short_notm}} UI のグループ機能は、限定されたベータ・プログラムの一部としてのみ提供されています。 今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。 この機能を試して、[ご意見をお寄せください ![外部リンク・アイコン](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

アクセス制御とグループの詳細や、{{site.data.keyword.iot_short_notm}} アクセス制御 API を使用してグループを管理する方法については、[リソース・レベル・アクセス制御の概説](reference/rlac_overview.html#RLAC_overview)を参照してください。

## グループの追加

**注:** グループを追加する前に、**「設定」**ページで**「グループ」**機能をオンにしてください。 

1. {{site.data.keyword.iot_short_notm}} ダッシュボードの左側のナビゲーション・バーで、**「アクセス管理 (Access Management)」**を選択します。
2. **「グループ」**タブを選択し、グループのリストを確認します。
3. **「グループの追加」**をクリックします。
4. **「グループの追加」**ウィンドウで、グループ名とオプションの説明を入力します。
5. **「次へ」**をクリックします。
6. **「グループにデバイスを追加 (Add Devices to Group)」**をクリックします。
7. グループに追加するデバイスを選択してから、**「完了」**をクリックします。
8. **「完了」**をクリックして、グループを作成します。
デバイスを追加したりデバイスを削除したりしてグループを編集できます。グループを追加することもできます。

