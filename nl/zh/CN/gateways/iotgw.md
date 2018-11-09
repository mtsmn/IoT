---

copyright:
years: 2016, 2018
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IoT 网关软件包 
{: #gw_package}

{{site.data.keyword.iot_full}} IoT 网关软件包使已向 {{site.data.keyword.iot_short_notm}} 注册的网关能够将事件发送到与网关相关联的资源组中的设备平台上。
{:shortdesc}


## 概述

IoT 网关软件包中包含以下实体：

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/watson-iot/iot-gateway` |软件包|org、domain、gatewayTypeId、gatewayId、gatewayToken  |使用 {{site.data.keyword.iot_short_notm}} 网关|
|`/watson-iot/iot-gateway/publishEvent` |操作|org、domain、gatewayTypeId、gatewayId、gatewayToken、eventType、typeId、deviceId、payload |将已注册网关的事件代表其关联设备发送到 {{site.data.keyword.iot_short_notm}}|

## 创建 IoT 网关软件包绑定
要创建 IoT 网关软件包绑定，必须指定以下参数：

|参数|描述|
| --- | ---  |
|org |组织标识|
|        gatewayTypeId      |已注册网关的网关类型标识|
|gatewayId |已注册网关的网关标识|
|gatewayToken |已注册网关的授权令牌|


完成以下步骤以创建软件包绑定：  
1. [登录到 IBM Cloud 控制台 ![外部链接图标](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/)。
2. 在 IBM Cloud 中创建 [Internet of Things Platform 服务](https://console.bluemix.net/docs/services/IoT/index.html)，并[记下 `API 密钥`和 `API 令牌`](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications)。创建网关类型和注册网关需要此信息。
3. [创建网关类型](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html)（例如，Watson IoT 组织中的 *myGWType*）和[注册网关的实例](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html)（例如 *myGWId*）。记下注册网关的*网关令牌*。
4. 使用以下示例命令创建带有 IoT 网关软件包的软件包绑定：
   ```
   bx wsk package bind /watson-iot/iot-gateway myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. 使用以下命令验证软件包绑定是否存在：  
   ```
   bx wsk package list
   ```

## 发布设备事件

`/watson-iot/iot-gateway/publishEvent` 操作代表其关联设备，从已注册 {{site.data.keyword.iot_short_notm}} 网关发布事件。下表描述了发布事件时使用的参数：  

参数|描述|示例
------------- | ------------- | -------------
org |已注册的网关所属的组织的标识。|`-p org "uguhsp"`
domain |可选。已注册的网关所属的域。缺省值指向 `messaging.internetofthings.ibmcloud.com`|`-p domain "messaging.internetofthings.ibmcloud.com"`
        gatewayTypeId      |已注册网关的网关类型标识。|`-p gatewayTypeId "myGatewayType"`
gatewayId |已注册网关的网关标识。对于给定的网关类型，标识在组织中必须是唯一的。|`-p gatewayId "00aabbccde03"`
gatewayToken |已注册网关用于连接到 Watson IoT Platform 的令牌（密码）。|`-p gatewayToken "ZZZ"`
eventType|已注册网关代表其关联设备将事件发布到的事件类型。|`-p eventType "evt"`
typeId|与已注册网关相关联的设备的设备类型。已注册网关代表相关联设备发布事件。|`-p typeId "myDeviceType"`
        deviceId      |与已注册网关相关联的设备的设备标识。已注册网关代表相关联设备发布事件。对于给定的网关类型，设备标识在组织中必须是唯一的。|`-p deviceId "00aabbccde03_0001"`
payload|已注册网关代表设备发布的有效内容。|`-p payload "{'d':{'temp':38}}"`


以下示例显示如何从 *iotgw* 软件包发布事件：

通过使用所创建的软件包绑定中的 *publishEvent* 操作来发布设备事件。必须将 `/myNamespace/myGateway` 替换为软件包名。

 ``` 
 bx wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## 使用存储库

完成以下步骤以使用 `installCatalog.sh` 部署软件包
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

其中，*AUTH* 是您的授权密钥，*APIHOST* 是 IBM Cloud Functions 主机名，*WSK_CLI* 是 IBM Cloud Functions CLI 二进制文件的位置。
