---

copyright:
 years: 2015, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}

{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 디바이스용 HTTP Messaging API
{: #api}


## 디바이스용 HTTP Messaging API 문서에 액세스
{: #rest_messaging_api}

{{site.data.keyword.iot_short_notm}} HTTP Messaging API 문서에 액세스하려면 [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}을 참조하십시오.


## 클라이언트 연결
{: #client_connections}

{{site.data.keyword.iot_short_notm}}에서 디바이스에 클라이언트를 연결하는 방법과 클라이언트 보안에 대한 정보는 [{{site.data.keyword.iot_short_notm}}에 애플리케이션, 디바이스 및 게이트웨이 연결](../reference/security/connect_devices_apps_gw.html)을 참조하십시오.

## 이벤트 공개 
{: #event_publication}

MQTT 메시징 프로토콜의 사용 외에도 HTTP REST API 명령을 사용하여 {{site.data.keyword.iot_short_notm}}에 HTTP를 통해 이벤트를 공개하도록 디바이스를 구성할 수도 있습니다.

다음 URL 중 하나를 사용하여 {{site.data.keyword.iot_short_notm}}에 연결된 디바이스에서 ``POST`` 요청을 제출하십시오.

### 이벤트 공개를 위한 비보안 POST 요청

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### 이벤트 공개를 위한 보안 POST 요청

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**참고:** 기본 SSL 포트인 443 포트가 보안 HTTP API 호출에 대해 지정될 수도 있습니다.

### 인증

모든 요청에는 권한 부여 헤더가 포함되어야 합니다. 기본 인증은 지원되는 유일한 메소드입니다. 디바이스가 {{site.data.keyword.iot_short_notm}} HTTP REST API를 통해 HTTP 요청을 작성할 때는 다음 인증 정보가 필요합니다.

|인증 정보|필수 입력|
|:---|:---|
|사용자 이름|`use-token-auth`
|비밀번호|디바이스를 등록할 때 자동으로 생성하거나 수동으로 지정한 인증 토큰입니다.


### Content-Type 요청 헤더

컨텐츠가 JSON이 아닌 경우 `Content-Type` 요청 헤더는 요청과 함께 제공되어야 합니다. 다음 표에는 지원되는 유형이 {{site.data.keyword.iot_short_notm}} 내부 형식으로 맵핑되는 방법이 표시되어 있습니다.

|Content-Type 헤더|{{site.data.keyword.iot_short_notm}}의 형식|
|:---|:---|
|text/plain|"text"
|application/json|"json"
|application/xml |"xml"
|application/octet-stream|"bin"


## 명령 수신
{: #receive_commands}

MQTT 메시징 프로토콜의 사용 외에도 HTTP Messaging API 명령을 사용하여 HTTP를 통해 {{site.data.keyword.iot_short_notm}}에서 명령을 수신하도록 디바이스를 구성할 수도 있습니다. 디바이스는 그 자체에 전달되는 명령을 수신할 수 있습니다.

다음 URL 중 하나를 사용하여 {{site.data.keyword.iot_short_notm}}에 연결된 디바이스에서 ``POST`` 요청을 제출하십시오.

### 명령 수신을 위한 비보안 POST 요청

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

### 명령 수신을 위한 보안 POST 요청

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/commands/<var class="keyword varname">command</var>/request</code></pre>

**참고:** 기본 SSL 포트인 443 포트가 보안 HTTP API 호출에 대해 지정될 수도 있습니다.

명령을 대기하는 시간의 최대수(초)를 표시하는 정수를 지정하도록 HTTP 요청의 본문에 매개변수 *waitTimeSecs*를 선택적으로 포함시킬 수 있습니다.
<pre class="pre"><code class="hljs">{"waitTimeSecs": 5} </code></pre>


**중요 참고사항:**
- *waitTimeSecs*의 값은 0 - 3600초 범위의 정수여야 합니다. 기본값은 0입니다.
- 명령을 수신하려면 {command} 컴포넌트에 대해 "any" 와일드카드 문자(+)를 사용하십시오. 와일드카드 문자가 사용되면 명령 ID가 응답 헤더 필드 *X-commandId*에 포함됩니다.
- HTTP 응답 상태 코드가 200인 경우, 명령 데이터는 응답의 본문에 포함됩니다. 컨텐츠 유형을 찾으려면 응답 헤더 필드 *Content-Type*을 검토하십시오.
- HTTP 응답 상태 코드가 204인 경우, 명령 데이터를 사용할 수 없습니다.


디바이스 개발에 대한 정보는 [{{site.data.keyword.iot_short_notm}}에서 디바이스 개발](../devices/device_dev_index.html)을 참조하십시오.
