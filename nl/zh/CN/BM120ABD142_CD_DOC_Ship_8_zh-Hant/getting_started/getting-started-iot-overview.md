---

copyright:
  years: 2017
lastupdated: "2017-06-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# {{site.data.keyword.iot_short_notm}} 的開始使用手冊
{: #getting-started}

「開始使用手冊」的設計旨在引導您使用 {{site.data.keyword.iot_short_notm}} 來開發現成可用的端對端 IoT 原型系統的基本觀念，並且是針對使用 {{site.data.keyword.iot_short_notm}} 的新手開發人員所撰寫的。
{:shortdesc}

每一個手冊都會提供逐步程序，快速引導您到已部署且執行中的解決方案，以示範一個以上的 {{site.data.keyword.iot_short_notm}} 特性。

## 使用手冊  
{: #using-guides}
在每一個手冊中，您至少會有一個根據 {{site.data.keyword.iot_short_notm}} 最佳作法所編碼的 GitHub 來源範例。這些範例可以進行調整、保護、與系統整合，而且通常可適合您的 devOps 開發及建置程序。

技術上可以擴充快速解決方案，方法是將範例程式碼調整為符合其專屬環境，或是使用硬體範例來補充快速解決方案，而硬體範例可進一步模擬您在正式作業環境中可能遇到的問題。這些手冊提供作業相關文件、API 文件、用戶端程式庫 (SDK)、指示影片及其他訓練資料的其他鏈結。

您可以依序遵循這些手冊，而第一個手冊提供後續手冊的建置基礎。如果您要轉跳並遵循自己的路徑來瀏覽這些系列，則每一個手冊也隨附有一份需求清單。例如，如果您已設定 {{site.data.keyword.iot_short_notm}} 組織，則可以直接跳至手冊 2 及手冊 3，並開始探索規則、動作及範例監視應用程式。這些需求會告訴您這些手冊所需的裝置資料格式，您可以相應地設定自己的環境。
{: tip}

## 手冊清單
{: #list-of-guides}  

下列是可用的手冊：

| 手冊  | 說明|    
| ----- | ---- |   
| [1. 連接輸送帶裝置](getting-started-iot-conveyor.html) | 藉由安裝 {{site.data.keyword.iot_short_notm}} 組織以開始使用，然後連接範例 GitHub 來源輸送帶模擬應用程式，或者，如果您偏好更實體的項目，則使用自己的 Raspberry-Pi 型輸送帶模擬器。</br> 然後，設定一些基本 {{site.data.keyword.iot_short_notm}} 儀表板卡片，以視覺化及監視您的裝置資料。|   
| [2. 使用基本即時規則及動作](getting-started-iot-rules.html) | 有一個以上的裝置連接至 {{site.data.keyword.iot_short_notm}} 組織並傳送資料後，您現在即可設定商業規則並觸發動作，例如，在輸送帶速度低於特定臨界值時通知工廠操作員。  
| [3. 監視裝置資料](getting-started-iot-monitoring.html) | 擴充內建 {{site.data.keyword.iot_short_notm}} 裝置監視儀表板，方法是連接 Node.js 或小組件程式庫型監視應用程式來即時查看輸送帶資料。  
| [4. 模擬大量裝置](getting-started-iot-large-scale-simulation.html) | 擴充簡單裝置模擬，方法是將大量裝置模擬器新增至環境，以在更真實的多裝置環境中試用先前手冊中的分析及監視。</br>**重要事項：**應用程式需要 512 MB 的記憶體，這超過依預設配置的數量，同時超出免費試用帳戶可用的數量（這些帳戶必須先升級為「訂閱」或「隨收隨付制」帳戶）。|   
