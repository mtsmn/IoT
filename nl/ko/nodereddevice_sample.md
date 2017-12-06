---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Node-RED 디바이스 시뮬레이터 작성 및 연결
시뮬레이션된 디바이스 데이터를 {{site.data.keyword.iot_full}} 조직으로 전송하는 디바이스 시뮬레이터를 작성하려면 Node-RED를 사용하십시오.  
{:shortdesc}

## 개요

Node-RED는 새롭고 흥미로운 방식으로 하드웨어 디바이스, API 및 온라인 서비스를 함께 연결하는 데 사용되는 도구입니다. 자세한 정보는 [Node-RED ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://nodered.org/){: new_window} 웹 사이트를 참조하십시오.   

고유 환경에서 Node-RED 인스턴스를 실행하고 {{site.data.keyword.Bluemix_notm}} 애플리케이션으로 사용할 수 있습니다. 다음 단계에는 {{site.data.keyword.Bluemix_notm}}에 대한 지시사항이 포함됩니다.

MQTT 디바이스 메시지를 {{site.data.keyword.iot_short_notm}}에 보내려면 Node-RED 디바이스 시뮬레이터를 작성하고 연결하십시오. 다음 예제에서는 디바이스 시뮬레이터가 MQTT 브로커(예: {{site.data.keyword.iot_short_notm}})로 운송 컨테이너의 데이터 전송을 시뮬레이션합니다.

## 단계

1. 다음 단계를 완료하여 Node-RED 디바이스 시뮬레이터를 작성합니다.   
    1. https://console.ng.bluemix.net에서 {{site.data.keyword.Bluemix_notm}}에 로그인합니다.
    2. **카탈로그** 탭을 선택합니다.
    3. 서비스 카탈로그의 표준 유형 섹션을 찾아 **Internet of Things Platform 스타터**를 클릭하십시오. **팁:** Internet of Things Platform 스타터 페이지로 바로 이동하려면 [여기(![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘"))](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window}를 클릭하십시오.
    4. Internet of Things Platform 스타터 페이지에서 Node-RED를 배치할 영역을 선택하고 앱 작성 선택사항을 확인하고 **작성**을 클릭하여 Node-RED를 Bluemix 조직에 추가하십시오. 예를 들어 다음과 같습니다.
    <ul>
     <li> 영역: dev
     <li> 앱 이름: myDevice
     <li> 호스트 이름: myDevice  
    </ul>  
다른 옵션은 기본값으로 두십시오. 애플리케이션이 배치되고 나면 Node-RED 페이지로 코딩 시작 페이지가 표시됩니다.
**참고:** 스테이징 프로세스는 몇 분이 걸릴 수 있습니다.  

2. 라우트 링크를 클릭하여 Node-RED를 열거나 앱 URL을 방문하십시오(예: `http://myDevice.mybluemix.net`).  
3. 다음 단계를 완료하여 **Node-RED 편집기를 보안**하십시오.
    1. **다음**을 클릭하십시오.
    2. 사용자 이름 및 비밀번호를 추가하십시오.  
    **참고:** 비밀번호의 경우 필수 복잡도 규칙을 충족시켜야 합니다. 그렇지 않으면 **다음** 단추가 비활성화됩니다.  
    3. 선택사항입니다. **편집기 보기는 허용하지만 변경하지는 못하도록 함** 또는 **권장되지 않음: 편집기 액세스 및 변경 허용**을 선택하십시오.
    4. 구성을 완료하려면 **완료**를 클릭하십시오.
4. **Node-RED 플로우 편집기로 이동**을 클릭하십시오.
5. 이전에 작성한 사용자 이름 및 비밀번호를 입력하십시오.  
디바이스 시뮬레이터 플로우를 플로우 편집기에서 사용할 수 있습니다. 플로우는 위치, 측정된 온도 및 습도 데이터를 {{site.data.keyword.iot_short_notm}}으로 전송하는 **온도 조절 장치**를 시뮬레이션합니다.  
6. **Send to IBM IoT Platform** 노드에 대해 메시지와 연결된 점이 표시되는지 검사하여 디바이스가 연결되어 있음을 확인하십시오. 이 검사는 **온도 모니터** 서브플로우의 **IBM IoT App In** 노드에서도 유효합니다.  
7. 다음 단계를 완료하여 MQTT 디바이스 메시지를 전송하고 수신하십시오.  
    1. **Send Data** 삽입 노드를 클릭하여 {{site.data.keyword.iot_short_notm}}으로 보낼 데이터를 트리거하십시오.
       **참고:** **Debug output payload** 디버그 노드를 활성화하여 전송되는 데이터를 확인하고 **Device payload** 기능 노드를 검사하여 페이로드를 빌드하는 코드를 확인하십시오. 
    2. **데이터 전송**을 클릭하면 MQTT 메시지가 {{site.data.keyword.iot_short_notm}}으로 전송되고 **IBM IoT App In** 노드에서 수신됩니다. **온도 모니터** 서브플로우는 온도가 정의된 한계 내에 있는지 여부를 설정하고 디버그 탭에 메시지를 표시합니다.
       **참고:** **temp thresh** 스위치 노드를 클릭하여 임계값을 확인하고 변경하십시오.
    3. 디버그 탭을 확인하십시오. 메시지가 표시됩니다(예: **Temperature (17) within safe limits**).
    
이제 샘플 IoT 디바이스를 {{site.data.keyword.iot_short_notm}}에 연결했으므로 디바이스 데이터를 볼 수 있습니다.
