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

# IoT 閘道套件 
{: #gw_package}

{{site.data.keyword.iot_full}} IoT 閘道套件會讓向 {{site.data.keyword.iot_short_notm}} 登錄的閘道，將事件傳送至與閘道相關聯的資源群組中之裝置的平台。
{:shortdesc}


## 概觀

IoT 閘道套件包括下列實體：

|實體   |類型|參數       |說明|
| --- | --- | --- | --- |
|`/watson-iot/iot-gateway` |套件 |org、domain、gatewayTypeId、gatewayId、gatewayToken  |使用 {{site.data.keyword.iot_short_notm}} 閘道|
|`/watson-iot/iot-gateway/publishEvent` |動作 |org、domain、gatewayTypeId、gatewayId、gatewayToken、eventType、typeId、deviceId、payload |代表關聯的裝置，將事件從已登錄閘道傳送至 {{site.data.keyword.iot_short_notm}}   |

## 建立 IoT 閘道套件連結
若要建立 IoT 閘道套件連結，您必須指定下列參數：

|參數|說明|
| --- | ---  |
|org |組織 ID|
| gatewayTypeId|已登錄閘道的閘道類型 ID|
|gatewayId |已登錄閘道的閘道 ID|
|gatewayToken |已登錄閘道的授權記號|


請完成下列步驟來建立套件連結：  
1. [登入 IBM Cloud 主控台 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/)。
2. 在 IBM Cloud 中建立 [Internet of Things 平台服務](https://console.bluemix.net/docs/services/IoT/index.html)，並[記下 `API 金鑰`及 `API 記號`](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications)。需要有此資訊才能建立閘道類型，以及登錄閘道。
3. [在 Watson IoT 組織中建立閘道類型](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html)（例如，*myGWType*），以及[登錄閘道實例](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html)（例如，*myGWId*）。請記下已登錄閘道的*閘道記號*。
4. 使用下列範例指令來建立與 IoT 閘道套件的套件連結：
   ```
   bx wsk package bind /watson-iot/iot-gateway myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. 使用下列指令來驗證套件連結已存在：  
   ```
   bx wsk package list
   ```

## 發佈裝置事件

`/watson-iot/iot-gateway/publishEvent` 動作代表其關聯的裝置，從已登錄 {{site.data.keyword.iot_short_notm}} 閘道發佈事件。下表說明發佈事件時所使用的參數：  

參數|說明|範例
------------- | ------------- | -------------
org |已登錄閘道所屬組織的 ID。|`-p org "uguhsp"`
domain |選用。已登錄閘道所屬的網域。預設值指向 `messaging.internetofthings.ibmcloud.com` |`-p domain "messaging.internetofthings.ibmcloud.com"`
 gatewayTypeId|已登錄閘道的閘道類型 ID。|`-p gatewayTypeId "myGatewayType"`
gatewayId |已登錄閘道的閘道 ID。在給定閘道類型的組織內，ID 必須是唯一的。|`-p gatewayId "00aabbccde03"`
gatewayToken |已登錄閘道用來連接至 Watson IoT Platform 的記號（密碼）。|`-p gatewayToken "ZZZ"`
eventType|已登錄閘道代表關聯的裝置，將事件發佈至其中的事件類型。|`-p eventType "evt"`
typeId|與已登錄閘道相關聯之裝置的裝置類型。已登錄閘道代表關聯的裝置發佈事件。|`-p typeId "myDeviceType"`
deviceId |與已登錄閘道相關聯之裝置的裝置 ID。已登錄閘道代表關聯的裝置發佈事件。在給定閘道類型的組織內，裝置 ID 必須是唯一的。|`-p deviceId "00aabbccde03_0001"`
payload|已登錄閘道代表裝置發佈的有效負載。|`-p payload "{'d':{'temp':38}}"`


下列範例顯示如何從 *iotgw* 套件發佈事件：

在所建立的套件連結中使用 *publishEvent* 動作，以發佈裝置事件。您必須將 `/myNamespace/myGateway` 取代為您的套件名稱。

 ``` 
 bx wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## 使用儲存庫

請完成下列步驟，以使用 `installCatalog.sh` 來部署套件
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

其中 *AUTH* 是授權碼、*APIHOST* 是 IBM Cloud Functions 主機名稱，而 *WSK_CLI* 是 IBM Cloud Functions CLI 二進位的位置。
