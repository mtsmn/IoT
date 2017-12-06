---

copyright:
years: 2016, 2017
lastupdated: "2017-10-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IoT 게이트웨이 패키지(베타)
{: #gw_package}

{{site.data.keyword.iot_full}} IoT 게이트웨이 패키지는 {{site.data.keyword.iot_short_notm}}에 등록된 게이트웨이를 사용하여 게이트웨이와 연관된 리소스 그룹에 있는 디바이스의 플랫폼으로 이벤트를 보냅니다.
{:shortdesc}

**중요:** IoT 게이트웨이 패키지는 제한된 베타 프로그램의 일부로만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

## 개요

IoT 게이트웨이 패키지에는 다음 엔티티가 포함됩니다.

| 엔티티 | 유형 | 매개변수 | 설명 |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | 패키지 | org, domain, gatewayTypeId, gatewayId, gatewayToken  | {{site.data.keyword.iot_short_notm}} 게이트웨이 작업 |
| `/watson-iot/iot-gateway/publishEvent` | 조치 | org, domain, gatewayTypeId, gatewayId, gatewayToken, eventType, typeId, deviceId, payload | 연관된 디바이스 대신 등록된 게이트웨이에서 {{site.data.keyword.iot_short_notm}}으로 이벤트 전송 |

## IoT 게이트웨이 패키지 바인딩 작성
IoT 게이트웨이 패키지 바인딩을 작성하려면 다음 매개변수를 지정해야 합니다.

| 매개변수 |  설명 |
| --- | ---  |
| org | 조직 ID |
|  gatewayTypeId| 등록된 게이트웨이의 게이트웨이 유형 ID |
|  gatewayId| 등록된 게이트웨이의 게이트웨이 ID |
| gatewayToken | 등록된 게이트웨이의 인증 토큰 |


패키지 바인딩을 작성하려면 다음 단계를 완료하십시오.  
1. [Bluemix 콘솔에 로그인 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/).
2. Bluemix에서 [Internet of Things Platform 서비스](https://console.bluemix.net/docs/services/IoT/index.html)를 작성하고 [`API 키`와 `API 토큰`을 기록](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications)해두십시오. 정보는 게이트웨이 유형을 작성하고 게이트웨이를 등록하는 데 필요합니다.
3. Watson IoT 조직에서 [게이트웨이 유형을 작성](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html)하고(예: *myGWType*) [게이트웨이의 인스턴스를 등록](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html)하십시오(예: *myGWId*). 등록된 게이트웨이에 대한 *게이트웨이 토큰*을 기록해두십시오.
4. 다음 명령 예제를 사용하여 IoT 게이트웨이 패키지와의 패키지 바인딩을 작성하십시오.
   ```
   wsk package bind /watson/iotgw myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. 다음 명령을 사용하여 패키지 바인딩이 있는지 확인하십시오.  
   ```
   wsk package list
   ```

## 디바이스 이벤트 공개

`/watson/iotgw/publishEvent` 조치는 연관된 디바이스 대신 등록된 {{site.data.keyword.iot_short_notm}} 게이트웨이에서 이벤트를 공개합니다. 다음 표에는 이벤트 공개 시 사용되는 매개변수가 설명되어 있습니다.  

매개변수 |  설명 | 예제
------------- | ------------- | -------------
org | 등록된 게이트웨이가 속한 조직의 ID. | `-p org "uguhsp"`
domain | 선택사항입니다. 등록된 게이트웨이가 속한 도메인. 기본값은 `messaging.internetofthings.ibmcloud.com`을 가리킵니다. | `-p domain "messaging.internetofthings.ibmcloud.com"`
 gatewayTypeId| 등록된 게이트웨이의 게이트웨이 유형 ID. | `-p gatewayTypeId "myGatewayType"`
 gatewayId| 등록된 게이트웨이의 게이트웨이 ID. ID는 지정된 게이트웨이 유형의 조직 내에서 고유해야 합니다. | `-p gatewayId "00aabbccde03"`
gatewayToken | 등록된 게이트웨이에서 Watson IoT Platform에 연결하는 데 사용되는 토큰(비밀번호). | `-p gatewayToken "ZZZ"`
eventType| 연관된 디바이스 대신 등록된 게이트웨이가 이벤트를 공개하는 이벤트 유형. | `-p eventType "evt"`
typeId| 등록된 게이트웨이와 연관된 디바이스의 디바이스 유형. 연관된 디바이스 대신 등록된 게이트웨이가 이벤트를 공개합니다. | `-p typeId "myDeviceType"`
 deviceId| 등록된 게이트웨이와 연관된 디바이스의 디바이스 ID. 연관된 디바이스 대신 등록된 게이트웨이가 이벤트를 공개합니다. 디바이스 ID는 지정된 게이트웨이 유형의 조직 내에서 고유해야 합니다. | `-p deviceId "00aabbccde03_0001"`
payload| 디바이스 대신 등록된 게이트웨이가 공개하는 페이로드. | `-p payload "{'d':{'temp':38}}"`


다음 예제는 *iotgw* 패키지의 이벤트를 공개하는 방법을 표시합니다.

작성한 패키지 바인딩에서 *publishEvent* 조치를 사용하여 디바이스 이벤트를 공개하십시오. `/myNamespace/myGateway`를 패키지 이름과 바꿔야 합니다.

 ```
  wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## 저장소 작업

`installCatalog.sh`를 사용하여 패키지를 배치하려면 다음 단계를 완료하십시오.
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

여기서 *AUTH*는 권한 키이고 *APIHOST*는 OpenWhisk 호스트 이름이며 *WSK_CLI*는 Openwhisk CLI 바이너리의 위치입니다.
