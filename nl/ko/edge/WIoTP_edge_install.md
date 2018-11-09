---

copyright:
  years: 2018
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 디바이스에 {{site.data.keyword.iot_short_notm}} 에지 설치(미리보기)
{: #edge_install_device}

**중요:** {{site.data.keyword.iot_full}} 에지 기능은 제한된 미리보기 프로그램의 일부로서만 사용 가능합니다. 향후 업데이트에는 이 기능의 현재 버전과 호환 가능한 변경사항이 포함될 수 있습니다. 시도해 보고 [의견을 보내주십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}

{{site.data.keyword.iot_short_notm}} 에지 미리보기 시스템을 {{site.data.keyword.iot_short_notm}}에서 IBM Horizon이 관리하도록 하려면, 우선 이에 Horizon+WIoTP 소프트웨어를 설치한 후에 Horizon Exchange에 시스템을 등록하십시오. 이러한 단계를 사용하면 임시 테스트 환경의 미리보기 개발 저장소에서 애플리케이션을 사용할 수 있습니다. 

## 설치하기 전에

### Ubuntu 기반 에지 노드의 전제조건

- 아직 설치되지 않은 경우 Docker를 레지스트리에 추가: 

`wget -qO- https://download.docker.com/linux/ubuntu/gpg| apt-key add - && add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

- 기본 Horizon 패키지 추가(ANAX 에이전트)

```
wget -qO - http://pkg.bluehorizon.network/bluehorizon.network-public.key | apt-key add -
   cat <<EOF > /etc/apt/sources.list.d/bluehorizon.list
   deb [arch=$(dpkg --print-architecture)] http://pkg.bluehorizon.network/linux/ubuntu $(lsb_release -cs)-updates main
EOF
```

### Raspberry PI의 전제조건

- `/etc/apt/sources.list.d/bluehorizon.list`에 다음 행을 추가하십시오. 
```
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-testing main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie main
deb [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
deb-src [arch=armhf] http://pkg.bluehorizon.network/linux/raspbian jessie-updates main
```

- `/etc/apt/sources.list.d/docker.list`에 다음 행을 추가하십시오. 

`deb [arch=armhf] https://download.docker.com/linux/raspbian stretch edge`

- `docker-ce`를 설치하십시오. 

`apt-get update && apt-get install docker-ce`


## Horizon 및 WIoTP Horizon 패키지 설치

`apt-get update && apt-get install -y horizon-wiotp`

## 에지 노드 등록

에이전트 설정을 실행하십시오. 

프로덕션 환경의 경우에는 다음을 실행하십시오.
`wiotp_agent_setup --org <orgId> --deviceType <deviceType> --deviceId <deviceId> --deviceToken <deviceToken>`

이는 다음 단계를 실행합니다.
- WIoTP 에지 코어 IoT 워크로드를 구성합니다. 
- edge-mqttbroker 인증서를 생성합니다. 
- hzn 도구를 사용하여 WIoT Platform에 대해 노드를 등록합니다(자세한 정보: `hzn --help`). 

디바이스에 둘 이상의 IP 주소가 있는 경우에는 내부 인증서 생성을 위해 하나를 선택하도록 프롬프트가 표시됩니다. 이 IP는 나중에 에지 노드와 데이터 전송 디바이스 간의 연결을 테스트하는 데 사용됩니다. 

실행 중에 오류 메시지가 없는지 확인하십시오.

잠시 대기한 후에 에지 컴포넌트가 작성되었는지 확인하십시오. 

에지 컴포넌트 이미지가 작성되었는지 여부를 확인하십시오. `docker images`(TAG 버전은 다를 수 있음).

**참고**: 이미지 다운로드 및 컨테이너 시작 프로세스는 몇 분이 걸릴 수 있습니다. syslog 파일을 확인하여 로깅 세부사항을 보십시오. 

```
# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
wiotp-connect/edge/amd64/edge-connector           1.0.1               3d98b90aaa16        3 hours ago         31.7MB
wiotp-infomgmt/edge/amd64/edge-simulated-im       1.0.3               6af448fb2204        7 hours ago         27.6MB
wiotp-connect/edge/amd64/edge-mqttbroker          1.0.2               1973bc0f3d8a        7 hours ago         31.2MB
```

컨테이너가 시작되었는지 확인하십시오. `docker ps`

예제:
```
# docker ps
CONTAINER ID        IMAGE                                                   COMMAND             PORTS
7cbfc719cb71        wiotp-infomgmt/edge/amd64/edge-simulated-im:1.0.1       "/start.sh"
a40f926f78f3        wiotp-connect/edge/amd64/edge-connector:1.0.0           "/start.sh"         0.0.0.0:1883->1883/tcp, 0.0.0.0:8883->8883/tcp
22d8e82f09f3        wiotp-connect/edge/amd64/edge-mqttbroker:1.0.1          "/start.sh"
```

## 에지 노드에서 디바이스 시뮬레이션

mosquitto-clients를 사용하여 에지 노드에 연결된 디바이스를 시뮬레이션할 수 있습니다. 

`apt-get install mosquitto-clients`

에지 노드에서 다음 명령을 실행하십시오(OrgId, DeviceType, DeviceID, DeviceToken 및 EdgeNodeIP를 해당되는 값으로 대체함).
전달해야 할 IP를 찾으려면 `wiotp_agent_setup` 명령을 실행한 후에 표시되는 메시지를 확인하십시오(`Generating Edge internal certificates` 파트 아래에서). 

```
mosquitto_pub -p 8883 -i 'd:<orgId>:<deviceType>:<deviceId>' -u 'use-token-auth' -P '<deviceToken>' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h <EdgeNodeIP>
```

예제:

```
mosquitto_pub -p 8883 -i 'd:nwr48y:myDevice:device1' -u 'use-token-auth' -P '12345678' -t 'iot-2/evt/evTest/fmt/json' -m '{"message": "Hello World"}' --cafile /var/wiotp-edge/persist/dc/ca/ca.pem -h 10.0.2.15
```

참고: 에지 노드 외부에서 mosquitto_pub를 실행하는 경우에는 -h `<host>` 옵션을 사용하고 --cafile 경로를 조정하십시오. 

### 에지 노드 등록 취소

에지 노드를 등록 취소하려면 다음을 실행하십시오.

`hzn unregister`

### Horizon-wiotp 설치 제거

노드에서 Horizon 에이전트 완전 제거: 

`apt-get purge -y horizon horizon-wiotp`

### 문제점 해결

Horizon 에이전트 버전 확인:

`dpkg -l | grep horizon`

Horizon 에이전트 실시간 로그 확인:

`journalctl -af -u horizon.service`

syslog 수집:

`tail -n <num-max-of-lines> /var/log/syslog `

컨테이너별 로그 메시지 필터링: 

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-connector'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-mqttbroker'`

`tail -n <num-max-of-lines> /var/log/syslog | grep 'edge-im'`


## 자세한 정보 찾기
{: #more_info}

{{site.data.keyword.iot_short_notm}} 에지에 대한 자세한 정보는 다음 주제를 참조하십시오. 
- [{{site.data.keyword.iot_short_notm}} 에지 개요](WIoTP_edge.html#edge_overview)
- [{{site.data.keyword.iot_short_notm}} 에지 구성](WIoTP_edge_config.html#edge_configure)
- [{{site.data.keyword.iot_short_notm}} 에지 개발](WIoTP_edge_dev.html#edge_dev)
