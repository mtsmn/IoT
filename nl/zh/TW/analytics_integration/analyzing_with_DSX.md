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

若要完成這些步驟，您必須能夠存取 [{{site.data.keyword.iot_short_notm}} ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} 與 [Cloudant NoSQL DB ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog/services/cloudant-nosql-db){: new_window}、存取 [Apache Spark ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} 服務，以及存取 [DSX 帳戶 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window}。


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
2. 等待部署完成，然後導覽至 IBM Cloud 儀表板。
3. 啟動部署程序所建立的 {{site.data.keyword.iot_short_notm}} 服務 "wiotp-for-weather-sensors-simulator"。
4. 繼續進行[步驟 2. 配置資料庫連接器](#DSX_config_db)。


## 步驟 2. 配置資料庫連接器
{: #DSX_config_db}

根據選取的儲存區間隔選項，裝置資料可以儲存在 Cloudant 的每日、每週或每月資料庫。以相同的儲存區間隔（日、週或月）從所有裝置收集到的資料，都會儲存在相同的資料庫。

不論儲存區間隔配置為何，連接器都會自動建立三個資料庫。一個資料庫是針對現行儲存區間隔建立、一個是針對即將到來的間隔建立，還有一個是針對配置資料庫建立。到達間隔結尾時，新間隔的裝置資料會儲存在儲存區資料庫，並會為後續儲存區建立新的資料庫。

若要搭配使用 {{site.data.keyword.cloudant_short_notm}} 與 DSX，您必須配置平台資料儲存空間，以將 Cloudant NoSQL DB 用來作為歷程服務。

1. 在 {{site.data.keyword.cloudant_short_notm}} 儀表板上，按一下導覽列中的**延伸規格**。
2. 在**歷程資料儲存空間**下，按一下**設定**。**配置歷程資料儲存空間**區段會列出與 {{site.data.keyword.cloudant_short_notm}} 相同的 IBM Cloud 空間內提供的所有 Cloudant NoSQL DB 服務。
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
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. 設定預先配置的 Jupyter Notebook
{: #setup_jupyter_notebook}

Jupyter Notebook 是一種容許您建立及共用文件的 Web 應用程式，而文件包含執行碼、數學公式、圖形/視覺化 (matplotlib) 及註解文字。

若要設定預先配置的 Jupyter Notebook 以瞭解資料並且偵測異常，請執行下列動作：
1. 使用支援的瀏覽器，利用 IBM Cloud ID 來登入 [DSX ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://datascience.ibm.com/){: new_window}。
2. 按一下 **+ 新建專案**來建立新專案，然後選取 **Jupyter Notebook** 磚。專案會建立空間，讓您收集及共用記事本、連接至資料來源、建立管線，以及新增資料集。
3. 按一下 **+ 新增至專案**下拉清單，然後選取**連線**。從服務清單中，選取 **Cloudant**，然後將您在 {{site.data.keyword.cloudant_short_notm}} 服務頁面的「服務認證」標籤中找到的 Cloudant URL、「使用者名稱」及「密碼」輸入顯示的欄位中。指定連線的名稱，然後按一下**建立**。
4. 確認連線在儀表板的「資料資產」區段下變為可用。
5. 按一下 **+ 新建記事本**來建立新的 Jupyter 筆記本。
6. 從「服務」的「選取運行環境」下拉清單中，選取 **spark-iw**。如果這不存在，表示尚未正確佈建 Apache Spark 服務。請在「IBM Cloud 儀表板」上檢查它是否存在，如果不存在，則請造訪服務頁面予以佈建。
7. 將「語言」設為 **Python 2**，並將 Spark 版本設為 **2.1**。
8. 選取**從 URL** 以載入現有記事本，並指定記事本的敘述性名稱，然後輸入下列 URL 來開啟範例記事本：
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. 按一下**建立記事本**。請確認已使用 meta 資料及程式碼建立記事本。
10. 選取開頭為 '!pip install --upgrade pixiedust, ,' 的儲存格，然後按一下**執行**來執行程式碼。
11. 安裝完成時，請按一下**重新啟動核心**圖示或從「核心」功能表中選取**重新啟動**，以重新啟動 Spark 核心。
12. 大約等待 10 秒，讓核心重新啟動，然後按一下含有註解的空白程式碼儲存格以進行選取。
13. 完成下列步驟，將 Cloudant 認證匯入至該儲存格：

    1. 按一下 ![尋找並新增資料](images/find_add_data_icon.png)。
    2. 選取**連線**標籤。
    3. 按一下**插入至程式碼**。使用 Cloudant 認證建立稱為 "credentials_1" 的字典。如果未將名稱指定為 "credentials_1"，請將字典重新命名為 "credentials_1"，因為這是執行記事本程式碼所需的名稱。
14. 在含資料庫名稱 (dbname) 的儲存格中，輸入為資料來源的 Cloudant 資料庫名稱（例如，`iotp_yourWIoTPorgId_default_Year-month-day`）。
15. 儲存記事本。記事本已備妥，可以執行。


### 2. 執行分析
{: #run_analysis}

1.	選取包含 Cloudant 認證的儲存格。
2.	按一下**播放**以執行儲存格程式碼。
3.	檢查執行結果，並分析每一個儲存格中所使用的 Python 程式碼。
4.	針對每一個儲存格，重複步驟 2 及 3。針對已指定**需要使用者輸入**的儲存格，您必須在執行之前，將新輸入值提供給下一個程式碼儲存格中所定義的變數。

**附註：**部分儲存格會執行背景 Spark 工作，而且可能需要較長的時間才能完成。當儲存格內的程式碼執行完成時，星號 `*` 會變成數字，例如，In `[*]` 會變成 In `[1]`。完成這些步驟之後，您可以在 {{site.data.keyword.iot_short_notm}} 中建立雲端規則，以在偵測到異常時自動產生警示。


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


## 下一步為何？

如需 DSX 的相關資訊，請參閱下列資源：

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [DSX 社群記事本及指導教學 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window}，遵循這些鏈結以進一步瞭解 Jupyter 記事本。
 - [Watson IoT Platform 錦囊妙計中的分析秘訣 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
