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


# 管理群組（測試版）
{: #groups_overview}

具有管理者角色的使用者可以使用 {{site.data.keyword.iot_full}} 群組，以授與成員及 API 金鑰對特定裝置的存取權。在您建立群組並新增裝置到其中之後，您會新增成員及 API 金鑰到群組，並指派群組內的角色給它們。角色和群組的組合，決定了使用者及 API 金鑰可以存取哪些裝置，以及它們能對裝置執行的動作。
{: shortdesc}

您可以使用 {{site.data.keyword.iot_short_notm}} 儀表板使用者介面，或使用 {{site.data.keyword.iot_short_notm}} 存取控制 API，來管理群組。

如需如何管理角色的詳細資料，請參閱[管理使用者角色](managing_user_roles.html#managing-user-roles)。

**重要事項：**{{site.data.keyword.iot_short_notm}} 使用者介面中的群組特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

如需存取控制及群組的相關資訊，以及使用 {{site.data.keyword.iot_short_notm}} 存取控制 API 管理群組的指示，請參閱[資源層次存取控制概觀](reference/rlac_overview.html#RLAC_overview)。

## 新增群組

**附註：**若要開始新增群組，請在**設定**頁面上開啟**群組**特性。 

1. 從 {{site.data.keyword.iot_short_notm}} 儀表板的左導覽列中，選取**存取管理**。
2. 選取**群組**標籤，然後檢視群組的清單。
3. 按一下**新增群組**。
4. 在**新增群組**視窗中，輸入群組名稱及選用性的說明。
5. 按**下一步**。
6. 按一下**將裝置新增至群組**。
7. 選取您要新增至群組的裝置，然後按一下**完成**。
8. 按一下**完成**，以建立群組。您可以新增其他裝置或移除裝置來編輯群組，也可以新增其他群組。

