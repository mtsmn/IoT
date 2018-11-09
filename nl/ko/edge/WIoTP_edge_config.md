---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} 에지 구성(미리보기)
{: #edge_configure}

**중요:** {{site.data.keyword.iot_full}} 에지 기능은 제한된 미리보기 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

에지 노드에 연결된 디바이스에서 데이터 수신을 시작하려면 게이트웨이를 {{site.data.keyword.iot_short_notm}}에 연결해야 합니다. {{site.data.keyword.iot_short_notm}}에 게이트웨이를 연결하려면 게이트웨이 디바이스 유형을 작성하고 게이트웨이를 {{site.data.keyword.iot_short_notm}}에 등록해야 합니다. 그러면 등록 정보를 사용하여 게이트웨이 디바이스를 {{site.data.keyword.iot_short_notm}}에 연결할 수 있습니다. 

## 상위 레벨 구성 플로우

1. {{site.data.keyword.iot_short_notm}} 에지 미리보기 테스트 환경 내의 [조직을 작성](#edge_org)하십시오. 
2. [{{site.data.keyword.iot_short_notm}} 미리보기를 사용하도록 설정](#edge_enable)하십시오.
3. [에지가 사용되는 게이트웨이 유형을 작성](#edge_gw_type)하고 [게이트웨이 디바이스를 추가](#edge_gw)하며 에지 노드에서 실행될 에지 서비스를 선택하여 {{site.data.keyword.iot_short_notm}}에서 에지 게이트웨이 노드를 사용하도록 조직을 구성하십시오. 필요하면 [사용자 정의 서비스를 작성](WIoTP_edge_dev.html#create_service)할 수 있습니다. 
4. [에지 서비스를 구성](#config_service)하십시오.
5. [디바이스에 에지를 설치](#edge_install_device)하십시오.
6. [에지 서비스를 관리하고 모니터](#monitor_service)하십시오.

## 조직 작성
{: #edge_org}

다음 단계를 완료하여 {{site.data.keyword.iot_short_notm}} 에지 미리보기 테스트 환경 내의 조직을 작성하십시오.
1. {{site.data.keyword.Bluemix_notm}} 조직에서 {{site.data.keyword.Bluemix_notm}} 계정을 등록하고 {{site.data.keyword.iot_short_notm}} 서비스의 인스턴스를 작성하십시오. [IBM Cloud 서비스 카탈로그의 {{site.data.keyword.iot_short_notm}} 페이지 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window}에서 직접 {{site.data.keyword.iot_short_notm}} 인스턴스를 작성할 수 있습니다.   
2. IBM Cloud 조직을 프로비저닝하기 위한 정보를 완료하십시오. **중요:** {{site.data.keyword.iot_short_notm}} 에지 미리보기를 사용하려면 **미국 남부** 위치에 IBM Cloud 조직을 배치해야 합니다. 
3. **작성**을 클릭하십시오.
4. 열리는 {{site.data.keyword.iot_short_notm}} 인스턴스에서 **실행**을 클릭하십시오.
새 {{site.data.keyword.iot_short_notm}} 조직이 작성됩니다. 조직 ID가 URL 시작 부분에서 및 사용자 이름 아래의 대시보드에 나타납니다. 이제 조직에 대해 {{site.data.keyword.iot_short_notm}} 에지를 사용할 수 있습니다. 

## {{site.data.keyword.iot_short_notm}} 에지 미리보기 사용
{: #edge_enable}

{{site.data.keyword.iot_short_notm}} 에지 미리보기는 기본적으로 꺼져 있습니다. {{site.data.keyword.iot_short_notm}} 에지를 사용하려면 다음을 수행하십시오. 

1. 조직의 {{site.data.keyword.iot_short_notm}} 대시보드에서 **설정**을 선택하십시오.
2. **시범 기능** 섹션에서 {{site.data.keyword.iot_short_notm}} 에지 미리보기를 포함하여 시범 기능을 활성화하십시오. 
3. 브라우저를 새로 고쳐서 대시보드 탐색 메뉴에서 새 **에지 서비스** 기능과 바로 가기를 보십시오. **참고:** **에지 서비스** 아이콘이 대시보드 탐색 메뉴에 나타나지 않으면 **미국 남부**를 위치로 선택했는지 확인하십시오. 

플래그가 논리 브라우저 스토리지에 추가됩니다. 

## 에지 기능이 사용되는 게이트웨이 유형 작성
{: #edge_gw_type}

{{site.data.keyword.iot_short_notm}}에 연결된 각 게이트웨이는 게이트웨이 유형과 연관되어야 합니다. 게이트웨이 유형은 공통 특성을 공유하는 디바이스의 그룹입니다.

1. {{site.data.keyword.iot_short_notm}} 대시보드 기본 탐색 메뉴에서 **디바이스**를 선택하십시오.
2. **디바이스 유형** 탭에서 **디바이스 유형 추가**를 클릭하십시오.
3. **유형 추가** 페이지의 **ID** 탭에서 **게이트웨이** 유형을 선택하고 게이트웨이 유형 이름(예: `my_gateway_type` 및 게이트웨이 유형에 대한 설명을 입력하십시오.
**중요:** 게이트웨이 유형 이름은 36자 이하여야 하며 다음 문자만 포함할 수 있습니다.
<ul>
 <li>영숫자 문자(a-z, A-Z, 0-9)</li>
 <li>하이픈(-)</li>
 <li>밑줄(&lowbar;)</li>
 <li>마침표(.)</li>
 </ul>
3. 선택사항: 게이트웨이 유형 속성 및 메타데이터를 입력하십시오.
4. **에지 기능** 단추를 켜십시오. 
5. 게이트웨이 유형의 아키텍처 유형을 선택하십시오. 아키텍처 유형은 디바이스의 아키텍처와 일치해야 합니다. 예를 들어, Odroid 디바이스는 ARM64 아키텍처에서 실행됩니다.
6. **다음**을 클릭하십시오.
7. 선택사항: **디바이스 정보** 탭에서 게이트웨이 디바이스 유형과 관련된 추가 세부사항 및 메타데이터를 입력하십시오. 
8. 에지 디바이스에 추가될 에지 서비스를 선택하십시오. 코어 IoT 서비스가 모든 디바이스에 추가되지만, 다음 단계를 완료하여 카탈로그에서 다른 서비스를 추가할 수 있습니다. 
 1. **서비스 추가**를 클릭하십시오. 
 2. **에지 서비스 추가** 창에서 추가할 서비스의 해당 버전을 선택한 후에 **완료**를 클릭하십시오. 
9. **게이트웨이 유형 추가** 창에서 **완료**를 클릭하십시오. [이 게이트웨이 유형에 게이트웨이 디바이스 추가](#edge_gw)를 진행할 수 있습니다. 

## 게이트웨이 디바이스 추가
{: #edge_gw}

1. {{site.data.keyword.iot_short_notm}} 대시보드 기본 탐색 메뉴에서 **디바이스**를 선택하십시오.
2. **디바이스 추가**를 클릭합니다.
3. **ID** 탭에서 디바이스가 추가되는 게이트웨이 디바이스 유형을 선택하고 추가하는 게이트웨이 디바이스의 고유 ID를 입력하십시오.
디바이스 ID는 {{site.data.keyword.iot_short_notm}} 대시보드에서 게이트웨이 디바이스를 식별하는 데 사용하고 게이트웨이 디바이스를 {{site.data.keyword.iot_short_notm}}에 연결하는 데도 필요한 매개변수입니다.
**중요:** 디바이스 ID는 36자 이하여야 하며 다음 문자만 포함할 수 있습니다.
 <ul>
 <li>영숫자 문자(a-z, A-Z, 0-9)</li>
 <li>하이픈(-)</li>
 <li>밑줄(&lowbar;)</li>
 <li>마침표(.)</li>
 </ul>
 **팁:** 네트워크 연결 디바이스의 경우 디바이스 ID는 예를 들어, 구분하는 콜론이 없는 디바이스 MAC 주소일 수 있습니다.
4. **다음**을 클릭하십시오.
5. 선택사항: **디바이스 정보** 탭에서 게이트웨이 디바이스와 관련된 추가 세부사항 및 메타데이터를 입력하십시오. 
6. **다음**을 클릭하십시오.
7. **디바이스 추가** 페이지의 **보안** 탭에서 인증 토큰을 자동 생성하거나 이 디바이스의 고유 인증 토큰을 제공하십시오. 고유 토큰을 작성하도록 선택하는 경우 길이가 8 - 36자이며 대소문자가 혼합되어 있고, 숫자, 하이픈, 밑줄 또는 마침표가 포함되는지 확인합니다. 토큰에는 반복된 문자 시퀀스, 사전 단어, 사용자 이름 또는 기타 사전 정의된 시퀀스가 포함되지 않아야 합니다.
9. **다음**을 클릭하십시오.
10. **완료**를 클릭하십시오.
11. 디바이스 정보 페이지에서 다음 디바이스 정보를 복사하고 저장하십시오. 
 - 조직 ID(예: `tubo8x`)
 - 디바이스 유형(예: `my_gateway_type`)
 - 디바이스 ID **팁:** 네트워크 연결 디바이스의 경우 디바이스 ID는 예를 들어, 구분하는 콜론이 없는 디바이스 MAC 주소일 수 있습니다.
 - 인증 메소드(예: `token`)
 - 인증 토큰 (예: `PtBVriRqIg4uh)_-Kl`).
  이 정보를 사용하여 게이트웨이를 {{site.data.keyword.iot_short_notm}}에 연결하고 게이트웨이에 연결된 디바이스에서 데이터 수신을 시작할 수 있습니다.

## 에지 서비스 구성
{: #config_service}

에지 기능이 사용되는 새 디바이스 유형이 작성되면 에지 노드에 설치할 다른 서비스를 선택할 수 있습니다. 

1. {{site.data.keyword.iot_short_notm}} 대시보드 기본 탐색 메뉴에서 **디바이스**를 선택하십시오. 
2. **디바이스 유형**을 클릭하십시오. 
3. 구성할 에지 디바이스 유형을 선택하십시오.
4. **에지 서비스** 탭에서 연필 아이콘을 선택하여 레코드를 편집하십시오.
6. **에지 서비스 추가**를 클릭하여 이 조직에 대해 업로드된 서비스의 목록을 보십시오. 
7. **에지 서비스 추가** 창에서 추가할 서비스의 해당 버전을 선택한 후에 **완료**를 클릭하십시오. 
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## 에지 서비스 관리 및 모니터링
{: #monitor_service}

에지 노드가 설치되면 {{site.data.keyword.iot_short_notm}}을 사용하여 에지 노드에서 실행 중인 서비스의 상태를 모니터할 수 있습니다. 

1.	{{site.data.keyword.iot_short_notm}} 대시보드 기본 탐색 메뉴에서 **디바이스**를 선택하십시오. 
2.	에지 노드에 해당하는 디바이스를 선택하십시오.
3.	**에지 서비스**를 클릭하여 에지 노드에 설치된 서비스의 상태를 보십시오. 

## 자세한 정보 찾기
{: #more_info}

{{site.data.keyword.iot_short_notm}}에 게이트웨이를 연결하는 데 관한 정보는 [게이트웨이용 MQTT 연결](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt)을 참조하십시오.

{{site.data.keyword.iot_short_notm}}에 대한 자세한 정보는 [Watson IoT Platform 시작하기](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp)를 참조하십시오. 

{{site.data.keyword.iot_short_notm}} 에지에 대한 자세한 정보는 다음 주제를 참조하십시오. 
- [{{site.data.keyword.iot_short_notm}} 에지 개요](WIoTP_edge.html#edge_overview)
- [에지 디바이스에 {{site.data.keyword.iot_short_notm}} 에지 설치](WIoTP_edge_install.html#edge_install_device)
- [{{site.data.keyword.iot_short_notm}} 에지 개발](WIoTP_edge_dev.html#edge_dev)
