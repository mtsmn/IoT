---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 開始使用 {{site.data.keyword.iot_short_notm}}
{: #gettingstartedtemplate}

{{site.data.keyword.iot_full}} for {{site.data.keyword.Bluemix_notm}} 提供一個多用途的工具箱，其包含閘道裝置、裝置管理，以及功能強大的應用程式存取權。您可以使用 {{site.data.keyword.iot_short_notm}}，收集連接裝置的資料以及分析組織產生的即時資料。
{:shortdesc}

## 開始之前
{: #byb}

在連接裝置及使用資料之前，請登錄 {{site.data.keyword.Bluemix_notm}} 帳戶，並在 {{site.data.keyword.Bluemix_notm}} 組織中建立 {{site.data.keyword.iot_short_notm}} 服務實例。您可以直接從 [IBM Cloud 服務型錄中的 {{site.data.keyword.iot_short_notm}} 頁面 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window} 建立 {{site.data.keyword.iot_short_notm}} 實例。  

如需如何在 {{site.data.keyword.Bluemix_notm}} 上註冊帳戶、配置地區以及其他帳戶管理設定的詳細資訊，請參閱[註冊 IBM Cloud](https://console.bluemix.net/docs/account/adminpublic.html#signing-up-for-ibm-cloud)。


您可以從儀表板設定及配置 {{site.data.keyword.iot_short_notm}} 實例。若要開啟儀表板，請移至 {{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.iot_short_notm}} 服務實例，然後按一下**啟動**。

## 關於此作業

下列步驟說明如何快速開始使用 {{site.data.keyword.iot_short_notm}} 服務。

同時提供一組更詳細的開始使用手冊及範例應用程式，可逐步完成使用 {{site.data.keyword.iot_short_notm}} 來開發現成可用的端對端 IoT 原型系統的基本觀念。如果您是使用 {{site.data.keyword.iot_short_notm}} 的新手開發人員，請使用[開始使用手冊](https://console.bluemix.net/docs/services/IoT/getting_started/getting-started-iot-overview.html#getting-started)小節中的逐步程序。

## 步驟 1：連接您的裝置
{: #up_and_running}

若要使用服務開始進行，請根據您的狀況來探索下列選項：

|  |已部署的服務|未部署的服務
 | -------------| ------------- | -------------
  |**我有要連接的裝置**|[將您的裝置連接至 {{site.data.keyword.iot_short_notm}}](iotplatform_task.html#iotplatform_task)。|在 [Play with {{site.data.keyword.iot_short_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play){:new_window} 中探索裝置連線。
  |**我沒有要連接的裝置** | [建立並連接 Node-RED 裝置模擬器](nodereddevice_sample.html){:new_window}。或[連接智慧型手機 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://discover-iot.eu-gb.mybluemix.net/?cm_mc_uid=44491599487314618721024&cm_mc_sid_50200000=1462798151#/play/device/smartphone){:new_window}。|開始使用 [Watson IoT Platform 入門範本](https://console.bluemix.net/docs/starters/IoT-starter/iot500.html)。

  
如需如何將特定裝置類型連接至 {{site.data.keyword.iot_short_notm}} 的相關資訊，請參閱 [developerWorks 秘訣 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){:new_window}。  

如需裝置連線開發人員文件，請參閱：
- [裝置的 MQTT 連線功能](devices/mqtt.html)。
- [閘道的 MQTT 連線功能](gateways/mqtt.html)。

<!--
## Step 2: Analyze your device data
{: #analyzing_data}
Start exploring the real-time data that the devices are sending to {{site.data.keyword.iot_short_notm}}.
{{site.data.keyword.iot_short_notm}} includes the following analytics tools:  
- [Boards and cards](data_visualization.html) to visualize your real-time device data.
- [Rules and actions](analytics.html) that are triggered by real-time device data.
For a quick getting started example, see the [Using Rules and Actions with IBM Watson IoT Platform Cloud Analytics ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/using-rules-and-actions-with-ibm-watson-iot-platform-cloud-analytics/){:new_window} developerWorks recipe.
-->

## 步驟 2：建立應用程式來使用您的裝置資料
{: #develop_applications}

建立並連接您自己的應用程式來使用裝置資料。

如需相關資訊，請參閱下列主題：   
- 探索[應用程式開發人員文件](applications/api.html)和 [{{site.data.keyword.iot_short_notm}} API 文件](reference/api.html)。
- 探索 [{{site.data.keyword.iot_short_notm}} 用戶端程式庫](iot_platform_client_lib.html)，其提供建置及開發程式碼的工具和檔案，可用來整合及連接裝置與應用程式。
- [將 {{site.data.keyword.cloudantfull}} 服務](cloudant_connector.html)連接至 {{site.data.keyword.iot_short_notm}}，以儲存歷程裝置資料。
- 使用新的[內嵌規則（測試版）](information_management/im_rules.html)特性建立自己的規則。
