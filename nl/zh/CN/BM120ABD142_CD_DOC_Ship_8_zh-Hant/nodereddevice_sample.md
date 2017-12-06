---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 建立及連接 Node-RED 裝置模擬器
使用 Node-RED 來建立裝置模擬器，以將模擬裝置資料傳送至 {{site.data.keyword.iot_full}} 組織。  
{:shortdesc}

## 概觀

Node-RED 是一種工具，用於以全新且有趣的方式，將硬體裝置、API 及線上服務連接在一起。如需相關資訊，請參閱 [Node-RED ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://nodered.org/){: new_window} 網站。  

您可以在自己的環境中執行 Node-RED 實例，或是將其用來作為 {{site.data.keyword.Bluemix_notm}} 應用程式。下列步驟包括 {{site.data.keyword.Bluemix_notm}} 的指示。

建立及連接 Node-RED 裝置模擬器，以將 MQTT 裝置訊息傳送至 {{site.data.keyword.iot_short_notm}}。在下列範例中，裝置模擬器會模擬將貨櫃資料傳送至 MQTT 分配管理系統，例如 {{site.data.keyword.iot_short_notm}}。

## 步驟

1. 完成下列步驟，以建立 Node-RED 裝置模擬器：   
    1. 在 https://console.ng.bluemix.net 登入 {{site.data.keyword.Bluemix_notm}}：
    2. 選取**型錄**標籤。
    3. 找到服務型錄的「樣板」區段，然後按一下 **Internet of Things 平台入門範本**。**提示：**按一下[這裡 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window}，直接移至「Internet of Things 平台入門範本」頁面。
    4. 在「Internet of Things 平台入門範本」頁面上，選取要部署 Node-RED 的空間，驗證「建立應用程式」選項，然後按一下**建立**，將 Node-RED 新增至您的 Bluemix 組織。例如：<ul>
     <li> 空間：dev<li> 應用程式名稱：myDevice
     <li> 主機名稱：myDevice  
    </ul>  
其他選項保留預設值。部署應用程式之後，即會顯示「開始使用 Node-RED 撰寫程式碼」頁面。
**附註：**暫置程序可能需要一些時間。  

2. 按一下「路徑」鏈結，以開啟 Node-RED 或造訪「應用程式 URL」（例如 `http://myDevice.mybluemix.net`）  
3. 完成下列步驟，以**保護 Node-RED 編輯器**：
    1. 按**下一步**
    2. 新增使用者名稱和密碼。  
    **附註：**針對密碼，您必須符合必要的複雜性規則，否則**下一步**按鈕會保持灰色。  
    3. 選用。勾選**容許任何人檢視編輯器，但不進行任何變更**或**不建議：容許任何人存取編輯器並進行變更**
    4. 按一下**完成**，以完成配置。
4. 按一下**移至 Node-RED 流程編輯器**
5. 輸入先前所建立的使用者名稱和密碼。  
裝置模擬器流程可供流程編輯器使用。此流程會模擬**恆溫器**，將其位置、測量溫度及濕度資料傳送至 {{site.data.keyword.iot_short_notm}}。  
6. 藉由檢查針對**傳送至 IBM IoT Platform** 節點顯示具有已連接訊息的點，來確認裝置已連接。此檢查也適用於**溫度監視器**子流程的 **IBM IoT App In** 節點。  
7. 完成下列步驟，以傳送及接收 MQTT 裝置訊息：  
    1. 按一下**傳送資料**注入節點，以觸發要傳送至 {{site.data.keyword.iot_short_notm}} 的資料。
       **附註：**您可以啟動**除錯輸出有效負載**除錯節點來查看要傳送的資料，並檢查**裝置有效負載**功能節點來查看建置有效負載的程式碼。 
    2. 在按一下**傳送資料**之後，MQTT 訊息會傳送至 {{site.data.keyword.iot_short_notm}}，並由 **IBM IoT App In** 節點接收。**溫度監視器**子流程會建立溫度是否在定義的限制內，並在除錯標籤上顯示訊息。
       **附註：**按一下**溫度臨界值**切換式節點，以檢查並變更臨界值
    3. 檢查除錯標籤。即會顯示一則訊息，例如，**安全限制內的溫度 (17)**。
    
您現在已將 IoT 裝置範例連接至 {{site.data.keyword.iot_short_notm}}，並且可以看到裝置資料。
