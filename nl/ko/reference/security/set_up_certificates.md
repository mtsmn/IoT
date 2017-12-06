---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-13"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# 인증서 구성
{: #set_up_certificates}

인증서는 디바이스 인증에 사용되거나 MQTT 메시징에 대한 기본 {{site.data.keyword.iot_full}} 서버 인증서를 대체하는 데 사용됩니다. 서명된 유효한 인증서가 없는 디바이스는 액세스가 거부되고 서버와 통신할 수 없습니다. 

디바이스에 대해 인증서 및 서버 액세스 권한을 구성하기 위해 시스템 운영자는 연관된 인증 기관(CA) 인증서를 등록하며, 선택적으로 메시지 서버 인증서를 {{site.data.keyword.iot_short_notm}} 플랫폼에 등록합니다. 

API를 사용하여 CA 인증서 및 메시징 서버 인증서를 관리하는 데 대한 정보는 [IBM Watson IoT Platform 인증 및 권한 API(![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘"))](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}를 참조하십시오.

## CA 인증서
조직은 CA 인증서를 사용하여 해당 디바이스가 서버에 연결될 수 있도록 디바이스의 클라이언트 인증서가 신뢰 가능하다고 인식할 수 있습니다. 

CA 인증서를 추가하거나 메시징 서버 인증서를 대체하는 경우, 모든 디바이스는 서버가 적합한 CA를 사용하여 디바이스를 인증할 수 있도록 SNI(Server Name Indication)를 지원하는 MQTT 클라이언트를 사용하여 연결해야 합니다. 

연결 보안 정책을 구성하여 연결 보안 레벨을 설정할 수 있습니다. 이를 수행하는 방법에 대한 자세한 정보는 [보안 정책 구성](set_up_policies.html)을 참조하십시오. 

## 클라이언트 인증서

개별 클라이언트(디바이스 또는 게이트웨이) 인증서는 디바이스에 남아 있으며 플랫폼으로 업로드되지 않습니다. 모든 디바이스 및 게이트웨이 인증서를 서명하는 데 사용되는 CA 서명 인증서가 플랫폼에 업로드하는 유일한 인증서입니다. 자체 서명된 메시징 서버 인증서를 사용하는 경우, 클라이언트 인증서(cert.pem) 서명에 사용되는 루트 및 중간 인증서를 업로드해야 합니다.

CA 인증서로 서명하는 개별 디바이스 또는 게이트웨이 인증서는 인증서에 CN(Common Name) 또는 SubjectAltName으로 입력된 디바이스 ID 또는 게이트웨이 ID가 있어야 합니다. 

디바이스의 경우, **CN** 필드 형식은 `CN=d:devtype:devid`이고 **SubjectAltName** 필드 형식은 `SubjectAltName=email:d:*devtype:devid*`이며, 여기서 `email:d`는 상수이고 `*devtype*`은 디바이스의 디바이스 유형이며 `*devid*`는 플랫폼에서 디바이스의 ID입니다.

게이트웨이의 경우, **CN** 필드 형식은 `CN=g:typeId:deviceId`이고 **SubjectAltName** 필드 형식은 SubjectAltName=email:g:*devtype:devid*입니다.

참고: `orgId`를 디바이스 또는 게이트웨이 인증서에 대한 **CN** 또는 **SubjectAltName** 필드에 포함하지 마십시오. `orgId`를 메시징 서버에 연결할 때 클라이언트 구현으로 제공되는 SNI 정보의 일부로 제공해야 합니다. 

클라이언트 인증서에 대한 자세한 정보는 [클라이언트 측 인증서를 사용하여 IBM Watson IoT Platform에 Raspberry Pi 연결 레시피(![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘"))](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}를 참조하십시오.

## 메시징 서버 인증서

메시징 서버 인증서는 기본 도메인, internetofthings.ibmcloud.com을 허용합니다. 인증서 CN 또는 SubjectAltName 다음에 다음 형식이 와야 합니다. 

- `orgId.messaging.internetofthings.ibmcloud.com`(IoTP 도메인)

다음 예에 서버 인증서에 유효한 CN이 표시됩니다. 

`mtxpd0.messaging.internetofthings.ibmcloud.com`

메시징 서버 인증서에 대한 자세한 정보는 [자체 서명 서버 인증서를 사용하여 IBM Watson IoT Platform에 Raspberry Pi 연결 레시피(![외부 링크 아이콘](../../../../icons/launch-glyph.svg "외부 링크 아이콘"))](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}를 참조하십시오.

### 사용자 정의 도메인(베타)
{: #custom_domains}

**중요:** 메시징 서버 인증서의 사용자 정의 도메인 기능은 제한된 베타 프로그램의 일부로서만 사용 가능합니다. 사용자 정의 도메인을 사용하려면 **설정** 페이지에서 **시범 기능**을 켜십시오. 

베타 기능의 일부로서 메시징 서버 인증서는 사용자 정의 도메인을 허용합니다. 인증서 CN 또는 SubjectAltName 다음에 다음 형식이 와야 합니다. 

- `orgId.messaging.<custom domain>`

**CN** 필드는 다음 예에 표시된 대로 사용자 정의 도메인에 와일드카드 문자를 허용합니다.

- `CN=*.messaging.mywiotpcustomdomain.com`

**중요**: 사용자 정의 도메인의 경우, {{site.data.keyword.iot_full}} 메시징 서버에 사용자 정의 도메인을 해석하려면 외부 DNS 서비스가 필요합니다. 플랫폼에서 DNS 서비스가 제공되지 않습니다. 

## 디바이스 인증을 위한 인증 기관(CA) 인증서 등록
{: #reg_ca_cert}

1. {{site.data.keyword.iot_short_notm}}에 로그인하고 **일반 설정**으로 이동하십시오. 
2. **보안** 섹션의 **CA 인증서** 아래에서 **인증서 추가**를 클릭하십시오. 
3. 찾아보기를 사용하여 업로드할 인증서 파일을 선택하거나 **인증서 추가** 창으로 파일을 끌어오십시오. 파일에는 인증서가 하나만 포함될 수 있으며 인증서 날짜가 유효해야 합니다. .pem 또는 .der 형식의 인증서만 허용됩니다. 선택된 파일 내의 인증서 정보를 미리볼 수 있습니다. 
4. 인증서 파일에 대한 설명을 입력하십시오. 
5. 올바른 파일이 선택되었는지 확인한 후 **저장**을 클릭하십시오. 선택된 인증서는 테이블에 나열되며, 기본적으로 활성화되어 있습니다. 

## 메시징 서버 인증서 등록
{: #reg_msg_cert}

기본 서버 인증서는 플랫폼에서 제공됩니다. 기본 인증서를 사용하거나 사용자 조직의 인증서를 업로드할 수 있습니다. 아직 사용할 인증서가 없는 경우에는 새 인증서에 대한 요청을 작성할 수 있습니다. 새 인증서를 수신하면 서명 후에 다시 플랫폼에 업로드해야 합니다. 

**참고:** 플랫폼 대시보드 페이지가 메시징 서버에 대한 내부 연결을 수행하여 디바이스 정보를 검색할 수 있습니다. 기본적으로 브라우저가 메시징 서버를 신뢰 서버로 인식하지 않기 때문에 조직에 대해 자체 서명된 서버 인증서를 설정할 때 대시보드 사용자는 연결 문제를 피하기 위해 해당 브라우저에서 신뢰 인증서로서 서버 인증서를 추가해야 합니다. 

## 사용자 조직에서 메시징 서버 인증서 업로드
{: #upload_cert}
1. **일반 설정**의 **보안** 섹션의 **메시징 서버 인증서** 아래에서 **인증서 추가**를 클릭하십시오. 
2. 찾아보기를 사용하여 업로드할 인증서 파일을 선택하거나 **인증서 추가** 창으로 파일을 끌어오십시오. 
3. 찾아보기를 사용하여 업로드할 개인 키를 선택하거나 **인증서 추가** 창으로 파일을 끌어오십시오. 
4. 개인 키가 비밀번호 문구로 암호화된 경우 개인 키의 비밀번호 문구를 입력하십시오.
5. 올바른 파일이 선택되었는지 확인한 후 **저장**을 클릭하십시오. 

## 메시징 서버 인증서 활성화

기본 인증서 또는 이미 업로드된 다른 인증서를 활성화하려면 **메시징 서버 인증서** 아래의 테이블에 있는 **기본 메시징 서버 인증서** 드롭 다운 목록에서 사용하고자 하는 인증서를 선택하십시오. 선택된 인증서가 활성 인증서로서 테이블에 나열됩니다. 

## 새 메시징 서버 인증서 요청
{: #request_cert}

새 메시징 서버 인증서를 사용하려면 인증서 서명 요청(CSR)을 생성하여 새 인증서를 요청할 수 있습니다. 

1. **일반 설정**의 **보안** 섹션의 **메시징 서버 인증서** 아래에서 **CSR 생성**을 클릭하십시오. 
2. 서버에 대한 CSR을 요청하기 위한 세부사항을 입력하고 **생성**을 클릭하십시오. 테이블에 CSR이 표시됩니다. 
3. 요청을 다운로드하여 서명을 위해 인증 기관에 제출하십시오. 
4. 인증서를 얻은 후 테이블의 CSR 항목으로 돌아가 새 인증서를 업로드하십시오. 인증서가 업로드되면 테이블의 CSR이 업로드된 인증서로 대체됩니다. 
