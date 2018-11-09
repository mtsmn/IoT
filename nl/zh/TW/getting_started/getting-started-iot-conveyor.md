---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# 手冊 1：連接輸送帶裝置  
使用將監視資料傳送至 {{site.data.keyword.Bluemix_notm}} 上的 {{site.data.keyword.iot_full}} 的 IoT 裝置來建立基本輸送帶。
{:shortdesc}

## 概觀及目標
{: #overview}  
本手冊是系列中的第一個項目，可引導您逐步執行將裝置連接至 {{site.data.keyword.iot_short_notm}}、監視及處理裝置資料等作業的程序。每一個手冊都以前一個手冊為建置基礎，而且每一個手冊中的範例程式碼都作為下一個手冊的輸入。您也可以藉由修改範例程式碼來滿足您的目的，以獨立使用這些手冊。

在這第一個手冊中，我們設定一個已連接的輸送帶，並使用它將 IoT 資料傳送至 {{site.data.keyword.iot_short_notm}}。根據您的技能水準，您可以追蹤下列其中一個或兩個路徑來設定輸送帶：
- 路徑 A  
此路徑可讓您在 {{site.data.keyword.Bluemix_notm}} 上安裝輸送帶模擬器應用程式，以快速啟動。該應用程式會向 {{site.data.keyword.iot_short_notm}} 自行登錄裝置，並自動將格式完整的資料傳送至平台。此路徑的指示位於[步驟 2A - 使用 GitHub 中的模擬器範例應用程式](#deploy_app)。  
- 路徑 B  
此路徑在技術上頗具挑戰性，並且需要額外的硬體、Python 程式設計技術，以及向 {{site.data.keyword.iot_short_notm}} 手動登錄您的裝置。此路徑的指示位於[步驟 2B - 使用 Raspberry Pi 建置實體輸送帶及電動馬達](#raspberry)。

在本手冊中，您將：
- 使用 Cloud Foundry CLI 來建立及部署 {{site.data.keyword.iot_short_notm}} 組織。
- 建置並部署範例輸送帶裝置。
- 將模擬的輸送帶裝置連接至 {{site.data.keyword.iot_short_notm}}。

若要使用不同的 IoT 裝置來開始使用 {{site.data.keyword.iot_short_notm}}，請參閱[入門指導教學](/docs/services/IoT/getting-started.html)。
{: tip}

## 必要條件
{: #prereqs}

您需要下列帳戶及工具：
* [{{site.data.keyword.Bluemix_notm}} 帳戶 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.ng.bluemix.net/registration/){: new_window}
* [Cloud Foundry 指令行介面 (cf CLI) ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
使用 cf CLI 將應用程式及服務部署至 {{site.data.keyword.Bluemix_notm}}。如需相關資訊，請參閱 [Cloud Foundry CLI 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.cloudfoundry.org/cf-cli/){: new_window}
* 選用項目：[Git ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://git-scm.com/downloads){: new_window}  
如果您選擇使用 Git 來下載程式碼範例，則必須同時具有 [GitHub.com 帳戶 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com){: new_window}。您也可以將程式碼下載為壓縮檔，而不需要使用 GitHub.com 帳戶。
* 選用項目：要執行*輸送帶* 範例 Web 應用程式以傳送加速感應器資料的行動電話。

## 步驟 1 - 部署 {{site.data.keyword.iot_short_notm}}  
{: #deploy_watson_iot_platform_service}
接下來的步驟將會在 {{site.data.keyword.Bluemix_notm}} 環境中部署名稱為 `iotp-for-conveyor` 的 {{site.data.keyword.iot_short_notm}} 服務實例。如果您已執行服務實例，則可以搭配使用該實例與本手冊，並跳過這個首要步驟。在繼續使用本手冊時，只需要確定您使用正確的服務名稱及 {{site.data.keyword.Bluemix_notm}} 空間。
{: tip}
1. 從指令行中，執行 cf api 指令來設定 API 端點。   
請將 `API-ENDPOINT` 值取代為您地區的 API 端點。
   ```
cf api API-ENDPOINT
   ```
範例：`cf api https://api.ng.bluemix.net`
<table>
<tr>
<th>地區</th>
<th>API 端點</th>
</tr>
<tr>
<td>美國南部</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>英國</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. 登入 {{site.data.keyword.Bluemix_notm}} 帳戶。
  ```
cf login
  ```
系統提示時，請選取您要在其中部署 {{site.data.keyword.iot_short_notm}} 及範例應用程式的組織及空間。
3. 將 {{site.data.keyword.iot_short_notm}} 服務部署至 {{site.data.keyword.Bluemix_notm}}。  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
   ```
針對 YOUR_IOT_PLATFORM_NAME，使用 *iotp-for-conveyor*。  
範例：`cf create-service iotf-service iotf-service-free iotp-for-conveyor`
3. 建立範例輸送帶裝置。  
 - 路徑 A：[步驟 2A - 使用 GitHub 中的模擬器範例應用程式](#deploy_app)。  
 - 路徑 B：[步驟 2B - 使用 Raspberry Pi 建置實體輸送帶及電動馬達](#raspberry)。  

## 步驟 2A - 部署範例輸送帶 Web 應用程式  
{: #deploy_app}

範例應用程式可讓您模擬 {{site.data.keyword.Bluemix_notm}} 連接的產業輸送帶。您可以啟動及停止輸送帶，並調整輸送帶的速度。會以應用程式中所顯示的 MQTT 訊息形式，將輸送帶的每個變更傳送至 {{site.data.keyword.Bluemix_notm}}。您可以使用預設儀表板卡片來監視輸送帶行為。範例應用程式是使用下列位置的 Node.js 用戶端程式庫所建置：[https://github.com/ibm-watson-iot/iot-nodejs ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![輸送帶應用程式](images/app_conveyor_belt.png "輸送帶應用程式")

1. 使用您慣用的 Git 工具來複製下列儲存庫：  
https://github.com/ibm-watson-iot/guide-conveyor-simulator
在 Git Shell 中，使用下列指令：
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. 在指令行上，將目錄切換至範例應用程式所在的目錄。
  ```bash
cd guide-conveyor-simulator
  ```
3. 從 *guide-conveyor-simulator* 目錄中，將您的應用程式推送至 {{site.data.keyword.Bluemix_notm}}，並取代 cf push 指令中的 *YOUR_APP_NAME*，以提供它新的名稱。因為在將應用程式連結至 {{site.data.keyword.iot_short_notm}} 之後，您將會在下一個階段中啟動該應用程式，所以請使用 `--no-start` 選項。
**附註：**部署應用程式可能需要一些時間。
  ```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. 在 *guide-conveyor-simulator* 目錄中，使用您為每一個實例提供的名稱，將應用程式連結至 {{site.data.keyword.iot_short_notm}} 實例。
  ```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
如需連結應用程式的相關資訊，請參閱[連接應用程式](/docs/services/IoT/platform_authorization.html#bluemix-binding)。
2. 啟動應用程式，以讓連結生效。
  ```bash
cf start YOUR_APP_NAME
  ```
在此階段，會將您的範例 Web 應用程式部署在 {{site.data.keyword.Bluemix_notm}}。
部署完成時，會顯示一則訊息，指出您的應用程式正在執行。   
範例：
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
start command:     ./vendor/initial_startup.rb

     state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
若要查看應用程式部署狀態及應用程式 URL，您可以執行下列指令：
  ```bash
cf apps
  ```
使用 `cf logs YOUR_APP_NAME --recent` 指令，以疑難排解部署程序中的錯誤。
{: tip}
1. 在瀏覽器中，存取應用程式。  
開啟下列 URL：`https://YOUR_APP_NAME.mybluemix.net`    
範例：`https://conveyorbelt.mybluemix.net/`。
2. 輸入您裝置的裝置 ID 及記號。  
範例應用程式會使用您提供的裝置 ID 及記號，自動登錄類型為 `iot-conveyor-belt` 的裝置。如需登錄裝置的相關資訊，請參閱[連接裝置](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1)。
4. 繼續進行[步驟 3 - 在 {{site.data.keyword.iot_short_notm}} 中查看原始資料](#see_live_data)。

## 步驟 2B - 建置 Raspberry Pi 加強型輸送帶
{: #raspberry}

Raspberry Pi 解決方案是使用下列位置的 Python 用戶端程式庫所建置：[https://github.com/ibm-watson-iot/iot-python ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/iot-python){: new_window}

### 電路的示意圖

![電路圖](images/raspi_circuit.png)

### 必要連線

Raspberry Pi 與 L298N Dual H Bridge Motor Driver Board 之間的下列連線是必要項目。請：

- 將 Raspberry Pi 的插腳 2 連接至 L298N 板上的 +5v
- 將 Raspberry Pi 的插腳 6 連接至 L298N 板上的 GND
- 將 Raspberry Pi 的插腳 13 連接至 L298N 板的 IN1
- 將 Raspberry Pi 的插腳 15 連接至 L298N 板的 IN2
- 將電池的 +9v 連接至 L298N 板上的 +12v
- 將電池的 -9v 連接至 L298N 板上的 GND
- 將馬達的 +ve 節點連接至 L298N 板上的 OUT1
- 將馬達的 -ve 節點連接至 L298N 板上的 OUT2

### 硬體需求

1. 含 Raspbian Jessie (8.x) 的 [Raspberry Pi(2/3) ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.raspberrypi.org/){: new_window}。
2. [L298N Dual H Bridge Motor Driver Board ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. 9V DC 馬達
4. 9V 電池

### Raspberry Pi 軟體需求
- 與 Raspbian Jessie (8.x) 一起安裝的 Python 2.7.9 及更高版本。
- [Watson IoT Python 程式庫 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- 選用項目：Git。  
如果您的 Raspberry Pi 上未安裝 Git，則可以使用下列指令進行安裝：  
```bash
$ sudo apt-get install git
```

### 詳細步驟

1. 開啟終端機，或 SSH 到 Raspberry Pi。
2. 使用您慣用的 Git 工具，以將下列儲存庫複製到 Raspberry Pi：  
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
在 Git Shell 中，使用下列指令：
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. 向 {{site.data.keyword.iot_short_notm}} 登錄裝置。  
如需登錄裝置的相關資訊，請參閱[連接裝置](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1)。
	 1. 在 {{site.data.keyword.Bluemix_notm}} 主控台中，按一下 {{site.data.keyword.iot_short_notm}} 服務詳細資料頁面上的**啟動**。{{site.data.keyword.iot_short_notm}} Web 主控台會在新瀏覽器分頁中開啟，其 URL 為：
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     其中，ORG_ID 是 [{{site.data.keyword.iot_short_notm}} 組織](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}的唯一六個字元 ID。
	 2. 在「概觀」儀表板中，從功能表窗格中選取**裝置**，然後按一下**新增裝置**。
	 3. 建立您要新增之裝置的裝置類型。
	     1. 按一下**建立裝置類型**。
	     2. 輸入裝置類型的裝置類型名稱 `iotp-for-conveyor` 及說明。  
	**提示：**您可以輸入任何裝置類型名稱，但系列中的其他手冊預期裝置使用裝置類型 `iotp-for-convor`。如果您使用不同裝置類型，則必須相應地修改其他手冊中的設定。
	     3. 選用項目：輸入裝置類型屬性和 meta 資料。  
 
	 4. 按**下一步**，開始新增所選取裝置類型的裝置。
	 5. 輸入裝置 ID（例如，`conveyor_belt`）。
	 5. 按**下一步**，以完成處理程序。
	 6. 提供鑑別記號，或接受自動產生的記號。
	 7. 驗證摘要資訊正確無誤，然後按一下**新增**，以新增連線。
	 8. 在裝置資訊頁面中，複製並儲存下列詳細資料：
	     * 組織 ID
  
	     * 裝置類型
	     * 裝置 ID
	     * 鑑別方法
	     * 鑑別記號
	     您需要有「組織 ID」、「裝置類型」、「裝置 ID」及「鑑別記號」的值，才能配置您的裝置來連接至 {{site.data.keyword.iot_short_notm}}。
4. 導覽至所複製儲存庫的 *guide-conveyor-rasp-pi* 根目錄。
5. 使用此指令 `sudo chmod +x setup.sh`，將 Shell Script 設為執行檔
6. 執行 *setup.sh* 檔案，並在系統提示時，輸入您從裝置資訊頁面中複製而來的詳細資料。
```bash  
./setup.sh
```  

7. 執行 deviceClient 程式。  
在執行程式時，Raspberry Pi 會啟動馬達，並且會根據參數設定，最多執行 2 分鐘。
```bash
python deviceClient.py -t 2
```

馬達正在執行時，程式會發佈事件類型為 `sensorData` 且具有下列範例有效負載結構的事件：  
```
{
"d": {
        "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. 繼續進行[步驟 3 - 在 {{site.data.keyword.iot_short_notm}} 中查看原始資料](#see_live_data)。

## 步驟 3 - 在 {{site.data.keyword.iot_short_notm}} 中查看原始資料
{: #see_live_data}
1. 驗證已向 {{site.data.keyword.iot_short_notm}} 登錄裝置。
 1. 登入 {{site.data.keyword.Bluemix_notm}} 儀表板，網址為：https://bluemix.net
 2. 從[服務清單 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://bluemix.net/dashboard/services){: new_window} 中，按一下 *iotp-for-conveyor* {{site.data.keyword.iot_short_notm}} 服務。
 3. 按一下*啟動*，以在新的瀏覽器標籤中開啟 {{site.data.keyword.iot_short_notm}} 儀表板。  
您可以將 URL 加入書籤，以方便稍後進行存取。   
範例：`https://*iot-org-id*.internetofthings.ibmcloud.com`。
 4. 從功能表中，選取**裝置**，並驗證已顯示新的裝置。
2. 將感應器資料傳送至平台。   
感應器讀數變更時，裝置會將資料傳送至 {{site.data.keyword.iot_short_notm}}。您可以停止、啟動或變更輸送帶的速度，來模擬此資料傳送。   
**路徑 A：**如果您要在行動瀏覽器上存取應用程式，請嘗試搖動智慧型手機，以觸發輸送帶的加速感應器資料。
{: tip}

## 下一步為何？
{: @whats_next}  
繼續進行下一個手冊，或跳至其他感興趣的主題：
- 路徑 A：修改輸送帶應用程式，以符合您的需求。  
如需技術詳細資料，請參閱：
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Node.js clienmt 程式庫 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **附註：**Bluemix 現在是 IBM Cloud。如需詳細資料，請參閱 [IBM Cloud 部落格](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")。
 
- 路徑 B：修改 Raspberry Pi 設定，以符合您的需求。  
如需技術詳細資料，請參閱：
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}


**附註：**Bluemix 現在是 IBM Cloud。如需詳細資料，請參閱 [IBM Cloud 部落格](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")。

- [手冊 2：監視裝置資料](getting-started-iot-monitoring.html)  
您已連接一個以上的裝置，並已開始善用裝置資料，所以現在可以開始監視裝置的集合。
- [手冊 3：模擬大量裝置](getting-started-iot-large-scale-simulation.html)  
路徑 A 中的輸送帶範例應用程式可讓您手動模擬一或數個輸送帶裝置。本手冊可讓您設定含有大量裝置的模擬環境。
- [進一步瞭解 {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html)
- [進一步瞭解 {{site.data.keyword.iot_short_notm}} API](/docs/services/IoT/reference/api.html)
