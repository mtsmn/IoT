---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 시작하기 튜토리얼
{: #getting-started-with-iotp}

이 {{site.data.keyword.iot_full}} 시작하기 튜토리얼에서 IoT 디바이스를 {{site.data.keyword.iot_short_notm}}에 연결합니다.
{:shortdesc}

<div id="prerequisites"></div>

## 시작하기 전에
{: #prereqs}

[IBM Cloud 계정 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/registration/){: new_window}, {{site.data.keyword.iot_short_notm}} 서비스의 인스턴스 및 다음 요구사항을 충족하는 디바이스가 필요합니다.

*	디바이스가 HTTP 또는 MQTT 프로토콜을 사용하여 통신할 수 있어야 합니다.

* 디바이스 메시지가 {{site.data.keyword.iot_short_notm}} 메시지 페이로드 요구사항을 준수해야 합니다.

자세한 정보는 [Watson IoT Platform에서 디바이스 개발](/docs/services/IoT/devices/device_dev_index.html)을 참조하십시오.

## 1단계: 디바이스 등록

[{{site.data.keyword.iot_short_notm}} 대시보드 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://internetofthings.ibmcloud.com){: new_window}에서 한 번에 하나씩 디바이스를 추가하거나 [Watson IoT Platform API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window}를 사용하여 여러 디바이스를 추가할 수 있습니다.

{{site.data.keyword.iot_short_notm}} 대시보드에서 디바이스를 추가하려면 다음을 수행하십시오.

1. {{site.data.keyword.Bluemix_notm}} 콘솔의 {{site.data.keyword.iot_short_notm}} 서비스 세부사항 페이지에서 **실행**을 클릭하십시오.

    {{site.data.keyword.iot_short_notm}} 웹 콘솔은 새 브라우저 탭에서 다음 URL을 엽니다.

    ```
 https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
    ```

    여기서 *org_id*는 [{{site.data.keyword.iot_short_notm}} 조직](iotplatform_overview.html#organizations){: new_window}의 ID입니다.

2. 개요 대시보드의 메뉴 분할창에서 **디바이스**를 선택한 다음 **디바이스 추가**를 클릭하십시오.

3. 추가하는 디바이스의 디바이스 유형을 작성하십시오.

    {{site.data.keyword.iot_short_notm}}에 연결된 각 디바이스는 디바이스 유형과 연관되어야 합니다. 디바이스 유형은 공통 특성을 공유하는 디바이스 그룹입니다.

    1. **디바이스 유형 작성**을 클릭합니다.
    2. 디바이스 이름(예: `my_device_type`) 및 디바이스 유형에 대한 설명을 입력합니다.
    
        > **참고:** 디바이스 유형 이름은 36자 이하여야 하며 영숫자 문자(a-z, A-Z, 0-9) 및 다음 문자(`_`, `.`, and `-`)만 포함할 수 있습니다.

    3. 선택사항: 디바이스 유형 속성 및 메타데이터를 입력합니다.

        속성과 메타데이터는 나중에 추가하고 편집할 수 있습니다.
        {: tip}

4. **다음**을 클릭하여 선택한 디바이스 유형의 디바이스를 추가하는 프로세스를 시작하십시오.

5. 디바이스 ID(예: `my_first_device`)를 입력하십시오.

    디바이스 ID는 {{site.data.keyword.iot_short_notm}} 대시보드에서 디바이스를 식별하는 데 사용하고 디바이스를 {{site.data.keyword.iot_short_notm}}에 연결하는 데도 필요한 매개변수입니다.

    > **참고:** 디바이스 유형 이름은 36자 이하여야 하며 영숫자 문자(a-z, A-Z, 0-9) 및 다음 문자(`_`, `.`, and `-`)만 포함할 수 있습니다.

    네트워크 연결 디바이스의 경우, 구분하는 콜론이 없는 디바이스 MAC 주소가 디바이스 ID가 될 수 있습니다.

5. **다음**을 클릭하여 프로세스를 완료하십시오.

6. 인증 토큰을 제공하거나 자동으로 생성된 토큰을 허용하십시오. 고유 토큰을 작성하도록 선택하는 경우, 길이는 8 - 36자이고 영숫자 문자와 특수 문자(`_`, `.`, `!`, `&`, `@`, `?`, `\*`, `+`, `(`, `)` 및 `-`)로만 구성되어야 합니다.

    토큰에는 반복된 문자 시퀀스, 사전 단어, 사용자 이름 또는 기타 사전 정의된 시퀀스가 포함되지 않아야 합니다.

7. 요약 정보가 올바른지 확인한 다음 **추가**를 클릭하여 연결을 추가하십시오.

8. 디바이스 정보 페이지에서 다음 세부사항을 복사하고 저장하십시오.

    * 조직 ID
    * 디바이스 유형
    * 디바이스 ID
    * 인증 방법
    * 인증 토큰

    {{site.data.keyword.iot_short_notm}}에 연결하도록 디바이스를 구성하려면 조직 ID, 디바이스 유형, 디바이스 ID 및 인증 토큰이 필요합니다.

## 2단계: {{site.data.keyword.iot_short_notm}}에 디바이스 연결

1. MQTT 메시징을 위한 디바이스를 설정하고 조직 ID, 디바이스 유형, 디바이스 ID 및 인증 토큰을 사용하여 인증합니다.

2. MQTT 프로토콜을 사용하여 {{site.data.keyword.iot_short_notm}} 조직에 디바이스 메시지를 보냅니다.

    디바이스에 연결할 때 다음 정보가 필요합니다.

    * URL: *org_id*.messaging.internetofthings.ibmcloud.com

      여기서 *org_id*는 {{site.data.keyword.iot_short_notm}} 조직의 ID입니다.

    * 포트:
      * 1883
      * 8883(암호화됨)
      * 443(websockets)
    * 디바이스 ID: d:_org_id:device_type:device_id_
    * 사용자 이름: use-token-auth
    * 비밀번호: _인증 토큰_
    * 이벤트 주제 형식: iot-2/evt/_event_id/fmt/format_string_

      여기서 _event_id_는 {{site.data.keyword.iot_short_notm}}에 표시된 이벤트 이름을 지정하며, _format_string_은 이벤트의 형식(예: JSON)입니다.

    * 메시지 형식: JSON

  자세한 정보는 [디바이스용 MQTT 연결](/docs/services/IoT/devices/mqtt.html)을 참조하십시오.

<!--## Step 3: Create boards and cards to keep track of device data

By using boards and cards, you can view graphics that represent data set values from one or more devices for a quick overview and understanding of the device data.

1. In your {{site.data.keyword.iot_short_notm}} dashboard, click **Create New Board**.

2. Enter a name and description for the board.

3. Click **Next** and then **Create**.

4. Click the board you just created, and then click **Add New Card**. Select Devices as the card type. The following table describes the visualization options you can choose from.

| Type | Data that is displayed |
| Generic visualization | The value of one or more data sets. Choose the large widget size to see up to three data point values in a small table. |
| Line chart | One or more data sets in a real-time scrolling chart. Use the Settings menu to set the data range and retention, the look and feel of the graphs, and more. |
| Bar chart | Data set values in labelled bars. Use the Settings menu to toggle horizontal or vertical bar direction. |
| Donut chart | Two or more data sets in a circular representation. |
| Value | The raw value of one or more data sets. |
| Gauge | The value of a data set shown as a gauge. Use the Settings menu to optionally set gauge thresholds for lower, middle, and upper data ranges. |
| Device properties | Specific properties for one or more devices. |
| All device properties | All properties for one or more devices. |
| Device list | A list to monitor multiple devices. A list can be used as a data source for other cards. |
| Device info | Basic information for a single device. |
| Device map | The location of devices in a device list. |

{: caption="Table 1. Visualization options" caption-side="top"}

## Step 4: Create device type schemas

At this point, you need to create a device type schema and map device properties to then create rules that are triggered based on the datapoints from your mapped device properties.

1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Devices > Manage Schemas** and click **Add Schema**.

2. Select a device type to associate with the message schema. Only one schema can be defined for a device type.

3. Click **Add property** and add one or more properties.

    You can select properties from a connected device, create virtual properties that modify or combine existing properties, or add properties manually.

    The available properties are defined in the payload of the messages that are sent by a device.
    {: tip}

    * To add a property manually, select the **Manual tab** and define the following property details:
        * Name
        * Data type
        * Property
        * Data unit (optional)
        * Decimal places (optional)

    * To create a virtual property, select the **Virtual Property** tab and define the following property details:
        * Name
        * Data type
        * Property
        * Calculation
        * Data unit (optional)
        * Decimal places

    * To select properties from a connected device, select the **From Connected** tab, and then select one or more properties to add to the schema. The selected properties are added and the description is set to the name of the property.

4. Click **Finish** to create the properties.

## Step 5: Create rules and actions

Rules are condition-based decision points that match real-time device data with predefined threshold values or other property data to trigger an alert if a condition is met. In addition to the alert that's displayed in the {{site.data.keyword.iot_short_notm}} dashboard, you can add one or more actions to run business logic when a rule is triggered.

1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Rules** and click **Create Cloud Rule**.

2. Enter a name and description for the rule, select a device type that the rule applies to, and click **Next**.

3. To set up the rule logic, add one or more IF conditions to use as triggers for the rule.

    You can add conditions in parallel rows to apply them as OR conditions, or you can add conditions in sequential columns to apply them as AND conditions. Take a look at the following examples:

      * A simple rule might trigger an alert if a parameter value is larger than a specified value: Condition = `temp_cpu>80`
      * A more complex rule might trigger when a combination of thresholds are met: Condition = `temp_cpu>60 AND cpu_load>90`

    To trigger a condition that compares two properties, or to trigger two or more property conditions that are combined sequentially by using AND, include the triggering data points in the same device message. If the data is received in more than one message, the condition or sequential conditions don't trigger.
    {: tip}

4. Configure conditional trigger requirements for your rule.

    You can use conditional trigger requirements to control the number of alerts that are triggered for a rule over a time period. The conditional triggering acts on any condition in the rule. For example, if a rule has five parallel conditions set by using OR, each condition that's true counts towards the conditional trigger count.

      1. In the rule editor, click the default **Trigger each time conditions are met** link to open the set frequency requirement dialog.
      2. Select and configure the conditional trigger you want to use in the rule.

        * Trigger every time conditions are met
        * Trigger if conditions are met N times in M _Unit of time_

5. Create or select one or more actions that occur if the rule conditions are met.

    For example, an action can be to send an email or post a webhook.

6. Optional: Select an alert priority for the rule.

    The priority is used to classify the alerts that are displayed in the **Boards > Rule-Based Analytics** board. The default priority is Low.

7. Click **Save** to save without activating,  or click **Activate** to save and activate your rule.

    When you activate the rule, an alert is added to the **Rule-Based Analytics** board when the conditions are met, and any rule action is run. -->

## 다음 단계

실시간 및 히스토리 디바이스 데이터를 이용하기 위해서 자체 앱을 작성하고 연결합니다.

  * 디바이스와 앱을 통합해서 연결하기 위해 코드를 빌드하도록 도구에 대한 [클라이언트 라이브러리](/docs/services/IoT/iot_platform_client_lib.html)를 체크아웃하십시오.

  * 고급 분석을 사용자의 연결된 디바이스와 앱에 임베드하기에 대한 추가 튜토리얼은 [Watson IoT Platform 레시피 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window}을 탐색하십시오.
