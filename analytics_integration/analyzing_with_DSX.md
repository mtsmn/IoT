---

copyright:
  years: 2017
lastupdated: "2017-09-15"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Analyzing data by using Data Science Experience
{: #DSX_integration}

You can use {{site.data.keyword.iot_full}} with Data Science Experience (DSX) to visualize and learn about the data that is sent from devices that are connected to the platform.
{: shortdesc}

## Overview and goals

This guide walks you, step-by-step, through the process of visualizing {{site.data.keyword.iot_short_notm}} device event data by using DSX as an analysis tool.

DSX provides an interactive, collaborative, cloud-based environment where you can use multiple tools to activate their insights. Use the Jupyter Notebook, which is available in IBM DSX, to load historical {{site.data.keyword.iot_short_notm}} data, and use the data to create visualizations to aid data analysis and anomaly identification. You can derive threshold values from historical data and use these values to create Cloud rules in{{site.data.keyword.iot_short_notm}}. Cloud rules alert users when an IoT device that is associated with a rule publishes a reading that is outside of the configured threshold limits.

The device data that is sent to {{site.data.keyword.iot_short_notm}} can be collected and stored in {{site.data.keyword.Bluemix}} using the {{site.data.keyword.cloudantfull}} NoSQL DB service. To collect the data, you must first connect {{site.data.keyword.iot_short_notm}} to the {{site.data.keyword.cloudant_short_notm}} service. Device data is stored in {{site.data.keyword.cloudant_short_notm}} daily, weekly, or monthly databases depending on the bucket interval that is configured.


![Overview of using DSX to analyze data](images/DSX_overview.png)

As part of this guide you will learn:

 - How to configure the platform data storage so that the Cloudant NoSQL DB is used as the historian service.
 - How to use the Weather Sensors simulator to generate data to be used by the platform.
 - How to export the data and then import it to DSX to analyze data.
 - How to visualize IoT data, detect anomalies, and configure alerts.


## Prerequisites

To complete these steps you must have access to [{{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} with [Cloudant NoSQL DB ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}, and access to a [DSX Account ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://datascience.ibm.com/docs/content/getting-started/get-started.html){: new_window}.


## Step 1. Set up the simulator

{:#DSX_sensor_data}

In order to conduct a meaningful analysis, you must have meaningful data. You can simulate real sensor data to learn about how the Watson IoT Platform device data can be analyzed by using DSX. This step provides instructions for **setting up the simulator with an existing instance of {{site.data.keyword.iot_short_notm}}** and for **setting up the simulator with a new instance of {{site.data.keyword.iot_short_notm}}**.


### Setting up the Weather Sensors simulator with an existing instance of {{site.data.keyword.iot_short_notm}}

{: #sim_existing_platform}

To simulate real sensor data events against your organizations by using the Weather Sensors simulator, you must first set up the simulator. These steps assume that you already have an instance of {{site.data.keyword.iot_short_notm}} up and running.

1. [Generate the apikey and token that are required to run the simulator. ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Deploy the Weather Sensors simulator web app ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} and follow the detailed instructions.

   For more information about the Weather Sensors, see [the Weather Sensors simulator guide ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Proceed to [Step 2. Configure database connector](#DSX_config_db).


### Setting up the Weather Sensors simulator with a new instance of {{site.data.keyword.iot_short_notm}}

{: #sim_new_platform}

To simulate real sensor data events against your organizations by using the Weather Sensors simulator, you must first set up the simulator. These steps include the instructions for creating a {{site.data.keyword.iot_short_notm}} instance along with the simulator.


1. [Deploy the Weather Sensors simulator web app with an instance of {{site.data.keyword.iot_short_notm}} ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} and follow the detailed steps.

   For more information about the Weather Sensors, see [the Weather Sensors simulator guide ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Wait for the deployment to complete and then navigate to Bluemix dashboard.
3. Launch the {{site.data.keyword.iot_short_notm}} service "wiotp-for-weather-sensors-simulator" that was created by the deployment process.
4. Proceed to [Step 2. Configure database connector](#DSX_config_db).


## Step 2. Configure database connector
{: #DSX_config_db}

Device data can be stored in Cloudant in daily, weekly, or monthly databases, depending on the selected bucket interval option. Data that is collected from all devices in the same bucket interval (day, week, or month) is stored in the same database.

Regardless of the bucket interval configuration, three databases are automatically created by the connector. One database is created for the current bucket interval, one for the upcoming interval, and one for the configuration database. When the end of the interval is reached, the device data is stored in the bucket database for the new interval, and a new database is created for the subsequent bucket.

To use {{site.data.keyword.cloudant_short_notm}} with DSX, you must configure the platform data storage so that the Cloudant NoSQL DB is used as the historian service.

1. On the {{site.data.keyword.cloudant_short_notm}} dashboard, click **Extensions** in the navigation bar.
2. Under **Historical Data Storage**, click **Setup**. The **Configure Historical Data Storage** section lists all of the Cloudant NoSQL DB services that are available within the same Bluemix space as the {{site.data.keyword.cloudant_short_notm}}.
3. Select the Cloudant NoSQL DB service that you want to connect.
4. Specify the following Cloudant NoSQL DB configuration options:
  - Bucket interval = Day
  - Time zone = UTC
  - Database name = default
5. Click **Done** and confirm authorization for the connection to Cloudant Service. Ensure that popups are enabled in your browser in order have access to the confirmation window. When you have successfully configured the Cloudant NoSQL DB, the Historical Data Storage status is changed to Configured and the device data is stored in {{site.data.keyword.cloudant_short_notm}} NoSQL DB.
6. Proceed to [Step 3. Run the simulator](#run_simulator).


## Step 3. Run the simulator
{: #run_simulator}

The simulator publishes real weather sensors data, from 17 weather stations located in the Haifa area, into your {{site.data.keyword.iot_short_notm}} organization.

1. Navigate to the simulator.
2. If you deployed the simulator with a bound {{site.data.keyword.iot_short_notm}} instance, proceed to step 3. If you deployed a standalone version of the simulator, enter the following details:
   - Watson IoT Platform organization
   - API key
   - authentication token

3. Click **Run Simulator**. The data will take a few minutes to generate.
4. Go to the Watson IoT platform while the simulator is running and verify that devices were created and that events are coming to these devices.
5. Proceed to [Step 4. Set up DSX and visualize data](#DSX_visualize_data).


## Step 4. Set up DSX and visualize data
{: #DSX_visualize_data}

To set up DSX and start visualizing data:

1. [Set up a pre-configured Jupyter Notebook](#setup_jupyter_notebook) to gain insights into your data and to detect anomalies.
2. [Run the analysis.](#run_analysis)
3. [Configure alerts on sensor anomalies](#config_alerts).


### 1. Set up a pre-configured Jupyter Notebook
{: #setup_jupyter_notebook}

The Jupyter notebook is a web application that allows one to create and share documents that contain executable code, mathematical formulae, graphics/visualization (matplotlib) and explanatory text.

To set up a pre-configured Jupyter Notebook to gain insights into your data and to detect anomalies:
1. Use a supported browser to log in to [DSX ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://datascience.ibm.com/){: new_window} with your Bluemix ID.
2. Click "+" and select **Create project** to create a new project. Projects create a space for you to collect and share notebooks, connect to data sources, create pipelines, and add data sets.
3. Specify the project name and click **Create**. During the DSX account setup, the Spark service and Object Storage instance are automatically created. Alternatively, you can create them by using the Bluemix interface and then associate them with your DSX project at a later stage.
4. Import your Cloudant credentials by selecting your project in the **DSX** menu and clicking ![Find and Add Data icon](images/find_add_data_icon.png).
5. Click the **Connections** tab.
6. Click **Create Connection** to import your Cloudant credentials to make them available in any notebook in your project.
7. Complete the **New Connections** screen by entering the following information:
   - Connection name
   - Set **Service Category** to `Data Service`.
   - Select your Cloudant service from the **Target service instance** dropdown menu.
   - Select the Cloudant database corresponding to the current date.
8. Click **Create**.
9. Click **add notebooks** to create a new Jupyter notebook.
10. Select **From URL** to load an existing notebook, then specify a descriptive name for the notebook and enter the following URL to open the sample notebook:
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```
11. Click **Create Notebook**. Check that the notebook is created with metadata and code.
12. Select the cell that starts with '!pip install --upgrade pixiedust, ,' and then click **Play** to run the code.
13. When the installation is complete, restart the Spark kernel by clicking the **Restart Kernel** icon.
14. Wait about 10 seconds for the kernel to restart and then click on the cell with the comment to select it.
15. Import your Cloudant credentials to that cell by completing the following steps:

    1. Click ![Find and Add Data icon](images/find_add_data_icon.png).
    2. Select the **Connections** tab.
    3. Click **Insert to code**.
A dictionary called credentials_1" is created with your Cloudant credentials. If the name is not specified as "credentials_1", rename the dictionary to "credentials_1" because this is the name that is required for the notebook code to run.
16. In the cell with the database name (dbname) enter the name of the Cloudant database that is the source of data, for example, `iotp_yourWIoTPorgId_default_Year-month-day`.
17. Save the notebook. The notebook is ready to be executed.


### 2. Run the analysis
{: #run_analysis}

1.	Select the cell that contains your Cloudant credentials.
2.	Click **Play** to execute the cell code.
3.	Check the execution results and analyze the python code that is used in each cell.
4.	Repeat steps 2 and 3 for each cell. For cells with the **User input required** specified, you must provide new input values to the variable that is defined in the next code cell before its execution.

**Note:** Some cells run background Spark jobs and might take longer to finish. When the code execution inside a cell is complete, the asterisk `*` turns into a number, for example, In `[*]` turns into In `[1]`. After completing the steps, you can create cloud rules in the {{site.data.keyword.iot_short_notm}} to automatically generate alerts when anomalies are detected.


### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails.


## What's next?

For more information about DSX, see the following resources:

 - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window}
 - [DSX Community Notebooks and Tutorials ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} following the links to learn more about Jupyter notebooks.
 - [Analytics recipes in the Watson IoT Platform cookbook ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
