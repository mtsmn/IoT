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

# 使用 REST API 開始使用資料管理
{: #im_example}

使用下列步驟，可協助您配置開始使用 {{site.data.keyword.iot_full}} 資料管理元件的裝置及資產對應項特性所需的資源。

如需 API 的詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 文件。


## 高階工作流程
{: #workflow}

使用下列步驟，可協助您使用裝置或資產對應項特性來配置開始對映裝置或「物品」資料所需的資源。

**提示：**如需每一個步驟的其他詳細資訊，請參閱範例情境，或使用鏈結直接移至範例情境中的特定步驟。[逐步手冊 1](ga_im_index_scenario.html#scenario) 會逐步執行如何建立異質溫度計裝置之裝置類型邏輯介面的步驟，[逐步手冊 2](../information_management/im_index_scenario_thing.html#scenario) 可讓情境成長，方法是說明如何建置邏輯介面，以讓您的應用程式取用合併為會議室類型「物品」之兩個不同氣候裝置類型的資料。

根據建立與裝置類型還是「物品」類型相關聯的邏輯介面，建立及使用邏輯介面的處理程序會有些不同。

### 開始之前
若要建立與裝置類型或「物品」類型相關聯的邏輯介面，您必須有[至少一個向 {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) 登錄並傳送含狀態內容之事件的裝置。  


### 步驟

1. 	定義送入的狀態內容。  
請先定義要讓邏輯介面可供應用程式使用的送入狀態內容。  
根據所建立的邏輯介面，執行下列兩項之一：
<dl>
<dt>裝置類型：建立實體介面。</dt>
<dd>
<ol>
<li>[建立事件綱目檔](ga_im_index_scenario.html#step1)。事件綱目檔是定義入埠事件結構及格式的本端 .JSON 檔案。<li>[建立事件類型的事件綱目資源](ga_im_index_scenario.html#step2)。事件綱目資源是 {{site.data.keyword.iot_short_notm}} 所使用的程式化建構。
<li>[建立參照事件綱目的事件類型](ga_im_index_scenario.html#step3)。{{site.data.keyword.iot_short_notm}} 使用事件類型，以將一個以上的事件綱目資源對映至實體介面。
<li>[建立實體介面](ga_im_index_scenario.html#step7)。
<li>[將事件類型新增至實體介面](ga_im_index_scenario.html#step8)。
<li>[將實體介面新增至裝置類型](ga_im_index_scenario.html#step9)。
</ol>
</dd>
<dt>物品類型：定義「物品」類型。</dt>
<dd>
<ol>
<li>[建立物品類型綱目檔](../information_management/im_index_scenario_thing.html#crt_composition_file)。  
「物品」類型綱目檔是定義物品類型組合的本端 .JSON 檔案，方法是指向現有邏輯介面。
<li>[建立物品類型綱目資源](../information_management/im_index_scenario_thing.html#crt_composition_resource)。  
請將本端 .JSON 檔案上傳至 {{site.data.keyword.iot_short_notm}}。
<li>[建立物品類型](../information_management/im_index_scenario_thing.html#crt_thing_type)。「物品」類型所提供的用途與裝置類型相同，在於它代表「物品」的類別。
</ol>
</dd>
</dl>
4. 	建立邏輯介面。
 1. 	建立[裝置類型](ga_im_index_scenario.html#step4)或[物品類型](../information_management/im_index_scenario_thing.html#crt_ai_schema_file)的邏輯介面綱目檔。  
邏輯介面綱目檔是定義裝置狀態以供應用程式使用的本端 .JSON 檔案。
 2. 	建立[裝置類型](ga_im_index_scenario.html#step5)或[物品類型](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource)的邏輯介面綱目資源。
 3.	建立[裝置類型](ga_im_index_scenario.html#step6)或[物品類型](../information_management/im_index_scenario_thing.html#crt_thing_ai)的邏輯介面。
 4.	將邏輯介面新增至[裝置類型](ga_im_index_scenario.html#step10)或[物品類型](../information_management/im_index_scenario_thing.html#add_thing_ai)。
5. 	定義[裝置類型](ga_im_index_scenario.html#step11)或[物品類型](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings)的對映。   
對映是用來將入埠內容對映至邏輯介面中的內容。  
6. 	驗證及啟動與草稿[裝置類型](ga_im_index_scenario.html#step15)或[物品類型](../information_management/im_index_scenario_thing.html#activate)相關聯的配置。
7. 	擷取[裝置](ga_im_index_scenario.html#step13)或[物品](../information_management/im_index_scenario_thing.html##verify_Thing_state)的狀態。  
請確認訂閱顯示已更新的裝置資料，或使用 REST 呼叫或訂閱主題字串來傳回已更新的裝置資料。  



