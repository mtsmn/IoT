---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-01"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Data Science Experience를 사용하여 데이터 분석
{: #DSX_integration}

Data Science Experience(DSX)와 함께 {{site.data.keyword.iot_full}}을 사용하여 플랫폼에 연결된 디바이스에서 전송된 데이터를 시각화하고 이에 대해 알아볼 수 있습니다.
{: shortdesc}

## 개요 및 목표

이 안내서를 통해 DSX를 분석 도구로 사용하여 {{site.data.keyword.iot_short_notm}} 디바이스 이벤트 데이터를 시각화하는 프로세스를 단계별로 수행할 수 있습니다.

DSX는 여러 도구를 사용하여 통찰이 가능하도록 하는 클라우드 기반의 대화식 협업 환경을 제공합니다. IBM DSX에서 사용 가능한 Jupyter Notebook을 사용하여 히스토리 {{site.data.keyword.iot_short_notm}} 데이터를 로드하고, 데이터 분석 및 이상 항목 식별에 도움이 되도록 데이터를 사용하여 시각화를 구축하십시오. 히스토리 데이터에서 임계값을 얻고 이러한 값을 사용하여 {{site.data.keyword.iot_short_notm}}에서 클라우드 규칙을 작성할 수 있습니다. 클라우드 규칙은 규칙과 연관된 IoT 디바이스가 구성된 임계값 한계를 벗어난 측정값을 공개할 때 사용자에게 알립니다.

{{site.data.keyword.iot_short_notm}}으로 전송된 디바이스 데이터는 {{site.data.keyword.cloudantfull}} NoSQL DB 서비스를 사용하여 수집되고 {{site.data.keyword.Bluemix}}에 저장될 수 있습니다. 데이터를 수집하려면 먼저 {{site.data.keyword.iot_short_notm}}을 {{site.data.keyword.cloudant_short_notm}} 서비스에 연결해야 합니다. 디바이스 데이터는 구성된 버킷 간격에 따라 {{site.data.keyword.cloudant_short_notm}}의 일간, 주간 또는 월간 데이터베이스에 저장됩니다.


![DSX를 사용한 데이터 분석에 대한 개요](images/DSX_overview.png)

이 안내서의 일부로 다음을 학습합니다.

 - Cloudant NoSQL DB가 히스토리언 서비스로 사용되도록 플랫폼 데이터 스토리지를 구성하는 방법.
 - 날씨 센서 시뮬레이터를 사용하여 플랫폼에서 사용할 데이터를 생성하는 방법.
 - 데이터를 내보낸 후 DSX에 가져와 데이터를 분석하는 방법.
 - IoT 데이터를 시각화하고 이상 항목을 발견하고 경보를 구성하는 방법.


## 전제조건

이러한 단계를 완료하려면 [Cloudant NoSQL DB ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/catalog/services/cloudant-nosql-db)가 포함된 [{{site.data.keyword.iot_short_notm}} ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")에 대한 액세스](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window}{: new_window}, [Apache Spark ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} 서비스에 대한 액세스, 그리고 [DSX 계정 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window}에 대한 액세스가 가능해야 합니다. 


## 1단계. 시뮬레이터 설정
{:#DSX_sensor_data}

의미 있는 분석을 수행하려면 의미 있는 데이터가 있어야 합니다. 실제 센서 데이터를 시뮬레이션하여 Watson IoT Platform 디바이스 데이터를 DSX를 사용하여 분석하는 방법에 대해 학습할 수 있습니다. 이 단계는 다음에 대한 지시사항을 제공합니다.
 - [**{{site.data.keyword.iot_short_notm}}의 기존 인스턴스**로 시뮬레이터 설정](#sim_existing_platorm)
 - [**{{site.data.keyword.iot_short_notm}}의 새 인스턴스**로 시뮬레이터 설정](#sim_new_platform)


### {{site.data.keyword.iot_short_notm}}의 기존 인스턴스로 날씨 센서 시뮬레이터 설정
{: #sim_existing_platform}

날씨 센서 시뮬레이터를 사용하여 조직에 대해 실제 센서 데이터 이벤트를 시뮬레이션하려면 먼저 시뮬레이터를 설정해야 합니다. 이러한 단계에서는 {{site.data.keyword.iot_short_notm}}의 인스턴스를 이미 시작하고 실행한다고 가정합니다.

1. [시뮬레이터를 실행하는 데 필요한 apikey 및 토큰을 생성하십시오. ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [날씨 센서 시뮬레이터 웹 앱 배치 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}를 수행하고 자세한 지시사항을 따르십시오.

   날씨 센서에 대한 자세한 정보는 [날씨 센서 시뮬레이터 안내서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}를 참조하십시오.
3. [2단계. 데이터베이스 커넥터 구성](#DSX_config_db)을 진행하십시오.


### {{site.data.keyword.iot_short_notm}}의 새 인스턴스로 날씨 센서 시뮬레이터 설정
{: #sim_new_platform}

날씨 센서 시뮬레이터를 사용하여 조직에 대해 실제 센서 데이터 이벤트를 시뮬레이션하려면 먼저 시뮬레이터를 설정해야 합니다. 이러한 단계에는 시뮬레이터와 함께 {{site.data.keyword.iot_short_notm}} 인스턴스를 작성하는 데 대한 지시사항이 포함됩니다.

1. [{{site.data.keyword.iot_short_notm}}의 인스턴스로 날씨 센서 시뮬레이터 웹 앱 배치 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window}를 수행하고 세부 단계를 수행하십시오.

   날씨 센서에 대한 자세한 정보는 [날씨 센서 시뮬레이터 안내서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}를 참조하십시오.
2. 배치가 완료되기를 기다린 후에 IBM Cloud 대시보드로 이동하십시오.
3. 배치 프로세스를 통해 작성된 {{site.data.keyword.iot_short_notm}} 서비스 "wiotp-for-weather-sensors-simulator"를 실행하십시오.
4. [2단계. 데이터베이스 커넥터 구성](#DSX_config_db)을 진행하십시오.


## 2단계. 데이터베이스 커넥터 구성
{: #DSX_config_db}

선택한 버킷 간격 옵션에 따라 Cloudant의 일간, 주간 또는 월간 데이터베이스에 디바이스 데이터를 저장할 수 있습니다. 동일한 버킷 간격(일, 주 또는 월)으로 모든 디바이스에서 수집된 데이터는 동일한 데이터베이스에 저장됩니다.

버킷 간격 구성에 관계없이 세 개의 데이터베이스가 커넥터를 통해 자동으로 작성됩니다. 현재 버킷 간격에 대해 하나의 데이터베이스가 작성되고, 향후 간격에 대해 하나, 구성 데이터베이스에 대해 하나가 작성됩니다. 간격의 끝에 도달한 경우, 디바이스 데이터는 새 간격의 버킷 데이터베이스에 저장되며 후속 버킷에 대해 새 데이터베이스가 작성됩니다.

DSX와 함께 {{site.data.keyword.cloudant_short_notm}}를 사용하려면 Cloudant NoSQL DB가 히스토리언 서비스로 사용되도록 플랫폼 데이터 스토리지를 구성해야 합니다.

1. {{site.data.keyword.cloudant_short_notm}} 대시보드의 탐색줄에서 **확장**을 클릭하십시오.
2. **히스토리 데이터 스토리지**에서 **설정**을 클릭하십시오. **히스토리 데이터 스토리지 구성** 섹션에는 {{site.data.keyword.cloudant_short_notm}}와 동일한 IBM Cloud 영역 내에서 사용 가능한 Cloudant NoSQL DB 서비스가 모두 나열됩니다. 
3. 연결할 Cloudant NoSQL DB 서비스를 선택하십시오.
4. 다음 Cloudant NoSQL DB 구성 옵션을 지정하십시오.
  - 버킷 간격 = 일
  - 시간대 = UTC
  - 데이터베이스 이름 = 기본값
5. **완료**를 클릭하고 Cloudant 서비스 연결을 위한 권한을 확인하십시오. 확인 창에 액세스하려면 브라우저에서 팝업이 사용 가능한지 확인하십시오. Cloudant NoSQL DB를 구성하면 히스토리 데이터 스토리지 상태가 구성됨 상태로 변경되고 디바이스 데이터가 {{site.data.keyword.cloudant_short_notm}} NoSQL DB에 저장됩니다.
6. [3단계. 시뮬레이터 실행](#run_simulator)을 진행하십시오.


## 3단계. 시뮬레이터 실행
{: #run_simulator}

시뮬레이터는 하이파 지역에 있는 17개 기상 관측소의 실제 날씨 센서 데이터를 {{site.data.keyword.iot_short_notm}} 조직에 공개합니다.

1. 시뮬레이터로 이동하십시오.
2. 바인드된 {{site.data.keyword.iot_short_notm}} 인스턴스로 시뮬레이터를 배치한 경우 3단계를 진행하십시오. 독립형 버전의 시뮬레이터를 배치한 경우 다음 세부사항을 입력하십시오.
   - Watson IoT Platform 조직
   - API 키
   - 인증 토큰

3. **시뮬레이터 실행**을 클릭하십시오. 데이터 생성에는 몇 분이 소요됩니다.
4. 시뮬레이터가 실행되는 동안 Watson IoT Platform으로 이동하고 디바이스가 작성되었고 이벤트가 이러한 디바이스로 제공되는지 확인하십시오.
5. [4단계. DSX 설정 및 데이터 시각화](#DSX_visualize_data)를 진행하십시오.


## 4단계. DSX 설정 및 데이터 시각화
{: #DSX_visualize_data}

DSX를 설정하고 데이터 시각화를 시작하려면 다음을 수행하십시오.

1. [미리 구성된 Jupyter Notebook을 설정](#setup_jupyter_notebook)하여 데이터에 대한 통찰을 얻고 이상 항목을 발견하십시오.
2. [분석을 실행하십시오.](#run_analysis)
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. 미리 구성된 Jupyter Notebook 설정
{: #setup_jupyter_notebook}

Jupyter Notebook은 실행 코드, 수학 공식, 그래픽/시각화(matplotlib) 및 설명 텍스트가 포함된 문서를 작성하고 공유하는 데 사용할 수 있는 웹 애플리케이션입니다.

미리 구성된 Jupyter Notebook을 설정하여 데이터에 대한 통찰을 얻고 이상 항목을 발견하려면 다음을 수행하십시오.
1. 지원되는 브라우저를 사용하여 IBM Cloud ID로 [DSX ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://datascience.ibm.com/){: new_window}에 로그인하십시오. 
2. **+ 새 프로젝트**를 클릭하여 새 프로젝트를 작성하고 **Jupyter Notebook** 타일을 선택하십시오. 프로젝트는 노트북을 수집하고 공유하는 데 사용할 영역을 작성하고 데이터 소스에 연결하고 파이프라인을 작성하고 데이터 세트를 추가합니다.
3. **+ 프로젝트에 추가** 드롭 다운을 클릭하고 **연결**을 선택하십시오. 서비스의 목록에서 **Cloudant**를 선택한 후에 {{site.data.keyword.cloudant_short_notm}} 서비스 페이지의 서비스 인증 정보 탭에서 찾은 Cloudant URL, 사용자 이름 및 비밀번호를 입력하고 표시된 필드에 이를 입력하십시오. 연결의 이름을 지정하고 **작성**을 클릭하십시오. 
4. 대시보드의 데이터 자산 섹션 아래에서 이제 연결을 사용할 수 있는지 확인하십시오.
5. **+ 새 노트북**을 클릭하여 새 Jupyter Notebook을 작성하십시오.
6. 서비스 아래의 런타임 선택 드롭 다운에서 **spark-iw**를 선택하십시오. 존재하지 않는 경우, 이는 Apache Spark 서비스가 제대로 프로비저닝되지 않았음을 의미합니다. IBM Cloud 대시보드에서 이의 존재 여부를 확인하고, 만일 존재하지 않으면 서비스 페이지를 방문하여 이를 프로비저닝하십시오. 
7. 언어를 **Python 2**로 설정하고 Spark 버전을 **2.1**로 설정하십시오. 
8. **시작 URL**을 선택하여 기존 노트북을 로드한 다음 노트북의 구체적인 이름을 지정하고 다음 URL을 입력하여 샘플 노트북을 여십시오.
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. **노트북 작성**을 클릭하십시오. 노트북이 메타데이터와 코드로 작성되었는지 확인하십시오.
10. '!pip install --upgrade pixiedust, ,'로 시작하는 셀을 선택한 후에 **실행**을 클릭하여 코드를 실행하십시오.
11. 설치가 완료되면 **커널 다시 시작** 아이콘을 클릭하거나 커널 메뉴에서 **다시 시작**을 선택하여 Spark 커널을 다시 시작하십시오. 
12. 커널이 다시 시작될 때까지 약 10초 정도 기다린 후에 주석과 함께 비어 있는 코드 셀을 클릭하여 이를 선택하십시오. 
13. 다음 단계를 완료하여 Cloudant 인증 정보를 해당 셀로 가져오십시오.

    1. ![데이터 찾기 및 추가](images/find_add_data_icon.png)를 클릭하십시오. 
    2. **연결** 탭을 선택하십시오.
    3. **코드에 삽입**을 클릭하십시오.
"credentials_1"이라고 하는 사전이 Cloudant 인증 정보를 사용하여 작성됩니다. 이름이 "credentials_1"으로 지정되지 않은 경우, 이 이름이 노트북 코드를 실행하는 데 필요하므로 사전 이름을 "credentials_1"으로 바꾸십시오.
14. 데이터베이스 이름(dbname)의 셀에 데이터 소스인 Cloudant 데이터베이스의 이름을 입력하십시오(예: `iotp_yourWIoTPorgId_default_Year-month-day`).
15. 노트북을 저장하십시오. 이 노트북을 실행할 준비가 되었습니다.


### 2. 분석 실행
{: #run_analysis}

1.	Cloudant 인증 정보가 포함된 셀을 선택하십시오.
2.	**재생**을 클릭하여 셀 코드를 실행하십시오.
3.	실행 결과를 확인하고 각 셀에서 사용된 python 코드를 분석하십시오.
4.	각 셀에 대해 2와 3단계를 반복하십시오. **사용자 입력 필요**가 지정된 셀의 경우 실행 전에 다음 코드 셀에 정의된 변수에 새 입력 값을 제공해야 합니다.

**참고:** 일부 셀은 배경 Spark 작업을 실행하며 완료에 시간이 오래 걸릴 수 있습니다. 셀 내 코드 실행이 완료되면 별표(`*`)가 숫자로 바뀝니다(예: In `[*]`가 In `[1]`로 바뀜). 단계를 완료한 후 {{site.data.keyword.iot_short_notm}}에서 클라우드 규칙을 작성하여 이상 항목 발견 시 자동으로 경보를 생성합니다.


<!-- ### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails. -->


## 다음 항목

DSX에 대한 자세한 정보는 다음 리소스를 참조하십시오.

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [DSX 커뮤니티 노트북 및 튜토리얼 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window}(Jupyter Notebook에 대해 자세히 보려면 링크를 따름).
 - [Watson IoT Platform 쿡북의 분석 레시피 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
