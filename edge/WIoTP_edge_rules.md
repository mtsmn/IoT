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


# Configuring {{site.data.keyword.iot_short_notm}} Edge routing rules (Preview)
{: #edge_rules}

{{site.data.keyword.iot_full}} Edge routing rules define how messages are forwarded within the Edge node.

**Important:** The {{site.data.keyword.iot_full}} Edge feature is available only as part of a limited preview program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

You can define multiple routing rules for an Edge node. The rules are evaluated for each message that a local device or application publishes directly to the Edge Node. Messages that are published to the Edge node from the cloud are always published locally and are not impacted by routing.

## Routing rules format
{: #edge_rules_format}

A routing rule is defined as a JSON object in the following format:

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

A routing rule is specified as array of the JSON objects in the `routing.json` file that is stored in the `/etc/wiotp-edge` folder on the Edge node.

The properties of each routing rule are defined as follows:

Property      | Type     | Description       
------------- | -----------| -----------
rule_id | integer, mandatory | An identifier for the rule. Routing rules are evaluated in ascending order according to the rule_id. There should not be more than one rule with the same rule_id.
matching_filter | string, mandatory | Used to evaluate if the rule should be applied to a published message. It should be in the format of an MQTT subscription. MQTT single-level (+) and multi-level (#) wildcards are supported. This filter is evaluated against the topic on which the message is published. This property is defined in {{site.data.keyword.iot_short_notm}} application topic format and include the `/type/deviceType/id/deviceId` fields. For example: `"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
Destination | string, mandatory | Defines where the message should be forwarded. Set one of the following values: `"CLOUD"`, `"IM"` (to forward the message to the Information Management component), or `"LOCAL"` (to forward the message locally on the Edge node). For example, `"destination" : "CLOUD"`
continue_matching | boolean, optional, default = true | Specifies whether additional messages should be evaluated using this rule if the rule is matched one time. This property provides better control over routing when overlapping routing rules exist.
forward_skip_levels | integer, optional, default = 0) | Specifies the number of levels from the original message topic that should be removed when the message is republished. The default value is 0, which means the original topic is maintained. For example, if `forward_skip_levels` is set to `1` and a message is published on topic `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json`, the message is republished on topic `iot-2/type/T1/id/D1/evt/E1/fmt/json`.
store | boolean, optional, default = true) | Defines whether the store-and-forward feature should be applied to messages that are routed by this rule. This property only applies when the destination property is set to `"CLOUD"`. If this property is set to false, messages that match the rule are not stored when the Edge node is disconnected from the cloud.

## Example of a routing.json file
{: #edge_rules_example}

In the following example, three rules are configured:
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

The first rule routes all events of type `AlarmEvent` directly to the cloud.

The second rule routes all events that are published on a topic starting with `sendToCloud/iot-2` directly to the cloud. The message is forwarded in a topic that starts with `iot-2` and does not include the `sendToCloud/` prefix.

The third rule is the default rule, which routes all other messages to the local MQTT broker. These messages can then be used by any local consumer with a matching subscription. The installation of the Edge node includes the default rule to route everything locally, in `/etc/wiotp-edge/routing.json`.

## Editing routing rules
{: #edge_rules_edit}

You can edit rules directly in the `/etc/wiotp-edge/routing.json` file. After you have edited the file, save your changes and then run `docker ps` to determine the ID of the edge-connector container. Restart the docker container using that ID.

## Troubleshooting routing rules
{: #edge_rules_ts}

If the edge-connector container keeps restarting after a change in `routing.json`, it means that routing rules file contains syntax errors. Check the log file `/var/wiotp-edge/trace/edgeConnector_trace.log` to see the error messages.
