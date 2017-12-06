---

copyright:
  years: 2015, 2017
lastupdated: "2017-06-01"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入門指導教學
{: #getting-started-with-iotp}

在此 {{site.data.keyword.iot_full}} 入門指導教學中，我們將 IoT 裝置連接至 {{site.data.keyword.iot_short_notm}}，並設定分析來探索即時資料。
{:shortdesc}

<div id="prerequisites"></div>

## 開始之前
{: #prereqs}

您需要有一個 [Bluemix 帳戶 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/registration/){: new_window}、一個 {{site.data.keyword.iot_short_notm}} 服務實例，以及一個符合下列需求的裝置：

*	您的裝置必須能夠使用 HTTP 或 MQTT 通訊協定進行通訊。

* 裝置訊息必須符合 {{site.data.keyword.iot_short_notm}} 訊息有效負載需求。

如需相關資訊，請參閱[在 Watson IoT Platform 上開發裝置](/docs/services/IoT/devices/device_dev_index.html)。

## 步驟 1：登錄裝置

您可以從 {{site.data.keyword.iot_short_notm}} 儀表板一次新增一個裝置，或是使用 [Watson IoT Platform API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Bulk_Configuration/post_bulk_devices_add){: new_window} 新增多個裝置。

若要從 {{site.data.keyword.iot_short_notm}} 儀表板新增裝置，請執行下列動作：

1. 在 {{site.data.keyword.Bluemix_notm}} 主控台中，按一下 {{site.data.keyword.iot_short_notm}} 服務詳細資料頁面上的**啟動**。

    {{site.data.keyword.iot_short_notm}} Web 主控台會在新瀏覽器分頁中開啟，其 URL 為：

    ```
 https://org_id.internetofthings.ibmcloud.com/dashboard/#/overview
 ```

    其中 *org_id* 是[您的 {{site.data.keyword.iot_short_notm}} 組織](iotplatform_overview.html#organizations){: new_window}的 ID。

2. 在「概觀」儀表板中，從功能表窗格中選取**裝置**，然後按一下**新增裝置**。

3. 建立您要新增之裝置的裝置類型。

    連接至 {{site.data.keyword.iot_short_notm}} 的每一個裝置都必須與裝置類型相關聯。裝置類型是一群共用相同性質的裝置。

    1. 按一下**建立裝置類型**。
    2. 輸入裝置名稱（例如 `my_device_type`）及裝置類型的說明。
    
        > **附註：**裝置類型名稱不得超過 36 個字元，且只能包含英數字元（a-z、A-Z、0-9）及下列任何字元：`_`、`.` 和 `-`。



    3. 選用項目：輸入裝置類型屬性和 meta 資料。  
 

        您可以稍後再新增及編輯屬性和 meta 資料。
        {: tip}

4. 按**下一步**，開始新增所選取裝置類型的裝置。

5. 輸入裝置 ID（例如 `my_first_device`）。

    裝置 ID 可用來識別 {{site.data.keyword.iot_short_notm}} 儀表板中的裝置，它也是將裝置連接至 {{site.data.keyword.iot_short_notm}} 的必要參數。

    > **附註：**裝置類型名稱不得超過 36 個字元，且只能包含英數字元（a-z、A-Z、0-9）及下列任何字元：`_`、`.` 和 `-`。



    若為連接網路的裝置，裝置 ID 可以是不含任何分隔冒號的裝置 MAC 位址。

5. 按**下一步**，以完成處理程序。

6. 提供鑑別記號，或接受自動產生的記號。如果您選擇建立自己的記號，請確定其長度為 8 到 36 個字元，並且只包含英數字元及下列字元：`_`、`.`、`!`、`&`、`@`、`?`、`\*`、`+`、`(`、`)` 及 `-`。

    記號不得包含重複的字元順序、字典單字、使用者名稱或其他預先定義的順序。

7. 驗證摘要資訊正確無誤，然後按一下**新增**，以新增連線。

8. 在裝置資訊頁面中，複製並儲存下列詳細資料：

    * 組織 ID
  
    * 裝置類型
    * 裝置 ID
    * 鑑別方法
  
    * 鑑別記號
  

    您需要有「組織 ID」、「裝置類型」、「裝置 ID」及「鑑別記號」，才能配置您的裝置來連接至 {{site.data.keyword.iot_short_notm}}。

## 步驟 2：將您的裝置連接至 {{site.data.keyword.iot_short_notm}}

1. 設定裝置來進行 MQTT 傳訊，並使用「組織 ID」、「裝置類型」、「裝置 ID」及「鑑別記號」進行鑑別。

2. 使用 MQTT 通訊協定將裝置訊息傳送至 {{site.data.keyword.iot_short_notm}} 組織。

    連接裝置時，需要下列資訊：

    * URL：*org_id*.messaging.internetofthings.ibmcloud.com

      其中 *org_id* 是 {{site.data.keyword.iot_short_notm}} 組織的 ID。

    * 埠：
      * 1883
      * 8883（已加密）
      * 443 (websockets)
    * 裝置 ID：d:_org_id:device_type:device_id_
    * 使用者名稱：use-token-auth
    * 密碼：_鑑別記號_
    * 事件主題格式：iot-2/evt/_event_id/fmt/format_string_

      其中 _event_id_ 指定顯示在 {{site.data.keyword.iot_short_notm}} 中的事件名稱，而 _format_string_ 是事件的格式，例如 JSON。

    * 訊息格式：JSON

  如需相關資訊，請參閱[裝置的 MQTT 連線功能](/docs/services/IoT/devices/mqtt.html)。

## 步驟 3：建立板及卡片來追蹤裝置資料

您可以使用板及卡片，檢視代表資料集值（來自一個以上裝置）的圖形，以快速瀏覽並瞭解裝置資料。

1. 在 {{site.data.keyword.iot_short_notm}} 儀表板中，按一下**建立新板**。

2. 輸入板的名稱及說明。

3. 按**下一步**，然後按一下**建立**。

4. 按一下您剛剛建立的板，然後按一下**新增卡片**。選取「裝置」作為卡片類型。下表說明您可從中進行選擇的視覺化選項。

| 類型| 顯示的資料|
|---------------|---------------|
| 一般視覺化| 一個以上資料集的值。選擇大型小組件大小，以在小型表格中查看三個資料點值。|
| 折線圖| 即時捲動圖表中的一個以上資料集。使用「設定」功能表，以設定資料範圍與保留、圖形外觀與操作方式以及其他項目。|
| 長條圖| 所標示長條中的資料集值。使用「設定」功能表，以切換水平或垂直長條方向。|
| 環圈圖| 以圓形呈現的兩個以上資料集。|
| 值| 一個以上資料集的原始值。|
| 量規| 顯示為量規之資料集的值。使用「設定」功能表，以選擇性地設定低、中及高資料範圍的量規臨界值。|
| 裝置內容| 一個以上裝置的特定內容。|
| 所有裝置內容| 一個以上裝置的所有內容。|
| 裝置清單| 監視多個裝置的清單。清單可以用作其他卡片的資料來源。|
| 裝置資訊| 單一裝置的基本資訊。|
| 裝置地圖| 裝置清單中裝置的位置。|

{: caption="表 1. 視覺化選項" caption-side="top"}

## 步驟 4：建立裝置類型綱目

此時，您需要建立裝置類型綱目，然後對映裝置內容，以建立根據所對映裝置內容的資料點而觸發的規則。

1. 在 {{site.data.keyword.iot_short_notm}} 儀表板中，移至**裝置 > 管理綱目**，然後按一下**新增綱目**。

2. 選取要與訊息綱目建立關聯的裝置類型。一種裝置類型只能定義一個綱目。

3. 按一下**新增內容**，然後新增一個以上內容。

    您可以從連接的裝置選取內容、建立用來修改或結合現有內容的虛擬內容，或是手動新增內容。

    可用的內容定義在裝置所傳送的訊息有效負載中。
    {: tip}

    * 若要手動新增內容，請選取**手動**標籤，然後定義下列內容詳細資料：
        * 名稱
        * 資料類型
        * 內容
        * 資料單位（選用）
        * 小數位數（選用）

    * 若要建立虛擬內容，請選取**虛擬內容**標籤，然後定義下列內容詳細資料：
        * 名稱
        * 資料類型
        * 內容
        * 計算
        * 資料單位（選用）
        * 小數位數

    * 若要選取所連接裝置的內容，請選取**從已連接的**標籤，然後選取一個以上要新增至綱目的內容。即會新增所選取的內容，且說明會設定成內容的名稱。

4. 按一下**完成**，以建立內容。

## 步驟 5：建立規則及動作

規則是條件型決策點，用來比對即時裝置資料與預先定義的臨界值或其他內容資料，以在符合條件時觸發警示。除了 {{site.data.keyword.iot_short_notm}} 儀表板中顯示的警示之外，您還可以新增一個以上的動作，以在觸發規則時執行商業邏輯。

1. 在 {{site.data.keyword.iot_short_notm}} 儀表板中，移至**規則**，然後按一下**建立雲端規則**。

2. 輸入規則的名稱及說明，並選取要套用規則的裝置類型，然後按**下一步**。

3. 若要設定規則邏輯，請新增一個以上的 IF 條件，用來作為規則的觸發程式。

    您可以在平行列中新增條件，將它們套用為 OR 條件，或者在循序欄中新增條件，將它們套用為 AND 條件。
請查看下列範例：

      * 如果參數值大於指定的值，則簡單的規則可能會觸發警示：
Condition = `temp_cpu>80`
      * 符合臨界值組合時，可能會觸發較複雜的規則：
Condition = `temp_cpu>60 AND cpu_load>90`

    若要觸發一個比較兩個內容的條件，或觸發兩個以上使用 AND 循序結合的內容條件，請將觸發資料點包括在相同的裝置訊息中。如果在多則訊息中收到資料，則不會觸發條件或循序條件。
    {: tip}

4. 配置規則的條件式觸發需求。

    您可以使用條件式觸發需求，來控制一段時間針對規則所觸發的警示數目。條件式觸發會對規則中的任何條件採取動作。例如，如果規則有五個使用 OR 所設定的平行條件，則每一個符合的條件都會列入條件式觸發計數的計算中。

      1. 在規則編輯器中，按一下預設的**每次符合條件時觸發**鏈結，以開啟「設定頻率需求」對話框。
      2. 選取並配置您要在規則中使用的條件式觸發程式。

        * 每次符合條件時觸發
        * 在 M 個_時間單位_內符合條件 N 次時觸發

5. 建立或選取在符合規則條件時發生的一個以上動作。

    例如，動作可以是傳送電子郵件或張貼 Webhook。

6. 選用項目：選取規則的警示優先順序。

    優先順序是用來分類**板 > 規則型分析**板中所顯示的警示。預設優先順序是「低」。

7. 按一下**儲存**進行儲存而不啟動，或按一下**啟動**進行儲存並啟動規則。

    當您啟動規則時，會在符合條件時將警示新增至**規則型分析**板，並執行任何規則動作。

## 後續步驟

建立並連接您自己的應用程式來使用即時和歷程裝置資料，以延伸資料分析特性。

  * 移出工具的[用戶端程式庫](/docs/services/IoT/iot_platform_client_lib.html)來建置程式碼，以整合及連接裝置和應用程式。

  * 探索 [Watson IoT Platform 秘訣 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/category/internet-of-things-iot/){: new_window}，以取得將進階分析內含至所連接裝置及應用程式的指導教學。
