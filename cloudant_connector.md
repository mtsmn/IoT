---

copyright:
  years: 2016, 2018
lastupdated: "2018-09-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting and configuring a historian service by using an {{site.data.keyword.cloudant_short_notm}} database service
{: #cloudant_main}

Connecting an {{site.data.keyword.cloudantfull}} service to {{site.data.keyword.iot_full}} allows you to store and access your device data. Device data is stored in daily, weekly, or monthly databases depending on your selected bucket interval.

When you begin using an {{site.data.keyword.cloudant_short_notm}} service to store device data, three databases are automatically created; one database is created for the current bucket interval, one for the upcoming interval, and one configuration database. Design documents can be added to the configuration database and are copied to new databases as they are created. When the end of an interval is reached, device data is stored in the bucket database for the new interval and a new database is created for the following interval.

When device data is sent to a database it can be stored in one of two ways. If the data is valid JSON and the format of the device event is set to `JSON`, the device data is stored in the following format:

```json

{
  "_id": "78bf4380-3311-11e6-a747-d7b140d1a70a",
  "_rev": "2-d13912b7c089f060a4ba7369fa86e46f",
  "typeId": "t",
  "deviceType": "0",
  "eventType": "json_payload",
  "format": "json",
  "timestamp": "2016-06-15T16:54:41.464+01",
  "data": {
    "a": 22
  }
}

```

If the device data is not valid JSON or if the format is not set to `JSON` the device data is stored as a base64 encoded string under the `payload` field in the following format:

```json

{
  "_id": "80f1ce10-3311-11e6-a747-d7b140d1a70a",
  "_rev": "1-bfcbf1e74389fe4188a9425c0cd2575a",
  "payload": "eHh4eHg=",
  "typeId": "t",
  "deviceType": "0",
  "eventType": "non_json_payload",
  "format": "notjson",
  "timestamp": "2016-06-15T16:54:55.217+01"
}

```
The quality of service (QoS) that is used by an MQTT device to send messages to {{site.data.keyword.iot_short_notm}} does not apply when messages are sent from {{site.data.keyword.iot_short_notm}} to {{site.data.keyword.cloudant_short_notm}}. Typically, a message is sent to {{site.data.keyword.cloudant_short_notm}} once. Rarely, it might be possible for a message to be sent more than once or not at all. 

## Before you begin  
{: #byb}

Before connecting an {{site.data.keyword.cloudant_short_notm}} service to {{site.data.keyword.iot_short_notm}}, complete the following tasks:

- Set up an {{site.data.keyword.cloudant_short_notm}} service in the same {{site.data.keyword.Bluemix_notm}} space as your {{site.data.keyword.iot_short_notm}} by using the {{site.data.keyword.Bluemix_notm}} Catalog.

Ensure that you have developer privileges in the {{site.data.keyword.Bluemix_notm}} organization and that you are signed in through {{site.data.keyword.Bluemix_notm}}. If you are not signed in through {{site.data.keyword.Bluemix_notm}}, or do not have developer privileges in this {{site.data.keyword.Bluemix_notm}} organization, you cannot authorize the binding of  {{site.data.keyword.cloudant_short_notm}} and {{site.data.keyword.iot_short_notm}}.

## Using the {{site.data.keyword.iot_short_notm}} dashboard to bind an {{site.data.keyword.cloudant_short_notm}} service to {{site.data.keyword.iot_short_notm}}

Complete the following steps to connect an {{site.data.keyword.cloudant_short_notm}} service:

1. On your {{site.data.keyword.iot_short_notm}} dashboard click **Extensions** in the navigation bar.
2. In the Historical Data Storage tile, click **Setup**.
2. All available {{site.data.keyword.cloudant_short_notm}} services within the same {{site.data.keyword.Bluemix_notm}} space as your {{site.data.keyword.iot_short_notm}} service are listed in the Configure historical data storage section.
3. Select the {{site.data.keyword.cloudant_short_notm}} service that you wish to connect.
4. Select your {{site.data.keyword.cloudant_short_notm}} configuration options:

  a. Select a bucket interval. The bucket interval controls how frequently new databases are created to store device data. New buckets are created at midnight in the selected timezone using your selected bucket interval.

  b. Select a time zone. The time in the selected timezone is used to determine which bucket device data is placed in. Timestamps on device data that is sent to {{site.data.keyword.cloudant_short_notm}} are converted into the timezone that is associated with the  database to which the data is sent.

  c. The database name is created in the format `iotp_<orgID>_<dbname>_<bucket_name>`, where:

   * `<orgID>` is your organization ID.
   * `<dbname>` is the name of the database. 
   * `<bucket_name>` is the name of the bucket.
     * For `day` bucket intervals, `<bucket_name>` is in the format `yyyy-mm-dd`.  For example, `2016-07-06` for events on July 6th 2016.
     * For `week` bucket intervals  `<bucket_name>` is in the format `yyyy-'w'ww` where `'w'ww` indicates a week number.  For example, `2016-w03` for events in the 3rd week of 2016.
     * For `month` bucket intervals `<bucket_name>` is in the format `yyyy-mm`.  For example, `2016-07` for events in July 2016.

5. Click **Authorize**.
6. Click **Confirm** in the authorization dialog box.

Your device data can now be stored in {{site.data.keyword.cloudant}}.

## Recipes on using Historian Service  
{: #recipes}

The following recipes describe how to use {{site.data.keyword.cloudant_short_notm}} to store data that is received from {{site.data.keyword.iot_short_notm}}:

- [Configure {{site.data.keyword.cloudant_short_notm}} as Historian Data Storage for {{site.data.keyword.iot_short}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/cloudant-nosql-db-as-historian-data-storage-for-ibm-watson-iot-parti/){: new_window} recipe demonstrates how to configure and store device data on {{site.data.keyword.cloudant_short_notm}}.

- [Query and Process {{site.data.keyword.iot_short_notm}} Device Data from {{site.data.keyword.cloudant_short_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/cloudant-nosql-db-as-historian-data-storage-for-ibm-watson-iot-partii){: new_window} recipe shows how to query and perform data processing operations on the device data that is stored in {{site.data.keyword.cloudant_short_notm}}.


