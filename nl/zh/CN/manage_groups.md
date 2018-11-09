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


# 管理组 (Beta)
{: #groups_overview}

具有管理员角色的用户可以使用 {{site.data.keyword.iot_full}} 组来授予成员和 API 密钥对特定设备的访问权。创建组并向其添加设备后，可以向组中添加成员和 API 密钥，并在组中为其分配角色。角色和组的组合可确定用户和 API 密钥可以访问哪些设备，以及可以对设备执行的操作。
{: shortdesc}

可以使用 {{site.data.keyword.iot_short_notm}} 仪表板用户界面或使用 {{site.data.keyword.iot_short_notm}} 访问控制 API 来管理组。

有关如何管理角色的详细信息，请参阅[管理用户角色](managing_user_roles.html#managing-user-roles)。

**重要信息：**{{site.data.keyword.iot_short_notm}} UI 中的组功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

有关访问控制和组的更多信息，以及有关使用 {{site.data.keyword.iot_short_notm}} 访问控制 API 来管理组的指示信息，请参阅[资源级别的访问控制概述](reference/rlac_overview.html#RLAC_overview)。

## 添加组

**注：**要开始添加组，请在**设置**页面上打开**组**功能。 

1. 在 {{site.data.keyword.iot_short_notm}} 仪表板中，选择左侧导航栏中的**访问管理**。
2. 选择**组**选项卡并查看组列表。
3. 单击**添加组**。
4. 在**添加组**窗口中，输入组名和可选的描述。
5. 单击**下一步**。
6. 单击**向组添加设备**。
7. 选择要添加到组的设备，然后单击**完成**。
8. 单击**完成**以创建组。您可以通过添加更多设备或除去设备来编辑组，还可以添加更多组。

