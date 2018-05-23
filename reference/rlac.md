---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configuring resource-level access control
{: #configure_RLAC}

Resource-level access control enables you to control user and API key access to manage devices. You use resource groups to define which devices in an organization each user or API key can manage. Users and API keys can be assigned a "role to groups" pair, which defines that they can perform only operations that are covered by the specified role on devices that are in the specified groups. For more information about resource-level access control, see [Resource-level access control overview](rlac_overview.html) and [{{site.data.keyword.iot_short_notm}} Access control API documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Configuring resource-level access control - process flow
{: #RLAC_process}

The following process flow is typically used for enabling and using resource-level access control:
1. [Create an organization](../iotplatform_overview.html#organizations).
2. [Create users](../add_users.html#adding-new-users) and [API keys](../platform_authorization.html#api-key).
3. [Create resource groups](rlac.html#create_delete_group).
4. [Assign role-to-groups mappings for the users and API keys](rlac.html#assign_roletogroup).
5. [Add devices to the resource groups](rlac.html#add_device).
6. [Enable resource-level access control](rlac.html#RLAC_enable).

Resource-level access control is enforced in various device-related APIs. For a list of affected APIs, see [APIs where resource-level access control is enforced](rlac_overview.html#RLAC_enforced_APIs).

## Creating and deleting resource groups
{: #create_delete_group}

Resource groups can be created and deleted independently of connecting gateways.

To create a resource group and return the group details, use the following API:

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

When you delete a resource group, devices that were in the group are removed from the group, but the devices themselves are not otherwise affected. To delete a resource group, use the following API:

    DELETE /api/v0002/groups/{groupUid}


## Assigning "role to groups" pairs to users or API keys
{: #assign_roletogroup}

Assigning a "role to groups" pair to a user or to an API key is required to restrict the user or API key to a group of devices. Users and API keys without any resource groups can manage all devices in their organization without restriction. An API key can have only one role if a "roles to groups" pair is used. The role that is specified in "rolesToGroups" must match the role that is specified in "roles".

To assign a "role to groups" pair to a user, use the following API:

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



To assign a "role to groups" pair to an API key, use the following API:

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## Adding devices to and removing devices from resource groups
{: #add_device}

Before a user or API key with a "role to groups" pair can manage a device, the device must be a member of the resource group that is assigned to the user or API key. When you add devices to a resource group, the group that you are adding devices to must be specified in the path of the request, and the devices that are added must be specified in the body of the request. To add multiple devices to a resource group at the same time, use the following API:

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

When you remove devices from a resource group, the devices that are specified in the body of the request are removed from the group that is specified in the path of the request. To remove multiple devices from a resource group, use the following API:

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

For more information on the request schema and response, see [{{site.data.keyword.iot_short_notm}} Access control API documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Enabling resource-level access control
{: #RLAC_enable}

Enabling resource level access control for an organization will enable users and API keys to be restricted to their assigned resource groups. To use resource-level access control, you must enable an organization-level configuration flag by using the following API:

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Finding resource groups
{: #find_group}

Resource groups can have associated search tags. Search tags can be used to retrieve the details of a resource group by using the following API:

    GET /api/v0002/groups

This API returns the resource groups that are associated with the search tag that is used. If no search tag is specified, all resource groups are returned.

To find the unique ID of resource groups that are assigned to a user, use the following API:

    GET /api/v0002/authorization/users/{userUid}

To find the unique ID of resource groups that are assigned to an API key, use the following API:

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## Querying resource groups
{: #query_group}

Resource groups can be queried by using various parameters to return either the full properties of all devices in the group, the unique identifiers of all devices in the group, or the properties of the resource group.

To return the full properties of all devices in the specified resource group, use the following API:

    GET /api/v0002/bulk/devices/{groupUid}

To return only the unique identifiers of the members of the resource group, use the following API:

    GET /api/v0002/bulk/devices/{groupUid}/ids

To return the properties of the resource group, including the name, description, search tags, and unique identifier that are specified in the path, use the following API:

    GET /api/v0002/groups/{groupUid}

This API does not return the list of members of the resource group.

## Updating group properties
{: #update_group}

To update the properties of a group, use the following API:

    PUT /api/v0002/groups/{groupId}
