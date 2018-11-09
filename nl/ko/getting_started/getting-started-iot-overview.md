---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# {{site.data.keyword.iot_short_notm}}에 대한 시작하기 안내서
{: #getting-started}

시작하기 안내서는 {{site.data.keyword.iot_full}}에서 프로덕션을 위한 준비된 엔드-투-엔드 IoT 프로토타입 시스템을 개발하기 위한 기본사항을 안내하도록 디자인되었으며 {{site.data.keyword.iot_short_notm}} 작업을 처음 수행하는 개발자를 위해 작성되었습니다.
{:shortdesc}

각 안내서는 하나 이상의 {{site.data.keyword.iot_short_notm}} 기능을 보여주며 배치되어 실행 중인 솔루션을 빠르게 제시하는 단계별 프로세스를 제공합니다.

## 안내서 사용  
{: #using-guides}
각 안내서에는 {{site.data.keyword.iot_short_notm}} 우수 사례에 따라 코딩된 GitHub 소스 샘플이 하나 이상 있습니다. 이러한 샘플은 스케일링되고 보안되고 시스템과 통합될 수 있으며, 일반적으로 devOps 개발에 적합하고 프로세스를 빌드할 수 있습니다.

기술적인 면에서 능숙한 사용자는 샘플 코드를 고유 환경에 맞추거나 프로덕션 환경에서 발생할 수 있는 상황을 실제와 같이 시뮬레이션하는 빠른 솔루션을 하드웨어 예제와 함께 완성하여 빠른 솔루션을 확장할 수 있습니다. 안내서는 태스크 관련 문서, API 문서, 클라이언트 라이브러리(SDK), 안내 동영상 및 기타 교육 자료에 대한 추가 링크를 제공합니다.

순서대로 안내서를 따를 수 있으며, 첫 번째 안내서는 다음 안내서 빌드의 기초가 되는 내용을 제공합니다. 안내서 시리즈를 건너뛰면서 원하는 경로로 보려는 경우 각 안내서에도 요구사항 목록이 제공됩니다. 요구사항에서는 이러한 안내서가 요구하는 디바이스 데이터의 형식을 알려주며, 사용자는 고유 환경을 적절하게 설정할 수 있습니다.
{: tip}

## 안내서 목록
{: #list-of-guides}  

다음 안내서를 사용할 수 있습니다.

|안내서 |설명 |    
| ----- | ---- |   
|[1. 컨베이어 벨트 디바이스 연결](getting-started-iot-conveyor.html) |{{site.data.keyword.iot_short_notm}} 조직을 설치하여 시작한 다음 샘플 GitHub 소스 컨베이어 벨트 시뮬레이션 애플리케이션 또는 고유 Raspberry-Pi 기반 컨베이어 벨트 시뮬레이터(구체적인 내용을 원하는 경우)를 연결하십시오. </br> 그런 다음 몇 가지 기본 {{site.data.keyword.iot_short_notm}} 대시보드 카드를 설정하여 디바이스 데이터를 시각화하고 모니터하십시오. |   
| [2. 디바이스 데이터 모니터링](getting-started-iot-monitoring.html) |실시간으로 컨베이어 벨트 데이터를 확인하려면 Node.js 또는 위젯 라이브러리 기반 모니터링 앱을 연결하십시오.   
| [3. 많은 수의 디바이스 시뮬레이션](getting-started-iot-large-scale-simulation.html) |많은 수의 디바이스 시뮬레이터를 사용자 환경에 추가하여 단순 디바이스 시뮬레이션을 확장하십시오. </br>**중요:** 애플리케이션에는 512MB의 메모리가 필요하며, 이 양은 기본적으로 할당되는 메모리보다 많고 무료 평가판 계정에서 사용할 수 있는 양을 초과하므로 먼저 구독 또는 종량과금제 계정으로 업그레이드해야 합니다. |   
