---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-18"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Resource-level access control overview
{: #RLAC_overview}

Resource-level access control enables you to control user and API key access to manage devices. You use resource groups to specify the devices in an organization that each user or API key can manage. For information on how to configure resource-level access control, see [Configuring resource-level access control](rlac.html#configure_RLAC).

## Resource-level access control concepts 
{: #RLAC_concepts}

Device groups are part of the resource-level access controls for {{site.data.keyword.iot_short_notm}}. You can use the device groups feature to manage the number of devices that personnel can access according to their role. Before you implement device groups, determine and record the intent and scope of the device groups to increase the efficiency of your solution. 

You can implement device groups as part of your IoT solution to enable a user or application to access a subset of devices rather than all devices that are associated with an organization. To increase the efficiency of device groups, use device groups when one person must have access to many devices.

For example, a company that employs a number of field engineers and operations personnel wants to implement an IoT solution across the United Kingdom. The company configures a device group for each of the 69 cities, 9 device groups for each of the geographical regions, and a device group for the whole of the United Kingdom. Devices are allocated to these groups, and the groups are assigned to 15 field engineers and operations personnel, some of whom have access to more than one group. Users that are responsible for just one or two devices can continue to use mobile applications and enterprise architectures that are external to {{site.data.keyword.iot_short_notm}} but compliment the overall IoT solution.

Questions about device groups can be raised in the [Questions in Internet of Things space ![External link icon](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window}.

## Resource group limits for access control

Resource group limits are enforced to ensure that the resource-level access control feature works effectively. The limits restrict the number of resource groups that are assigned to a subject, the number of devices within the groups, and the number of groups that a resource can belong to.

The following limits are enforced:

 - A maximum of 10 resource groups can be assigned to an API key, user, or gateway.
 - A resource group can have a maximum of 300 resources.
 - A resource can belong to a maximum of 10 groups.

## Resource-level access control terminology

You can use the following sections to help you to understand the terminology that is used in the resource-level access control feature. 

### Subjects
{: #subjects}

A subject is an authenticated entity that requests platform access, which must be authorized. The following types of subjects are valid:

- *Users*: End users who are the people using the applications. A member is a user whose account has no expiration date, and a guest is a user whose account has an expiration date.
- *Devices*: Physical devices that access the {{site.data.keyword.iot_short_notm}} platform and use the type Device.
- *Gateways*: Physical devices that access the {{site.data.keyword.iot_short_notm}} platform and use the type Gateway.
- *Applications*: Applications or services that access the {{site.data.keyword.iot_short_notm}} platform by using API keys for authorization and access control.

### Resources
{: #resources}

A resource is an entity that the subject performs the action on. The subject requests authorized platform access for the resource. 

### Actions
{: #actions}

The action is what the subject performs, which most often is Create, Read, Update, Delete, or Execute. Operations are used to group actions on resources.

### Applications
{: #applications}

An application can act on behalf of a subject that is unknown to the platform, such as a mobile application that controls a domestic appliance, and an application can act on behalf of a real or simulated device that is known or unknown to the platform. An application also can be a true server-side application that is not acting on behalf of any other kind of device.

Authentication is provided by and access to the {{site.data.keyword.iot_short_notm}} platform is given based on an API key or token.

### Operations
{: #operations}

An operation contains one ore more actions that are executed on a certain resource types by using user interfaces and programmatic interfaces, such as REST services and MQTT interfaces. They are used by different subjects including members, applications, and devices. Operations are identified by their Operation IDs (OID).

### Roles
{: #roles}

Roles are sets of operations that are used to provide permission to use the operations. A subject can have multiple roles, and the role is an attribute of a subject.

**Roles format**

Array of 0..n roles

    { "roles": [ "PD_ADMIN_APP" ] }

**Roles To Groups format**

Map of 0..n <string, list<string>> pairs

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### Permissions
{: #permissions}

Permissions allow a subject to run operations on a resource or resource group.

### Resource groups
{: #resource_groups}

A resource group is a collection of devices as resources. Devices can be added to resource groups and resource groups can be assigned to subjects in role-to-groups pairs. These relationships allow the subjects to perform operations for a given role on specific groups of devices.

### Users
{: #users}

Users log in to the {{site.data.keyword.iot_short_notm}} platform dashboard by using an ID that is a member of an organization. Users are assigned roles that grant them authorization to call certain sets of HTTP APIs. For more information about user roles, see [Levels of access for user roles](roles_access.html#levels-of-access-for-user-roles).

### API keys
{: #API_keys}

API keys are tokens that are used to call the {{site.data.keyword.iot_short_notm}} platform HTTP APIs. API keys are assigned roles that grant them authorization to call certain sets of HTTP APIs. For more information about API key roles, see [Levels of access for application roles](app_roles_access.html#levels-of-access-for-application-roles).

## APIs where resource-level access control is enforced
{: #RLAC_enforced_APIs}
When resource-level access control is enabled, the device-related APIs that are included in this section only work if the specified device is accessible to the caller.

### Device APIs (individual)

**List devices of type**

    GET /api/v0002/device/types/${typeId}

Only the subset of devices that belong to the appropriate groups is returned.

**Get device**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

Returns a 404 error if the device is not accessible by the caller.

**Get device management details**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

Returns a 404 error if the device is not accessible by the caller.

**Get devices registered by a gateway**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

Returns a 404 error if the gateway is not accessible by the caller. Only the subset of devices that belong to the appropriate groups is returned. The devices and the gateways must be in the group of the caller.

**Update device**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

Returns a 404 error if the device is not accessible by the caller.

**Delete device**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

Returns a 404 error if the device is not accessible by the caller.

### Device APIs (bulk)

**List devices**

    GET /api/v0002/bulk/devices

Only the subset of devices that belong to the appropriate groups is returned.

**Delete devices**

    DELETE /api/v0002/bulk/devices/remove

Only the subset of devices that belong to the appropriate groups is deleted. Devices that are not accessible still return with success = true.

**Update devices:**

    PUT /api/v0002/bulk/devices/update

### Device Management APIs

**Initiate request**

    POST /api/v0002/mgmt/requests

Fails if any device is not accessible to the caller.

**Delete request**

    DELETE /api/v0002/mgmt/requests/${requestId}

Fails if any device is not accessible to the caller.

**View request**

    GET /api/v0002/mgmt/requests/${requestId}

**List requests**

    GET /api/v0002/mgmt/requests

**Get one request**

    GET /api/v0002/mgmt/requests/${requestId}

**Get request device details**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**Get request device details for one device**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### Problem Determination APIs

**Connection logs for a device**

    GET /api/v0002/logs/connection

Returns an empty array if the device is not accessible.

**Add device error code**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Returns a 404 error if the device is not accessible by the caller.

**List device error codes**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Returns a 404 error if the device is not accessible by the caller.

**Clear device error codes**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

Returns a 404 error if the device is not accessible by the caller.

**Add device diagnostic log**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Returns a 404 error if the device is not accessible by the caller.

**List device diagnostic log**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Returns a 404 error if the device is not accessible by the caller.

**Get one device diagnostic log**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Returns a 404 error if the device is not accessible by the caller.

**Clear device diagnostic log**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

Returns a 404 error if the device is not accessible by the caller.

**Delete one device diagnostic log**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

Returns a 404 error if the device is not accessible by the caller.

###Last Event Cache APIs

**Get events for the device**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

Returns a 404 error if the device is not accessible by the caller.

**Get events for the device**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

Returns a 404 error if the device is not accessible by the caller.

**Get logical interface for the device**

    GET /api/v0002/device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}

Returns a 404 error if the device is not in the caller's group.
