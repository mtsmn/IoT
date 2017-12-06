---

copyright:
  years: 2017
lastupdated: "2017-10-13"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 资源级别访问控制概述 (Beta)
{: #RLAC_overview}

资源级别的访问控制使您能够控制对管理设备的用户和 API 密钥访问权。您可以使用资源组来指定每个用户或 API 密钥可管理的组织中的设备。有关如何配置资源级别访问控制的信息，请参阅[配置资源级别访问控制](rlac.html#configure_RLAC)。

**重要信息：**{{site.data.keyword.iot_full}} 资源级别访问控制功能只作为受限 Beta 程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

## 资源级别访问控制概念 
{: #RLAC_concepts}

设备组是 {{site.data.keyword.iot_short_notm}} 的资源级别访问控制的一部分。您可以使用设备组功能，根据角色来管理人员可以访问的设备数。在实现设备组之前，请确定并记录设备组的意图和作用域，以提高解决方案的效率。 

您可以将设备组作为 IoT 解决方案的一部分来实现，以使用户或应用程序能够访问设备的子集，而不是访问与组织关联的所有设备。要提高设备组的效率，请在一个人员必须具有对许多设备的访问权时，使用设备组。

例如，一家采用许多现场工程师和操作人员的公司希望在整个英国实施 IoT 解决方案。该公司为 69 个城市中的每一个配置设备组，每个地理区域 9 个设备组，整个英国配置一个设备组。将设备分配给这些组，并且这些组将分配给 15 个现场工程师和操作人员，其中一些人员具有对多个组的访问权。仅负责一个或两个设备的用户可以继续使用属于 {{site.data.keyword.iot_short_notm}} 外部的移动应用程序和企业体系结构，但这是对整个 IoT 解决方案的补充。

有关设备组的问题可在[物联网问题空间 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/smartspace/internet-of-things/){: new_window} 中提出。

## 访问控制的资源组限制

强制实施资源组限制，以确保资源级别的访问控制功能有效工作。这些限制可限制分配给主题的资源组数、组中的设备数以及资源可以属于的组数。

将实施以下限制：

 - 最多可以将 10 个资源组指定给 API 密钥、用户或网关。
 - 资源组最多可以有 300 个资源。
 - 一个资源最多可以属于 10 个组。

## 资源级别的访问控制术语

您可以使用以下部分来帮助您了解资源级别访问控制功能中使用的术语。 

### 主体
{: #subjects}

主体是请求平台访问权的已认证实体，必须对其进行授权。以下类型的主体有效：

- *用户*：使用应用程序的最终用户。成员是帐户没有截止日期的用户，访客是帐户具有截止日期的用户。
- *设备*：访问 {{site.data.keyword.iot_short_notm}} 平台并使用设备类型的物理设备。
- *网关*：访问 {{site.data.keyword.iot_short_notm}} 平台并使用网关类型的物理设备。
- *应用程序*：通过将 API 密钥用于授权和访问控制来访问 {{site.data.keyword.iot_short_notm}} 平台的应用程序或服务。

### 资源
{: #resources}

资源是主体对其执行操作的实体。主体请求对资源进行授权平台访问。设备是资源级别访问控制 Beta 发行版中唯一支持的资源。

### 操作
{: #actions}

操作是主体执行的操作，通常为“创建”、“读取”、“更新”、“删除”或“执行”。操作 (operation) 用于对资源的操作 (action) 进行分组。

### 应用程序
{: #applications}

应用程序可以代表对平台未知的主体执行操作，例如控制家用设备的移动应用程序，应用程序可以代表对平台已知或未知的真实或模拟设备进行操作。应用程序也可以是不代表任何其他类型的设备执行操作的真实服务器端应用程序。

通过 API 密钥或令牌提供对 {{site.data.keyword.iot_short_notm}} 平台的认证和访问权。

### 操作
{: #operations}

操作 (operation) 包含一个或多个在特定资源类型上执行的操作 (action)。它们由不同的主体（包括成员、应用程序和设备）使用。操作由其操作标识 (OID) 进行标识。

### 角色
{: #roles}

角色是用于提供使用操作许可权的操作集。主体可以具有多个角色，角色是主体的属性。

**角色格式**

0..n 角色数组

    { "roles": [ "PD_ADMIN_APP" ] }

**角色到组格式**

0..n <string, list<string>> 对的映射

    { "rolesToGroups": { "PD_ADMIN_APP": [ "group1" ] } }

### 许可权
{: #permissions}

许可权允许主体对资源或资源组运行操作。

### 资源组
{: #resource_groups}

资源组是作为资源的设备集合。可以将设备添加到资源组，并且可以将资源组指定给角色到组对中的主体。这些关系允许主体对特定设备组上的给定角色执行操作。

### 用户
{: #users}

用户使用作为组织成员的标识登录到 {{site.data.keyword.iot_short_notm}} 平台仪表板。用户会分配有角色，从而授权他们调用特定 HTTP API 集。有关用户角色的更多信息，请参阅[用户角色的访问级别](roles_access.html#levels-of-access-for-user-roles)。

### API 密钥
{: #API_keys}

API 密钥是用于调用 {{site.data.keyword.iot_short_notm}} 平台 HTTP API 的令牌。API 密钥会分配有角色，从而授权他们调用特定 HTTP API 集。有关 API 密钥角色的更多信息，请参阅[应用程序角色的访问级别](app_roles_access.html#levels-of-access-for-application-roles)。

## 强制实施资源级别访问控制的 API
{: #RLAC_enforced_APIs}
如果启用资源级别的访问控制，那么仅当调用者可以访问指定的设备时，此部分中包括的与设备相关的 API 才有效。

### 设备 API（个别）

**列出特定类型的设备**

    GET /api/v0002/device/types/${typeId}

仅返回属于相应组的设备子集。

**获取设备**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}

如果调用者无法访问设备，那么返回 404 错误。

**获取设备管理详细信息**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/mgmt

如果调用者无法访问设备，那么返回 404 错误。

**获取由网关注册的设备**

    GET /api/v0002/device/types/${typeId}/devices/${gatewayId}/devices

如果调用者无法访问网关，那么返回 404 错误。仅返回属于相应组的设备子集。设备和网关必须位于调用者的组中。

**更新设备**

    PUT /api/v0002/device/types/${typeId}/devices/${deviceId}

如果调用者无法访问设备，那么返回 404 错误。

**删除设备**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}

如果调用者无法访问设备，那么返回 404 错误。

### 设备 API（批量）

**列出设备**

    GET /api/v0002/bulk/devices

仅返回属于相应组的设备子集。

**删除设备**

    DELETE /api/v0002/bulk/devices/remove

仅删除属于相应组的设备子集。对于不可访问的设备，仍返回“success = true”。

**更新设备：**

    PUT /api/v0002/bulk/devices/update

### 设备管理 API

**发起请求**

    POST /api/v0002/mgmt/requests

如果任何设备对于调用者来说是不可访问的，那么将失败。

**删除请求**

    DELETE /api/v0002/mgmt/requests/${requestId}

如果任何设备对于调用者来说是不可访问的，那么将失败。

**查看请求**

    GET /api/v0002/mgmt/requests/${requestId}

**列出请求**

    GET /api/v0002/mgmt/requests

**获取一个请求**

    GET /api/v0002/mgmt/requests/${requestId}

**获取请求设备详细信息**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus

**获取一个设备的请求设备详细信息**

    GET /api/v0002/mgmt/requests/${requestId}/deviceStatus/${typeId}/${deviceId}

### 问题确定 API

**设备的连接日志**

    GET /api/v0002/logs/connection

如果设备不可访问，那么返回一个空数组。

**添加设备错误代码**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

如果调用者无法访问设备，那么返回 404 错误。

**列出设备错误代码**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

如果调用者无法访问设备，那么返回 404 错误。

**清除设备错误代码**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/errorCodes

如果调用者无法访问设备，那么返回 404 错误。

**添加设备诊断日志**

    POST /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

如果调用者无法访问设备，那么返回 404 错误。

**列出设备诊断日志**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

如果调用者无法访问设备，那么返回 404 错误。

**获取一个设备诊断日志**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

如果调用者无法访问设备，那么返回 404 错误。

**清除设备诊断日志**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs

如果调用者无法访问设备，那么返回 404 错误。

**删除一个设备诊断日志**

    DELETE /api/v0002/device/types/${typeId}/devices/${deviceId}/diag/logs/${logId}

如果调用者无法访问设备，那么返回 404 错误。

###最后一个事件高速缓存 API

**获取设备的事件**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events

如果调用者无法访问设备，那么返回 404 错误。

**获取设备的事件**

    GET /api/v0002/device/types/${typeId}/devices/${deviceId}/events/${eventId}

如果调用者无法访问设备，那么返回 404 错误。
