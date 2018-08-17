---

copyright:
years: 2018
lastupdated: "2018-04-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 管理组 (Beta)
{: #groups_overview}

您可以使用 {{site.data.keyword.iot_full}} 组来授予成员和 API 密钥对特定设备的访问权。创建组并向其添加设备后，可向该组添加成员和 API 密钥，并为其分配在组内角色。角色和组的组合将确定用户和 API 密钥可以访问哪些设备，以及可以在设备上执行的操作。
{: shortdesc}

可以使用 {{site.data.keyword.iot_short_notm}} 仪表板用户界面或使用 {{site.data.keyword.iot_short_notm}} 访问控制 API 来管理组。

**重要信息：**{{site.data.keyword.iot_short_notm}} UI 中的组功能仅可作为受限 beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

有关访问控制和组的更多信息，以及有关使用 {{site.data.keyword.iot_short_notm}} 访问控制 API 来管理组的指示信息，请参阅[资源级别访问控制概述](reference/rlac_overview.html#RLAC_overview)。

## 添加组

**注：**要开始添加组，请启用**设置**页面上的**组**功能。 

1. 在 {{site.data.keyword.iot_short_notm}} 仪表板中的左侧导航栏中，选择**访问权管理**。
2. 选择**组**选项卡，并查看组列表。
3. 单击**添加组**。
4. 在**添加组**窗口，输入组名和（可选）描述。
5. 单击**下一步**。
6. 单击**向组添加设备**。
7. 选择要添加到组的设备，然后单击**完成**。
8. 单击**完成**以创建组。您可以通过添加更多设备或除去设备来编辑组，也可以添加更多组。

