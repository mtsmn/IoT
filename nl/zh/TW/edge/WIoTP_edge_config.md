---

copyright:
  years: 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 配置 {{site.data.keyword.iot_short_notm}} Edge（預覽）
{: #edge_configure}

**重要事項：**{{site.data.keyword.iot_full}} Edge 特性僅是有限預覽程式的一部分。未來更新可能包含與此特性的目前版本不相容的變更。請試用，並且[讓我們知道您的想法 ![外部鏈結圖示](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

您必須先將閘道連接至 {{site.data.keyword.iot_short_notm}}，才能開始接收來自連接至 Edge 節點之裝置的資料。若要將閘道連接至 {{site.data.keyword.iot_short_notm}}，需要建立閘道裝置類型，並且向 {{site.data.keyword.iot_short_notm}} 登錄閘道。然後，您就可以使用登錄資訊，將閘道裝置連接至 {{site.data.keyword.iot_short_notm}}。

## 高階配置流程

1. 在 {{site.data.keyword.iot_short_notm}} Edge 預覽測試環境內[建立組織](#edge_org)。
2. [啟用 {{site.data.keyword.iot_short_notm}} 預覽](#edge_enable)。
3. [建立已啟用 Edge 的閘道類型](#edge_gw_type)、[新增閘道裝置](#edge_gw)，以及選取要在 Edge 節點上執行的 Edge 服務，以將組織配置成搭配使用 Edge 閘道節點與 {{site.data.keyword.iot_short_notm}}。必要的話，您可以[建立自訂服務](WIoTP_edge_dev.html#create_service)。
4. [配置 Edge 服務](#config_service)。
5. [在裝置上安裝 Edge](#edge_install_device)。
6. [管理及監視 Edge 服務](#monitor_service)。

## 建立組織
{: #edge_org}

完成下列步驟，以在 {{site.data.keyword.iot_short_notm}} Edge 預覽測試環境內建立組織。
1. 登錄 {{site.data.keyword.Bluemix_notm}} 帳戶，並在 {{site.data.keyword.Bluemix_notm}} 組織中建立 {{site.data.keyword.iot_short_notm}} 服務實例。您可以直接從 [IBM Cloud 服務型錄的 {{site.data.keyword.iot_short_notm}} 頁面 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.{DomainName}/catalog/services/internet-of-things-platform/){:new_window} 建立 {{site.data.keyword.iot_short_notm}} 實例。  
2. 完成佈建 IBM Cloud 組織的資訊。**重要事項：**您必須在**美國南部**位置部署 IBM Cloud 組織，以使用 {{site.data.keyword.iot_short_notm}} Edge 預覽。
3. 按一下**建立**。
4. 在開啟的 {{site.data.keyword.iot_short_notm}} 實例中，按一下**啟動**。即會建立新的 {{site.data.keyword.iot_short_notm}} 組織。組織 ID 會顯示在 URL 的開頭，以及儀表板中的使用者名稱下。您現在可以啟用組織的 {{site.data.keyword.iot_short_notm}} Edge。

## 啟用 {{site.data.keyword.iot_short_notm}} Edge 預覽
{: #edge_enable}

依預設，會關閉 {{site.data.keyword.iot_short_notm}} Edge 預覽。若要啟用 {{site.data.keyword.iot_short_notm}} Edge，請執行下列動作：

1. 從組織的 {{site.data.keyword.iot_short_notm}} 儀表板中，選取**設定**。
2. 在**實驗性特性**區段中，啟動實驗性特性（包括 {{site.data.keyword.iot_short_notm}} Edge 預覽）。
3. 重新整理瀏覽器，以檢視儀表板導覽功能表中的新 **Edge 服務**特性及捷徑。**附註：**如果 **Edge 服務**圖示未出現在儀表板導覽功能表中，請確定您已選取**美國南部**作為您的位置。

此旗標會新增至本端瀏覽器儲存空間。

## 建立已啟用 Edge 功能的閘道類型
{: #edge_gw_type}

每個連接至 {{site.data.keyword.iot_short_notm}} 的閘道都必須與閘道類型相關聯。閘道類型是一群共用相同性質的裝置。

1. 從 {{site.data.keyword.iot_short_notm}} 儀表板主導覽功能表中，選取**裝置**。
2. 在**裝置類型**標籤上，按一下**新增裝置類型**。
3. 在**新增類型**頁面的**身分**標籤上，選取類型**閘道**，然後輸入閘道類型名稱（例如 `my_gateway_type`）及閘道類型的說明。
**重要事項：**閘道類型名稱不得超過 36 個字元，且只能包含：
<ul>
 <li>英數字元（a-z 、A-Z、0-9）</li>
 <li>連字號 (-)</li>
 <li>底線 (&lowbar;)</li>
 <li>句點 (.) </li>
 </ul>
3. 選用項目：輸入閘道類型屬性及 meta 資料。
4. 開啟 **Edge 功能**按鈕。
5. 選取閘道類型的架構類型。架構類型必須符合裝置的架構。例如，Odroid 裝置是在 ARM64 架構上執行。
6. 按**下一步**。
7. 選用項目：在**裝置資訊**標籤上，輸入與閘道裝置類型相關的其他詳細資料及 meta 資料。
8. 選取要新增至 Edge 裝置的 Edge 服務。Core IoT 服務會新增至所有裝置，但您可以完成下列步驟，以從型錄中新增其他服務：
 1. 按一下**新增服務**。
 2. 在**新增 Edge 服務**視窗中，選取您要新增的適當服務版本，然後按一下**完成**。
9. 在**新增閘道類型**視窗中，按一下**完成**。您可以繼續[將閘道裝置新增至此閘道類型](#edge_gw)。

## 新增閘道裝置
{: #edge_gw}

1. 從 {{site.data.keyword.iot_short_notm}} 儀表板主導覽功能表中，選取**裝置**。
2. 按一下**新增裝置**。
3. 在**身分**標籤上，選取要新增裝置的閘道裝置類型，然後輸入您要新增之閘道裝置的唯一 ID。裝置 ID 可用來識別 {{site.data.keyword.iot_short_notm}} 儀表板中的閘道裝置，它也是將閘道裝置連接至 {{site.data.keyword.iot_short_notm}} 的必要參數。**重要事項：**裝置 ID 不得超過 36 個字元，且只能包含：
 <ul>
 <li>英數字元（a-z 、A-Z、0-9）</li>
 <li>連字號 (-)</li>
 <li>底線 (&lowbar;)</li>
 <li>句點 (.) </li>
 </ul>
 **提示：**若為連接網路的裝置，裝置 ID 可以是（例如）不含任何分隔冒號的裝置 MAC 位址。
4. 按**下一步**。
5. 選用項目：在**裝置資訊**標籤上，輸入與閘道裝置相關的其他詳細資料及 meta 資料。
6. 按**下一步**。
7. 在**新增裝置**頁面的**安全**標籤上，自動產生鑑別記號，或為此裝置提供您自己的鑑別記號。如果您選擇自行建立記號，請確定其長度介於 8 到 36 個字元之間、包含大小寫混合的字母、數字，以及連字號、底線或句點。記號不得包含重複的字元順序、字典單字、使用者名稱或其他預先定義的順序。
9. 按**下一步**。
10. 按一下**完成**。
11. 在裝置資訊頁面上，複製並儲存下列裝置資訊：
 - 組織 ID，例如 `tubo8x`
 - 裝置類型，例如 `my_gateway_type`
 - 裝置 ID。**提示：**若為連接網路的裝置，這個值可以是（例如）沒有任何分隔冒號的 MAC 位址。
 - 鑑別方法，例如 `token`
 - 鑑別記號（例如 `PtBVriRqIg4uh)_-Kl`）
  使用此資訊來將閘道連接至 {{site.data.keyword.iot_short_notm}}，並開始接收來自連接至閘道之裝置的資料。

## 配置 Edge 服務
{: #config_service}

在建立已啟用 Edge 功能的新裝置類型之後，您可以選取要在 Edge 節點中安裝的不同服務。

1. 在 {{site.data.keyword.iot_short_notm}} 儀表板主導覽功能表中，選取**裝置**。
2. 按一下**裝置類型**。
3. 選取您要配置的 Edge 裝置類型。
4. 在 **Edge 服務**標籤上，選取鉛筆圖示以編輯記錄。
6. 按一下**新增 Edge 服務**，查看已針對此組織上傳的服務清單。
7. 在**新增 Edge 服務**視窗中，選取您要新增的適當服務版本，然後按一下**完成**。
<!--7. In preview phase an unregister and register on Edge node is required in order to the new services t being installed.-->

## 管理及監視 Edge 服務
{: #monitor_service}

安裝 Edge 節點之後，{{site.data.keyword.iot_short_notm}} 可讓您監視在 Edge 節點上執行之服務的狀態。

1.	在 {{site.data.keyword.iot_short_notm}} 儀表板主導覽功能表中，選取**裝置**。
2.	選取對應至 Edge 節點的裝置
3.	按一下 **Edge 服務**，查看 Edge 節點上已安裝服務的狀態。

## 尋找相關資訊
{: #more_info}

如需有關將閘道連接至 {{site.data.keyword.iot_short_notm}} 的詳細資訊，請參閱[閘道的 MQTT 連線功能](https://console.bluemix.net/docs/services/IoT/gateways/mqtt.html#mqtt)。

如需 {{site.data.keyword.iot_short_notm}} 的其他詳細資訊，請參閱[開始使用 Watson IoT Platform](https://console.bluemix.net/docs/services/IoT/getting-started.html#getting-started-with-iotp)。

如需 {{site.data.keyword.iot_short_notm}} Edge 的相關資訊，請參閱下列主題：
- [{{site.data.keyword.iot_short_notm}} Edge 概觀](WIoTP_edge.html#edge_overview)
- [在 Edge 裝置上安裝 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_install.html#edge_install_device)
- [開發 {{site.data.keyword.iot_short_notm}} Edge](WIoTP_edge_dev.html#edge_dev)
