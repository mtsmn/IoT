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


# 数据备份
{: #back_up}

使用以下信息来了解 {{site.data.keyword.iot_full}} 的数据备份策略。

## 备份什么类型的数据？

以下类型的客户机数据当前作为 {{site.data.keyword.iot_short_notm}} 策略的一部分进行备份：

- 组织信息
- 设备信息
- API 密钥和令牌
- 用户信息
- 设备管理请求的所有记录，包括任何初始请求的历史记录，例如请求的当前状态
- 定制设备管理请求束的定义

**注：**在服务取消供应后，组织的所有数据将保留 14 天。请在 14 天内联系支持人员以复原组织。

## 不备份什么类型的数据？

{{site.data.keyword.iot_short_notm}} 中不备份以下类型的数据：

- 设备事件
- 瞬态消息传递状态，例如，进行中的数据
- 作为设备管理请求一部分发送和接收的 MQTT 消息
<!-- - Analytics rules and alert configuration -->

## 数据备份的频率及其存储位置？

每 24 小时备份一次数据。

从主 {{site.data.keyword.iot_short_notm}} 服务外异地存储数据，提供地理冗余并支持在发生重大事件时的恢复服务。如果需要复原数据，那么将与客户机取得联系。所有数据都已加密，并且符合本地数据保护需求。

以下异地位置目前用于数据备份：

 位置|备份位置
------------- | -------------
IBM Cloud 美国南部（达拉斯）|华盛顿
IBM Cloud 英国（伦敦）|法兰克福
IBM Cloud 德国（法兰克福）|伦敦
IBM Cloud Dedicated|订购 {{site.data.keyword.iot_short_notm}} Dedicated 时按客户要求

**注：**将来的位置可能会更改以反映数据隐私法，例如，英国退欧对欧盟数据主权规则的潜在影响。
