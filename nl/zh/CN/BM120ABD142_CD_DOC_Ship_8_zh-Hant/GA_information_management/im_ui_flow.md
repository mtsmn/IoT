---

copyright:
years: 2017
lastupdated: "2017-08-10"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 Web 介面開始使用 Data Management（測試版）
{: #gs_web}

**重要事項：**{{site.data.keyword.iot_full}} Data Management Web 介面特性僅是有限測試版程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} 提供線上工具，以協助您將資料正規化及轉換為 Data Management 特性的一部分。除了所提供的 [Watson IoT Platform Data Management API ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 之外，您還可以使用「簡單介面建立程式」或「進階介面建立程式」來配置資源，並且開始對映裝置資料。線上介面建立程式會引導您執行使用者介面中的步驟。

如需 Data Management 特性及其好處的相關資訊，請參閱 [Data Management 簡介](../GA_information_management/ga_im_device_twin.html#device_twins)。

如需您需要配置之資源的相關資訊，請參閱[瞭解 Data Management](../GA_information_management/ga_im_definitions.html#definitions_resource)。

使用「簡單介面建立程式」，您可以新增一個實體介面，並會自動建立一個邏輯介面，而且這兩個邏輯介面會對映在一起。使用「進階介面建立程式」，您可以進一步控制配置。您可以新增一個實體介面以及一個以上的邏輯介面，然後新增其間的對映。

即會以草稿格式新增介面及對映。在新增並驗證介面及對映之後，就必須啟動它們。如需草稿及作用中資源的相關資訊，請參閱[建立、更新、啟動及取消啟動資源](../GA_information_management/ga_im_definitions.html#draft_active_resources)。



## 高階流程
{: #interface_flow}

使用下列步驟，可協助您配置開始使用資料管理特牲所需的資源。

**開始之前**
此程序假設您的裝置類型至少具有一個已連接及已登錄的裝置。如需這些必要條件的相關資訊，以及如何建立裝置類型及登錄裝置的指示，請參閱[配置裝置以與 Data Management 特性搭配使用](im_config_devices.html)。

1. 選取您要為其正規化及轉換資料的裝置類型。
2. 選取「簡單介面建立程式」或「進階介面建立程式」：

a. 簡單流程  
   i. [建立草稿實體介面](#create_physical_interface_simple)。  
   
b. 進階流程  
   i. [建立草稿實體介面](#create_physical_interface_advanced)。  
   ii. [建立一個以上的草稿邏輯介面](#create_logical_interface)（當您使用簡單流程時，此步驟會自動完成）。  
   iii. [將實體介面對映至邏輯介面](#create_interface_mappings)。  
     
     
3. [設定通知喜好設定](#set_notifications)。
4. [啟動配置](#validate_activate)。

*附註：*您也可以使用 API 來配置 Data Management 特性。如需配置 Data Management 特性以使用 API 正規化及轉換裝置資料的詳細資訊，請參閱[開始使用 Data Management]{ga_im_example.html#im_example}。[逐步手冊：如何透過共用介面使用裝置的詳細範例](../GA_information_management/ga_im_index_scenario.html#scenario)會引導您完成各步驟，以建立異質溫度計裝置的裝置類型邏輯介面。

如需 API 的詳細資料，請參閱 [{{site.data.keyword.iot_short_notm}} HTTP REST API ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} 文件。


## 建立草稿實體介面（簡單流程）
{: #create_physical_interface_simple}

1. 從主導覽功能表中，選取**裝置**。
2. 選取**裝置類型**，然後選取您要為其建立介面的裝置類型。您也可以[建立新的裝置類型]。
3. 視需要檢視及更新裝置類型資訊，然後儲存所有變更。
4. 按一下**介面**。
5. 按一下**簡單流程**，以開啟「簡單介面建立程式」。
6. 按一下**建立介面**。
7. 按一下**新增內容**，以開始將事件及內容新增至實體介面。  
 a. 在**將內容新增至介面**視窗中，選取您要新增至介面的事件。系統會接聽所選取裝置類型之已連接裝置的作用中事件。您可以選取最後一個已快取事件的裝置 ID，或從清單中選取另一個事件。  
  b. 選取事件中的內容，然後按一下**新增**。  
  c. 視需要新增其他內容。
8. 按一下**完成**。即會建立實體介面。如果您要新增其他詳細資料，則可以選取**使用進階介面建立程式**鏈結來升級介面。


## 建立草稿實體介面（進階流程）
{: #create_physical_interface_advanced}

1. 從主導覽功能表中，選取**裝置**。
2. 選取**裝置類型**，然後選取您要為其建立介面的裝置類型。您也可以[建立新的裝置類型]。
2. 檢視裝置類型資訊，然後按一下**介面**。
4. 按一下**進階流程**，以開啟「進階介面建立程式」。
5. 按一下**建立介面**
7. 在**建立實體介面**頁面的**身分**標籤上，鍵入實體介面的名稱及說明。
7. 選用項目：如果您在介面中使用已啟用邊緣功能的裝置，請開啟「邊緣分析」開關。
8. 按**下一步**。
9. 藉由新增事件類型及有效負載，來定義實體介面：
 a. 按一下**建立事件類型**。 
 b. 選取最後一個已快取事件、從清單中選取事件，或手動新增事件。
10. 按一下**完成**。即會以草稿格式建立實體介面。您可以繼續新增一個以上的邏輯介面。

## 建立草稿邏輯介面
{: #create_logical_interface}

1. 建立草稿實體介面之後，請按一下**新增邏輯介面**。
2. 在**建立邏輯介面**頁面的**身分**標籤上，鍵入邏輯介面的名稱及說明。
3. 按**下一步**。
4. 按一下**新增內容**，以開始定義邏輯介面的內容。
5. 在**建立內容**視窗中，從實體介面中選取您要新增至邏輯介面的內容，然後儲存變更。
6. 視需要新增其他邏輯介面，然後按**下一步**。
7. {: #set_notifications}設定介面的通知喜好設定。選取下列其中一個選項，以決定何時要傳送裝置狀態通知：
 - 無事件通知者 - 未傳送任何通知。您可以使用 REST API 來擷取裝置狀態變更資訊。
 - 狀態變更時 - 您會在裝置狀態變更時收到通知。
 - 在每個事件時 - 每次平台處理裝置事件時，您都會收到通知，即使事件未導致狀態變更。
8. 按一下**完成**。即會以草稿格式建立介面。
9. {: #validate_activate} 按一下**部署**，以驗證配置。
10. 按一下**部署**，以啟動配置。配置狀態會設為「已部署」。 

