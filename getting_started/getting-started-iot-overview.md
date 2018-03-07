---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting Started guides for {{site.data.keyword.iot_short_notm}}
{: #getting-started}

The Getting Started guides are designed to guide you through the basics of developing a ready-for-production end-to-end IoT prototype system with {{site.data.keyword.iot_full}} and are written for developers who are new to working with {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

Each guide provides a step-by-step process that quickly leads you to a deployed and running solution that demonstrates one or more {{site.data.keyword.iot_short_notm}} features.

## Using the guides  
{: #using-guides}
In each guide, you get at least one GitHub-sourced sample that is coded according to {{site.data.keyword.iot_short_notm}} best practices. These samples can be scaled, secured, integrated with systems, and can generally fit right into your devOps development and build processes.

The technically inclined can expand on the quick solution by adapting the sample code to their own environment or by complementing the quick solution with hardware examples that more closely simulates what you might encounter in your production environment. The guides provide additional links to task-relevant documentation, API docs, client libraries (SDKs), instructional videos, and other training material.

You can follow the guides in order, where the first guide provides a foundation upon which the following guides build. If you want to jump around and follow your own path through the series, each guide also comes with a list of requirements. The requirements tell you the format of the device data that these guides require, and you can set up your own environment accordingly.
{: tip}

## List of guides
{: #list-of-guides}  

The following guides are available:

| Guide | Description |    
| ----- | ---- |   
| [1. Connecting a conveyor belt device](getting-started-iot-conveyor.html) | Get started by installing an {{site.data.keyword.iot_short_notm}} organization and then connect a sample GitHub-sourced conveyor belt simulation application, or if you prefer something more physical, your own Raspberry-Pi-based conveyor belt simulator. </br> Then set up some basic {{site.data.keyword.iot_short_notm}} dashboard cards to visualize and monitor your device data. |   
| [2. Monitoring your device data](getting-started-iot-monitoring.html) | Connect a Node.js or widget-library-based monitoring app to see conveyor belt data in real time.  
| [3. Simulating a large number of devices](getting-started-iot-large-scale-simulation.html) | Expand on the simple device simulation by adding large numbers of device simulators to your environment. </br>**Important:** The application requires 512 MB of memory, which is more than is allocated by default and which also exceeds the amount available to free trial accounts, which must first be upgraded to a Subscription or Pay-As-You-Go account. |   
