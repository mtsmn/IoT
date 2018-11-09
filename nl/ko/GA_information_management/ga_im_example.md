---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# REST API를 사용하여 데이터 관리 시작하기
{: #im_example}

다음 단계를 사용하면 {{site.data.keyword.iot_full}} 데이터 관리 컴포넌트의 쌍둥이 디바이스 및 자산 기능을 사용하여 시작해야 하는 리소스의 구성에 도움이 됩니다. 

API에 대한 세부사항은 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 문서를 참조하십시오.


## 상위 레벨 워크플로우
{: #workflow}

다음 단계를 사용하면 쌍둥이 디바이스 또는 자산 기능을 사용하여 디바이스 또는 사물 데이터의 맵핑을 시작해야 하는 리소스의 구성에 도움이 됩니다. 

**팁:** 각 단계에 대한 자세한 정보를 보려면 예제 시나리오를 참조하거나 링크를 사용하여 예제 시나리오의 특정 단계로 직접 이동하십시오. [단계별 안내서 1](ga_im_index_scenario.html#scenario)은 이기종 온도계 디바이스의 디바이스 유형 논리 인터페이스를 작성하는 단계를 안내하며, [단계별 안내서 2](../information_management/im_index_scenario_thing.html#scenario)는 애플리케이션이 룸 유형 사물에 융합된 두 개의 서로 다른 날씨 디바이스 유형의 데이터를 이용할 수 있도록 허용하는 논리 인터페이스의 빌드 방법을 설명함으로써 이 시나리오를 확장합니다. 

논리 인터페이스를 작성하고 이용하기 위한 프로세스는 디바이스 유형 또는 사물 유형과 연관된 논리 인터페이스를 작성하는지 여부에 따라 다소 차이가 있습니다. 

### 시작하기 전에
디바이스 유형 또는 사물 유형과 연관된 논리 인터페이스를 작성하려면 [{{site.data.keyword.iot_short_notm}}에 등록된 하나 이상의 디바이스](ga_im_index_scenario.html#step14) 및 상태 특성의 전송 이벤트가 있어야 합니다.   


### 단계

1. 	수신 상태 특성을 정의하십시오.  
우선 논리 인터페이스가 애플리케이션에서 사용할 수 있도록 할 수신 상태 특성을 정의하십시오.   
작성 중인 논리 인터페이스에 따라 둘 중에서 하나를 수행하십시오. 
<dl>
<dt>디바이스 유형: 실제 인터페이스를 작성하십시오.</dt>
<dd>
<ol>
<li>[이벤트 스키마 파일을 작성](ga_im_index_scenario.html#step1)하십시오. 이벤트 스키마 파일은 인바운드 이벤트의 구조와 형식을 정의하는 로컬 .JSON 파일입니다.
<li>[이벤트 유형의 이벤트 스키마 리소스를 작성](ga_im_index_scenario.html#step2)하십시오. 이벤트 스키마 리소스는 {{site.data.keyword.iot_short_notm}}에서 사용하는 프로그램 방식의 구성입니다.
<li>[이벤트 스키마를 참조하는 이벤트 유형을 작성](ga_im_index_scenario.html#step3)하십시오. 이벤트 유형은 하나 이상의 이벤트 스키마 리소스를 실제 인터페이스에 맵핑하기 위해 {{site.data.keyword.iot_short_notm}}에 의해 사용됩니다.
<li>[실제 인터페이스를 작성](ga_im_index_scenario.html#step7)하십시오.
<li>[이벤트 유형을 실제 인터페이스에 추가](ga_im_index_scenario.html#step8)하십시오.
<li>[실제 인터페이스를 디바이스 유형에 추가](ga_im_index_scenario.html#step9)하십시오.
</ol>
</dd>
<dt>사물 유형: 사물 유형을 정의하십시오. </dt>
<dd>
<ol>
<li>[사물 유형 스키마 파일을 작성](../information_management/im_index_scenario_thing.html#crt_composition_file)하십시오.   
사물 유형 스키마 파일은 기존 논리 인터페이스를 지시하여 사물 유형의 구성을 정의하는 로컬 .JSON 파일입니다.
<li>[사물 유형 스키마 리소스를 작성](../information_management/im_index_scenario_thing.html#crt_composition_resource)하십시오.   
로컬 .JSON 파일을 {{site.data.keyword.iot_short_notm}}에 업로드하십시오.
<li>[사물 유형을 작성](../information_management/im_index_scenario_thing.html#crt_thing_type)하십시오. 사물의 클래스를 표현한다는 점에서 사물 유형은 디바이스 유형과 동일한 용도를 제공합니다.
</ol>
</dd>
</dl>
4. 	논리 인터페이스를 작성하십시오.
 1. 	[디바이스 유형](ga_im_index_scenario.html#step4) 또는 [사물 유형](../information_management/im_index_scenario_thing.html#crt_ai_schema_file)의 논리 인터페이스 스키마 파일을 작성하십시오.   
논리 인터페이스 스키마 파일은 애플리케이션에서 사용 가능한 디바이스 상태를 정의하는 로컬 .JSON 파일입니다.
 2. 	[디바이스 유형](ga_im_index_scenario.html#step5) 또는 [사물 유형](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource)의 논리 인터페이스 스키마 리소스를 작성하십시오. 
 3.	[디바이스 유형](ga_im_index_scenario.html#step6) 또는 [사물 유형](../information_management/im_index_scenario_thing.html#crt_thing_ai)의 논리 인터페이스를 작성하십시오. 
 4.	[디바이스 유형](ga_im_index_scenario.html#step10) 또는 [사물 유형](../information_management/im_index_scenario_thing.html#add_thing_ai)에 논리 인터페이스를 추가하십시오. 
5. 	[디바이스 유형](ga_im_index_scenario.html#step11) 또는 [사물 유형](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings)에 대한 맵핑을 정의하십시오.    
맵핑은 논리 인터페이스의 특성에 인바운드 특성을 맵핑하는 데 사용됩니다.  
6. 	드래프트 [디바이스 유형](ga_im_index_scenario.html#step15) 또는 [Thing_type](../information_management/im_index_scenario_thing.html#activate)과 연관된 구성을 유효성 검증하고 활성화하십시오. 
7. 	[디바이스](ga_im_index_scenario.html#step13) 또는 [사물](../information_management/im_index_scenario_thing.html##verify_Thing_state)의 상태를 검색하십시오.   
구독이 업데이트된 디바이스 데이터를 표시하는지 또는 업데이트된 디바이스 데이터가 REST 호출을 사용하거나 주제 문자열을 구독하여 리턴되는지 확인하십시오.  



