---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iot_short_notm}} 시작하기
{: #gettingstartedtemplate}

{{site.data.keyword.iot_full}} for {{site.data.keyword.Bluemix_notm}}에서는 게이트웨이 디바이스, 디바이스 관리 및 강력한 애플리케이션 액세스를 포함하는 다용도 툴킷을 제공합니다. {{site.data.keyword.iot_short_notm}}을 사용하면 연결된 디바이스 데이터를 수집하고 조직의 실시간 데이터에 대한 분석을 수행할 수 있습니다.
{:shortdesc}

## 시작하기 전에
{: #byb}

디바이스를 연결하고 데이터를 이용하기 전에 {{site.data.keyword.Bluemix_notm}} 계정을 등록하고 {{site.data.keyword.Bluemix_notm}} 조직에서 {{site.data.keyword.iot_short_notm}} 서비스의 인스턴스를 작성하십시오. [IBM Cloud 서비스 카탈로그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}의 {{site.data.keyword.iot_short_notm}} 페이지에서 직접 {{site.data.keyword.iot_short_notm}} 인스턴스를 작성할 수 있습니다.   

{{site.data.keyword.Bluemix_notm}}에서 계정을 등록하고 영역을 구성하는 방법과 기타 계정 관리 설정에 대한 세부사항은 [IBM Cloud 계정 관리](https://console.ng.bluemix.net/docs/admin/account.html#signup)를 참조하십시오. 

대시보드에서 {{site.data.keyword.iot_short_notm}} 인스턴스를 설정하고 구성할 수 있습니다. 대시보드를 열려면 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.iot_short_notm}} 서비스 인스턴스로 이동한 다음 **실행**을 클릭하십시오.

## 이 태스크에 관한 정보

다음 단계에서는 {{site.data.keyword.iot_short_notm}} 서비스를 빨리 시작하는 방법에 대해 설명합니다.

{{site.data.keyword.iot_short_notm}}에서 프로덕션을 위해 준비된 엔드-투-엔드 IoT 프로토타입 시스템을 개발하기 위한 기본사항을 안내하는 시작하기 안내서 및 샘플 애플리케이션에 대한 세부사항도 사용 가능합니다. 개발자가 {{site.data.keyword.iot_short_notm}}에 대한 작업을 처음 수행하는 경우, [시작하기 안내서](https://console.bluemix.net/docs/services/IoT/getting_started/getting-started-iot-overview.html#getting-started) 절의 단계별 프로세스를 사용하십시오.

## 1단계: 디바이스 연결
{: #up_and_running}

서비스를 시작하고 실행하려면 상황에 따라 다음 옵션을 탐색하십시오.

|  |서비스가 배치됨 |서비스가 배치되지 않음
 | -------------| ------------- | -------------
  |**연결할 디바이스가 있음**|[{{site.data.keyword.iot_short_notm}}에 디바이스 연결](iotplatform_task.html#iotplatform_task).|[{{site.data.keyword.iot_short_notm}}으로 재생 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play){:new_window}에서 디바이스 연결을 탐색합니다.
  |**I 디바이스가 연결되지 않도록 함** | [Node-RED 디바이스 시뮬레이터 작성 및 연결](nodereddevice_sample.html){:new_window}을 수행합니다. 또는 [스마트폰에 연결 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play/device/smartphone){:new_window}합니다. |[Watson IoT Platform 스타터](https://console.bluemix.net/docs/starters/IoT-starter/iot500.html)를 시작합니다.
  
특정 디바이스 유형을 {{site.data.keyword.iot_short_notm}}에 연결하는 방법에 대한 자세한 정보는 [developerWorks 레시피 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){:new_window}를 참조하십시오.  

디바이스 연결 개발자 문서는 다음을 참조하십시오.
- [디바이스용 MQTT 연결](devices/mqtt.html).
- [게이트웨이용 MQTT 연결](gateways/mqtt.html).

<!--
## Step 2: Analyze your device data
{: #analyzing_data}
Start exploring the real-time data that the devices are sending to {{site.data.keyword.iot_short_notm}}.
{{site.data.keyword.iot_short_notm}} includes the following analytics tools:  
- [Boards and cards](data_visualization.html) to visualize your real-time device data.
- [Rules and actions](analytics.html) that are triggered by real-time device data.
For a quick getting started example, see the [Using Rules and Actions with IBM Watson IoT Platform Cloud Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/using-rules-and-actions-with-ibm-watson-iot-platform-cloud-analytics/){:new_window} developerWorks recipe.
-->

## 2단계: 디바이스 데이터를 이용하는 애플리케이션 작성
{: #develop_applications}

디바이스 데이터를 이용하기 위해서 자체 앱을 작성하고 연결합니다. 

자세한 정보는 다음 주제를 참조하십시오.   
- [애플리케이션 개발자 문서](applications/api.html) 및 [{{site.data.keyword.iot_short_notm}} API 문서](reference/api.html)를 탐색하십시오.
- 디바이스와 애플리케이션을 통합하고 연결하는 코드를 빌드하고 개발하기 위한 도구와 파일을 제공하는 [{{site.data.keyword.iot_short_notm}} 클라이언트 라이브러리](iot_platform_client_lib.html)를 탐색합니다.
- [히스토리 디바이스 데이터를 저장하기 위해 {{site.data.keyword.cloudantfull}} 서비스](cloudant_connector.html)를 {{site.data.keyword.iot_short_notm}}에 연결합니다.
- [임베디드 규칙(베타)](information_management/im_rules.html) 기능을 사용하여 자체 규칙을 작성합니다. 
