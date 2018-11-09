---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} 에지 개요(미리보기)
{: #edge_overview}

**중요:** {{site.data.keyword.iot_full}} 에지 기능은 제한된 미리보기 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

{{site.data.keyword.iot_short_notm}} 에지를 사용하면 {{site.data.keyword.iot_short_notm}} 기능을 에지 디바이스로 확장할 수 있습니다. 에지 디바이스는 조직 내의 네트워크의 에지에 상주합니다. 현장(예: 공장)의 센서 및 산업용 제어기를 예로 들 수 있습니다. {{site.data.keyword.iot_short_notm}} 에지를 사용하면 클라우드로 전송되기 전에 데이터가 디바이스의 내부에서 처리될 수 있습니다. 

{{site.data.keyword.iot_short_notm}} 에지를 사용하려면 에지 노드를 작성하고 내부에서 실행될 서비스로 해당 노드를 구성하십시오. 그러면 WIoTP 에지가 해당 서비스를 노드에 배치합니다. 내부에서 실행되는 에지 서비스를 사용하여 에지 노드는 에지 게이트웨이로 디바이스에서 메시지를 전송할 수 있습니다. 게이트웨이는 메시지를 처리하고 데이터를 변환한 후에 인터페이스가 구성된 방법에 따라 데이터를 클라우드로 전송할 수 있습니다. 

{{site.data.keyword.iot_short_notm}} 에지 미리보기는 사용자 정의 서비스(예: 제조업체 플로어 로봇의 장애를 예측하고 장애 알림을 발송하는 서비스) 작성 기능과 함께 코어 IoT 기본 서비스를 제공합니다. 에지 디바이스의 예에는 CPU 사용량을 모니터하고 사용량이 특정 백분율을 초과할 때 경보를 발송하는 센서가 있습니다. 

## {{site.data.keyword.iot_short_notm}} 에지 컴포넌트

**에지 노드**
에지 노드는 네트워크의 에지에 상주하는 게이트웨이와 디바이스로 구성되어 있습니다. 

**에지 디바이스**
네트워크의 에지에 상주하며 클라우드로 전송되기 전에 데이터를 처리하는 서비스를 실행하는 디바이스(예: 센서)입니다. 에지 디바이스의 예에는 CPU 사용량을 모니터하고 사용량이 특정 백분율을 초과할 때 경보를 발송하는 센서가 있습니다. 

**에지 게이트웨이**
게이트웨이는 애플리케이션과 디바이스의 결합된 기능을 보유한 특수 서비스이며, 기타 디바이스의 액세스 지점 역할을 수행할 수 있도록 허용합니다. 에지가 사용되는 경우, 에지 게이트웨이는 인터넷에 직접 연결할 수 없는 디바이스가 우선 에지에서 게이트웨이 디바이스에 연결하여 {{site.data.keyword.iot_short_notm}} 서비스에 액세스할 수 있도록 합니다. 

**에지 게이트웨이 유형**
게이트웨이 유형은 모델 번호 또는 위치 등의 속성을 공유하는 게이트웨이 디바이스를 그룹화합니다. 에지 기능이 사용되고 에지 게이트웨이 디바이스가 {{site.data.keyword.iot_short_notm}}에 추가된 경우, 에지 게이트웨이 유형의 속성은 새 에지 게이트웨이 디바이스에 대한 템플리트로서 사용됩니다. 

**에지 서비스**
서비스는 {{site.data.keyword.iot_short_notm}} 에지 게이트웨이에 추가되는 기능이며 데이터 조작 등의 프로세스를 수행합니다. 사용자는 서비스를 사용하는 애플리케이션을 빌드합니다.

**에지 서비스 카탈로그**
기본 코어 IoT 서비스는 카탈로그에 포함되어 있으며, 모든 에지 노드에 추가됩니다. 사용자는 자체 비즈니스 요구사항에 따라 사용자 정의 서비스를 추가할 수 있습니다. 에지 서비스 카탈로그에는 사용 가능한 모든 사용자 정의 서비스가 보관됩니다. 

**파일 서버 저장소**
파일 서버 저장소는 Docker 컨테이너 및 이미지를 보관합니다. 모든 에지 처리 기능 또는 서비스는 Docker 컨테이너 내부에서 작동됩니다. 사용자는 디바이스의 내부에서 실행되는 Docker 이미지를 선택합니다.

## 자세한 정보 찾기
{: #more_info}

{{site.data.keyword.iot_short_notm}} 에지의 구성, 설치 및 개발에 대한 자세한 정보는 다음 주제를 참조하십시오. 
 - [{{site.data.keyword.iot_short_notm}} 에지 구성](WIoTP_edge_config.html#edge_configure)
 - [에지 디바이스에 {{site.data.keyword.iot_short_notm}} 에지 설치](WIoTP_edge_install.html#edge_install_device)
 - [{{site.data.keyword.iot_short_notm}} 에지 개발](WIoTP_edge_dev.html#edge_dev)
