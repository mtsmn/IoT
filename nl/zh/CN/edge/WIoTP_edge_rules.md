---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 配置 {{site.data.keyword.iot_short_notm}} Edge 路由规则（预览）
{: #edge_rules}

{{site.data.keyword.iot_full}} Edge 路由规则定义如何在边缘节点中转发消息。

**重要信息：**{{site.data.keyword.iot_full}} Edge 功能只作为受限预览程序的一部分提供。未来更新可能会包含与此功能当前版本不兼容的更改。请尝试此功能，[让我们了解您的想法 ![外部链接图标](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}。

可以为一个边缘节点定义多个路由规则。系统会针对本地设备或应用程序直接发布到边缘节点的每条消息，对规则求值。从云发布到边缘节点的消息始终在本地发布，并且不受路由的影响。

## 路由规则格式
{: #edge_rules_format}

路由规则定义为以下格式的 JSON 对象：

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

路由规则指定为 `routing.json` 文件中的 JSON 对象数组，此文件存储在边缘节点上的 `/etc/wiotp-edge` 文件夹中。

每个路由规则的属性定义如下：

属性|类型|描述
------------- | -----------| -----------
rule_id|整数，必需|规则的标识。将根据 rule_id 按升序对路由规则进行求值。不应该存在多个具有相同 rule_id 的规则。
matching_filter|字符串，必需|用于求值以确定规则是否应该应用于发布的消息。此项应该采用 MQTT 预订的格式。支持 MQTT 单级别 (+) 和多级别 ( #) 通配符。将针对在其上发布消息的主题对此过滤器求值。此属性以 {{site.data.keyword.iot_short_notm}} 应用程序主题格式进行定义，并包含 `/type/deviceType/id/deviceId` 字段。例如：`"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
destination|字符串，必需|定义应该转发消息的位置。设置下列其中一个值：`"CLOUD"`、`"IM"`（用于将消息转发到信息管理组件）或 `"LOCAL"`（用于在边缘节点上以本地方式转发消息）。例如，`"destination" : "CLOUD"`
continue_matching|布尔值，可选，缺省值为 true|指定此规则匹配一次后，是否应使用此规则对其他消息求值。存在重叠的路由规则时，此属性能更好地控制路由。
forward_skip_levels|整数，可选，缺省为 0|指定原始消息主题中应该在重新发布消息时除去的级别数。缺省值为 0，表示保留原始主题。例如，如果 `forward_skip_levels` 设置为 `1`，并且消息在主题 `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json` 上发布，那么该消息会在主题 `iot-2/type/T1/id/D1/evt/E1/fmt/json` 上重新发布。
store|布尔值，可选，缺省值为 true|定义是否应该将存储转发功能应用于按此规则路由的消息。仅当 destination 属性设置为 `"CLOUD"` 时，才会应用此属性。如果此属性设置为 false，那么当边缘节点与云断开连接时，不会存储与规则匹配的消息。

## routing.json 文件的示例
{: #edge_rules_example}

在以下示例中，配置了三个规则：
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

第一个规则将类型为 `AlarmEvent` 的所有事件直接路由到云。

第二条规则将在以 `sendToCloud/iot-2` 开头的主题上发布的所有事件直接路由到云。消息将在以 `iot-2` 开头且不包含 `sendToCloud/` 前缀的主题中进行转发。

第三个规则是缺省规则，用于将其他所有消息路由到本地 MQTT 代理程序。然后，任何具有匹配预订的本地使用者都可以使用这些消息。边缘节点的安装包含 `/etc/wiotp-edge/routing.json` 中用于本地路由一切内容的缺省规则。

## 编辑路由规则
{: #edge_rules_edit}

您可以直接在 `/etc/wiotp-edge/routing.json` 文件中编辑规则。编辑该文件后，保存更改，然后运行 `docker ps` 以确定 edge-connector 容器的标识。使用该标识重新启动相应的 Docker 容器。

## 对路由规则进行故障诊断
{: #edge_rules_ts}

如果在 `routing.json` 中进行更改后，edge-connector 容器一直处于重新启动状态，说明该路由规则文件包含语法错误。请检查日志文件 `/var/wiotp-edge/trace/edgeConnector_trace.log` 以查看错误消息。
