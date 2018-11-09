---
copyright:
  years: 2016, 2018
lastupdated: "2018-02-23"
---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 비용 청구

유료 {{site.data.keyword.iot_full}} 서비스 플랜('Lite' 이외의 플랜)은 1개월의 기간 동안 교환된 메가바이트의 개념을 기반으로 합니다. 이 문서에는 {{site.data.keyword.iot_short_notm}}이 데이터를 측정하여 서비스 사용 비용을 판별하는 사용량 정보를 작성하는 방법이 설명되어 있습니다.  사용량 정보를 사용하여 디바이스, 애플리케이션 및 게이트웨이의 디자인 및 수를 기준으로 {{site.data.keyword.iot_short_notm}} 사용 비용의 근사치를 계산할 수 있습니다.

교환된 데이터의 각 메가바이트 비용에 대한 정보는 필요한 지역의 {{site.data.keyword.Bluemix_notm}} 카탈로그에서 {{site.data.keyword.iot_short_notm}} 서비스를 참조하십시오. 

[가격 계산기 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://iot-cost-calculator.ng.bluemix.net/)를 사용하여 {{site.data.keyword.iot_short_notm}} 서비스의 비용을 계산할 수 있습니다.

다음 항목이 측정되고 사용량에 따라 청구됩니다. 

## 교환되는 데이터
*교환되는 데이터* 계산에는 HTTP API를 사용하여 애플리케이션이 교환하는 데이터뿐만 아니라 MQTT 또는 HTTP 메시징을 사용하여 애플리케이션, 디바이스 및 게이트웨이에서 교환되는 계정 데이터가 포함됩니다.

### 교환되는 MQTT 데이터
MQTT 사용 시 TCP 스트림 내용이 *교환되는 데이터*에 포함됩니다.  MQTT 메시지의 페이로드는 MQTT 프로토콜을 사용하여 교환되는 데이터의 누적에 포함됩니다.  메시지가 구독자에게 전달될 때만이 아니라 메시지가 공개될 때도 페이로드 데이터 크기가 누적됩니다.

MQTT 프로토콜은 가능한 한 경량으로 디자인됩니다.  *교환되는 데이터* 누적 시 {{site.data.keyword.iot_short_notm}}에 MQTT 패킷의 크기가 포함되지만 경량 프로토콜이므로 이 오버헤드는 낮습니다.  다음 표에는 각 MQTT 패킷의 최소 크기가 표시됩니다.

|MQTT 패킷                    |최소 크기 바이트  |변수 및 연관된 최소 크기(바이트)|
|-------------------------------|--------------------|-------------------------------------------------|
|CONNECT                        |56                  |클라이언트 ID(d:xxxxxx:t:i인 경우 12), 주제(0) 및 설정된 경우 메시지(0), 사용자 이름(디바이스의 경우 14 - 'use-token-auth') 및 비밀번호(자동 생성된 경우 18)|
|CONNACK                        |4                   |없음|
|PUBLISH                        |21                  |주제 이름(iot-2/evt/i/fmt/f인 경우 17) 및 페이로드(0이 최소값임).  인바운드 및 아웃바운드 PUBLISH 패킷 둘 다에 적용됩니다.|
|PUBACK                         |4                   |없음|
|PUBREC                         |4                   |없음|
|PUBREL                         |4                   |없음|
|PUBCOMP                        |4                   |없음|
|SUBSCRIBE                      |26                  |주제 필터(iot-2/evt/i/fmt/f인 경우 19)와 QoS(1)의 배수가 포함될 수 있음|
|SUBACK                         |5                   |SUBSCRIBE의 모든 주제 필터에 대해 (1)|
|UNSUBSCRIBE                    |23                  |다중 주제 필터(iot-2/evt/i/fmt/f의 경우 19)가 포함될 수 있음|
|UNSUBACK                       |4                   |없음|
|PINGREQ                        |2                   |없음|
|PINGRESP                       |2                   |없음|
|DISCONNECT                     |2                   |없음|

TLS를 사용하는 경우 보안 핸드쉐이크도 고려됩니다. 핸드쉐이크는 약 8KB입니다. 따라서 가능한 한 오래 MQTT 연결을 열어두는 것이 비용 면에서 효율적입니다. 열린 연결은 클라이언트에 대해 고유하고 지정된 활성 유지 기간에 의존하는 활성 유지 간격당 PINGREQ 및 PINGRESP 오버헤드(4바이트)만 발생시킵니다.  인증서가 어느 방향으로도 전송되지 않으므로 기존 TLS 세션을 사용하여 TLS 연결을 다시 열면 교환되는 바이트 수가 줄어듭니다.  많은 TLS 클라이언트가 기본적으로 이를 지원하지만 세션의 수명은 제한되어 있습니다.  클라이언트 인증서를 사용하면 클라이언트 인증서의 크기에 따라 TLS 핸드쉐이크의 크기가 늘어납니다. 

TLS 레코드 및 TCP와 IP 오버헤드는 고려되지 않습니다.

**참고** - {{site.data.keyword.iot_short_notm}} 대시보드를 사용하여 디바이스를 보는 경우, 대시보드에 라이브 이벤트가 표시될 수 있도록 MQTT 구독이 작성됩니다.  또한 이 MQTT 구독은 교환되는 데이터의 총 양에 포함됩니다.

### 교환되는 HTTP 메시징 데이터
HTTP 메시징을 사용하면, 이벤트를 공개하거나 명령을 수신하는 경우에 메시지 페이로드가 *교환되는 데이터* 누적에 포함됩니다.

MQTT의 경우와 같이 HTTP 오버헤드가 계수됩니다.  HTTP 오버헤드는 MQTT 오버헤드보다 훨씬 큽니다. 요청당 HTTP 오버헤드는 약 300바이트입니다. 메시지 페이로드 크기도 추가해야 합니다.

TLS 사용 시 보안 핸드쉐이크가 고려됩니다.  핸드쉐이크는 약 8KB입니다.  따라서 가능한 한 오래 HTTPS 연결을 열어두는 것이 비용 면에서 효율적입니다.  열린 연결은 *교환되는 데이터* 면에서 추가 오버헤드를 발생시키지 않습니다.  인증서가 어느 방향으로도 전송되지 않으므로 기존 TLS 세션을 사용하여 TLS 연결을 다시 열면 교환되는 바이트 수가 줄어듭니다.  많은 TLS 클라이언트가 기본적으로 이를 지원하지만 세션의 수명은 제한되어 있습니다.  클라이언트 인증서를 사용하면 클라이언트 인증서의 크기에 따라 TLS 핸드쉐이크의 크기가 늘어납니다.

TLS 레코드 및 TCP와 IP 오버헤드는 고려되지 않습니다.

### 교환되는 HTTP API 데이터
애플리케이션이 임의 API를 호출하면 요청과 응답 본문 둘 다 *교환되는 데이터*에 포함됩니다.  각각의 크기는 API에 따라 크게 다를 수 있습니다.

MQTT 및 HTTP 메시징과 달리, 교환되는 HTTP API 데이터에서는 HTTP 오버헤드와 TLS 오버헤드 둘 다 고려되지 않습니다.

**참고** - {{site.data.keyword.iot_short_notm}} 대시보드 사용 시 HTTP API는 대시보드에서 디바이스, 디바이스 유형 및 디바이스 연결 로그를 포함한 정보를 나열하는 데 사용됩니다.  이러한 HTTP API 호출은 *교환되는 데이터*에 포함됩니다.

<!-- ## Data Analyzed
The *data analyzed* calculation measures event data that is processed by the rules engine within the platform.  Data is considered processed by the rules engine when device events are evaluated by one or more rules, based on a specific device and event type. 
## Edge Data Analyzed
The *edge data analyzed* calculation measures event data that is processed on a gateway device by the {{site.data.keyword.iot_short_notm}} Edge Analytics Agent.  Data is considered processed by the edge agent when device events are evaluated by one or more edge rules, based on a specific device and event type.  -->
