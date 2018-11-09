---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# 안내서 1: 컨베이어 벨트 디바이스 연결  
{{site.data.keyword.iot_full}} on {{site.data.keyword.Bluemix_notm}}에 모니터링 데이터를 전송하는 IoT 디바이스를 사용하여 기본 컨베이어 벨트를 작성하십시오.
{:shortdesc}

## 개요 및 목표
{: #overview}  
이 안내서는 {{site.data.keyword.iot_short_notm}}에 대한 디바이스 연결, 디바이스 데이터 모니터링 및 이에 대한 조치 등의 프로세스를 통해 단계별로 진행할 수 있는 시리즈의 첫 번째 안내서입니다. 각 안내서는 이전 안내서에서 빌드되며 각 안내서의 샘플 코드는 다음 안내서에 대한 입력으로 사용됩니다. 또한 용도에 맞도록 샘플 코드를 수정하여 이러한 안내서를 별도로 사용할 수 있습니다.

이 첫 번째 안내서에서는 연결된 컨베이어 벨트를 설정하고 이를 사용하여 IoT 데이터를 {{site.data.keyword.iot_short_notm}}에 전송합니다.
스킬 레벨에 따라 다음 경로 중 하나 또는 둘 다를 사용하여 컨베이어 벨트를 설정할 수 있습니다.
- 경로 A  
이 경로에서는 컨베이어 벨트 시뮬레이터 앱을 {{site.data.keyword.Bluemix_notm}}에 설치하여 빨리 시작할 수 있습니다. 앱은 {{site.data.keyword.iot_short_notm}}에 디바이스를 자체 등록하고 올바르게 구성된 데이터를 플랫폼에 자동으로 전송합니다. 이 경로에 대한 지시사항은 [2A단계 - GitHub에서 시뮬레이터 샘플 앱 사용](#deploy_app)에 있습니다.  
- 경로 B  
이 경로는 기술적으로 더 어려우며 추가 하드웨어, Python 프로그래밍 스킬 및 {{site.data.keyword.iot_short_notm}}에 대한 디바이스 수동 등록을 필요로 합니다. 이 경로에 대한 지시사항은 [2B단계 - Raspberry Pi 및 전자 모터로 실제 컨베이어 벨트 빌드](#raspberry)를 참조하십시오.

이 안내서의 일부로 다음을 수행합니다.
- Cloud Foundry CLI를 사용하여 {{site.data.keyword.iot_short_notm}} 조직을 작성하고 배치합니다.
- 샘플 컨베이어 벨트 디바이스를 빌드하고 배치합니다.
- 시뮬레이션된 컨베이어 벨트 디바이스를 {{site.data.keyword.iot_short_notm}}에 연결합니다.
- {{site.data.keyword.iot_short_notm}} 대시보드를 사용하여 디바이스 데이터를 모니터하고 시각화합니다.

다른 IoT 디바이스를 사용하여 {{site.data.keyword.iot_short_notm}}을 시작하려면 [시작하기 튜토리얼](/docs/services/IoT/getting-started.html)을 참조하십시오.
{: tip}

## 전제조건
{: #prereqs}

다음 계정 및 도구가 필요합니다.
* [{{site.data.keyword.Bluemix_notm}} 계정 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/registration/){: new_window}
* [Cloud Foundry 명령 인터페이스(cf CLI) ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
cf CLI를 사용하여 앱 및 서비스를 {{site.data.keyword.Bluemix_notm}}에 배치하십시오. 자세한 정보는 [Cloud Foundry CLI 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.cloudfoundry.org/cf-cli/){: new_window}를 참조하십시오.
* 선택사항: [Git ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git-scm.com/downloads){: new_window}  
Git을 사용하여 코드 샘플을 다운로드하도록 선택하는 경우 [GitHub.com 계정 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com){: new_window}이 있어야 합니다. GitHub.com 계정 없이 코드를 압축 파일로 다운로드할 수도 있습니다.
* 선택사항: 가속도계 데이터를 전송하기 위해 *컨베이어 벨트* 샘플 웹 애플리케이션을 실행하는 휴대전화.

## 1단계 - {{site.data.keyword.iot_short_notm}} 배치  
{: #deploy_watson_iot_platform_service}
{{site.data.keyword.iot_short_notm}}에서는 신속하게 분석 애플리케이션, 시각화 대시보드 및 모바일 IoT 앱을 작성할 수 있도록 IoT 디바이스와 데이터에 대한 강력한 애플리케이션 액세스를 제공합니다.
다음 단계에서는 {{site.data.keyword.Bluemix_notm}} 환경에 이름이 `iotp-for-conveyor`인 {{site.data.keyword.iot_short_notm}} 서비스의 인스턴스를 배치합니다. 서비스 인스턴스가 이미 실행 중인 경우 안내서에 해당 인스턴스를 사용하여 이 첫 번째 단계를 건너뛸 수 있습니다. 안내서를 진행하는 경우 올바른 서비스 이름 및 {{site.data.keyword.Bluemix_notm}} 영역을 사용하는지 확인하십시오.
{: tip}
1. 명령행에서 cf api 명령을 실행하여 API 엔드포인트를 설정하십시오.   
 `API-ENDPOINT` 값을 지역의 API 엔드포인트로 바꾸십시오.
   ```
cf api API-ENDPOINT
   ```
예제: `cf api https://api.ng.bluemix.net`
<table>
<tr>
<th>지역</th>
<th>API 엔드포인트</th>
</tr>
<tr>
<td>미국 남부</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>영국</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. {{site.data.keyword.Bluemix_notm}} 계정에 로그인하십시오.
  ```
cf login
  ```
프롬프트가 표시되면 {{site.data.keyword.iot_short_notm}} 및 샘플 앱을 배치할 조직과 영역을 선택하십시오.
3. {{site.data.keyword.iot_short_notm}} 서비스를 {{site.data.keyword.Bluemix_notm}}에 배치하십시오.  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
  ```
YOUR_IOT_PLATFORM_NAME의 경우 *iotp-for-conveyor*를 사용하십시오.  
예제: `cf create-service iotf-service iotf-service-free iotp-for-conveyor`
3. 샘플 컨베이어 벨트 디바이스를 작성하십시오.
 - 경로 A: [2A단계 - GitHub에서 시뮬레이터 샘플 앱 사용](#deploy_app).
 - 경로 B: [2B단계 - Raspberry Pi와 전기 모터로 실제 컨베이어 벨트 빌드](#raspberry).  

## 2A단계 - 샘플 컨베이어 벨트 웹 애플리케이션 배치
{: #deploy_app}

샘플 앱을 사용하면 {{site.data.keyword.Bluemix_notm}}에 연결된 산업용 컨베이어 벨트를 시뮬레이션할 수 있습니다. 벨트를 시작하고 중지하여 벨트의 속도를 조정할 수 있습니다. 벨트의 모든 변경사항은 앱에 표시된 MQTT 메시지의 양식으로 {{site.data.keyword.Bluemix_notm}}에 전송됩니다. 기본 대시보드 카드를 사용하여 벨트 동작을 모니터링할 수 있습니다.
샘플 앱은 다음의 Node.js 클라이언트 라이브러리를 사용하여 빌드됩니다. [https://github.com/ibm-watson-iot/iot-nodejs ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![컨베이어 벨트 앱](images/app_conveyor_belt.png "컨베이어 벨트 앱")

1. 원하는 git 도구를 사용하여 다음 저장소를 복제하십시오.
https://github.com/ibm-watson-iot/guide-conveyor-simulator
Git 쉘에서 다음 명령을 사용하십시오.
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. 명령행에서 디렉토리를 샘플 앱이 있는 디렉토리로 변경하십시오.
  ```bash
cd guide-conveyor-simulator
  ```
3. *guide-conveyor-simulator* 디렉토리에서 앱을 {{site.data.keyword.Bluemix_notm}}로 푸시하고 cf push 명령에서 *YOUR_APP_NAME*을 바꿔 새 이름을 지정하십시오. {{site.data.keyword.iot_short_notm}}에 바인드된 앱을 다음 단계에서 시작할 예정이므로 `--no-start` 옵션을 사용하십시오.
**참고:** 애플리케이션 배치에 수 분이 걸릴 수 있습니다.
   ```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. *guide-conveyor-simulator* 디렉토리에서 각각에 대해 제공한 이름을 사용하여 {{site.data.keyword.iot_short_notm}}의 인스턴스에 앱을 바인드하십시오.
  ```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
애플리케이션 바인딩에 대한 자세한 정보는 [애플리케이션 연결](/docs/services/IoT/platform_authorization.html#bluemix-binding)을 참조하십시오.
2. 바인딩을 적용하려면 애플리케이션을 시작하십시오.
  ```bash
cf start YOUR_APP_NAME
  ```
이 상태에서 샘플 웹 애플리케이션이 {{site.data.keyword.Bluemix_notm}}에 배치됩니다.
배치가 완료되면 앱이 실행 중임을 설명하는 메시지가 표시됩니다.   
예제:
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
start command:     ./vendor/initial_startup.rb

     state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
앱 배치 상태 및 앱 URL을 둘 다 보기 위해 다음 명령을 실행할 수 있습니다.
  ```bash
cf apps
  ```
`cf logs YOUR_APP_NAME --recent` 명령을 사용하여 배치 프로세스에서 오류의 문제점을 해결하십시오.
{: tip}
1. 브라우저에서 앱에 액세스하십시오.  
URL `https://YOUR_APP_NAME.mybluemix.net`을 여십시오.    
예제: `https://conveyorbelt.mybluemix.net/`.
2. 디바이스의 디바이스 ID 및 토큰을 입력하십시오.  
샘플 앱은 디바이스 ID와 토큰이 제공된 `iot-conveyor-belt` 유형의 디바이스를 자동으로 등록합니다. 디바이스 등록에 대한 자세한 정보는 [디바이스 연결](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1)을 참조하십시오.
4. [3단계 - {{site.data.keyword.iot_short_notm}}의 원시 데이터 보기](#see_live_data)를 계속하십시오.

## 2B단계 - Raspberry Pi 구동 컨베이어 벨트 빌드
{: #raspberry}

Raspberry Pi 솔루션은 다음의 Python 클라이언트 라이브러리를 사용하여 빌드됩니다. [https://github.com/ibm-watson-iot/iot-python ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/iot-python){: new_window}

### 회선에 대한 스키마 다이어그램

![회선 다이어그램](images/raspi_circuit.png)

### 필수 연결

Raspberry Pi와 L298N 듀얼 H 브릿지 모터 드라이버 보드 간에 다음 연결이 필요합니다. 다음과 같이 연결하십시오.

- Raspberry Pi의 핀 2를 L298N 보드의 +5v에 연결
- Raspberry Pi의 핀 6를 L298N 보드의 GND에 연결
- Raspberry Pi의 핀 13을 L298N 보드의 IN1에 연결
- Raspberry Pi의 핀 15를 L298N 모드의 IN2에 연결
- 배터리의 +9v를 L298N 보드의 +12v에 연결
- 배터리의 -9v를 L298N 보드의 GND에 연결
- 모터의 +ve 노드를 L298N 보드의 OUT1에 연결
- 모터의 -ve 노드를 L298N 보드의 OUT2에 연결

### 하드웨어 요구사항

1. [Raspberry Pi(2/3) ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.raspberrypi.org/){: new_window}(Raspbian Jessie(8.x) 포함).
2. [L298N 듀얼 H 브릿지 모터 드라이버 보드 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. 9V DC 모터
4. 9V 배터리

### Raspberry Pi 소프트웨어 요구사항
- Python 2.7.9 이상(Raspbian Jessie(8.x)와 함께 설치됨)
- [Watson IoT Python 라이브러리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- 선택사항: Git.  
Raspberry Pi에 git이 설치되어 있지 않으면 다음 명령을 사용하여 설치할 수 있습니다.  
```bash
$ sudo apt-get install git
```

### 세부 단계

1. Raspberry Pi에 대한 SSH 또는 터미널을 여십시오.
2. 원하는 git 도구를 사용하여 Raspberry Pi에 다음 저장소를 복제하십시오.
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
Git 쉘에서 다음 명령을 사용하십시오.
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. {{site.data.keyword.iot_short_notm}}에 디바이스를 등록하십시오.  
디바이스 등록에 대한 자세한 정보는 [디바이스 연결](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1)을 참조하십시오.
	 1. {{site.data.keyword.Bluemix_notm}} 콘솔의 {{site.data.keyword.iot_short_notm}} 서비스 세부사항 페이지에서 **실행**을 클릭하십시오.
	     {{site.data.keyword.iot_short_notm}} 웹 콘솔은 새 브라우저 탭에서 다음 URL을 엽니다.
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     여기서 ORG_ID는 [{{site.data.keyword.iot_short_notm}} 조직](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}의 고유한 6자 ID입니다.
	 2. 개요 대시보드의 메뉴 분할창에서 **디바이스**를 선택한 다음 **디바이스 추가**를 클릭하십시오.
	 3. 추가하는 디바이스의 디바이스 유형을 작성하십시오.
	     1. **디바이스 유형 작성**을 클릭합니다.
	     2. 디바이스 유형 이름 `iotp-for-conveyor` 및 디바이스 유형의 설명을 입력합니다.  
	**팁:** 임의의 디바이스 유형 이름을 입력할 수 있지만, 시리즈의 다른 안내서에서는 디바이스 유형 `iotp-for-conveyor`를 사용하는 디바이스를 예상합니다. 다른 디바이스 유형을 사용하는 경우 다른 안내서의 설정을 적절하게 수정해야 합니다.
	     3. 선택사항: 디바이스 유형 속성 및 메타데이터를 입력합니다.
	 4. **다음**을 클릭하여 선택한 디바이스 유형의 디바이스를 추가하는 프로세스를 시작하십시오.
	 5. 디바이스 ID(예: `conveyor_belt`)를 입력하십시오. 
	 5. **다음**을 클릭하여 프로세스를 완료하십시오.
	 6. 인증 토큰을 제공하거나 자동으로 생성된 토큰을 허용하십시오.
	 7. 요약 정보가 올바른지 확인한 후에 **추가**를 클릭하여 연결을 추가하십시오.
	 8. 디바이스 정보 페이지에서 다음 세부사항을 복사하고 저장하십시오.
	     * 조직 ID
	     * 디바이스 유형
	     * 디바이스 ID
	     * 인증 방법
	     * 인증 토큰.
	     {{site.data.keyword.iot_short_notm}}에 연결할 디바이스를 구성하려면 조직 ID, 디바이스 유형, 디바이스 ID 및 인증 토큰에 대한 값이 필요합니다. 
4. 복제된 저장소의 *guide-conveyor-rasp-pi* 루트로 이동하십시오. 
5. 이 명령 `sudo chmod +x setup.sh`를 사용하여 쉘 스크립트가 실행 가능하도록 설정하십시오. 
6. *setup.sh* 파일을 실행하고 프롬프트가 표시되면 디바이스 정보 페이지에서 복사한 세부사항을 입력하십시오. 
```bash  
./setup.sh
```  

7. deviceClient 프로그램을 실행하십시오.   
프로그램을 실행하면 Raspberry Pi가 모터를 시작하고 매개변수 설정에 따라 최대 2분 동안 실행합니다.
```bash
python deviceClient.py -t 2
```

모터가 실행되는 동안 프로그램은 다음 샘플 페이로드 구조가 있는 이벤트 유형 `sensorData`의 이벤트를 공개합니다.   
```
{
"d": {
  "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. [3단계 - {{site.data.keyword.iot_short_notm}}에서 원시 데이터 보기](#see_live_data)를 진행하십시오. 

## 3단계 - {{site.data.keyword.iot_short_notm}}에서 원시 데이터 보기
{: #see_live_data}
1. 디바이스가 {{site.data.keyword.iot_short_notm}}에 등록되어 있는지 확인하십시오.
 1. {{site.data.keyword.Bluemix_notm}} 대시보드에 로그인하십시오(https://bluemix.net). 
 2. [서비스 목록 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://bluemix.net/dashboard/services){: new_window}에서 *iotp-for-conveyor* {{site.data.keyword.iot_short_notm}} 서비스를 클릭하십시오. 
 3. *시작*을 클릭하여 새 브라우저 탭에서 {{site.data.keyword.iot_short_notm}} 대시보드를 여십시오.  
나중에 쉽게 액세스할 수 있도록 URL을 책갈피로 표시할 수 있습니다.   
예: `https://*iot-org-id*.internetofthings.ibmcloud.com`.
 4. 메뉴에서 **디바이스**를 선택하고 새 디바이스가 표시되는지 확인하십시오. 
2. 원시 데이터를 보십시오. 
 1. 메뉴에서 **보드**를 선택하십시오. 
 3. **디바이스 중심 분석** 보드를 선택하십시오.
 4. **디바이스 I 관리** 카드를 찾고 디바이스를 선택하십시오.   
디바이스 이름이 디바이스 특성 카드에 표시됩니다.
4. 플랫폼에 센서 데이터를 전송하십시오.    
센서 측정값이 변경되면 디바이스는 데이터를 {{site.data.keyword.iot_short_notm}}에 전송합니다. 컨베이어 벨트를 중지, 시작하거나 해당 속도를 변경하여 이 데이터 전송을 시뮬레이션할 수 있습니다.    
**경로 A:** 모바일 브라우저에서 앱에 액세스하는 경우에는 스마트폰을 흔들어서 컨베이어 벨트에 대한 가속도계 데이터를 트리거해 보십시오.
{: tip}
3. 공개된 메시지에 해당하는 업데이트된 디바이스 데이터 점이 디바이스 특성 카드에 표시되는지 확인하십시오.  
메시지 예제 A:
  ```
{
	"d": {
		"id": "belt1",
		"ts": 1494341288662,
		"ay": "0.00",
		"running": true,
		"rpm": "0.6"
	}
}
  ```
메시지 예제 B:
  ```
{
	"d": {
    "elapsed": 1,
    "running": true,
    "temperature": 35.00,
    "ay": "0.00",
    "rpm": "0.6"
  }
}
  ```


## 단계 4 - {{site.data.keyword.iot_short_notm}}에서 라이브 데이터 시각화
{: #add_card}
라이브 컨베이어 벨트 데이터를 보기 위한 대시보드 카드를 작성하려면 다음을 수행하십시오. 
1. 동일한 디바이스 중심 분석 보드에서 **카드 새로 추가**를 클릭한 후에 **선형 차트**를 선택하십시오. 
2. 카드 소스 데이터에 대해 **카드**를 클릭하십시오.   
카드 이름 목록이 표시됩니다.
3. **디바이스 I 관리**를 선택한 후에 **다음**을 클릭하십시오.
4. **새 데이터 세트 연결**을 클릭하고 데이터 세트 매개변수에 대해 다음 값을 입력하십시오.
  - 이벤트: sensorData
  - 특성: d.rpm
  - 이름: Belt RPM
  - 유형: Float
  - 단위: rpm
5. **다음**을 클릭하십시오.
6. 카드 미리보기 페이지에서 **L**을 선택한 후에 **다음**을 클릭하십시오.
7. 카드 정보 페이지에서 제목의 이름을 `Belt data`로 변경한 후에 **제출**을 클릭하십시오. 
8. 벨트의 속도를 변경하여 새 카드에서 라이브 데이터를 보십시오.
9. 선택사항: 두 번째 데이터 세트를 추가하여 벨트에 대한 가속 데이터를 추가하십시오.  
전화를 사용하여 샘플 앱에 연결하는 경우 전화를 흔들어 벨트에 대한 가속 데이터를 전송할 수 있습니다.
 1. 카드에서 메뉴 아이콘을 클릭하고 카드 편집을 선택하십시오. 
 2. 카드 소스 데이터에 대해 **카드**를 선택하십시오.   
 3. **디바이스 I 관리**를 선택하고 **다음**을 클릭하십시오.
 4. **새 데이터 세트 연결**을 클릭하고 다음 값을 입력하십시오.
    - 이벤트: sensorData
    - 특성: d.ay
    - 이름: Accel. Y
    - 유형: Float
    - 단위: gs
 5. **다음**을 클릭하십시오.
 6. 경로 A만: 전화를 흔들어서 새 카드의 라이브 가속도계 데이터를 보십시오.
보드 및 카드 작성에 대한 자세한 정보는 [보드 및 카드를 사용하여 실시간 데이터 시각화](/docs/services/IoT/data_visualization.html#boards_and_cards)를 참조하십시오. 

## 다음에 수행할 작업
{: @whats_next}  
다음 안내서를 계속하거나 관심 있는 다른 주제로 건너뛰십시오.
- 경로 A: 요구사항에 맞게 컨베이어 벨트 앱을 수정하십시오.  
기술적 세부사항은 다음을 참조하십시오.
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Node.js clienmt 라이브러리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **참고:** Bluemix는 이제 IBM Cloud입니다. 세부사항은 [IBM Cloud 블로그](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")를 체크아웃하십시오. 
 
- 경로 B: 요구에 맞게 Raspberry Pi 설정을 수정하십시오.  
기술적 세부사항은 다음을 참조하십시오.
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}
- [안내서 2: 기본 실시간 규칙 및 조치 사용](getting-started-iot-rules.html)  
컨베이어 벨트를 설정하고 {{site.data.keyword.iot_short_notm}}에 연결하며 일부 데이터를 전송했으므로, 규칙 및 조치를 사용하여 해당 데이터가 작동되도록 합니다.

**참고:** Bluemix는 이제 IBM Cloud입니다. 세부사항은 [IBM Cloud 블로그](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")를 체크아웃하십시오. 

- [안내서 3: 디바이스 데이터 모니터링](getting-started-iot-monitoring.html)  
하나 이상의 디바이스에 연결했고 디바이스 데이터를 잘 이용하기 시작했으므로 디바이스 콜렉션 모니터링을 시작합니다.
- [안내서 4: 많은 디바이스 시뮬레이션](getting-started-iot-large-scale-simulation.html)  
경로 A의 컨베이어 벨트 샘플 앱을 사용하면 하나 이상의 컨베이어 벨트 디바이스를 수동으로 시뮬레이션할 수 있습니다. 이 안내서를 사용하면 디바이스가 많이 있는 시뮬레이션된 환경을 설정할 수 있습니다.
- [{{site.data.keyword.iot_short_notm}}에 대해 자세히 보기](/docs/services/IoT/iotplatform_overview.html)
- [{{site.data.keyword.iot_short_notm}} API에 대해 자세히 보기](/docs/services/IoT/reference/api.html)
