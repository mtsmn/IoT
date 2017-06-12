# Resource Level Access Control (Beta)

Users and API Keys have the ability to manage devices. Resource groups define which devices within an
organization that each user or API Key can manage. Users and API Keys can be assigned a "role to groups"
pair. Users or API Keys can only perform operations covered by the specified role against devices
contained within the specified groups.

Important: This is a beta feature.

## Enabling Resource Level Access Control

In order for resource level access controls to take effect, an organization level configuration flag
must be enabled.

To enable resource level access control, use the following API:

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Assigning a "role to groups" pair to a user:

Assigning a "role to groups" pair to a user is mandatory for the user to be restricted to a group of devices.
Users without any resource groups can manage all devices within their organization.

To assign a "role to groups" pair to a user, use the following API:

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "rolesToGroups": {
            "PD_OPERATOR_USER": ["groupUid"]
        }
    }

## Assigning a "role to groups" pair to an API Key:

Assigning a "role to groups" pair to an API Key is mandatory for the API Key to be restricted to a group of devices.
API Keys without any resource groups can manage all devices within their organization.

To assign a "role to groups" pair to an API Key, use the following API:

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/roles

    {
        "rolesToGroups": {
            "PD_OPERATOR_APP": ["groupUid"]
        }
    }

## Adding devices to and removing devices from a resource group

Before an API Key or user with a role to group pair can manage a device, the device must be a member of the resource group assigned to the API Key or user. To add many devices to a resource group at the same time, use the following API:

    PUT /bulk/devices/{groupId}/add

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

The group to add devices to must be specified in the path of the request, and the devices to be added must be specified in the body of the request. For more information on the request schema and responses, see ...

To remove multiple devices from a resource group, use the following API:

    PUT /bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

The devices specified in the body of the request will be removed from the group specified in the path of the request. For more information on the request schema and response, see ...

## Finding a resource group

Resource groups can have associated search tags. Search tags can be used to retrieve the details of a resource group by using the following API:

    GET /groups

This API returns the resource groups associated with the search tag used. If no search tag is specified, all resource groups are returned.

The ID of resource groups which a device is a member of can be found by using the following API:

    GET /authorization/devices/{deviceUid}

This API returns the unique identifier of the resource group(s) this device is a member of. More information on this API can be found in the ...

## Querying a resource group

Resource groups can be queried within various parameters to return either the full properties of all devices in the group, the unique identifiers of all devices in the group, or the properties of the resource group.

To return the full properties of all devices in the specified resource group, use the following API:

    GET /bulk/devices/{groupUid}

This API returns the full properties list for all members of the specified resource group. For more information on the request schema, responses, and how to page through results, see the Watson IoT Platform Limited Gateway API documentation External link icon.

To return only the unique identifiers of the members of the resource group, use the following API:

    GET /bulk/devices/{groupUid}/ids

This API returns the unique identifiers of all devices that are members of the resource group.

To return the properties of the resource group, use the following API:

    GET /groups/{groupUid}

This API returns the properties of the resource group (name, description, search tags, and unique identifier) specified in the path, but does not return the list of members of the resource group.

## Creating and deleting a resource group.

Resource groups can be created and deleted independently of connecting gateways. To create a resource group, use the following API:

    POST /groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

This API creates a resource group, and returns the group details.

To delete a resource group, use the following API:

    DELETE /groups/{groupUid}

This API deletes the specified resource group. Devices which were a member of the group are removed from the group, but the devices themselves are not otherwise affected.

## Updating group properties

    PUT /groups/{groupId}

This API updates the properties of the specified group.
