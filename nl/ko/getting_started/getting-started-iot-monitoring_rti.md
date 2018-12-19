---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 안내서 3: 디바이스 데이터 모니터링
하나 이상의 디바이스가 연결되어 있으므로 실시간으로 디바이스 데이터 모니터링을 시작합니다.
{:shortdesc}

## 개요 및 목표
{: #overview}  

이 안내서에서 {{site.data.keyword.Bluemix}}에 모니터링 애플리케이션을 배치하여 디바이스의 데이터를 봅니다.

안내서 1과 같이 다음 경로 중 하나 또는 둘 다를 따를 수 있습니다.
- 경로 A: [1A단계 - 모니터링 웹 애플리케이션 배치 및 연결](#deploy_app)  
경로 A를 따라 조직에서 실행 중인 컨베이어 벨트 디바이스를 모니터하는 미리 준비된 앱을 배치합니다.
- 경로 B: [1B단계 - 위젯 라이브러리를 사용하여 모니터링 사용자 인터페이스 작성](#widget-library)
더 복잡한 경로 B는 위젯 라이브러리를 도입하여 일부 기본 사용자 인터페이스를 작성할 수 있도록 합니다.

## 전제조건
{: #prereqs}  
시작하기 전에 다음 요구사항이 충족되는지 확인하십시오.

### 로컬 환경
- [Node.js ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://nodejs.org){: new_window} 버전 6.x 이상.
- 경로 A: [Angular CLI ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/angular/angular-cli){: new_window} 버전 1.x 이상.  
- [Cloud Foundry CLI ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
cf CLI를 사용하여 앱 및 서비스를 {{site.data.keyword.Bluemix_notm}}에 배치하십시오. 자세한 정보는 [Cloud Foundry CLI 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.cloudfoundry.org/cf-cli/){: new_window}를 참조하십시오.  
- 선택사항: [Git ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git-scm.com/downloads){: new_window}  
Git을 사용하여 코드 샘플을 다운로드하도록 선택하는 경우 [GitHub.com 계정 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com){: new_window}이 있어야 합니다. GitHub.com 계정 없이 코드를 압축 파일로 다운로드할 수도 있습니다.

### 기타 요구사항
또한 이벤트 이름이 `sensorData`이고 다음 특성이 포함된 메시지 페이로드가 있는 이벤트를 전송하는 디바이스 유형 `iot-conveyor-belt`의 연결된 디바이스가 있어야 합니다.
```
{
	"d": {
		"id": "belt1",
		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
디바이스 이벤트 및 메시징 형식에 대한 자세한 정보는 [이벤트 공개](/docs/services/IoT/devices/mqtt.html#publishing_events)를 참조하십시오.  
[안내서 1: {{site.data.keyword.iot_short_notm}} 및 시뮬레이션된 컨베이어 벨트 시작하기](getting-started-iot-conveyor.html)를 완료하면 모든 준비가 된 것입니다.  
{: tip}

## 1A단계 - 모니터링 웹 애플리케이션 배치 및 연결
{: #deploy_app}

공장 모니터링 샘플 앱은 RPM, 마지막 업데이트 날짜 및 디바이스 ID와 같은 이벤트 데이터의 서브세트와 함께 {{site.data.keyword.iot_short_notm}} 조직에 연결된 모든 iot-conveyor-belt 유형 디바이스를 나열합니다.

샘플 앱은 Node.js 클라이언트 라이브러리([https://github.com/ibm-watson-iot/iot-nodejs ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window})를 사용하여 빌드됩니다.

![Node.js 기반 모니터링 앱](images/app_monitor.png "Node.js 기반 모니터링 앱")

이 단계의 일부로 다음을 수행합니다.
- Cloud Foundry를 사용하여 GitHub 소스 샘플 모니터링 웹 애플리케이션을 배치합니다.
- API 키 및 인증 토큰을 사용하여 {{site.data.keyword.iot_short_notm}}에 연결하도록 샘플 앱을 구성합니다.
- 웹 애플리케이션을 사용하여 연결된 컨베이어 벨트 디바이스를 모니터합니다.  

### 모니터링 애플리케이션의 배치 및 연결을 위한 세부 단계
다음 단계를 통해 {{site.data.keyword.Bluemix_notm}}에서 앱을 작성하고 배치할 수 있습니다. 앱을 로컬로 실행하는 데 대한 정보는 GitHub에서 README 파일을 참조하십시오.
1. Node.js *공장 모니터링* 샘플 앱 GitHub 저장소를 복제하십시오.  
원하는 git 도구를 사용하여 다음 저장소를 복제하십시오.  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular
Git 쉘에서 다음 명령을 사용하십시오.
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular
  ```
2. 앱의 API 키 및 인증 토큰 조합을 작성하십시오.  
조직에 연결할 앱을 구성할 때 다음 단계가 필요합니다. 디바이스 등록에 대한 자세한 정보는 [애플리케이션 연결](/docs/services/IoT/platform_authorization.html)을 참조하십시오.  
 1. {{site.data.keyword.iot_short_notm}} 대시보드를 여십시오.
 2. **앱**을 연결하십시오.
 3. **API 키 생성**을 클릭하십시오.
 4. 나열된 API 키 및 인증 토큰 값을 복사하십시오.
 5. **시각화 애플리케이션**을 API 역할로 선택하십시오.  
**팁:** 애플리케이션에 기능을 추가하는 경우 더 높은 역할로 승격시켜야 합니다.
 6. 이 API 키 및 인증 토큰 조합을 쉽게 식별할 수 있도록 주석을 추가하십시오.
 7. **생성**을 클릭하십시오.
3. {{site.data.keyword.Bluemix_notm}}에 연결할 앱을 구성하십시오.
*guide-conveyor-ui-angular* 저장소의 루트로 이동하고 다음 컨텐츠가 포함된 `basicConfig.json` 파일을 작성하십시오.
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
매개변수 값을 {{site.data.keyword.Bluemix_notm}} 조직의 해당 값(작성한 조직 ID, API 키 및 인증 토큰)으로 바꾸십시오.  
예제:
```
 {   
  "org": "3v5whr",
  "apiKey": "a-3v5whr-jhkmsghlqv",
  "apiToken": "-q0MkPN2cNYB6+?ISk"
}
```
4. cloudfoundry CLI를 사용하여 {{site.data.keyword.Bluemix_notm}} 계정에 로그인하십시오.  
자세한 정보는 [Cloud Foundry CLI 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.cloudfoundry.org/cf-cli/){: new_window}를 참조하십시오.  
 명령행에서 다음 명령을 입력하십시오.  
  ```
cf login
  ```
 프롬프트가 표시되면 모니터링 샘플 앱을 배치할 조직과 영역을 선택하십시오.
5. 필요한 경우 cf api 명령을 실행하여 API 엔드포인트를 설정하십시오.   
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
6. 디렉토리를 샘플 앱이 있는 디렉토리로 변경하십시오.  
  ```
cd guide-conveyor-ui-angular
  ```
7. `npm install -g @angular/cli`를 실행하여 Angular CLI를 설치하십시오.
8. `npm install`을 실행하십시오.
9. `npm run push`를 실행하여 프로젝트를 빌드하고 조직으로 푸시하십시오.  
샘플 웹 애플리케이션이 {{site.data.keyword.Bluemix_notm}}에 배치됩니다.  
배치가 완료되면 앱이 실행 중임을 설명하는 메시지가 표시됩니다.   
예제:  
  ```
requested state: started
instances: 1/1
usage: 64M x 1 instances
urls: iotmonitoringcontrol-undertided-eng.mybluemix.net
last uploaded: Tue May 16 19:01:13 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     state     since                    cpu    memory     disk        details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
10. 웹 앱을 여십시오.  
{{site.data.keyword.Bluemix_notm}} 앱 대시보드에서 **앱 URL 방문**을 클릭하여 웹 애플리케이션을 여십시오.  
**라우트**를 클릭하여 애플리케이션 URL에 액세스하고 이를 관리하십시오.   
기본 URL은 다음과 같습니다.  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## 1B단계 - 위젯 라이브러리를 사용하여 모니터링 사용자 인터페이스 작성
{: #widget-library}

위젯 라이브러리 기반 샘플 앱에는 모터 속도 게이지, 가속도계 데이터 게이지 및 {{site.data.keyword.iot_short_notm}} 조직에 연결된 단일 iot-conveyor-belt 유형 디바이스의 데이터를 표시하는 모터 속도 다이어그램이 포함됩니다. 샘플 코드를 사용하여 {{site.data.keyword.iot_short_notm}} 연결 디바이스의 전체 프론트 엔드 애플리케이션을 빌드할 수 있습니다.

![위젯 라이브러리 기반 모니터링 앱](images/app_monitor_b.png "위젯 라이브러리 기반 모니터링 앱")

이 단계의 일부로 다음을 수행합니다.
- Cloud Foundry를 사용하여 GitHub 소스 샘플 웹 애플리케이션을 배치합니다.
- API 키 및 인증 토큰을 사용하여 {{site.data.keyword.iot_short_notm}}에 연결하도록 샘플 앱을 구성합니다.
- 디바이스 데이터를 게이지 및 그래프로 표시하도록 세 개의 사용자 인터페이스 위젯을 구성합니다.
- 웹 애플리케이션을 사용하여 연결된 컨베이어 벨트 디바이스를 모니터합니다.  

### 위젯 라이브러리를 사용하여 모니터링 사용자 인터페이스를 작성하기 위한 세부 단계
다음 단계를 통해 {{site.data.keyword.Bluemix_notm}}에서 앱을 작성하고 배치할 수 있습니다. 앱을 로컬로 실행하는 데 대한 정보는 GitHub에서 README 파일을 참조하십시오.
1. *위젯 라이브러리 모니터링* 샘플 앱 GitHub 저장소를 복제하십시오.  
원하는 git 도구를 사용하여 다음 저장소를 복제하십시오.  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui
Git 쉘에서 다음 명령을 사용하십시오.
```
git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui
```
2. 애플리케이션 종속 항목을 설치하십시오.  
*guide-conveyor-ui-html* 저장소의 루트로 이동하고 다음 명령을 실행하십시오.
```
npm install
```
3. 사용자 인터페이스를 생성하십시오.  
애플리케이션 사용자 인터페이스를 빌드하려면 각 사용자 인터페이스 컴포넌트에 대해 애플리케이션 index.html 파일에 JavaScript 코드로 위젯을 추가해야 합니다.  
각 위젯은 다음 JavaScript 매개변수를 사용합니다.  
`WIoTPWidget.CreateWIDGET_TYPE("ELEMENT_ID","EVENT_NAME", "DEVICE_TYPE", "DEVICE_ID", "PROPERTY" , {WIDGET_DEFAULT_OVERRIDE}, [WIDGET_SPECIFIC_SETTINGS])`

다음 표에는 매개변수에 대한 설명이 제공됩니다.

|매개변수 |설명 |    
| ----- | ---- |   
|WIDGET_TYPE |작성할 위젯의 유형입니다. 예: `Gauge` 또는 `Chart` |
|ELEMENT_ID |애플리케이션에 표시되는 위젯의 요소 ID입니다. 예: `RPM` |
|EVENT_NAME |표시할 특성이 포함된 디바이스 이벤트 이름입니다. 예: `sensorData` |
|DEVICE_TYPE |디바이스 유형입니다. 예: `iot-conveyor-belt` |
|DEVICE_ID |표시할 데이터를 제공하는 디바이스의 ID입니다. 예: `belt1` |
|PROPERTY |표시할 디바이스 메시지 페이로드 특성입니다. 예: `rpm` |
|WIDGET_DEFAULT_OVERRIDE |기본 설정을 대체할 위젯 구성 설정입니다.|
|WIDGET_SPECIFIC_SETTINGS |위젯에 대한 하나 이상의 추가 매개변수입니다. 예제를 참조하십시오. |

각 위젯 유형에 대한 세부사항은 뒤에 오는 예제와 [IoT Widgets GitHub 저장소 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/iot-widgets){: new_page}의 문서를 참조하십시오.
 1. RPM 게이지를 추가하십시오.  
이 게이지는 컨베이어 벨트 rpm을 최소값이 0이고 최대값이 5rpm인 게이지로 표시합니다.
    1. 편집을 위해 다음 템플리트를 여십시오. `public/index.html`  
    2. rpm 게이지 플레이스홀더를 찾으십시오. `<!--- place holder for rpm gauge  -->`
    3. 다음과 같이 표시된 고유 ID의 div 요소를 추가하십시오.
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. JavaScript 플레이스홀더를 찾으십시오. `/// Add your scripts here`
    4. rpm JavaScript 코드를 추가하십시오.  
예제:  
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iot-conveyor-belt", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. 가속 게이지를 추가하십시오.  
이 게이지는 가속도계 측정값을 -1과 1 사이의 측정값을 가진 게이지로 표시합니다.
    1. 가속도계 게이지 플레이스홀더를 찾으십시오. `<!--- place holder for accelerometer gauge  -->`
    2. 다음과 같이 표시된 고유 ID의 div 요소를 추가하십시오.
 ```html
 <div id="aygauge" ></div>
 ```  
    3. javascript 플레이스홀더를 찾으십시오. `/// Add your scripts here`
    4. 가속도계 JavaScript 코드를 추가하십시오.  
예제:   
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iot-conveyor-belt", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
          },
          show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. 모터 속도 차트를 추가하십시오.  
이 차트에는 모터 속도가 라인 다이어그램으로 표시됩니다.
    1. 모터 속도 게이지 플레이스홀더를 찾으십시오. `<!--- place holder for motor speed gauge  -->`
    2. 다음과 같이 표시된 고유 ID의 div 요소를 추가하십시오.
 ```html
 <div id="speedchart" ></div>
 ```  
    3. JavaScript 플레이스홀더를 찾으십시오. `/// Add your scripts here`
    4. 속도 차트 JavaScript 코드를 추가하십시오.  
예제:  
 ```javascript
 WIoTPWidget.CreateChart("speedchart ","sensorData", "iot-conveyor-belt", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. {{site.data.keyword.Bluemix_notm}}에 애플리케이션 배치  
 1. {{site.data.keyword.iot_short_notm}} 서비스 이름으로 manifest.yml 파일을 업데이트하십시오.  
예를 들어, {{site.data.keyword.iot_short_notm}} 서비스를 [안내서 1: 컨베이어 벨트 디바이스 연결](getting-started-iot-monitoring.html)의 일부로 배치한 경우 YOUR_PLATFORM_NAME은 `iotp-for-conveyor`입니다.
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. cloudfoundry CLI를 사용하여 {{site.data.keyword.Bluemix_notm}} 계정에 로그인하십시오.  
자세한 정보는 [Cloud Foundry CLI 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.cloudfoundry.org/cf-cli/){: new_window}를 참조하십시오.  
 명령행에서 다음 명령을 입력하십시오.  
   ```
 cf login
   ```
 프롬프트가 표시되면 모니터링 샘플 앱을 배치할 조직과 영역을 선택하십시오.
 5. 필요한 경우 cf api 명령을 실행하여 API 엔드포인트를 설정하십시오.   
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
 6. 디렉토리를 샘플 앱이 있는 디렉토리로 변경하십시오.  
   ```
 cd guide-conveyor-ui-html
   ```
 2. cf push 명령을 실행하여 애플리케이션을 {{site.data.keyword.Bluemix_notm}}로 푸시하십시오.  
애플리케이션에 고유 이름을 제공하십시오.
```
cf push YOUR_APP_NAME
```
5. 위젯 라이브러리 기반 웹 앱을 여십시오.  
{{site.data.keyword.Bluemix_notm}} 앱 대시보드에서 **앱 URL 방문**을 클릭하여 웹 애플리케이션을 여십시오.  
**라우트**를 클릭하여 애플리케이션 URL에 액세스하고 이를 관리하십시오.   
기본 URL은 다음과 같습니다.  
`https://YOUR_APP_NAME.mybluemix.net`

## 2단계 - 연결된 디바이스 보기
{: #view_devices}

웹 콘솔이 시작되고 실행되므로 연결된 컨베이어 벨트 디바이스를 볼 수 있습니다.
1. 웹 콘솔 **디바이스** 섹션에서 컨베이어 벨트가 나열되어 있고 올바른 RPM 및 실행 중 상태가 표시되는지 확인하십시오.
2. 컨베이어 벨트 디바이스의 RPM 값을 변경하고 값이 모니터링 앱에서 예상대로 업데이트되었는지 확인하십시오.


## 다음 항목
{: @whats_next}  
다음 안내서를 계속하거나 관심 있는 다른 주제로 건너뛰십시오.
- 경로 A: 요구에 맞게 모니터링 앱을 수정하십시오.  
기술적 세부사항은 다음을 참조하십시오.
 - [https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular/blob/master/README.md ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui-angular/blob/master/README.md){: new_window}
 - [Node.js 클라이언트 라이브러리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- 경로 B: 요구에 맞도록 위젯 라이브러리 앱을 수정하십시오.  
기술적 세부사항은 다음을 참조하십시오.
 - [https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui/blob/master/README.md ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/guide-conveyor/tree/master/ui/blob/master/README.md){: new_window}
- [안내서 4: 많은 디바이스 시뮬레이션](getting-started-iot-large-scale-simulation.html)  
많은 수의 자체 실행 시뮬레이터를 환경에 추가하여 기본 시뮬레이션을 확장하십시오. 이 확장을 통해 좀 더 현실적인 다중 디바이스 환경에서 이전 안내서의 기본 분석 및 모니터링을 테스트 드라이브할 수 있습니다.
- [{{site.data.keyword.iot_short_notm}}에 대해 자세히 보기](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [{{site.data.keyword.iot_short_notm}} API에 대해 자세히 보기](/docs/services/IoT/reference/api.html){:new_window}
