---

copyright:
  years: 2016, 2018
lastupdated: "2017-03-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 即時 IoT 資料分析
{: #analytics}  

**重要事項：**我們即將發表測試版，以提供在 IoT 裝置資料上定義規則的新方式，作為更廣泛變更計劃的一部分，以改善 {{site.data.keyword.iot_full}} 提供規則和動作的方式。

若要進一步了解，請參閱部落格文章 [An alternative approach to defining Rules on IoT data ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}。

若要開始定義您自己的規則，請參閱[建立內嵌規則（測試版）](information_management/im_rules.html)文件。

## 關於即時 IoT 資料分析

使用 Watson {{site.data.keyword.iot_short_notm}} 分析，可從您裝置產生的原始資料中取得您需要的即時分析資訊。  
{: shortdesc}

您可以使用[板及卡片](data_visualization.html)，檢視代表資料集值（來自一個以上裝置）的圖形，以快速瀏覽並瞭解裝置資料。

透過分析規則，您可以指定觸發動作的條件。例如，您可以建立一個規則，以確保當裝置掉落時，或裝置溫度驟升時，將警示傳送至使用者裝置上的儀表板，並將電子郵件傳送給管理者。

對直接連接至雲端中 {{site.data.keyword.iot_short_notm}} 的裝置使用[雲端規則](cloud_analytics.html)來觸發規則，以及對連接至啟用邊緣分析功能之閘道的裝置使用[邊緣規則](edge_analytics.html)來觸發規則。
