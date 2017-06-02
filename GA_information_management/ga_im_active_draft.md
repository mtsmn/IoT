---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Draft and active versions of resources
{: #draft_active_resources}

There can be two versions of a resource, a draft version and an active version. When you create a resource, that resource is created as a draft version. The draft version is a working copy of your resource that you can query, update and delete directly by using APIs. Create an active version of a draft resource by activating either a draft device type or a draft logical interface which references that resource.
{: shortdesc}

To differentiate between draft and active resources when using REST APIs, the prefix *draft/* is used to identify resources that are in a draft state.

The following example retrieves metadata for a draft schema definition with a specified id:

```
GET /api/v0002/draft/schemas/{schemaId}
```
The following example retrieves metadata for an active schema definition with a specified id:
```
GET /api/v0002/schemas/{schemaId}
```
*Note:* The identifier remains the same for the draft and active version of a given resource.

You can activate only a draft device type or a draft logical interface resource. To activate other resources, for example schemas, you must activate a drafter device type or draft logical interface which references that resource.

To activate a draft device type or a draft logical interface resource, complete the following steps as part of a PATCH request:
1. Perform a **validation-configuration** operation against a draft version of the device type or logical interface resource to ensure that the associated metadata is valid. If the metadata is invalid, a list of issues is returned in the body of the response.
2. Perform an **activate-configuration** operation against the validated draft version of the device type or logical interface resource to activate that resource and its associated metadata.

The following example shows a PATCH request where a **validation-configuration** operation is performed against a draft version of a device type that is called "TemperatureSensor":  
```
PATCH /api/v0002/draft/device/types/TemperatureSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "validate-configuration"
  }
```

The following example shows an unsuccessful response to the PATCH request:
```
{
"message": "CUDIM0303I: State update configuration for Device Type 'TemperatureSensor' is not valid.",
"details": {
  "id": "CUDIM0303I",
  "properties": [
    "Device Type",
    "TemperatureSensor"
  ]
},
"failures": [
  {
    "message": "CUDVS0301E: The device type 'TemperatureSensor' does not have any mappings defined for it",
    "details": {
      "id": "CUDVS0301E",
      "properties": [
        "TemperatureSensor"
      ]
    }
  }
]
}

```


The following example shows a PATCH request where a **activate-configuration** operation is performed against the validated draft version of a device type that is called "TemperatureSensor" and the associated metadata.  An active version of a resource is read-only.

```
PATCH /api/v0002/draft/device/types/TemperatureSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "activate-configuration"
  }
```

The following example shows a successful response to the PATCH request:
```
{
  "message": "CUDIM0300I: State update configuration for Device Type 'TemperatureSensor' has been successfully submitted for activation.",
  "details": {
    "id": "CUDIM0300I",
    "properties": [
      "Device Type",
      "TemperatureSensor"
    ]
  },
  "failures": []
}
```

Use the **list-differences** operation to return a list of the differences between the active and draft configuration for a logical interface or device type resource. The **list-differences** operation must be performed against the draft version of a logical interface and device type. The following example shows a PATCH request against a draft version of a device type:
```
PATCH /api/v0002/draft/device/types/TemperatureSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "list-differences"
  }
```

Use the **deactivate-configuration** operation to remove the active configuration that is associated with a resource. The deactivate-configuration operation can only be performed against the active version of a logical interface and device type.   The following example shows a PATCH request against an active version of a device type:
```
PATCH /api/v0002/device/types/TemperatureSensor
```
where the payload of the PATCH body contains the following content:
```
  {
    "operation": "deactivate-configurations"
  }
```
The following example shows a successful response to the PATCH request:
```
{
  "message": "CUDIM0305I: State update configuration for Device Type 'TemperatureSensor' has been successfully submitted for deactivation.",
  "details": {
    "id": "CUDIM0305I",
    "properties": [
      "Device Type",
      "TemperatureSensor"
    ]
  },
  "failures": []
}

```
*Notes:*
- If you delete a device type resource, the state of any existing instances of that device type are deleted.
- An active resource is read only. You can filter and sort draft and active resources by using query parameters.
- You can activate only logical interfaces and device types directly by using APIs. Other resources, for example schemas, physical interfaces, and event types are activated if they are referenced by a logical interface or device type that is made active.  If you activate a draft schema resource, the logical interface and device types that reference that schema are automatically updated.
- You must perform the **activate-configuration** operation against a draft version of a logical interface before any state is generated for device types that are associated with that logical interface.

T
