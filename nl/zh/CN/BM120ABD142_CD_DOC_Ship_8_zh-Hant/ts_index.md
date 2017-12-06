---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-03"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iot_short_notm}} 疑難排解
{: #ts}

這裡會回答有關在 {{site.data.keyword.Bluemix_notm}} 上使用 {{site.data.keyword.iot_full}} 的常見疑難排解問題。
{:shortdesc}

## 存取 {{site.data.keyword.iot_short_notm}} 組織時發生問題
{: #access-expiry-problem}

您無法登入您所擁有的 {{site.data.keyword.iot_short_notm}} 組織。
{:shortdesc}

您無法使用組織 URL 或使用 `https://internetofthings.ibmcloud.com` 直接登入 {{site.data.keyword.iot_short_notm}} 組織。
{: tsSymptoms}

您對 {{site.data.keyword.iot_short_notm}} 組織的存取權可能已過期。使用 {{site.data.keyword.Bluemix}} 所建立的 {{site.data.keyword.iot_short_notm}} 組織，預設會使用暫存使用者設定檔。
{: tsCauses}

您可以藉由使用 {{site.data.keyword.Bluemix_notm}} 來存取 {{site.data.keyword.iot_short_notm}} 組織以及變更使用者設定檔的到期設定來解決此問題。若要變更使用者到期設定，請執行下列動作：

1. 從 {{site.data.keyword.Bluemix_notm}} 儀表板中，開啟 {{site.data.keyword.iot_short_notm}} 服務。
2. 從導覽列中，按一下**成員**。
3. 按一下**編輯**圖示。
4. 清除**存取到期**方框，然後按一下**儲存**。
{: tsResolve}

## 連接至 {{site.data.keyword.iot_short_notm}} 時發生問題
{: #connection_problem}

連線至 {{site.data.keyword.iot_short_notm}} 突然中斷。
{:shortdesc}

當您嘗試連接至 {{site.data.keyword.iot_short_notm}} 時，裝置或應用程式收到錯誤。
{: tsSymptoms}

您可能有兩台裝置嘗試以相同的用戶端 ID 和認證進行連線。每個用戶端 ID 僅允許用於一個唯一的連線。您不能有兩個使用相同 ID 的並行連線。應用程式可以共用相同的 API 金鑰，但 MQTT 需要用戶端 ID 一律是唯一的。
{: tsCauses}

若要解決此問題，請確認沒有兩台裝置嘗試使用相同的認證進行連接。
{: tsResolve}

## 裝置與 {{site.data.keyword.iot_short_notm}} 間歇性中斷連線
{: #disconnection_problem}

裝置與 {{site.data.keyword.iot_short_notm}} 的連線間歇性突然中斷。
{:shortdesc}

連接至 {{site.data.keyword.iot_short_notm}} 服務的裝置間歇性中斷連線。裝置會重新連線，但過沒多久又突然斷線。
{: tsSymptoms}

這可能是因為當您連接時，用於 MQTT ping 選項的值太低，讓它看起來像是連線逾時。例如，如果用戶端 MQTT 設定不正確，則無法及時收到 ping，連線也會關閉。
{: tsCauses}

若要修正此問題，請確認已為連線設定適當的 ping 和 KeepAlive 參數。   
{: tsResolve}

