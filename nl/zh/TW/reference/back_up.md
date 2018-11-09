---

copyright:

years: 2017, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 資料備份
{: #back_up}

使用下列資訊，以瞭解 {{site.data.keyword.iot_full}} 的資料備份策略。

## 要備份哪些類型的資料？

目前已備份下列類型的用戶端資料作為 {{site.data.keyword.iot_short_notm}} 策略的一部分：

- 組織資訊
- 裝置資訊
- API 金鑰及記號
- 使用者資訊
- 所有裝置管理要求記錄，包括任何起始要求的歷程 - 例如，要求的現行狀態
- 自訂裝置管理要求組合的定義

**附註：**取消佈建服務之後，會將組織的所有資料保留 14 天。在 14 天的時間範圍內與支援中心聯絡，以還原組織。

## 不備份哪些類型的資料？

在 {{site.data.keyword.iot_short_notm}} 中，不備份下列類型的資料：

- 裝置事件
- 暫時性傳訊狀態，例如進行中資料
- 在裝置管理要求中，傳送及接收的 MQTT 訊息
<!-- - Analytics rules and alert configuration -->

## 資料的備份頻率及其儲存位置為何？

資料會每 24 小時備份一次。

資料是以離站方式從主要 {{site.data.keyword.iot_short_notm}} 服務儲存，並提供地理備援，以及啟用在重要突發事件發生時還原服務。如果需要資料還原，則會聯絡用戶端。所有資料都會加密，並符合本端資料保護需求。

下列離站位置目前用於資料備份：

 位置|備份位置                             
------------- | -------------
IBM Cloud 美國南部（達拉斯）|華盛頓州
IBM Cloud 英國（倫敦）|法蘭克福
IBM Cloud 德國（法蘭克福）|倫敦
IBM Cloud 專用|根據每位客戶訂購「{{site.data.keyword.iot_short_notm}} 專用」時的要求

**附註：**未來位置可能會變更以反映資料隱私權法律，例如，「英國脫歐」對歐盟資料主權規則的潛在影響。
