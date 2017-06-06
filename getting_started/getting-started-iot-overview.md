---

copyright:
  years: 2017
lastupdated: "2017-06-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="\_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting started guides for {{site.data.keyword.iot_short_notm}}
{: #getting-started}

The getting started guides are designed to guide you through the basics of developing a ready-for-production end-to-end IoT prototype system with {{site.data.keyword.iot_short_notm}} and are written for developers that are new to working with {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

With each guide you get one or more GitHub-sourced sample that is coded according to {{site.data.keyword.iot_short_notm}} best practices. These samples can be scaled, secured, integrated with systems, and can generally fit right into your devOps development and build processes.

Each guide provides you with a step-by-step process that quickly leads you to a deployed and running solution that demonstrates one or more {{site.data.keyword.iot_short_notm}} features.

The technically inclined can expand on the quick solution by expanding on and adapting the sample code to their own environment, or by complementing the quick solution with hardware examples that more closely simulates what you might encounter in your production environment. The guides provide additional links to task-relevant documentation, API docs, client libraries (SDKs), instructional videos, and other training material.

You can follow the guides in order, where the first guide provides a foundation upon which the following guides build. If you want to jump around and follow your own path through the series, each guide also comes with a list of requirements. For example, if you already have a {{site.data.keyword.iot_short_notm}} organization set up, you can jump straight into guides number two and three, and start exploring rules, actions, and a sample monitoring app. The requirements tell you the format of the device data that these guides require, and you can set up your own environment accordingly.
{: tip}

## List of guides
{: #list-of-guides}  

The following guides are available:

| Guide | Description | Screenshot |   
| ----- | ---- | --- |  
| [1. Connecting a conveyor belt device](getting-started-iot-conveyor.html) | Use CloudFoundry CLI to install an {{site.data.keyword.iot_short_notm}} organization and a sample GitHub-sourced conveyor belt simulation application. </br> If you prefer something more physical, you can set up your own Raspberry Pi based conveyor belt simulator, and connect it to your new {{site.data.keyword.iot_short_notm}} org. </br> As part of this guide you will also set up some basic {{site.data.keyword.iot_short_notm}} dashboard cards to visualize and monitor your device data. | ![Conveyor belt app](images/app_conveyor_belt.png "Conveyor belt app") |  
| [2. Using basic real-time rules and actions](getting-started-iot-rules.html) | With one or more devices connected to your {{site.data.keyword.iot_short_notm}} organization and sending data, you can now set up business rules and trigger actions based on the datapoints that are received.  In our example we set up an alert action that notifies a plant operator if the speed of his conveyor belt drops below a certain threshold. | ![Example rule](images/slow_rule.svg "Example rule") |  
| [3. Monitoring your device data](getting-started-iot-monitoring.html) | This guide shows you how to expand on the built-in {{site.data.keyword.iot_short_notm}} device monitoring cards by connecting a GitHub-sourced monitoring app to see conveyor belt data in real time. | ![Monitor app](images/app_monitor.png "Monitor app") |  
| [4. Simulating a large number of devices](getting-started-iot-large-scale-simulation.html) | In the first guide you set up a basic device simulator that let you manually simulate one or more conveyor belts. In this guide we expand on this simulation by adding large numbers of self running simulators to your environment. This will let you test drive the basic analytics and monitoring from the previous guides in a more realistic, multi device environment. </br>**Important:** The application requires 512 MB of memory, which is more than is allocated by default and which also exceeds the amount available to free trial accounts (Bluemix Trial Account and Standard Account). Subscription and Pay-As-You-Go account holders can increase the allocated memory to 512 MB.  Free trial account holders need to upgrade to a Subscription or Pay-As-You-Go account. For more information about {{site.data.keyword.Bluemix_notm}} account types, see [Account types](/docs/pricing/index.html#pricing). | ![Monitor app](images/multi_device.png "Monitor app") |  
