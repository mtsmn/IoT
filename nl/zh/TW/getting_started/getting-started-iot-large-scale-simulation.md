---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# 手冊 3：模擬大量裝置
在第一個手冊中，您可以設定基本裝置模擬器，以手動模擬一條以上的輸送帶。在本手冊中，我們藉由將大量自我執行模擬器新增至環境中來擴充此模擬，以在更真實的多裝置環境中測試分析及監視。
{:shortdesc}

**重要事項：**應用程式需要 512 MB 的記憶體，這超過依預設配置的數量，同時超出免費試用帳戶（包括 {{site.data.keyword.Bluemix}} 試用帳戶及標準帳戶）可用的數量。「訂閱」及「隨收隨付制」帳戶持有者可以將已配置的記憶體增加到 512 MB。免費試用帳戶持有者則需要升級為「訂閱」或「隨收隨付制」帳戶。如需 {{site.data.keyword.Bluemix_notm}} 帳戶類型的相關資訊，請參閱[帳戶類型](/docs/pricing/index.html#pricing)。

**附註：**Bluemix 現在是 IBM Cloud。如需詳細資料，請參閱 [IBM Cloud 部落格](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")。

## 概觀及目標
{: #overview}

在本手冊中，您將設定 {{site.data.keyword.Bluemix_notm}} 應用程式使用 [Node-RED ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://nodered.org){: new_window}「後端」來模擬多個輸送帶裝置。

應用程式包含三個流程，用於：   
1. 建立多個裝置。   
2. 模擬並行事件發佈。
3. 刪除裝置。   

![裝置建立及刪除](images/device_creation.png "裝置建立及刪除")

![裝置事件模擬](images/device_event_simulation.png "裝置事件模擬")

完成本手冊中的指示，即可：
- 使用 Cloud Foundry 來部署 Node-RED 型及 Webhook 已啟用裝置模擬器應用程式。
- 使用 API 呼叫來建立及登錄裝置、發佈裝置事件，以及刪除裝置。

**重要事項：**模擬並行將裝置資料傳送至 {{site.data.keyword.iot_full}} 的大量裝置，可能會用掉大量資料。 

## 必要條件
{: #prereqs}  
您需要下列帳戶及工具：

* [{{site.data.keyword.Bluemix_notm}} 帳戶](https://console.ng.bluemix.net/registration/)，含    
 - 超過 512 MB RAM   
 - 超過 1 GB 磁碟限額  
 - 兩個可用的服務
* [Cloud Foundry 指令行介面 (cf CLI) ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
使用 cf CLI 來部署及管理 {{site.data.keyword.Bluemix_notm}} 應用程式。
* 選用項目：[Git ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://git-scm.com/downloads){: new_window}  
如果您選擇使用 Git 來下載程式碼範例，則必須同時具有 [GitHub.com 帳戶 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com){: new_window}。您也可以將程式碼下載為壓縮檔，而不需要使用 GitHub.com 帳戶。

針對後續步驟中的任何 REST 呼叫，您可以使用 Mozilla 中可用的 cURL 或 REST 用戶端附加外掛程式。  
{: tip}

## 步驟 1 - 將應用程式部署至 {{site.data.keyword.Bluemix_notm}}
{: #step1}

請遵循以下步驟，以手動建立及部署應用程式。   

1. 複製 *Lesson4* 範例應用程式 GitHub 儲存庫。  
使用您慣用的 Git 工具來複製下列儲存庫：  
https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
在 Git Shell 中，使用下列指令：
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor/tree/master/device-simulator
```
3. 編輯 manifest.yml 檔案，以針對您的環境配置應用程式。  
要編輯的內容：
 - 若要使用現有 {{site.data.keyword.iot_short_notm}} 服務，請更新 `lesson4-simulate-iotf-service` 的所有實例，以反映該服務名稱。例如，如果您要使用手冊 1 中的 {{site.data.keyword.iot_short_notm}} 服務，請使用 `iotp-for-conveyor` 作為服務名稱。    
 - 設定裝置名稱及主機。   
在應用程式區段中，將 `name` 及 `host` 項目變更為唯一的項目（例如 `YOUR_NAME-lesson4-simulate`）。   
**提示：**您用來存取應用程式的 ROUTES URL 是從 `host` 項目所建立（例如：`https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`）。  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plan: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
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
6. 在 {{site.data.keyword.Bluemix_notm}} 中建立必要服務。   
 1. 使用下列指令來建立 cloudantNoSQLDB「精簡」服務：
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. 使用下列指令來建立 {{site.data.keyword.iot_short_notm}} 服務。
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**重要事項：**如果您是使用現有 {{site.data.keyword.iot_short_notm}} 服務，而且已相應地更新 manifest.yml 檔案，請確定您使用現有的服務名稱。例如，從手冊 1，使用 `iotp-for-conveyor`，而非 cf 呼叫中的 `lesson4-simulate-iotf-service`。
7. 執行 `cf push` 指令來建置專案，並推送至您的組織。  
您的範例應用程式會部署在 {{site.data.keyword.Bluemix_notm}}。  
部署完成時，會顯示一則訊息，指出您的應用程式正在執行。   
範例：  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. 取得應用程式的服務認證。
 1. 在下列位置登入 {{site.data.keyword.Bluemix_notm}}：  
 [https://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://bluemix.net){: new_window}。
 2. 選取您已部署應用程式的帳戶及空間。
 3. 從功能表中，選取**應用程式**，然後選取**儀表板**。
 4. 在「Cloud Foundry 應用程式」下，按一下您剛才部署之應用程式的名稱。
 4. 選取**連線**。
 5. 找出您與應用程式搭配使用的 iotf-service，然後按一下**檢視認證**。  
 6. 在服務認證頁面上，找出 `apiKey` 及 `apiToken`。   
在使用應用程式 API 來建立及執行模擬裝置時，會使用 API 金鑰及鑑別記號作為認證。

## 步驟 2 - 保護 Node-RED 流程
{: #step2}

雖然這是選用步驟，但卻是保護 Node-RED 流程的最好作法。

1. 保護編輯器，只讓授權使用者進行存取。
2. 若要保護 Node-RED 流程，請移至 {{site.data.keyword.Bluemix_notm}} 儀表板，然後選取您先前已部署的應用程式。
3. 按一下**運行環境**，然後按一下**環境變數**。
4. 新增下列使用者定義的環境變數及其個別的值：
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. 儲存流程。

當 Node-RED 流程安全時，如果您計劃使用 REST API 指令來建立、刪除或模擬多個裝置，則必須在基本鑑別期間提供 Node-RED 使用者名稱及密碼認證。

## 步驟 3 - 建立及連接裝置
{: #step3}

您可以使用 Node-RED 流程介面或應用程式 REST API 來完成下列作業。

### 使用 Node-RED 建立及連接裝置  
若要登錄多個裝置，請執行下列動作：  
1. 在下列位置登入 {{site.data.keyword.Bluemix_notm}}：  
[https://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://bluemix.net){: new_window}。
2. 選取您已部署應用程式的帳戶及空間。
3. 從功能表中，選取**應用程式**，然後選取**儀表板**。
4. 在「Cloud Foundry 應用程式」下，按一下您剛才部署之應用程式的 **ROUTE** URL。  
ROUTE_URL 是從您在 manifest.yml 檔案中所使用的 `host` 項目所建置：`HOST.mybluemix.net`  
範例：`https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`。  
即會開啟 Node-RED 介面。
5. 按一下**移至 Node-RED 流程編輯器**。
6. 在流程編輯器中，選取**裝置類型及實例**標籤。
7. 若要登錄五個裝置，請按一下標籤為**登錄 5 個 motorController 裝置**的注入節點。

### 使用 REST API 建立及連接裝置  
若要登錄多個裝置，請執行下列動作：  

1. 對下列 URL 提出 HTTP POST 要求：`ROUTE_URL/rest/devices`  
範例：`https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - 將 'Content-Type' 及 'Accept' 設為 'application/json'  
 - 使用下列 JSON 有效負載：  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
其中：  
    - numberDevices 是要建立及登錄的裝置數目。
    - 選用項目：typeId 是裝置將登錄為的裝置類型。如果未提供 typeId，則預設為 "iotp-for-conveyor"。**提示：**您可以輸入任何裝置類型名稱，但系列中的其他手冊預期裝置的裝置類型為 `iotp-for-conveyor`。如果您使用不同裝置類型，則必須相應地修改手冊中的設定。
    - 選用項目：authToken 是要用來登錄裝置的授權記號。
    - 選用項目：如果未提供 chunkSize，則依預設，會將它設為 500。'chunksize' 必須小於及 numberDevices 的因數。
    - deviceName 是已建立裝置之 deviceID 的型樣。

## 步驟 4 - 模擬裝置事件
{: #step4}

因為已向 {{site.data.keyword.iot_short_notm}} 登錄模擬裝置，所以您現在可以執行模擬器來開始傳送裝置事件。

### 使用 Node-RED 模擬裝置事件  
若要傳送裝置事件，請執行下列動作：  
1. 在 Node-RED 流程編輯器中，選取**模擬多個裝置**標籤。
7. 若要模擬五個裝置，請按一下標籤為**模擬 5 個裝置**的注入節點。
 


### 使用 Rest API 模擬裝置事件  
若要傳送裝置事件，請執行下列動作：

1. 對下列 URL 提出 HTTP POST 要求：`ROUTE_URL/rest/runtest`  
範例：`https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - 將 'Content-Type' 及 'Accept' 設為 'application/json'  
 - 使用下列 JSON 有效負載：   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
其中：
    - numberDevices 是您要模擬其資料的裝置數目。
    - numberEvents 是每一個模擬裝置所傳送的事件數目。
    - timeInterval 是事件的間距（毫秒）。
    - deviceType 是您為其建立模擬裝置的裝置類型。
    - deviceName 是已建立裝置之 deviceID 的型樣。
 

## 步驟 5 - 刪除裝置
{: #deleting}

### Node-RED  
若要刪除裝置，請執行下列動作：  
1. 在 Node-RED 流程編輯器中，選取**裝置類型及實例**標籤。
2. 若要刪除五個裝置，請按一下標籤為**刪除 5 個裝置**的注入節點。



### REST API  

1. 對下列 URL 提出 HTTP POST 要求：`ROUTE_URL/rest/deleteDevices`  
範例：`https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - 將 'Content-Type' 及 'Accept' 設為 'application/json'  
 - 使用下列 JSON 有效負載：      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName": "belt"  
}</code></pre>
  


## 下一步為何？
{: @whats_next}  
跳至其他感興趣的主題：
- [手冊 2：監視裝置資料](getting-started-iot-monitoring.html)  
您已連接一個以上的裝置，並已開始善用裝置資料，所以現在可以開始監視裝置的集合。
- [進一步瞭解 {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [進一步瞭解 {{site.data.keyword.iot_short_notm}} API](/docs/services/IoT/reference/api.html){:new_window}
