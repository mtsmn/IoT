---

copyright:
  years: 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} Edge 概觀（預覽）
{: #edge_overview}

**重要事項：**{{site.data.keyword.iot_full}} Edge 特性僅是有限預覽程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

{{site.data.keyword.iot_short_notm}} Edge 可讓您將 {{site.data.keyword.iot_short_notm}} 功能擴充至 Edge 裝置。Edge 裝置位於組織內的網路邊緣。範例包括位於實體位置的感應器及工業控制器（例如工廠）。使用 {{site.data.keyword.iot_short_notm}} Edge，可以先在裝置內部處理資料，再將其傳送至雲端。

若要使用 {{site.data.keyword.iot_short_notm}} Edge，您可以建立 Edge 節點，並使用要在其中執行的服務來配置那些節點。WIoTP Edge 接著會將這些服務部署至節點。在其中執行 Edge 服務，Edge 節點就可以從具有 Edge 閘道的裝置傳送訊息。閘道會處理訊息，並轉換資料，然後可以將資料傳送至雲端（視介面的配置方式而定）。

{{site.data.keyword.iot_short_notm}} Edge 預覽提供 Core IoT 預設服務，以及建立自訂服務（例如，預測製造地板機器人故障並傳送故障通知的服務）的能力。Edge 裝置的範例是用於監視 CPU 使用率並在使用率超過特定百分比時傳送警示的感應器。

## {{site.data.keyword.iot_short_notm}} Edge 元件

**Edge 節點**
Edge 節點是由位於網路邊緣的閘道及裝置所組成。

**Edge 裝置**
裝置（例如感應器）位於網路邊緣，並執行在傳送至雲端之前處理資料的服務。Edge 裝置的範例是用於監視 CPU 使用率並在使用率超過特定百分比時傳送警示的感應器。

**Edge 閘道**
閘道是特殊化裝置，結合了應用程式與裝置的功能，因而能夠作為其他裝置的存取點。啟用 Edge 時，Edge 閘道可讓無法直接連接至網際網路的裝置先連接至 Edge 上的閘道裝置，來存取 {{site.data.keyword.iot_short_notm}} 服務。

**Edge 閘道類型**
閘道類型可將共用屬性（例如型號或位置）的閘道裝置群組在一起。啟用 Edge 功能並將 Edge 閘道裝置新增至 {{site.data.keyword.iot_short_notm}} 時，會使用 Edge 閘道類型中的屬性作為新 Edge 閘道裝置的範本。

**Edge 服務**
服務是新增至 {{site.data.keyword.iot_short_notm}} Edge 閘道並執行處理程序（例如資料操作）的功能。您可以建置使用服務的應用程式。

**Edge Services 型錄**
預設 Core IoT Service 內含在型錄中，並新增至所有 Edge 節點。您可以根據商業需要來新增自訂服務。「Edge Services 型錄」保留所有可用的自訂服務。

**檔案伺服器儲存庫**
「檔案伺服器儲存庫」保留 Docker 容器及映像檔。所有 Edge 處理功能或服務都在 Docker 容器內運作。您可以選取要在裝置內部執行的 Docker 映像檔。

## 尋找相關資訊
{: #more_info}

如需配置、安裝及開發 {{site.data.keyword.iot_short_notm}} Edge 的相關資訊，請參閱下列主題：
 - [配置 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_config.html#edge_configure)
 - [在 Edge 裝置上安裝 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_install.html#edge_install_device)
 - [開發 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
