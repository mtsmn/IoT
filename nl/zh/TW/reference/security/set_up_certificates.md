---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# 配置憑證
{: #set_up_certificates}

憑證是用於裝置鑑別，或取代預設 {{site.data.keyword.iot_full}} 伺服器憑證以進行 MQTT 傳訊。沒有有效簽章憑證的任何裝置，都會被拒絕存取，且無法與伺服器通訊。

為了配置裝置的憑證和伺服器存取權，系統操作員會登錄關聯的憑證管理中心 (CA) 憑證，以及選擇性地將訊息伺服器憑證登錄至 {{site.data.keyword.iot_short_notm}} 平台。

如需使用 API 管理 CA 憑證及傳訊伺服器憑證的相關資訊，請參閱 [IBM Watson IoT Platform 鑑別及授權 API ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/security.html){: new_window}。

## CA 憑證
CA 憑證可讓組織將裝置上的用戶端憑證辨識為受信任，讓裝置可以連接至伺服器。

如果您新增 CA 憑證，或取代傳訊伺服器憑證，則所有裝置都必須使用支援「伺服器名稱指示 (SNI)」的 MQTT 用戶端來連接，因此，伺服器可以使用適當的 CA 來鑑別裝置。

您可以配置連線安全原則來設定連線安全層次。如需如何執行這項作業的相關資訊，請參閱[配置安全原則](set_up_policies.html)。

## 用戶端憑證

個別用戶端（裝置或閘道）憑證會保留在裝置上，而且不會上傳至平台。用來簽署所有裝置及閘道憑證的 CA 簽署憑證是您上傳至平台的唯一憑證。如果您使用的是自簽傳訊伺服器憑證，則必須上傳用來簽署用戶端憑證 (cert.pem) 的主要及中繼憑證。

使用 CA 憑證所簽署的個別裝置或閘道憑證必須輸入裝置 ID 或閘道 ID 作為憑證中的「通用名稱 (CN)」或 SubjectAltName。

針對裝置，**CN** 欄位格式為 `CN=d:devtype:devid`，而 **SubjectAltName** 欄位格式為 `SubjectAltName=email:d:*devtype:devid*`，其中 `email:d` 是常數、`*devtype*` 是裝置的「裝置類型」，而 `*devid*` 是平台中裝置的 ID。

針對閘道，**CN** 欄位格式為 `CN=g:typeId:deviceId`，而 **SubjectAltName** 欄位格式為 SubjectAltName=email:g:*devtype:devid*

附註：針對裝置或閘道憑證，請不要在 **CN** 或 **SubjectAltName** 欄位中包括 `orgId`。連接至傳訊伺服器時，應該將 `orgId` 提供為用戶端實作所提供 SNI 資訊的一部分。

如需用戶端憑證的相關資訊，請參閱[使用用戶端憑證將 Raspberry Pi 連接至 IBM Watson IoT Platform 秘訣 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-client-side-certificates/){: new_window}

## 傳訊伺服器憑證

傳訊伺服器憑證接受預設網域：internetofthings.ibmcloud.com。憑證 CN 或 SubjectAltName 必須遵循下列格式：

- `orgId.messaging.internetofthings.ibmcloud.com`（IoTP 網域）

下列範例顯示伺服器憑證的有效 CN：

`mtxpd0.messaging.internetofthings.ibmcloud.com`

如需傳訊伺服器憑證的相關資訊，請參閱[使用自簽伺服器憑證將 Raspberry Pi 連接至 IBM Watson IoT Platform 秘訣 ![外部鏈結圖示](../../../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/recipes/tutorials/connect-raspberry-pi-to-ibm-watson-iot-platform-using-selfsigned-server-certificate/){: new_window}

### 自訂網域
{: #custom-domains}

傳訊伺服器憑證接受自訂網域。憑證 CN 或 SubjectAltName 必須遵循下列格式：

- `orgId.messaging.<custom domain>`

**CN** 欄位接受自訂網域的萬用字元（如下列範例所示）：

- `CN=*.messaging.mywiotpcustomdomain.com`

**重要事項**：針對自訂網域，外部 DNS 服務需要將自訂網域解析為 {{site.data.keyword.iot_full}} 傳訊伺服器。平台未提供此 DNS 服務。

## 登錄憑證管理中心 (CA) 憑證來進行裝置鑑別
{: #reg_ca_cert}

1. 登入 {{site.data.keyword.iot_short_notm}}，並導覽至**一般設定**。
2. 在**安全**區段的 **CA 憑證**下，按一下**新增憑證**。
3. 瀏覽並選取要上傳的憑證檔案，或將檔案拖入**新增憑證**視窗中。該檔案中只能有一個憑證，且憑證日期必須有效。僅接受 .pem 或 .der 格式的憑證。您可以預覽所選取檔案中的憑證資訊。
4. 輸入憑證檔案的說明。
5. 確認已選取正確的檔案，然後按一下**儲存**。所選取的憑證會列在表格中，且預設為作用中。

## 登錄傳訊伺服器憑證
{: #reg_msg_cert}

平台附有預設伺服器憑證。您可以使用預設憑證，或是從組織上傳憑證。如果您還沒有憑證可使用，則可以建立要求來取得新憑證。接收新憑證之後，您必須加以簽署，然後上傳回平台。

**附註：**平台儀表板頁面可能會執行與傳訊伺服器的內部連線來擷取裝置資訊。設定組織的自簽伺服器憑證時，儀表板使用者必須在其瀏覽器中將伺服器憑證新增為授信憑證以避免連線問題，因為瀏覽器預設不會將傳訊伺服器辨識為授信伺服器。

## 從組織上傳傳訊伺服器憑證
{: #upload_cert}
1. 在**一般設定**的**安全**區段中，按一下**傳訊伺服器憑證**下的**新增憑證**。
2. 瀏覽並選取要上傳的憑證檔案，或將檔案拖入**新增憑證**視窗中。
3. 瀏覽並選取要上傳的私密金鑰檔案，或將檔案拖入**新增憑證**視窗中。
4. 輸入私密金鑰的通行詞組（如果私密金鑰有使用通行詞組來加密）。
5. 確認已選取正確的檔案，然後按一下**儲存**。

## 啟動傳訊伺服器憑證

若要啟動預設憑證，或已上傳的另一個憑證，請從**傳訊伺服器憑證**之表格的**預設傳訊伺服器憑證**下拉清單中，選取您要使用的憑證。所選取的憑證會在表格中列為作用中憑證。

## 要求新的傳訊伺服器憑證
{: #request_cert}

如果您要使用新的傳訊伺服器憑證，則可以產生「憑證簽署要求 (CSR)」來要求新憑證。

1. 在**一般設定**的**安全**區段中，按一下**傳訊伺服器憑證**下的**產生 CSR**。
2. 輸入詳細資料來為您的伺服器要求 CSR，然後按一下**產生**。CSR 即顯示在表格中。
3. 下載要求，並將其提交至憑證管理中心進行簽署。
4. 取得憑證之後，請回到表格中的 CSR 項目，並上傳新的憑證。上傳憑證之後，表格中的 CSR 就會被上傳的憑證所取代。
