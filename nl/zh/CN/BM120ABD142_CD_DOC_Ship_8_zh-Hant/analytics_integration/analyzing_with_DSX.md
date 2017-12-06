---

copyright:
  years: 2017
lastupdated: "2017-09-18"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 使用 Data Science Experience 分析資料
{: #DSX_integration}

您可以搭配使用 {{site.data.keyword.iot_full}} 與 Data Science Experience (DSX)，以視覺化並瞭解從連接至平台之裝置送出的資料。
{: shortdesc}

## 概觀及目標

本手冊會引導您逐步執行使用 DSX 作為分析工具來視覺化 {{site.data.keyword.iot_short_notm}} 裝置事件資料的程序。

DSX 提供一個互動式、協同的雲端型環境，可讓您使用多種工具來啟動其見解。請使用 IBM DSX 中提供的 Jupyter Notebook 來載入歷程 {{site.data.keyword.iot_short_notm}} 資料，然後使用資料來建立視覺化，以輔助進行資料分析及異常識別。您可以從歷程資料衍生臨界值，然後使用這些值在 {{site.data.keyword.iot_short_notm}} 中建立「雲端」規則。在與某個規則相關聯的 IoT 裝置發佈的讀數超出已配置的臨界值限制時，「雲端」規則會警示使用者。

您可以使用 {{site.data.keyword.cloudantfull}} NoSQL DB 服務，將傳送至 {{site.data.keyword.iot_short_notm}} 的裝置資料收集並儲存在 {{site.data.keyword.Bluemix}}。若要收集資料，您必須先將 {{site.data.keyword.iot_short_notm}} 連接至 {{site.data.keyword.cloudant_short_notm}} 服務。根據配置的儲存區間隔，將裝置資料儲存在 {{site.data.keyword.cloudant_short_notm}} 每日、每週或每月資料庫。


![使用 DSX 分析資料的概觀](images/DSX_overview.png)

在本手冊中，您將學習：

 - 如何配置平台資料儲存空間，以將 Cloudant NoSQL DB 用來作為歷程服務。
 - 如何使用 Weather Sensors 模擬器來產生平台所要使用的資料。
 - 如何匯出資料，然後將資料匯入至 DSX 來分析資料。
 - 如何視覺化 IoT 資料、偵測異常，以及配置警示。


## 必要條件

若要完成這些步驟，您必須能夠存取 [{{site.data.keyword.iot_short_notm}} ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} 與 [Cloudant NoSQL DB ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}，以及存取 [DSX 帳戶 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://datascience.ibm.com/docs/content/getting-started/get-started.html){: new_window}。


## 步驟 1. 設定模擬器
{:#DSX_sensor_data}

若要進行有意義的分析，您必須具有有意義的資料。您可以模擬實際感應器資料，以瞭解如何使用 DSX 來分析 Watson IoT Platform 裝置資料。此步驟提供下列指示：
 - [設定含**現有 {{site.data.keyword.iot_short_notm}} 實例**的模擬器](#sim_existing_platorm)
 - [設定含**新 {{site.data.keyword.iot_short_notm}} 實例**的模擬器](#sim_new_platform)


### 設定含現有 {{site.data.keyword.iot_short_notm}} 實例的 Weather Sensors 模擬器
{: #sim_existing_platform}

若要使用 Weather Sensors 模擬器來模擬組織的實際感應器資料事件，您必須先設定模擬器。這些步驟假設您已開始進行 {{site.data.keyword.iot_short_notm}} 實例。

1. [產生執行模擬器所需的 API 金鑰及記號。![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [部署 Weather Sensors 模擬器 Web 應用程式 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}，並遵循詳細指示。

   如需 Weather Sensors 的相關資訊，請參閱 [Weather Sensors 模擬器手冊 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}。
3. 繼續進行[步驟 2. 配置資料庫連接器](#DSX_config_db)。


### 設定含新 {{site.data.keyword.iot_short_notm}} 實例的 Weather Sensors 模擬器
{: #sim_new_platform}

若要使用 Weather Sensors 模擬器來模擬組織的實際感應器資料事件，您必須先設定模擬器。這些步驟包括建立 {{site.data.keyword.iot_short_notm}} 實例與模擬器的指示。

1. [部署含 {{site.data.keyword.iot_short_notm}} 實例的 Weather Sensors 模擬器 Web 應用程式 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window}，並遵循詳細步驟。

   如需 Weather Sensors 的相關資訊，請參閱 [Weather Sensors 模擬器手冊 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}。
2. 等待部署完成，然後導覽至 Bluemix 儀表板。
3. 啟動部署程序所建立的 {{site.data.keyword.iot_short_notm}} 服務 "wiotp-for-weather-sensors-simulator"。
4. 繼續進行[步驟 2. 配置資料庫連接器](#DSX_config_db)。


## 步驟 2. 配置資料庫連接器
{: #DSX_config_db}

根據選取的儲存區間隔選項，裝置資料可以儲存在 Cloudant 的每日、每週或每月資料庫。以相同的儲存區間隔（日、週或月）從所有裝置收集到的資料，都會儲存在相同的資料庫。

不論儲存區間隔配置為何，連接器都會自動建立三個資料庫。一個資料庫是針對現行儲存區間隔建立、一個是針對即將到來的間隔建立，還有一個是針對配置資料庫建立。到達間隔結尾時，新間隔的裝置資料會儲存在儲存區資料庫，並會為後續儲存區建立新的資料庫。

若要搭配使用 {{site.data.keyword.cloudant_short_notm}} 與 DSX，您必須配置平台資料儲存空間，以將 Cloudant NoSQL DB 用來作為歷程服務。

1. 在 {{site.data.keyword.cloudant_short_notm}} 儀表板上，按一下導覽列中的**延伸規格**。
2. 在**歷程資料儲存空間**下，按一下**設定**。**配置歷程資料儲存空間**區段會列出與 {{site.data.keyword.cloudant_short_notm}} 相同的 Bluemix 空間內提供的所有 Cloudant NoSQL DB 服務。
3. 選取您要連接的 Cloudant NoSQL DB 服務。
4. 指定下列 Cloudant NoSQL DB 配置選項：
  - 儲存區間隔 = 日
  - 時區 = UTC
  - 資料庫名稱 = default
5. 按一下**完成**，確認「Cloudant 服務」連線的授權。請確定在瀏覽器中已啟用蹦現視窗，以存取確認視窗。若已順利配置 Cloudant NoSQL DB，「歷程資料儲存空間」狀態會變更為「已配置」，而且裝置資料會儲存在 {{site.data.keyword.cloudant_short_notm}} NoSQL DB。
6. 繼續進行[步驟 3. 執行模擬器](#run_simulator)。


## 步驟 3. 執行模擬器
{: #run_simulator}

模擬器會將實際天氣感應器資料（來自位於「海法」區域的 17 個氣象站）發佈至您的 {{site.data.keyword.iot_short_notm}} 組織。

1. 導覽至模擬器。
2. 如果您已使用連結的 {{site.data.keyword.iot_short_notm}} 實例來部署模擬器，請繼續進行步驟 3。如果您已部署獨立式版本的模擬器，請輸入下列詳細資料：
   - Watson IoT Platform 組織
   - API 金鑰
   - 鑑別記號

3. 按一下**執行模擬器**。產生資料需要幾分鐘的時間。
4. 在模擬器執行時移至 Watson IoT Platform，並驗證裝置已建立，而且事件將進入這些裝置。
5. 繼續進行[步驟 4. 設定 DSX 並視覺化資料](#DSX_visualize_data)。


## 步驟 4. 設定 DSX 並視覺化資料
{: #DSX_visualize_data}

若要設定 DSX，並開始視覺化資料，請執行下列動作：

1. [設定預先配置的 Jupyter Notebook](#setup_jupyter_notebook)，以瞭解資料並且偵測異常。
2. [執行分析。](#run_analysis)
3. [配置感應器異常的警示](#config_alerts)。


### 1. 設定預先配置的 Jupyter Notebook
{: #setup_jupyter_notebook}

Jupyter Notebook 是一種容許您建立及共用文件的 Web 應用程式，而文件包含執行碼、數學公式、圖形/視覺化 (matplotlib) 及註解文字。

若要設定預先配置的 Jupyter Notebook 以瞭解資料並且偵測異常，請執行下列動作：
1. 使用支援的瀏覽器，利用 Bluemix ID 來登入 [DSX ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://datascience.ibm.com/){: new_window}。
2. 按一下 "+"，然後選取**建立專案**來建立新的專案。專案會建立空間，讓您收集及共用記事本、連接至資料來源、建立管線，以及新增資料集。
3. 指定專案名稱，然後按一下**建立**。在 DSX 帳戶設定期間，會自動建立 Spark 服務及 Object Storage 實例。或者，您也可以使用 Bluemix 介面來建立它們，然後在之後的階段將它們與 DSX 專案相關聯。
4. 在 **DSX** 功能表中選取專案，然後按一下 ![「尋找並新增資料」圖示](images/find_add_data_icon.png)，以匯入 Cloudant 認證。
5. 按一下**連線**標籤。
6. 按一下**建立連線**來匯入 Cloudant 認證，以在專案的任何記事本中使用這些認證。
7. 輸入下列資訊，以完成**新建連線**畫面：
   - 連線名稱
   - 將**服務種類**設為`資料服務`。
   - 從**目標服務實例**下拉功能表中，選取 Cloudant 服務。
   - 選取對應至現行日期的 Cloudant 資料庫。
8. 按一下**建立**。
9. 按一下**新增記事本**，以建立新的 Jupyter 筆記本。
10. 選取**從 URL** 以載入現有記事本，並指定記事本的敘述性名稱，然後輸入下列 URL 來開啟範例記事本：
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```
11. 按一下**建立記事本**。請確認已使用 meta 資料及程式碼建立記事本。
12. 選取開頭為 '!pip install --upgrade pixiedust, ,' 的儲存格，然後按一下**播放**來執行程式碼。
13. 安裝完成時，請按一下**重新啟動核心**圖示，以重新啟動 Spark 核心。
14. 大約等待 10 秒，讓核心重新啟動，然後按一下含有註解的儲存格以進行選取。
15. 完成下列步驟，將 Cloudant 認證匯入至該儲存格：

    1. 按一下 ![「尋找並新增資料」圖示](images/find_add_data_icon.png)。
    2. 選取**連線**標籤。
    3. 按一下**插入至程式碼**。
使用 Cloudant 認證建立稱為 "credentials_1" 的字典。如果未將名稱指定為 "credentials_1"，請將字典重新命名為 "credentials_1"，因為這是執行記事本程式碼所需的名稱。
16. 在含資料庫名稱 (dbname) 的儲存格中，輸入為資料來源的 Cloudant 資料庫名稱（例如，`iotp_yourWIoTPorgId_default_Year-month-day`）。
17. 儲存記事本。記事本已備妥，可以執行。


### 2. 執行分析
{: #run_analysis}

1.	選取包含 Cloudant 認證的儲存格。
2.	按一下**播放**以執行儲存格程式碼。
3.	檢查執行結果，並分析每一個儲存格中所使用的 Python 程式碼。
4.	針對每一個儲存格，重複步驟 2 及 3。針對已指定**需要使用者輸入**的儲存格，您必須在執行之前，將新輸入值提供給下一個程式碼儲存格中所定義的變數。

**附註：**部分儲存格會執行背景 Spark 工作，而且可能需要較長的時間才能完成。當儲存格內的程式碼執行完成時，星號 `*` 會變成數字，例如，In `[*]` 會變成 In `[1]`。完成這些步驟之後，您可以在 {{site.data.keyword.iot_short_notm}} 中建立雲端規則，以在偵測到異常時自動產生警示。


### 3. 配置感應器異常的警示
{: #config_alerts}


您可以在 {{site.data.keyword.iot_short_notm}} 中建立雲端規則。如果在發佈的事件數超過記事本中所衍生的臨界值時偵測到異常，則這些規則可以產生警示。

在此範例中，我們使用 Nitrogen Dioxide (NO2) 及某個特定裝置的高低臨界值。我們將會建立電子郵件動作，因此只要 NO2 值超過我們所設定的臨界值，就會將電子郵件傳送至指定的電子郵件位址。

若要建立雲端規則，請執行下列動作：

1. 建立綱目：
    1. 在 {{site.data.keyword.iot_short_notm}} 儀表板的**裝置**標籤中，選取**管理綱目**標籤。
    2. 按一下**新增綱目**。
    3. 選取建立綱目的 DeviceType WS，然後按**下一步**。
    4. 按一下**新增內容**，以從連接的裝置中新增資料點。
    5. 從**手動**標籤中，將資料類型欄位設為 `Float`，並將內容欄位設為 `NO2`。
    6. 按一下**確定**。
    7. 按一下**完成**。

2. 建立動作：
    1. 選取 {{site.data.keyword.iot_short_notm}} 儀表板中的**規則**標籤。
    2. 選取**動作**標籤。
    3. 按一下 **+ 建立動作**。
    4. 在**建立新的動作**畫面中，輸入名稱，然後選取「傳送電子郵件」作為動作類型。
    5. 按**下一步**。
    6. 在**編輯動作**畫面中，開啟**包括資料**切換開關。
    7. 按一下**完成**。
    8. 從**規則**標籤中，選取**瀏覽**標籤。
    9. 按一下 **+ 建立雲端規則**。
    10. 在**新增雲端規則**畫面中，輸入規則的名稱，然後在**套用至**欄位中選取綱目名稱。在此範例中，綱目名稱是 "WS"。
    11. 按**下一步**。

3. 設定條件 - 在 NO2 圖表旁邊，於記事本中執行的最後一個程式碼片段中，可以找到您在這些步驟中使用的臨界值：
    1. 按一下「新建條件」，然後設定第一個條件：
    - 在**內容**欄位中，輸入 `Nitrogen Dioxide`。
    - 在**運算子**欄位中，選取大於圖示 `>`。
    - 在**值**欄位中，輸入高臨界值。
    - 按一下**確定**。
    2. 選取 OR，然後新增第二個條件：
    - 在**內容**欄位中，輸入 `Nitrogen Dioxide`。
    - 在**運算子**欄位中，選取小於圖示 `<`。
    - 在**值**欄位中，輸入低臨界值。
    - 按一下**確定**。現在已設定觸發規則的條件。
4. 將動作設為「傳送電子郵件」，然後按一下**確定**以啟動規則。只要裝置所發佈的 Nitrogen Dioxide 讀數值超過任一臨界值，就會產生電子郵件警示。您可以執行模擬器，以查看警示電子郵件。


## 下一步為何？

如需 DSX 的相關資訊，請參閱下列資源：

 - [WIoTP 雲端規則 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window}
 - [DSX 社群記事本及指導教學 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window}，遵循這些鏈結以進一步瞭解 Jupyter 記事本。
 - [Watson IoT Platform 錦囊妙計中的分析秘訣 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
