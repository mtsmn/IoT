---

copyright:
  years: 2017
lastupdated: "2017-07-19"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# リソース・レベル・アクセス制御の構成 (ベータ)
{: #configure_RLAC}

**重要:** {{site.data.keyword.iot_full}} リソース・レベル・アクセス制御機能は、限定されたベータ・プログラムの一部としてのみ使用できます。今後の更新によって、この機能の現行バージョンと互換性のない変更が行われる可能性があります。この機能を試して、[ご意見をお寄せください![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

リソース・レベル・アクセス制御を使用すれば、デバイスを管理するためにユーザーと API キーのアクセスを制御できます。それぞれのユーザーや API キーで管理できる組織内のデバイスを定義するために、リソース・グループを使用します。ユーザーや API キーに「役割とグループ」のペアを割り当てることによって、指定のグループに含まれているデバイスに対して、指定の役割でカバーされている操作だけを実行できる、という動作を定義することも可能です。リソース・レベル・アクセス制御の詳細については、[リソース・レベル・アクセス制御の概説](rlac_overview.md)と [{{site.data.keyword.iot_short_notm}}アクセス制御 API の資料 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window} を参照してください。

## リソース・レベル・アクセス制御の構成 - プロセス・フロー
{: #RLAC_process}

リソース・レベル・アクセス制御を有効にして使用するための一般的なプロセス・フローは、以下のとおりです。
1. [組織を作成します](../iotplatform_overview.html#organizations)。
2. [ユーザーを作成し](../add_users.html#adding-new-users)、[API キー](../platform_authorization.html#api-key)を作成します。
3. [リソース・グループを作成します](rlac.html#create_delete_group)。
4. [「役割とグループ」のマッピングをユーザーと API キーに割り当てます](rlac.html#assign_roletoegroup)。
5. [リソース・グループのデバイスを追加します](rlac.html#add_device)。
6. [リソース・レベル・アクセス制御を有効にします](rlac.html#RLAC_enable)。

リソース・レベル・アクセス制御は、各種のデバイス関連 API に適用されます。影響を受ける API のリストについては、[リソース・レベル・アクセス制御が適用される API](rlac_overview.html#RLAC_enforced_APIs) を参照してください。

## リソース・グループの作成と削除
{: #create_delete_group}

リソース・グループは、接続先のゲートウェイとは独立して作成し、削除できます。

リソース・グループを作成して、グループの詳細を返すには、以下の API を使用します。

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

リソース・グループを削除すると、そのグループに入っていたデバイスがグループから削除されますが、デバイス自体にそれ以外の影響はありません。リソース・グループを削除するには、以下の API を使用します。

    DELETE /api/v0002/groups/{groupUid}


## ユーザーや API キーへの「役割とグループ」のペアの割り当て
{: #assign_roletogroup}

ユーザーや API キーを一群のデバイスだけに制限するには、ユーザーや API キーに「役割とグループ」のペアを割り当てる必要があります。リソース・グループが設定されていないユーザーや API キーは、組織内の全デバイスを無制限で管理できます。「役割とグループ」のペアを使用する場合、API キーに設定できる役割は 1 つだけになります。rolesToGroups で指定する役割と roles で指定する役割は一致していなければなりません。

ユーザーに「役割とグループ」のペアを割り当てるには、以下の API を使用します。

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



API キーに「役割とグループ」のペアを割り当てるには、以下の API を使用します。

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## リソース・グループのデバイスの追加と削除
{: #add_device}

「役割とグループ」のペアが設定されているユーザーや API キーがデバイスを管理するには、そのユーザーや API キーに割り当てられているリソース・グループのメンバーとして対象のデバイスを追加しなければなりません。リソース・グループにデバイスを追加する時には、デバイスの追加先のグループを要求のパスで指定し、追加するデバイスを要求の本体で指定する必要があります。リソース・グループに複数のデバイスを同時に追加するには、以下の API を使用します。

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

リソース・グループからデバイスを削除する時には、グループを要求のパスで指定し、デバイスを要求の本体で指定します。リソース・グループから複数のデバイスを除去するには、以下の API を使用します。

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

要求のスキーマと応答の詳細については、[{{site.data.keyword.iot_short_notm}}アクセス制御 API の資料 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window} を参照してください。

## リソース・レベル・アクセス制御の有効化
{: #RLAC_enable}

組織でリソース・レベル・アクセス制御を有効にすると、ユーザーや API キーを割り当て先のリソース・グループだけに制限できます。リソース・レベル・アクセス制御を使用するには、以下の API を使用して、組織レベルの構成フラグを有効にしなければなりません。

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## リソース・グループの検索
{: #find_group}

リソース・グループに検索タグを関連付けることができます。以下の API によって、検索タグを使用してリソース・グループの詳細を取得できます。

    GET /api/v0002/groups

この API によって、使用した検索タグに関連するリソース・グループが返されます。検索タグを指定しない場合は、すべてのリソース・グループが返されます。 

ユーザーに割り当てられているリソース・グループの固有の ID を検索するには、以下の API を使用します。

    GET /api/v0002/authorization/users/{userUid}

API キーに割り当てられているリソース・グループの固有の ID を検索するには、以下の API を使用します。

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## リソース・グループの照会
{: #query_group}

リソース・グループ内のすべてのデバイスの全プロパティー、グループ内のすべてのデバイスの固有 ID、リソース・グループのプロパティーを返すために、さまざまなパラメーターを使用してリソース・グループを照会できます。

指定のリソース・グループ内のすべてのデバイスの完全なプロパティーを返すには、以下の API を使用します。

    GET /api/v0002/bulk/devices/{groupUid}

リソース・グループのメンバーの固有 ID のみを返すには、以下の API を使用します。

    GET /api/v0002/bulk/devices/{groupUid}/ids

リソース・グループのプロパティー (パスで指定されている名前、説明、検索タグ、固有 ID など) を返すには、以下の API を使用します。

    GET /api/v0002/groups/{groupUid}

この API では、リソース・グループのメンバー・リストは返されません。

## グループ・プロパティーの更新
{: #update_group}

グループのプロパティーを更新するには、以下の API を使用します。

    PUT /api/v0002/groups/{groupId}
