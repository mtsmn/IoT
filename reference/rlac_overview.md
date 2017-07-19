---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Resource-level access control overview (Beta)
{: #RLAC_overview}

Resource-level access control enables you to control user and API key access to manage devices. You use resource groups to specify the devices in an organization that each user or API key can manage. For information on how to configure resource-level access control, see [Configuring resource-level access control](rlac.html#configure_RLAC).

**Important:** The {{site.data.keyword.iot_full}} resource-level access control feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg "Externl link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Resource-level access control concepts and terminology
{: #RLAC_concepts}

### Subjects
{: #subjects}

A subject is an authenticated entity that requests platform access, which must be authorized. The following types of subjects are valid:

- *Users*: End users who are the people using the applications. A member is a user whose account has no expiration date, and a guest is a user whose account has an expiration date.
- *Devices*: Physical devices that access the [{{site.data.keyword.iot_short_notm}} platform and use the type Device.
- *Gateways*: Physical devices that access the [{{site.data.keyword.iot_short_notm}} platform and use the type Gateway.
- *Applications*: Applications or services that access the [{{site.data.keyword.iot_short_notm}} platform by using API keys for authorization and access control.

### Resources
{: #resources}

A resource is an entity that the subject performs the action on. The subject requests authorized platform access for the resource. Devices are the only resources that are support in the resource-level access control Beta release.

### Actions
{: #actions}

The action is what the subject performs, which most often is Create, Read, Update, Delete, or Execute. Operations are used to group actions on resources.

### Applications
{: #applications}

An application can act on behalf of a subject that is unknown to the platform, such as a mobile application that controls a domestic appliance, and an application can act on behalf of a real or simulated device that is known or unknown to the platform. An application also can be a true server-side application that is not acting on behalf of any other kind of device.

Authentication is provided by and access to the [{{site.data.keyword.iot_short_notm}} platform is given based on an API key or token.

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

    GET /api/v0002/devices/type/${typeId}

Only the subset of devices that belong to the appropriate groups is returned.

**Get device**

    GET /api/v0002/devices/type/${typeId}/device/${deviceId}

Returns a 404 error if the device is not accessible by the caller.

**Get device management details**

    GET /api/v0002/devices/type/${typeId}/device/${deviceId}/mgmt

Returns a 404 error if the device is not accessible by the caller.

**Get devices registered by a gateway**

    GET /api/v0002/devices/type/${typeId}/device/${gatewayId}/devices

Returns a 404 error if the gateway is not accessible by the caller. Only the subset of devices that belong to the appropriate groups is returned. The devices and the gateways must be in the group of the caller.

**Update device**

    PUT /api/v0002/devices/type/${typeId}/device/${deviceId}

Returns a 404 error if the device is not accessible by the caller.

**Delete device**

    DELETE /api/v0002/devices/type/${typeId}/device/${deviceId}

Returns a 404 error if the device is not accessible by the caller.

### Device APIs (bulk)

**List devices**

    GET /api/v0002/bulk/devices

Only the subset of devices that belong to the appropriate groups is returned.

**Delete devices**

    DELETE /api/v0002/bulk/devices/remove

Only the subset of devices that belong to the appropriate groups is deleted. Devices that are not accessible still return with success = true.

**Update devices:**

    PUT /api/v0002/bulk/devices/upsert

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

    POST /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/errorCodes

Returns a 404 error if the device is not accessible by the caller.

**List device error codes**

    GET /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/errorCodes

Returns a 404 error if the device is not accessible by the caller.

**Clear device error codes**

    DELETE /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/errorCodes

Returns a 404 error if the device is not accessible by the caller.

**Add device diagnostic log**

    POST /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/logs

Returns a 404 error if the device is not accessible by the caller.

**List device diagnostic log**

    GET /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/logs

Returns a 404 error if the device is not accessible by the caller.

**Get one device diagnostic log**

    GET /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/logs/${logId}

Returns a 404 error if the device is not accessible by the caller.

**Clear device diagnostic log**

    DELETE /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/logs

Returns a 404 error if the device is not accessible by the caller.

**Delete one device diagnostic log**

    DELETE /api/v0002/devices/type/${typeId}/device/${deviceId}/diag/logs/${logId}

Returns a 404 error if the device is not accessible by the caller.

###Last Event Cache APIs

**Get events for the device**

    GET /api/v0002/devices/type/${typeId}/device/${deviceId}/events

Returns a 404 error if the device is not accessible by the caller.

**Get events for the device**

    GET /api/v0002/devices/type/${typeId}/device/${deviceId}/events/${eventId}

Returns a 404 error if the device is not accessible by the caller.
