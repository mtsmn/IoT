---

copyright:

years: 2017
lastupdated: "2017-08-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Data backup
{: #back_up}

Use the following information to understand the data backup strategy for {{site.data.keyword.iot_full}}.

## What type of data is backed up?

The following types of client data are currently backed up as part of the {{site.data.keyword.iot_short_notm}} strategy:

- Organizational information
- Device information
- API keys and tokens
- User information
- All records of device management requests, including the history of any initiated requests - for example the current state of the request
- Definitions of custom device management request bundles

## What type of data is not backed up?

The following types of data are not backed up in {{site.data.keyword.iot_short_notm}}:

- Device events
- Transient messaging state, for example in-flight data
- Analytics rules and alert configuration
- MQTT messages that are sent and received as part of a device management request

## How frequently is data backed up and where is it stored?

Data is backed up once every 24 hours.

Data is stored off-site from the main {{site.data.keyword.iot_short_notm}} service, providing geographical redundancy and enabling services to be restored in the event of a significant incident. Clients will be contacted if data restoration is ever required. All data is encrypted and complies with local data protection requirements.

The following off-site locations are currently used for data backup:

Location                   | Backup location                      
------------- | -------------
Bluemix US South (Dallas)| Washington
Bluemix United Kingdom (London) | Frankfurt
Bluemix Germany (Frankfurt) | London
Bluemix Dedicated | As per customer request when ordering {{site.data.keyword.iot_short_notm}} Dedicated

**Note:** Future locations might change to reflect data privacy laws, for example the potential impact of Brexit on EU data sovereignty rules.
