---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-14"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ユーザーのアクセス権限の管理
{: #managing-user-access}

「メンバー」ダッシュボードから、{{site.data.keyword.iot_full}} 組織に対するアクセス権限を制御、管理できます。 ユーザーを追加する方法には、追加、招待<!--, registering-->、インポートがあります。 また、役割の割り当てによって、さまざまなレベルのアクセス権限をユーザーに付与できます。
{:shortdesc}

## ユーザーの追加
{: #adding-new-users}

ダッシュボードの**「メンバー」**タブで、「追加」機能を使用してメンバーを個別に追加できます。 また、「インポート」機能を使用して、同時に複数のメンバーを追加することも可能です。

デフォルトでは、メンバー・アカウントに有効期限はありません。 ユーザーを {{site.data.keyword.iot_short_notm}} 組織に追加するときに、オプションでユーザー・アカウントの有効期限日付を設定できます。

### {{site.data.keyword.iot_short_notm}} 組織へのメンバーの追加

組織にメンバーを追加するには、メンバーの IBMid が必要です。 メンバーを追加するときに、そのメンバーからの確認は不要です。

1 人のメンバーを {{site.data.keyword.iot_short_notm}} 組織に追加するには、次の手順を実行します。
1. {{site.data.keyword.iot_short_notm}} ダッシュボードで**「メンバー」**に移動します。
2. **「メンバーの追加」**をクリックし、**「追加」**タブを選択します。
3. メンバーの IBMid を入力します。
4. メンバーの役割を選択します。
5. オプション: メンバーの有効期限日付を設定します。
6. **「追加」**をクリックします。

複数のメンバーを同時に追加するには、各メンバーの IBMid、役割、(オプションの) 有効期限日付が含まれた `.csv` ファイルをアップロードする必要があります。 詳しくは、[CSV ファイルの作成](#constructing-your-csv)を参照してください。
1. {{site.data.keyword.iot_short_notm}} ダッシュボードで**「メンバー」**に移動します。
2. **「メンバーの追加」**をクリックし、**「インポート」**タブを選択します。
3. ファイルを参照するか、`.csv` ファイルを**「CSV のアップロード (Upload CSV)」**ウィンドウにドラッグします。
4. CSV ファイルで指定されている役割が認識されない場合に使用する、デフォルトの役割を選択します。
5. CSV ファイル内の列番号を、対応する IBMid、役割、有効期限日付 (オプション) のエントリーにマップします。
6. `.csv` ファイルで使用されている列区切り文字と一致する区切り文字 (コンマまたはセミコロン) を選択します。
7. **「インポート」**をクリックして IBMids をインポートし、メンバーを作成します。

<!--
### Inviting members to your {{site.data.keyword.iot_short_notm}} organization

When you invite a user to become a member of your {{site.data.keyword.iot_short_notm}} organization, the user receives an email that contains an invitation link. Invitation links expire 48 hours after they are sent. If an invitation link is not used within 48 hours, the user must be invited again to receive a new invitation link.

**Important:** The invite feature requires a configured mail service. For more information, see the Email section of the [External service integrations](reference/extensions/index.html#email) topic.

To invite a member to your {{site.data.keyword.iot_short_notm}} organization:
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select the **Invite** tab.
3. Enter the email address of the member.
4. Select a role for this member.
5. Optional: Set an expiry date for the member.
6. Click **Invite Member**.

To invite multiple members simultaneously, you must upload a `.csv` file that contains the email address, role and the optional expiry date of each member. For information, see [Constructing your CSV file](#constructing-your-csv).
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select the **Import** tab.
3. Browse your files or drag the `.csv` file into the **Upload CSV** window.
4. Select a default role to use if a role specified in the CSV file is not recognized.
5. Map the column numbers in your CSV file to the corresponding email address, role, and (optional) expiry date entries.
6. Select the appropriate comma or semicolon column separator to match the separator used in your `.csv` file.
7. Click **Import** to send out the invitations. -->

<!-- ### Registering a member with your {{site.data.keyword.iot_short_notm}} organization

If your organization is using {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.ssoshort}}, you can add individual members to your organization by registering them, which does not require an IBMid.

To register a member with your {{site.data.keyword.iot_short_notm}} organization:
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select **Invite**.
3. Enter the email address of the member.
4. Select a role for this member.
5. Enter the subject, realm name, and issuer.
   **Important:** Ensure that the `Subject`, `Realm Name`, and `Issuer` fields comply with the OpenID Connect recommendations and standards. For more information, see the [OpenID Connect ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://openid.net/connect/){: new_window} website.
6. Optional: Set an expiry date for the member.
7. Click **Register Member**.

To register multiple members simultaneously, you must upload a CSV (`.csv`) file that contains the email address, role, subject, realm name, issuer, and the optional expiry date of each member.
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Access**.
2. Click **Add Member** and select **Import**.
3. Click **Bulk Register**.
4. Select a default role and ensure that the column numbers on your CSV file match the column numbers in the CSV settings.
5. Ensure the column separator in your CSV file matches the column separator in the CSV settings.
6. Click **Browse your files** or drag the CSV file into the **Upload CSV** window. -->

### CSV ファイルの作成
{: #constructing-your-csv}

複数のメンバーを組織にインポートするために CSV ファイルを作成するときには、使用する方式に応じて必要な情報を含めてください。 列の区切りには、コンマまたはセミコロンを使用します。  
**重要:** CSV ファイルをアップロードするときに、**「CSV 設定 (CSV settings)」**で、列の区切り文字として正しいものを選択してください。

コンマ区切りのサンプル CSV ファイル:  
```
user1@sample.com,PD_DEVELOPER_USER,2018-03-13
user2@sample.com,PD_OPERATOR_USER,2018-03-13
user3@sample.com,PD_ADMIN_USER,2018-03-13
```
ここでは次の列エントリーが使用されています。  
- 1 列目: ユーザーの E メール・アドレス。  
メンバーを追加する場合は、有効な IBMid に対応する E メール・アドレスを使用していることを確認してください。 メンバーを招待する場合は、任意の有効な E メール・アドレスを使用できます。
- 2 列目: ユーザーに割り当てられる役割。  
以下のいずれかの役割を入力します。
 - PD_ADMIN_USER
 - PD_OPERATOR_USER
 - PD_DEVELOPER_USER
 - PD_ANALYST_USER
 - PD_READER_USER  
 ユーザー役割について詳しくは、[ユーザー、アプリケーション、ゲートウェイの役割](roles_index.html#user_roles)を参照してください。
- 3 列目: ユーザーの有効期限日付に対応する ISO 6801 日付 (例: `2018-03-13`)。

## ユーザーの編集
{: #editing-users}

ユーザーを編集して、役割の変更、アクセス権限の有効期限の追加、削除、変更、または組織へのアクセス権限の追加、削除を行うことができます。

1. {{site.data.keyword.iot_short_notm}} ダッシュボードから、左側のナビゲーション・バーの**「メンバー」**をクリックしてください。
2. 編集するユーザーの横にある**「編集」**アイコンをクリックします。
3. ユーザーに対して必要な変更を行います。
4. **「保存」**をクリックします。

## ユーザー・アクセスのブロック
{: #blocking-users}

組織のメンバーシップは保持したままで、ユーザーによる {{site.data.keyword.iot_short_notm}} 組織へのアクセスをブロックすることができます。

1. {{site.data.keyword.iot_short_notm}} ダッシュボードから、左側のナビゲーション・バーの**「メンバー」**をクリックしてください。
2. {{site.data.keyword.iot_short_notm}} 組織へのアクセスをブロックするユーザーの横にあるトグルをクリックします。

## ユーザー・アクセスの制限
{: #limiting-users}

リソース・レベル・アクセス制御を行うことによって、組織内のデバイスへのアクセスを制限することができます。 リソース・グループを使用して、各ユーザーまたは API キーが管理できるデバイスを指定できます。 リソース・レベル・アクセス制御を構成する方法については、[リソース・レベル・アクセス制御の構成](reference/rlac.html#configure_RLAC)を参照してください。

## ユーザーの削除
{: #removing-users}

ユーザーを {{site.data.keyword.iot_short_notm}} 組織から完全に削除することができます。 ユーザーを削除すると、元に戻すことはできません。アクセス権限を復元するためには、ユーザーを[プラットフォームに追加する](#adding-new-users)必要があります。

1. {{site.data.keyword.iot_short_notm}} ダッシュボードから、左側のナビゲーション・バーの**「メンバー」**をクリックしてください。
2. 削除するユーザーの横にある**「削除」**アイコンをクリックします。
3. 確認のダイアログで**「削除」**をクリックします。
