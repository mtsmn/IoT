---

copyright:
years: 2018
lastupdated: "2018-04-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Managing groups (Beta)
{: #groups_overview}

You can use {{site.data.keyword.iot_full}} groups to grant members and API keys access to specific devices. After you create a group and add devices to it, you add members and API keys to the group and assign them roles within the group. The combination of roles and groups determines which devices the users and API keys can access, and the actions that they can perform on the devices.
{: shortdesc}

You can manage groups by using the {{site.data.keyword.iot_short_notm}} dashboard user interface or by using the {{site.data.keyword.iot_short_notm}} access control APIs.

**Important:** The groups feature in the {{site.data.keyword.iot_short_notm}} UI is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

For more information about access control and groups, and for instructions on using {{site.data.keyword.iot_short_notm}} access control APIs to manage groups, see [Resource-level access control overview](reference/rlac_overview.html#RLAC_overview).

## Adding groups

1. From your {{site.data.keyword.iot_short_notm}} dashboard, select **Access Management** in the left navigation bar.
2. Select the **Groups** tab and view the list of groups.
3. Click **Add Group**.
4. In the **Add Group** window, enter the group name and an optional description.
5. Click **Next**.
6. Click **Add Devices to Group**.
7. Select the devices that you want to add to the group and then click **Done**.
8. Click **Finish** to create the group.
You can edit the group by add more devices or removing devices, and you can add more groups.

