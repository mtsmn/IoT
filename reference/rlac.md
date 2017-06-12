---

copyright:
  years: 2017
lastupdated: "2017-06-12"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Resource-level access control (Beta)

**Important:** The {{site.data.keyword.iot_full}} resource-level access control feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

When resource-level access control is enabled, users and API keys can manage devices. You use resource groups to define which devices within an organization each user or API key can manage. Users and API keys can be assigned a "role to groups" pair and they can perform only operations that are covered by the specified role and that are performed against devices in the specified groups.

## Enabling resource-level access control

To use resource-level access control, you must enable an organization-level configuration flag by using the following API:

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Assigning a "role to groups" pair to a user

Assigning a "role to groups" pair to a user is mandatory for the user to be restricted to a group of devices.
Users without any resource groups can manage all devices within their organization.

To assign a "role to groups" pair to a user, use the following API:

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "rolesToGroups": {
            "PD_OPERATOR_USER": ["groupUid"]
        }
    }

## Assigning a "role to groups" pair to an API key:

Assigning a "role to groups" pair to an API Key is mandatory for the API key to be restricted to a group of devices.
API keys without any resource groups can manage all devices within their organization.

To assign a "role to groups" pair to an API key, use the following API:

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/roles

    {
        "rolesToGroups": {
            "PD_OPERATOR_APP": ["groupUid"]
        }
    }

## Adding devices to and removing devices from a resource group

Before user or API key with a "role to group" pair can manage a device, the device must be a member of the resource group that is assigned to the user or API key. To add multiple devices to a resource group at the same time, use the following API:

    PUT /bulk/devices/{groupId}/add

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

The group that you are adding devices to must be specified in the path of the request, and the devices that are to be added must be specified in the body of the request. For more information on the request schema and responses, see [{{site.data.keyword.iot_short_notm}} Access control API documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/securityb.html#!){: new_window}.

To remove multiple devices from a resource group, use the following API:

    PUT /bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

The devices that are specified in the body of the request are removed from the group that is specified in the path of the request. For more information on the request schema and response, see [{{site.data.keyword.iot_short_notm}} Access control API documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/securityb.html#!){: new_window}.

## Finding a resource group

Resource groups can have associated search tags. Search tags can be used to retrieve the details of a resource group by using the following API:

    GET /groups

This API returns the resource groups that are associated with the search tag used. If no search tag is specified, all resource groups are returned.

To find ID of resource groups that a device is a member of, use the following API:

    GET /authorization/devices/{deviceUid}

This API returns the unique identifier of the resource groups that this device is a member of. More information on this API can be found in the ...

## Querying a resource group

Resource groups can be queried within various parameters to return either the full properties of all devices in the group, the unique identifiers of all devices in the group, or the properties of the resource group.

To return the full properties of all devices in the specified resource group, use the following API:

    GET /bulk/devices/{groupUid}

This API returns the full properties list for all members of the specified resource group. For more information on the request schema, responses, and how to page through results, see [{{site.data.keyword.iot_short_notm}} Access control API documentation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/securityb.html#!){: new_window}.

To return only the unique identifiers of the members of the resource group, use the following API:

    GET /bulk/devices/{groupUid}/ids

This API returns the unique identifiers of all devices that are members of the resource group.

To return the properties of the resource group, use the following API:

    GET /groups/{groupUid}

This API returns the properties of the resource group (name, description, search tags, and unique identifier) that are specified in the path. The API does not return the list of members of the resource group.

## Creating and deleting a resource group.

Resource groups can be created and deleted independently of connecting gateways. To create a resource group, use the following API:

    POST /groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

This API creates a resource group and returns the group details.

To delete a resource group, use the following API:

    DELETE /groups/{groupUid}

This API deletes the specified resource group. Devices that were in the group are removed from the group, but the devices themselves are not otherwise affected.

## Updating group properties

To update the properties of a group, use the following API:

    PUT /groups/{groupId}

This API updates the properties of the specified group.
