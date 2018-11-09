---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-19"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 {{site.data.keyword.messagehub}} 連接及配置歷程服務  
{: #messagehub_main}

將 {{site.data.keyword.messagehub_full}} 連接至 {{site.data.keyword.iot_full}} 可提供可擴充的高傳輸量訊息匯流排，以進行歷程資料儲存。{{site.data.keyword.messagehub}} 是以 Apache Kafka 為建置基礎，而後者是開放程式碼的高傳輸量傳訊系統，可提供低延遲平台來處理即時資料資訊來源。

Message Hub 會使用分割區索引鍵來轉遞分割區事件。索引鍵的形成，是藉由連結 {{site.data.keyword.iot_short}} 6 個字元的組織 ID 與裝置類型及裝置 ID。有效負載欄位，包括時間戳記及事件 ID，並不會用來形成分割區索引鍵。此配置可確保來自特定裝置的所有事件會傳送至相同的分割區，以便事件按照它們傳送的順序處理。 

將訊息從 {{site.data.keyword.iot_short_notm}} 傳送至 {{site.data.keyword.messagehub}} 時，不會套用 MQTT 裝置用來將訊息傳送至 {{site.data.keyword.iot_short_notm}} 的服務品質 (QoS)。一般而言，訊息會傳送至 {{site.data.keyword.messagehub}} 一次。在極罕見的情況下，可能會傳送訊息多次，或完全不傳送訊息。

## 開始之前  
{: #byb}

將 {{site.data.keyword.messagehub}} 連接至 {{site.data.keyword.iot_short}} 服務之前，請完成下列作業：

- 使用「{{site.data.keyword.Bluemix_notm}} 型錄」，在與 {{site.data.keyword.iot_short_notm}} 相同的 {{site.data.keyword.Bluemix_notm}} 空間中設定 {{site.data.keyword.messagehub}}。如需 {{site.data.keyword.messagehub}} 的相關資訊，請參閱[開始使用 {{site.data.keyword.messagehub}}](https://console.{DomainName}/docs/services/MessageHub/index.html)。

- 請確定您具有 {{site.data.keyword.Bluemix_notm}} 組織中的開發人員專用權，並且透過 {{site.data.keyword.Bluemix_notm}} 登入。如果您未透過 {{site.data.keyword.Bluemix_notm}} 登入，或沒有此 {{site.data.keyword.Bluemix_notm}} 組織中的開發人員專用權，則無法授權 {{site.data.keyword.messagehub}} 與 {{site.data.keyword.iot_short_notm}} 的連結。


## 連接

若要連接 {{site.data.keyword.messagehub}} 以進行歷程資料儲存，請執行下列動作：

1. 在 {{site.data.keyword.iot_short}} 儀表板上，按一下導覽列中的**延伸規格**。
2. 在「歷程資料儲存空間」磚中，按一下**設定**。
4. 選取您要連接的 {{site.data.keyword.messagehub}} 服務。
  
與 {{site.data.keyword.iot_short}} 服務相同之 {{site.data.keyword.Bluemix_notm}} 空間內的所有可用 {{site.data.keyword.messagehub}} 服務，都會列在「配置歷程資料儲存空間」區段中。
5. 選取 {{site.data.keyword.messagehub}} 配置選項：
 1. 選取時區。
   
傳送至 {{site.data.keyword.messagehub}} 之裝置資料上的時間戳記會轉換為選取的時區。
 2. 選取預設主題。
   
裝置事件會傳送至這裡定義的預設主題。若要使用更精細的主題指派，請讓預設主題留白，並新增自訂轉遞規則。
 3. 選用項目：指定自訂轉遞規則。  
指定用於轉遞裝置事件的其他規則。只會轉遞符合自訂轉遞規則的事件。  
   
 **附註：**事件可能符合多個轉遞規則，並且轉遞給多個 {{site.data.keyword.messagehub}} 主題。
 4. 選用項目：配置主題。  
若要建立超過預設兩個分割區的主題，您可以指定分割區配置。
 5. 按一下**完成**。
5. 按一下**授權**。
6. 在「授權」對話框中，按一下**確認**。

您的裝置資料現在儲存在 {{site.data.keyword.messagehub}} 中。
