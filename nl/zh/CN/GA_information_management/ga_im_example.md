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

# 使用 REST API 开始进行数据管理
{: #im_example}

使用以下步骤可帮助您配置开始使用 {{site.data.keyword.iot_full}} 数据管理组件的双联设备和双联资产功能所需的资源。

有关 API 的详细信息，请参阅 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 文档。



## 高级别工作流
{: #workflow}

使用以下步骤可帮助您配置使用双联设备或双联资产功能开始映射设备或事物数据所需的资源。

**提示：**有关每个步骤的更多详细信息，请参阅示例场景或使用链接直接转至示例场景中的特定步骤。[逐步指南 1](ga_im_index_scenario.html#scenario) 将全程指导您为异构温度计设备创建设备类型逻辑接口的步骤，[逐步指南 2](../information_management/im_index_scenario_thing.html#scenario) 通过描述如何构建逻辑接口，以允许应用程序使用合并为一种房间类型事物的两种不同气候设备类型中的数据，进一步扩展了该场景。

根据您要创建的逻辑接口是与设备类型关联还是与事物类型关联，创建和使用逻辑接口的过程会有所不同。

### 开始之前
要创建与设备类型或事物类型关联的逻辑接口，必须[至少有一个向 {{site.data.keyword.iot_short_notm}} 注册的设备](ga_im_index_scenario.html#step14)，并且该设备发送带有状态属性的事件。  


### 步骤

1. 	定义传入状态属性。  
首先定义您希望逻辑接口为应用程序提供的传入状态属性。  
根据您要创建的逻辑接口，执行下列两项操作之一：
<dl>
<dt>设备类型：创建物理接口。</dt>
<dd>
<ol>
<li>[创建事件模式文件](ga_im_index_scenario.html#step1)。事件模式文件是本地 .JSON 文件，用于定义入站事件的结构和格式。
<li>[创建事件类型的事件模式资源](ga_im_index_scenario.html#step2)。事件模式资源是 {{site.data.keyword.iot_short_notm}} 所使用的程序化构造。
<li>[创建引用事件模式的事件类型](ga_im_index_scenario.html#step3)。事件类型由 {{site.data.keyword.iot_short_notm}} 使用，用于将一个或多个事件模式资源映射到物理接口。
<li>[创建物理接口](ga_im_index_scenario.html#step7)。
<li>[向物理接口添加事件类型](ga_im_index_scenario.html#step8)。
<li>[向设备类型添加物理接口](ga_im_index_scenario.html#step9)。
</ol>
</dd>
<dt>事物类型：定义事物类型。</dt>
<dd>
<ol>
<li>[创建事物类型模式文件](../information_management/im_index_scenario_thing.html#crt_composition_file)。  
事物类型模式文件是本地 .JSON 文件，用于通过指向现有逻辑接口来定义事物类型的组合。
<li>[创建事物类型模式资源](../information_management/im_index_scenario_thing.html#crt_composition_resource)。  
将本地 .JSON 文件上传到 {{site.data.keyword.iot_short_notm}}。
<li>[创建事物类型](../information_management/im_index_scenario_thing.html#crt_thing_type)。事物类型的作用与设备类型相同，都是用于表示一类事物。
</ol>
</dd>
</dl>
4. 	创建逻辑接口。
 1. 	为[设备类型](ga_im_index_scenario.html#step4)或[事物类型](../information_management/im_index_scenario_thing.html#crt_ai_schema_file)创建逻辑接口模式文件。  
逻辑接口模式文件是本地 .JSON 文件，用于定义向应用程序提供的设备状态。
 2. 	为[设备类型](ga_im_index_scenario.html#step5)或[事物类型](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource)创建逻辑接口模式资源。
 3.	为[设备类型](ga_im_index_scenario.html#step6)或[事物类型](../information_management/im_index_scenario_thing.html#crt_thing_ai)创建逻辑接口。
 4.	为[设备类型](ga_im_index_scenario.html#step10)或[事物类型](../information_management/im_index_scenario_thing.html#add_thing_ai)添加逻辑接口。
5. 	为[设备类型](ga_im_index_scenario.html#step11)或[事物类型](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings)定义映射。   
映射用于将入站属性映射到逻辑接口中的属性。  
6. 	验证并激活与草稿[设备类型](ga_im_index_scenario.html#step15)或[事物类型](../information_management/im_index_scenario_thing.html#activate)关联的配置。
7. 	检索[设备](ga_im_index_scenario.html#step13)或[事物](../information_management/im_index_scenario_thing.html##verify_Thing_state)的状态。  
验证预订中是否显示已更新的设备数据，或者是否已通过使用 REST 调用或通过预订主题字符串来返回已更新的设备数据。  



