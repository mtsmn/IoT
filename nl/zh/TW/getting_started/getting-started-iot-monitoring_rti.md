---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 手冊 3：監視裝置資料
您已連接一個以上的裝置，所以現在可以開始監視裝置資料。
{:shortdesc}

## 概觀及目標
{: #overview}  

在本手冊中，您將在 {{site.data.keyword.Bluemix}} 上部署監視應用程式，以從裝置中檢視資料。

就像手冊 1，您可以追蹤下列其中一個或兩個路徑：
- 路徑 A：[步驟 1A - 部署及連接監視 Web 應用程式](#deploy_app)  
透過遵循路徑 A，您將部署現成可用的應用程式，以監視您正在組織中執行的輸送帶裝置。
- 路徑 B：[步驟 1B - 使用小組件程式庫建立監視使用者介面](#widget-library)
稍微複雜的路徑 B 會建立小組件程式庫，並引導您建立一些基本使用者介面。

## 必要條件
{: #prereqs}  
開始之前，請確定符合下列需求。

### 本端環境
- [Node.js ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://nodejs.org){: new_window} 6.x 版或更新版本。
- 路徑 A：[Angular CLI ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/angular/angular-cli){: new_window} 1.x 版或更新版本。  
- [Cloud Foundry CLI ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
使用 cf CLI 將應用程式及服務部署至 {{site.data.keyword.Bluemix_notm}}。如需相關資訊，請參閱 [Cloud Foundry CLI 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
- 選用項目：[Git ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://git-scm.com/downloads){: new_window}  
如果您選擇使用 Git 來下載程式碼範例，則必須同時具有 [GitHub.com 帳戶 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com){: new_window}。您也可以將程式碼下載為壓縮檔，而不需要使用 GitHub.com 帳戶。

### 其他需求
您也必須具有裝置類型為 `iot-conveyor-belt` 的已連接裝置，以傳送事件名稱為 `sensorData` 且訊息有效負載包括下列內容的事件：
```
{
	"d": {
		"id": "belt1", 		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
如需裝置事件及傳訊格式的相關資訊，請參閱[發佈事件](/docs/services/IoT/devices/mqtt.html#publishing_events)。  
如果您已完成[手冊 1：開始使用 {{site.data.keyword.iot_short_notm}} 及模擬輸送帶](getting-started-iot-conveyor.html)，表示已設定所有項目。  
{: tip}

## 步驟 1A - 部署及連接監視 Web 應用程式
{: #deploy_app}

Plant Floor Monitoring 範例應用程式列出所有已連接至 {{site.data.keyword.iot_short_notm}} 組織的 iot-conveyor-belt 類型裝置，其中同時包含事件資料的子集（例如 RPM、前次更新及裝置 ID）。

範例應用程式是使用下列位置的 Node.js 用戶端程式庫所建置：[https://github.com/ibm-watson-iot/iot-nodejs ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![Node.js 型監視應用程式](images/app_monitor.png "Node.js 型監視應用程式")

在此步驟中，您將：
- 使用 Cloud Foundry，部署 GitHub 來源範例監視 Web 應用程式。
- 使用 API 金鑰及鑑別記號，將範例應用程式配置為連接至 {{site.data.keyword.iot_short_notm}}。
- 使用 Web 應用程式來監視已連接的輸送帶裝置。  

### 部署及連接監視應用程式的詳細步驟
下列步驟引導您在 {{site.data.keyword.Bluemix_notm}} 上建立及部署應用程式。如需在本端執行應用程式的相關資訊，請參閱 GitHub 中的 README 檔。
1. 複製 Node.js *Plant Floor Monitoring* 範例應用程式 GitHub 儲存庫。  
使用您慣用的 Git 工具來複製下列儲存庫：  
https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
在 Git Shell 中，使用下列指令：
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
  ```
2. 建立應用程式的 API 金鑰及鑑別記號組合。  
在配置應用程式以連接至組織時，您需要這些項目。如需登錄裝置的相關資訊，請參閱[連接應用程式](/docs/services/IoT/platform_authorization.html)。  
 1. 開啟 {{site.data.keyword.iot_short_notm}} 儀表板。
 2. 選取**應用程式**。
 3. 按一下**產生 API 金鑰**
 4. 複製列出的 API 金鑰及鑑別記號值。
 5. 選取**視覺化應用程式**作為 API 角色。  
**提示：**如果您將其他特性新增至應用程式，則需要將它提升為更高的角色。
 6. 新增註解，以輕鬆識別此 API 金鑰及鑑別記號組合。
 7. 按一下**產生**。
3. 配置應用程式以連接至 {{site.data.keyword.Bluemix_notm}}。
導覽至 *guide-conveyor-ui-angular* 儲存庫的根目錄，並建立包含下列內容的 `basicConfig.json` 檔案：
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
將參數值取代為 {{site.data.keyword.Bluemix_notm}} 組織的對應值：您剛才建立的 orgID、API 金鑰及鑑別記號。  
範例：
```
 {   
  "org": "3v5whr",    
  "apiKey": "a-3v5whr-jhkmsghlqv",  
  "apiToken": "-q0MkPN2cNYB6+?ISk"  
}
```
4. 使用 cloudfoundry CLI，登入 {{site.data.keyword.Bluemix_notm}} 帳戶。  
如需相關資訊，請參閱 [Cloud Foundry CLI 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
從指令行中，輸入下列指令：  
  ```
cf login
  ```
系統提示時，請選取您要在其中部署監視範例應用程式的組織及空間。
5. 必要的話，請執行 cf api 指令來設定 API 端點。   
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
6. 將目錄切換至範例應用程式所在的目錄。  
  ```
cd guide-conveyor-ui-angular
  ```
7. 執行 `npm install -g @angular/cli` 來安裝 Angular CLI。
8. 執行 `npm install`。
9. 執行 `npm run push` 來建置專案，並推送至您的組織。  
您的範例 Web 應用程式會部署在 {{site.data.keyword.Bluemix_notm}}。  
部署完成時，會顯示一則訊息，指出您的應用程式正在執行。   
範例：  
  ```
requested state: started
instances: 1/1
usage: 64M x 1 instances
urls: iotmonitoringcontrol-undertided-eng.mybluemix.net
last uploaded: Tue May 16 19:01:13 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     state     since                    cpu    memory     disk        details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
10. 開啟 Web 應用程式。  
從 {{site.data.keyword.Bluemix_notm}} 應用程式儀表板中，按一下**造訪應用程式 URL** 來開啟 Web 應用程式。  
按一下**路徑**，以存取及管理應用程式 URL。   
預設 URL 與下列類似：  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## 步驟 1B - 使用小組件程式庫建立監視使用者介面
{: #widget-library}

小組件程式庫型範例應用程式包括一個馬達速度量規、一個加速感應器資料量規及一個馬達速度圖，此圖顯示連接至 {{site.data.keyword.iot_short_notm}} 組織之單一 iot-conveyor-belt 類型裝置的資料。您可以使用範例程式碼，為 {{site.data.keyword.iot_short_notm}} 已連接裝置建置完整前端應用程式。

![小組件程式庫型監視應用程式](images/app_monitor_b.png "小組件程式庫型監視應用程式")

在此步驟中，您將：
- 使用 Cloud Foundry，部署 GitHub 來源範例 Web 應用程式。
- 使用 API 金鑰及鑑別記號，將範例應用程式配置為連接至 {{site.data.keyword.iot_short_notm}}。
- 配置三個使用者介面小組件，以量規及圖形顯示裝置資料。
- 使用 Web 應用程式來監視已連接的輸送帶裝置。  

### 使用小組件程式庫建立監視使用者介面的詳細步驟
下列步驟引導您在 {{site.data.keyword.Bluemix_notm}} 上建立及部署應用程式。如需在本端執行應用程式的相關資訊，請參閱 GitHub 中的 README 檔。
1. 複製 *Widget Library Monitoring* 範例應用程式 GitHub 儲存庫。  
使用您慣用的 Git 工具來複製下列儲存庫：  
https://github.com/ibm-watson-iot/guide-conveyor-ui-html
在 Git Shell 中，使用下列指令：
```
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-html
```
2. 安裝應用程式相依關係。  
導覽至 *guide-conveyor-ui-html* 儲存庫的根目錄，並執行下列指令：
```
npm install
```
3. 建構使用者介面。  
若要建置應用程式使用者介面，您必須在應用程式 index.html 檔案中，將每一個使用者介面元件的小組件新增為 JavaScript 程式碼。  
每一個小組件都會使用下列 JavaScript 參數：  
`WIoTPWidget.CreateWIDGET_TYPE("ELEMENT_ID","EVENT_NAME", "DEVICE_TYPE", "DEVICE_ID", "PROPERTY" , {WIDGET_DEFAULT_OVERRIDE}, [WIDGET_SPECIFIC_SETTINGS])`

下表提供參數的說明：

|參數|說明|    
| ----- | ---- |   
|WIDGET_TYPE |要建立的小組件類型。範例：`Gauge` 或 `Chart` |
|ELEMENT_ID |出現在應用程式中之小組件的元素 ID。範例：`RPM` |
|EVENT_NAME |包括要顯示之內容的裝置事件名稱。範例：`sensorData` |
|DEVICE_TYPE |裝置類型。範例：`iot-conveyor-belt` |
|DEVICE_ID |提供要顯示之資料的裝置 ID。範例：`belt1` |
|PROPERTY |要顯示的裝置訊息有效負載內容。範例：`rpm` |
|WIDGET_DEFAULT_OVERRIDE |用來置換預設值的小組件配置設定。|
|WIDGET_SPECIFIC_SETTINGS |小組件的一個以上其他參數，請參閱範例。|

如需每一個小組件類型的詳細資料，請參閱下列範例以及 [IoT Widgets GitHub 儲存庫 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/iot-widgets){: new_page} 中的文件。
 1. 新增 RPM 量規。  
此量規會將輸送帶 rpm 顯示為最小值為 0 且最大值為 5 rpm 的量規。
    1. 開啟下列範本進行編輯：`public/index.html`  
    2. 找出 rpm 量規位置保留元：`<!--- place holder for rpm gauge  -->`
    3. 新增下列具有唯一 ID 的 div 元素，如下所示：
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. 找出 JavaScript 位置保留元：`/// Add your scripts here`
    4. 新增 rpm JavaScript 程式碼。  
範例：  
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iot-conveyor-belt", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. 新增加速量規。  
此量規會將加速感應器讀數顯示為讀數介於 -1 到 1 之間的量規。
    1. 找出加速感應器量規位置保留元：`<!--- place holder for accelerometer gauge  -->`
    2. 新增下列具有唯一 ID 的 div 元素，如下所示：
 ```html
 <div id="aygauge" ></div>
 ```  
    3. 找出 javascript 位置保留元：`/// Add your scripts here`
    4. 新增加速感應器 JavaScript 程式碼。  
範例：   
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iot-conveyor-belt", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
          },
          show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. 新增馬達速度圖。  
此圖表以折線圖顯示馬達速度。
    1. 找出馬達速度量規位置保留元：`<!--- place holder for motor speed gauge  -->`
    2. 新增下列具有唯一 ID 的 div 元素，如下所示：
 ```html
 <div id="speedchart" ></div>
 ```  
    3. 找出 JavaScript 位置保留元：`/// Add your scripts here`
    4. 新增 speedchart JavaScript 程式碼。  
範例：  
 ```javascript
 WIoTPWidget.CreateChart("speedchart ","sensorData", "iot-conveyor-belt", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. 將應用程式部署至 {{site.data.keyword.Bluemix_notm}}  
 1. 使用 {{site.data.keyword.iot_short_notm}} 服務名稱更新 manifest.yml 檔案。  
例如，如果您在[手冊 1：連接輸送帶裝置](getting-started-iot-monitoring.html)中已部署 {{site.data.keyword.iot_short_notm}} 服務，則 YOUR_PLATFORM_NAME 是 `iotp-for-conveyor`。
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. 使用 cloudfoundry CLI，登入 {{site.data.keyword.Bluemix_notm}} 帳戶。  
如需相關資訊，請參閱 [Cloud Foundry CLI 文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
從指令行中，輸入下列指令：  
   ```
 cf login
   ```
系統提示時，請選取您要在其中部署監視範例應用程式的組織及空間。
 5. 必要的話，請執行 cf api 指令來設定 API 端點。   
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
 6. 將目錄切換至範例應用程式所在的目錄。  
   ```
 cd guide-conveyor-ui-html
   ```
 2. 執行 cf push 指令，以將應用程式推送至 {{site.data.keyword.Bluemix_notm}}：  
請為應用程式提供唯一名稱。
```
cf push YOUR_APP_NAME
```
5. 開啟小組件程式庫型 Web 應用程式。  
從 {{site.data.keyword.Bluemix_notm}} 應用程式儀表板中，按一下**造訪應用程式 URL** 來開啟 Web 應用程式。  
按一下**路徑**，以存取及管理應用程式 URL。   
預設 URL 與下列類似：  
`https://YOUR_APP_NAME.mybluemix.net`

## 步驟 2 - 檢視已連接的裝置
{: #view_devices}

Web 主控台已開始運行，所以您可以檢視已連接的輸送帶裝置。
1. 從 Web 主控台**裝置**區段中，驗證已列出輸送帶，並顯示正確的 RPM 及「執行中」狀態。
2. 變更輸送帶裝置的 RPM 值，並驗證已如預期更新監視應用程式上的值。


## 下一步為何？
{: @whats_next}  
繼續進行下一個手冊，或跳至其他感興趣的主題：
- 路徑 A：修改監視應用程式，以符合您的需求。  
如需技術詳細資料，請參閱：
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md){: new_window}
 - [Node.js 用戶端程式庫 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- 路徑 B：修改小組件程式庫應用程式，以符合您的需求。  
如需技術詳細資料，請參閱：
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md){: new_window}
- [手冊 4：模擬大量裝置](getting-started-iot-large-scale-simulation.html)  
藉由將大量自我執行模擬器新增至環境中來擴充基本模擬。此擴充可讓您在更真實的多裝置環境中，試用先前手冊中的基本分析及監視。
- [進一步瞭解 {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [進一步瞭解 {{site.data.keyword.iot_short_notm}} API](/docs/services/IoT/reference/api.html){:new_window}
