---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 안내서 3: 많은 수의 디바이스 시뮬레이션
첫 번째 안내서에서 하나 이상의 컨베이어 벨트를 수동으로 시뮬레이션하도록 기본 디바이스 시뮬레이터를 설정합니다. 이 안내서에서는 좀 더 현실적인 다중 디바이스 환경에서 분석 및 모니터링을 테스트하도록 사용자 환경에 많은 수의 자체 실행 시뮬레이터를 추가하여 이 시뮬레이션을 확장합니다.
{:shortdesc}

**중요:** 애플리케이션에서는 512MB의 메모리가 필요합니다. 이는 기본적으로 할당되는 양보다 많으며 {{site.data.keyword.Bluemix}} 평가판 계정 및 표준 계정을 포함하여 무료 평가판 계정에서 사용할 수 있는 양도 초과합니다. 구독 및 종량과금제 계정 보유자는 할당된 메모리를 512MB로 늘릴 수 있습니다. 무료 평가판 계정 보유자는 구독 또는 종량과금제 계정으로 업그레이드해야 합니다. {{site.data.keyword.Bluemix_notm}} 계정 유형에 대한 자세한 정보는 [계정 유형](/docs/pricing/index.html#pricing)을 참조하십시오.

**참고:** Bluemix는 이제 IBM Cloud입니다. 세부사항은 [IBM Cloud 블로그](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")를 체크아웃하십시오. 

## 개요 및 목표
{: #overview}

이 안내서에서는 [Node-RED ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://nodered.org){: new_window} "백엔드"를 사용하여 다중 컨베이어 벨트 디바이스를 시뮬레이션하도록 {{site.data.keyword.Bluemix_notm}} 애플리케이션을 설정합니다. 

애플리케이션에는 다음을 수행하는 세 가지 플로우가 있습니다.   
1. 다중 디바이스를 작성합니다.   
2. 동시 이벤트 공개를 시뮬레이션합니다.
3. 디바이스를 삭제합니다.   

![디바이스 작성 및 삭제](images/device_creation.png "디바이스 작성 및 삭제")

![디바이스 이벤트 시뮬레이션](images/device_event_simulation.png "디바이스 이벤트 시뮬레이션")

이 안내서에 있는 지시사항을 완료하여 다음을 수행합니다.
- Cloud Foundry를 사용하여 Node-RED 기반 및 웹훅 사용 디바이스 시뮬레이터 애플리케이션을 배치합니다.
- API 호출을 사용하여 디바이스를 작성하고 등록하며 디바이스 이벤트를 공개하고 디바이스를 삭제합니다.

**중요:** 디바이스 데이터를 동시에 {{site.data.keyword.iot_full}}에 전송하는 많은 디바이스를 시뮬레이션하면 많은 양의 데이터가 소비될 수 있습니다. 

## 전제조건
{: #prereqs}  
다음 계정 및 도구가 필요합니다.

* [{{site.data.keyword.Bluemix_notm}} 계정](https://console.ng.bluemix.net/registration/)에 제공되는 내용    
 - 512MB 초과 RAM   
 - 1GB 초과 디스크 할당량  
 - 두 개의 서비스 사용 가능
* [Cloud Foundry 명령 인터페이스(cf CLI) ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
{{site.data.keyword.Bluemix_notm}} 애플리케이션을 배치하고 관리하려면 cf CLI를 사용하십시오.
* 선택사항: [Git ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git-scm.com/downloads){: new_window}  
Git을 사용하여 코드 샘플을 다운로드하도록 선택하는 경우 [GitHub.com 계정 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com){: new_window}이 있어야 합니다. GitHub.com 계정 없이 코드를 압축 파일로 다운로드할 수도 있습니다.

다음 단계의 REST 호출에 대해 Mozilla에서 사용 가능한 REST 클라이언트 추가 기능 플러그인 또는 cURL을 사용할 수 있습니다.  
{: tip}

## 1단계 - 애플리케이션을 {{site.data.keyword.Bluemix_notm}}에 배치
{: #step1}

다음 단계를 수행하여 앱을 수동으로 작성하고 배치하십시오.   

1. *Lesson4* 샘플 앱 GitHub 저장소를 복제하십시오.  
원하는 git 도구를 사용하여 다음 저장소를 복제하십시오.  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
Git 쉘에서 다음 명령을 사용하십시오.
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
```
3. manifest.yml 파일을 편집하여 환경에 맞는 애플리케이션을 구성하십시오.  
편집할 내용:
 - 기존 {{site.data.keyword.iot_short_notm}} 서비스를 사용하려면 해당 서비스 이름을 반영하도록 `lesson4-simulate-iotf-service`의 모든 인스턴스를 업데이트하십시오. 예를 들어, 안내서 1에서 {{site.data.keyword.iot_short_notm}} 서비스를 사용하는 경우 서비스 이름에 `iotp-for-conveyor`를 사용하십시오.    
 - 디바이스 이름 및 호스트를 설정하십시오.   
애플리케이션 섹션에서 `name` 및 `host` 항목을 고유한 항목(예: `YOUR_NAME-lesson4-simulate`)으로 변경하십시오.   
**팁:** 앱에 액세스하는 데 사용하는 ROUTES URL은 `host` 항목에서 작성됩니다(예: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`).  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plan: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
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
6. {{site.data.keyword.Bluemix_notm}}에서 필수 서비스를 작성하십시오.   
 1. 다음 명령을 사용하여 cloudantNoSQLDB Lite 서비스를 작성하십시오.
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. 다음 명령을 사용하여 {{site.data.keyword.iot_short_notm}} 서비스를 작성하십시오.
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**중요:** 기존 {{site.data.keyword.iot_short_notm}} 서비스를 사용하고 있고 manifest.yml 파일을 적절하게 업데이트한 경우 기존 서비스 이름을 사용해야 합니다. 예를 들어, 안내서 1에서 cf 호출에 `lesson4-simulate-iotf-service` 대신 `iotp-for-conveyor`를 사용하십시오.
7. `cf push` 명령을 실행하여 프로젝트를 빌드하고 조직으로 푸시하십시오.  
샘플 애플리케이션이 {{site.data.keyword.Bluemix_notm}}에 배치됩니다.  
배치가 완료되면 앱이 실행 중임을 설명하는 메시지가 표시됩니다.   
예제:  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. 앱에 대한 서비스 인증 정보를 가져오십시오.
 1. 다음에서 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.  
 [https://bluemix.net ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://bluemix.net){: new_window}.
 2. 앱을 배치한 계정 및 영역을 선택하십시오.
 3. 메뉴에서 **앱**을 선택한 다음 **대시보드**를 선택하십시오.
 4. Cloud Foundry 앱에서 배치된 애플리케이션의 이름을 클릭하십시오.
 4. **연결**을 선택하십시오.
 5. 앱에서 사용하는 iotf-service를 찾고 **인증 정보 보기**를 클릭하십시오.  
 6. 서비스 인증 정보에서 `apiKey` 및 `apiToken`을 찾으십시오.   
API 키 및 인증 토큰은 앱 API를 사용하여 시뮬레이션된 디바이스를 작성하고 실행할 때 인증 정보로 사용됩니다.

## 2단계 - Node-RED 플로우 보안
{: #step2}

이 단계는 선택사항이지만 Node-RED 플로우를 보안하는 것이 좋습니다.

1. 권한 부여된 사용자만 액세스할 수 있도록 편집기를 보안하십시오.
2. Node-RED 플로우를 보안하려면 {{site.data.keyword.Bluemix_notm}} 대시보드로 이동하고 이전에 배치한 애플리케이션을 선택하십시오.
3. **런타임**을 클릭한 다음 **환경 변수**를 클릭하십시오.
4. 다음 사용자 정의 환경 변수 및 해당 값을 추가하십시오.
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. 플로우를 저장하십시오.

Node-RED 플로우가 보안되는 경우 REST API 명령을 사용하여 여러 디바이스를 작성하거나 삭제하거나 시뮬레이션하려면 기본 인증 중에 Node-RED 사용자 이름 및 비밀번호 인증 정보를 제공해야 합니다.

## 3단계 - 디바이스 작성 및 연결
{: #step3}

Node-RED 플로우 인터페이스 또는 애플리케이션 REST API를 사용하여 다음 태스크를 완료할 수 있습니다.

### Node-RED를 사용하여 디바이스 작성 및 연결  
여러 디바이스를 등록하려면 다음을 수행하십시오.  
1. 다음에서 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오.  
[https://bluemix.net ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://bluemix.net){: new_window}.
2. 앱을 배치한 계정 및 영역을 선택하십시오.
3. 메뉴에서 **앱**을 선택한 다음 **대시보드**를 선택하십시오.
4. Cloud Foundry 앱에서 배치된 애플리케이션의 **ROUTE** URL을 클릭하십시오.  
ROUTE_URL은 manifest.yml 파일에서 사용한 `host` 항목(`HOST.mybluemix.net`)에서 빌드됩니다.  
예제: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
Node-RED 인터페이스가 열립니다.
5. **Node-RED 플로우 편집기로 이동**을 클릭하십시오.
6. 플로우 편집기에서 **디바이스 유형 및 인스턴스** 탭을 선택하십시오.
7. 5개의 디바이스를 등록하려면 **Register 5 motorController devices**라고 레이블 지정된 삽입 노드를 클릭하십시오.

### REST API를 사용하여 디바이스 작성 및 연결  
여러 디바이스를 등록하려면 다음을 수행하십시오.  

1. 다음 URL에 대한 HTTP POST 요청을 작성하십시오. `ROUTE_URL/rest/devices`  
예제: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - 'Content-Type' 및 'Accept'를 'application/json'으로 설정  
 - 다음 JSON 페이로드 사용  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
여기서,  
    - numberDevices는 작성하고 등록할 디바이스 수입니다.
    - 선택사항: typeId는 디바이스가 등록될 디바이스 유형입니다. typeId가 제공되지 않으면 기본값은 "iotp-for-conveyor"로 설정됩니다. **팁:** 디바이스 유형 이름을 입력할 수 있지만 시리즈의 다른 안내서에서는 디바이스 유형 `iotp-for-conveyor`의 디바이스를 요구합니다. 다른 디바이스 유형을 사용하는 경우 안내서의 설정을 적절하게 수정해야 합니다.
    - 선택사항: authToken은 디바이스가 등록하는 데 사용하는 인증 토큰입니다.
    - 선택사항: chunkSize가 제공되지 않으면 기본값 500으로 설정됩니다. 'chunksize'는 더 작아야 하며 numberDevices의 요인이어야 합니다.
    - deviceName은 작성된 디바이스의 deviceID에 대한 패턴입니다.

## 4단계 - 디바이스 이벤트 시뮬레이션
{: #step4}

시뮬레이션된 디바이스가 {{site.data.keyword.iot_short_notm}}에 등록되어 있으므로 시뮬레이터를 실행하여 디바이스 이벤트 전송을 시작할 수 있습니다.

### Node-RED를 사용하여 디바이스 이벤트 시뮬레이션  
디바이스 이벤트를 전송하려면 다음을 수행하십시오.  
1. Node-RED 플로우 편집기에서 **여러 디바이스 시뮬레이션** 탭을 선택하십시오.
7. 5개의 디바이스를 시뮬레이션하려면 **Simulate 5 devices**라고 레이블 지정된 삽입 노드를 클릭하십시오.
 


### Rest API를 사용하여 디바이스 이벤트 시뮬레이션  
디바이스 이벤트를 전송하려면 다음을 수행하십시오.

1. 다음 URL에 대한 HTTP POST 요청을 작성하십시오. `ROUTE_URL/rest/runtest`  
예제: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - 'Content-Type' 및 'Accept'를 'application/json'으로 설정  
 - 다음 JSON 페이로드 사용   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
여기서,
    - numberDevices는 데이터를 시뮬레이션할 디바이스 수입니다.
    - numberEvents는 시뮬레이션된 각 디바이스가 전송하는 이벤트의 수입니다.
    - timeInterval은 이벤트의 간격(밀리초)입니다.
    - deviceType은 사용자가 작성한 시뮬레이션된 디바이스의 디바이스 유형입니다.
    - deviceName은 작성된 디바이스의 deviceID에 대한 패턴입니다.
 

## 5단계 - 디바이스 삭제
{: #deleting}

### Node-RED  
디바이스를 삭제하려면 다음을 수행하십시오.  
1. Node-RED 플로우 편집기에서 **디바이스 유형 및 인스턴스** 탭을 선택하십시오.
2. 5개의 디바이스를 삭제하려면 **Delete 5 devices**라고 레이블 지정된 삽입 노드를 클릭하십시오.



### Rest API  

1. 다음 URL에 대한 HTTP POST 요청을 작성하십시오. `ROUTE_URL/rest/deleteDevices`  
예제: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - 'Content-Type' 및 'Accept'를 'application/json'으로 설정  
 - 다음 JSON 페이로드 사용      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName": "belt"  
}</code></pre>
  


## 다음 항목
{: @whats_next}  
관심 있는 다른 주제로 건너뛰십시오.
- [안내서 2: 디바이스 데이터 모니터링](getting-started-iot-monitoring.html)  
하나 이상의 디바이스에 연결했고 디바이스 데이터를 잘 이용하기 시작했으므로 디바이스 콜렉션 모니터링을 시작합니다.
- [{{site.data.keyword.iot_short_notm}}에 대해 자세히 보기](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [{{site.data.keyword.iot_short_notm}} API에 대해 자세히 보기](/docs/services/IoT/reference/api.html){:new_window}
