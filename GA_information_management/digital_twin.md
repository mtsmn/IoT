---

copyright:
  years: 2016, 2017
lastupdated: "2017-05-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Digital twins
{: #digital_twin}

A digital twin is any digital representation of a device or system and how it behaves within its environment. IBM Watson IoT Platform is solving this problem with the introduction of ‘Device Twins’ and ‘Asset Twins’.  
{: shortdesc}

Device Twins and Asset Twins can help you:
- Provide your application developers with consistent interfaces to access event-driven device data in a REST-like manner.
- Normalize data from devices of different makes or models that publish data in different formats.
- Combine event data from several different device types to model any given IoT thing.

With Watson IoT Platform you can realize the digital twin concept using:
- Application Interfaces/Logical Interfaces
- Device type meta data
- Device instance meta data
- Thing instance metadata (API)
- Device management (unclear how this works with interfaces)

## Device twin  
{: device_twin}  
A device twin is a logical model of the properties and events coming from a particular sensor/device. Once defined and instantiated, it provides a consistent means of interacting with a device in a REST-like manner. Because the models can be shared by multiple devices of different makes and models, the IoT application is now insulated from variability and change within the device ecosystem.

## Asset twin  
{: #asset_twin}  
An Asset Twin lets you take this concept one step further.  Now you can model higher level ‘things’ that group together sensors and devices into a single entity for a developer to interact with.  For example your Asset Twin might represent a class of automobile that in turn represents hundreds of underlying sensors monitoring the drivetrain, suspension, & fuel systems.  Now let’s say an IoT based fleet management system called on the state & properties of the automobile Asset Twin, it has a simple and consistent way to monitor all its vehicles regardless of make and model.
