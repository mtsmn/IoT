---

copyright:
years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 디바이스용 HTTP API
{: #api}

{{site.data.keyword.iot_full}}에서 사용할 수 있는 두 가지 유형의 HTTP API가 있습니다. HTTP REST API를 사용하여 디바이스를 작성, 업데이트, 나열 및 삭제할 수 있습니다. HTTP Messaging API를 사용하여 디바이스의 이벤트 정보를 클라우드로 보내고 클라우드의 애플리케이션에서 명령을 받을 수 있습니다. 

## 클라이언트 연결
{: #client_connections}

{{site.data.keyword.iot_short_notm}}에서 디바이스에 클라이언트를 연결하는 방법과 클라이언트 보안에 대한 정보는 [{{site.data.keyword.iot_short_notm}}에 애플리케이션, 디바이스 및 게이트웨이 연결](../reference/security/connect_devices_apps_gw.html)을 참조하십시오.

**참고:** HTTP 포트 1883은 기본적으로 사용되지 않습니다. 기본 설정 변경에 대한 정보는 [보안 정책 구성](../reference/security/set_up_policies.html#set_up_policies.md)을 참조하십시오. 

## 인증

모든 인증 요청에는 권한 부여 헤더가 포함되어야 합니다. 기본 인증은 지원되는 유일한 메소드입니다. 디바이스가 {{site.data.keyword.iot_short_notm}} HTTP REST API를 통해 HTTP 요청을 작성할 때는 다음 인증 정보가 필요합니다.

|인증 정보|필수 입력|
|:---|:---|
|사용자 이름|`use-token-auth`
|비밀번호|디바이스를 등록할 때 자동으로 생성하거나 수동으로 지정한 인증 토큰입니다.


## Content-Type 요청 헤더

`Content-Type` 요청 헤더는 요청과 함께 제공되어야 합니다. 다음 표에는 지원되는 유형이 {{site.data.keyword.iot_short_notm}} 내부 형식으로 맵핑되는 방법이 표시되어 있습니다.

|Content-Type 헤더|{{site.data.keyword.iot_short_notm}}의 형식|
|:---|:---|
|text/plain|"text"
|application/json|"json"
|application/xml |"xml"
|application/octet-stream|"bin"

## 서비스 품질(QoS)

MQTT 서비스 품질(QoS) "최대 한 번" 전달 서비스 레벨 0과 유사하게 HTTP REST 메시징은 비지속적 메시지 전달을 제공하지만, 이는 요청이 올바른지와 HTTP 응답을 전송하기 전에 이를 서버에 전달할 수 있는지를 유효성 검증합니다. HTTP 상태 코드 200이 포함된 응답은 메시지가 서버에 전달되었는지 확인합니다. "최대 한 번" MQTT 서비스 품질(QoS) 레벨 또는 HTTP 등가물을 사용하여 이벤트 메시지를 전달하는 경우, 디바이스 및 애플리케이션은 전달을 보장하기 위한 재시도 로직을 구현해야 합니다.

MQTT 프로토콜 및 {{site.data.keyword.iot_short_notm}}의 서비스 품질(QoS) 레벨에 대한 자세한 정보는 [MQTT 메시징](../reference/mqtt/index.html)을 참조하십시오.

HTTP Messaging API를 사용하여 디바이스의 이벤트를 클라우드에 공개하고 클라우드의 애플리케이션에서 명령을 받을 수 있습니다. 

## 이벤트 공개
{: #event_publication}

MQTT 메시징 프로토콜의 사용 외에도 HTTP Messaging API 명령을 사용하여 HTTP를 통해 {{site.data.keyword.iot_short_notm}}에 이벤트를 공개하도록 디바이스를 구성할 수도 있습니다. 

다음 URL 중 하나를 사용하여 {{site.data.keyword.iot_short_notm}}에 연결된 디바이스에서 ``POST`` 요청을 제출하십시오.

### 이벤트 공개를 위한 비보안 POST 요청

<pre class="pre"><code class="hljs">http://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:1883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

### 이벤트 공개를 위한 보안 POST 요청

<pre class="pre"><code class="hljs">https://<var class="keyword varname">orgId</var>.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/<var class="keyword varname">typeId</var>/devices/<var class="keyword varname">deviceId</var>/events/<var class="keyword varname">eventId</var></code></pre>

**참고:** 기본 SSL 포트인 443 포트가 보안 HTTP API 호출에 대해 지정될 수도 있습니다.

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

{{site.data.keyword.iot_short_notm}} HTTP REST API에 액세스하려면 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}를 참조하십시오. 

{{site.data.keyword.iot_short_notm}} HTTP Messaging API에 액세스하려면 [{{site.data.keyword.iot_short_notm}} HTTP Messaging API ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html){: new_window}를 참조하십시오. 

디바이스 개발에 대한 정보는 [{{site.data.keyword.iot_short_notm}}에서 디바이스 개발](../devices/device_dev_index.html)을 참조하십시오.

## 보안 POST 요청의 예

다음 예제는 HTTP 프로토콜을 사용하여 디바이스의 메시지를 {{site.data.keyword.iot_short_notm}}에 전송합니다. 애플리케이션은 메시지가 공개된 주제를 구독하여 메시지를 수신합니다. 다음 표에서는 디바이스에 대한 정보를 제공합니다.  

|매개변수|값|
|:---|:---|
|조직 ID |myOrgID
|디바이스 유형| TestDevices
|디바이스 ID | TestPublishEvent
|인증 방법|use-token-auth
|인증 토큰|passw0rd


다음 예제에서는 curl을 사용하여 *TestMessage*라고 하는 이벤트를 디바이스 *TestPublishEvent*에서 {{site.data.keyword.iot_short_notm}}으로 HTTP 프로토콜을 통해 전송합니다.  

```
curl -v -X POST -H "Content-Type: application/json" -u "use-token-auth:passw0rd" -d @message.txt  https://myOrgID.messaging.internetofthings.ibmcloud.com:8883/api/v0002/device/types/TestDevices/devices/TestPublishEvent/events/TestMessage
```

메시지는 *message.txt*라고 하는 텍스트 파일에 포함되어 있습니다. *message.txt* 파일의 컨텐츠는 다음 예에 표시되어 있습니다.  
```
{ "d": { "myName": "Publish Test","cputemp": 46,"sine": -10,"cpuload": 1.45 }  } 
```
메시지는 주제 *iot-2/evt/TestMessage/fmt/json*에 공개됩니다. 주제 필터 *iot-2/type/+/id/+/evt/+/fmt/+*을 구독하는 *testSubscriber*라고 하는 애플리케이션이 작성됩니다. 주제 필터는 메시지가 공개되는 주제와 일치됩니다. 

[python 라이브러리 ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://github.com/ibm-watson-iot/iot-python/tree/master/samples/simpleApp)에서 제공하는 python 샘플 *simpleApp.py*를 사용하면 주제 필터 *iot-2/type/+/id/+/evt/+/fmt/+*를 구독하도록 애플리케이션을 구성하는 데 도움이 될 수 있습니다.  

다음의 python 스크립트는 메시지가 공개된 주제 문자열과 일치하는 주제 필터를 구독합니다. 

```
python simpleApp.py -o "myOrgID" -i "testSubscriber" -k "a-myOrgID-jg95dkpscn" -t "L(r4gfPa3bLP2kz4Nb"
```
여기서,

- **-o**는 조직 이름입니다. 
- **-i**는 애플리케이션의 이름입니다. 
- **-k**는 애플리케이션이 작성될 때 생성되는 API 키입니다.   
- **-t**는 애플리케이션이 작성될 때 생성되는 인증 토큰입니다.    


다음 예제는 *TestPublishEvent* 디바이스에서 공개된 메시지의 수신에 대한 성공적인 응답을 표시합니다.  
```
=============================================================================
Timestamp                        Device                        Event
2017-12-06 15:09:40,438   ibmiotf.application.Client  INFO    Connected successfully: a:myOrgID:testSubscriber
=============================================================================
2017-12-06T15:09:59.691290+00:00 TestDevices:TestPublishEvent  TestMessage: {"d": {"myName": "Publish Test", "cputemp": 46, "sine": -10, "cpuload": 1.45}}
2017-12-06T15:10:01.056000+00:00 TestDevices:TestPublishEvent  Connect 199.999.99.99
2017-12-06T15:10:01.446000+00:00 TestDevices:TestPublishEvent  Disconnect 199.999.99.99 (None)

```

## 마지막 이벤트 캐시
{: #last-event-cache}

마지막 이벤트 캐시를 사용하면 연결된 디바이스에 의해 {{site.data.keyword.iot_short_notm}}에 전송된 마지막 이벤트에 대한 정보를 저장할 수 있습니다. [API ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}를 사용하면 디바이스에 대한 특정 event-id의 마지막으로 기록된 값을 검색하거나 특정 디바이스에서 보고하는 각 event-id의 마지막으로 기록된 값을 검색할 수 있습니다.  

### 마지막 이벤트 캐시 구성

마지막 이벤트 캐시 기능은 기본적으로 사용되지 않지만, 다음과 같은 방법으로 이 기능을 사용할 수 있습니다.  

-	API를 사용하여 *enabled* 매개변수를 **true**로 설정하십시오. 
-	{{site.data.keyword.iot_short_notm}} 대시보드의 *설정* 페이지에서 **LEC 사용**을 **켜기(On)**로 설정하십시오. 

이벤트 데이터가 캐시에 저장되는 기간은 TTL(Time To Live) 값으로 지정됩니다. 특정 이벤트의 마지막 이벤트 데이터는 기본적으로 7일 동안 저장됩니다. API를 사용하여 **ttlDays** 매개변수를 업데이트하거나 {{site.data.keyword.iot_short_notm}} 대시보드의 *설정* 페이지에서 **이벤트 데이터 TTL** 필드에 대한 값을 선택하여 지속 기간을 변경할 수 있습니다. 

Lite 플랜의 경우에는 최소 1일에서 최대 7일 동안 정보를 저장할 수 있습니다. 기타 플랜의 경우에는 최소 1일에서 최대 45일 동안 정보를 저장할 수 있습니다. 

다음 정보를 사용하면 API를 사용하여 마지막 이벤트 캐시 설정을 구성하는 데 도움이 됩니다. 

조직에 대해 마지막 이벤트 캐시의 현재 구성을 검색하려면 다음 API를 사용하십시오. 

```
    GET /api/v0002/config/lec
```   

   
다음 예제는 GET 메소드에 대한 성공적인 응답을 표시합니다.

```
{
    "enabled": false,
    "ttlDays": 7
}
```
조직의 사용자 또는 애플리케이션에는 이 엔드포인트에 액세스할 수 있는 권한이 부여됩니다.  

{{site.data.keyword.iot_short_notm}} 관리자에 의해 마지막 이벤트 캐시를 조직에 사용할 수 없는 경우에는 다음의 HTTP 403 FORBIDDEN 상태 코드가 GET 메소드에 대한 응답으로 리턴됩니다. 

```
{
    "exception": {
        "id": "CUDCS0105E",
        "properties": ["myOrgID"]
    },
    "message": "CUDCS0105E: Last event cache is disabled for this organization. For more information, contact your support team."
}
```

마지막 이벤트 캐시가 사용되지 않는 경우에는 HTTP 404 상태 코드가 GET 메소드에 대한 응답으로 리턴됩니다. 조직에 대해 마지막 이벤트 캐시 기능을 사용하려면 다음 API를 사용하십시오. 

```
    PUT /api/v0002/config/lec
```

이 경우 다음의 예제 페이로드를 사용하십시오.  
```
{
    "enabled": true,
    "ttlDays": 5
}
```
여기서, 

- **enabled**는 필수이며, 조직에 대해 마지막 이벤트 캐시가 사용되는지 여부를 지정합니다. 값은 부울 값이어야 합니다. 

- **ttlDays**는 선택사항이며, 조직의 마지막 이벤트 캐시에서 이벤트가 보관되는 일 수를 지정합니다. 값은 1 - 45 사이의 정수(경계값 포함)여야 합니다. 
   
 
관리자, 운영자 사용자 또는 운영자 애플리케이션에만 이 엔드포인트에 액세스할 수 있는 권한이 부여됩니다. 매개변수 또는 JSON이 올바르지 않은 경우에는 HTTP 401 BAD REQUEST 상태 코드가 PUT 메소드에 대한 응답으로 리턴됩니다.  

**참고:** 구성 변경이 완료되는 데는 최대 30초 정도 걸릴 수 있습니다. 

### 마지막 이벤트 캐시에서 정보 검색

API를 사용하면 디바이스에서 전송한 마지막 이벤트를 검색할 수 있습니다. 디바이스의 실제 위치나 사용 상태와 무관하게 디바이스 상태를 검색할 수 있습니다. 특정 디바이스에 대한 이벤트 ID의 마지막으로 기록된 값 또는 특정 디바이스가 보고한 각 이벤트 ID의 마지막으로 기록된 값을 검색할 수 있습니다. 디바이스의 마지막 이벤트 데이터는 최대 45일 전에 발생한 특정 이벤트에 대해서만 검색할 수 있습니다.

특정 이벤트 ID의 최근 값을 요청하려면 다음 API 요청을 사용하십시오. 이 요청은 “power” 이벤트 ID의 마지막으로 기록된 값을 리턴합니다.

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events/power
```

응답은 다음 JSON 형식으로 리턴됩니다.

```
{
    "deviceId": "<device-id>",
    "eventId": "power",
    "format": "json",
    "payload": "eyJzdGF0ZSI6Im9uIn0=",
    "timestamp": "2016-03-14T14:12:06.527+0000",
    "typeId": "<device-type>"
}
```

**참고:** API 응답이 JSON 형식일 때 이벤트 페이로드를 임의 형식으로 쓸 수 있습니다. 마지막 이벤트 캐시 API에 의해 리턴된 페이로드는 base64로 인코딩됩니다.

디바이스가 보고한 각 이벤트 ID의 최근 값을 요청하려면 다음 API 요청을 사용하십시오.

```
GET /api/v0002/device/types/<device-type>/devices/<device-id>/events
```

응답에는 디바이스가 보낸 모든 이벤트 ID가 포함됩니다. 다음 예에서 “power” 및 “temperature” 이벤트에 대한 값이 리턴됩니다.

```
[
    {
        "deviceId": "<device-id>",
        "eventId": "power",
        "format": "json",
        "payload": "eyJzdGF0ZSI6Im9uIn0=",
        "timestamp": "2016-03-14T14:12:06.527+0000",
        "typeId": "<device-type>"
    },
    {
        "deviceId": "<device-id>",
        "eventId": "temperature",
        "format": "json",
        "payload": "eyJpbnRlcm5hbCI6MjIsICJleHRlcm5hbCI6MTZ9",
        "timestamp": "2016-03-14T14:17:44.891+0000",
        "typeId": "<device-type>"
    }
]
```

{{site.data.keyword.iot_short_notm}} 마지막 이벤트 캐시 API에 액세스하려면 [{{site.data.keyword.iot_short_notm}} HTTP 정보 관리 API ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/info-mgmt.html#!/Last_Event_Cache){: new_window}를 참조하십시오. 
