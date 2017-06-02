---

copyright:
  years: 2016

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Viewing real-time data with dashboard cards
Use the built in boards and cards to build your own dashboards that visualize your device data in real time.
{:shortdesc}

The raw message data that is sent by a connected device can be viewed in real time by going to Devices and selecting your device. In the window that opens you will see the basic connection information for your device as well as a list of recent events and the raw message payload of the device, broken into event, datapoint, value, and time received.
With dashboard cards you can graphically visualize data set values from one or more devices for quick overview and understanding. Add cards that display the data as raw numbers, real-time graphs, gauges, and more. Arrange the cards and add explanatory text dividers to fine tune your presentation.

**Note:** The boards and cards are currently an experimental feature, and the features and functionality might change. To enable the experimental dashboards, go to Configuration Settings > General and click the Experimental Features switch. Then scroll down and click Confirm all changes.  

## Visualizing the real-time message payload data
IoT Platform provides a built-in dashboard that you can use to display the real-time data that your device is returning. By default, the Overview page displays usage information about your IoT Platform organization such as data and storage consumed. Add device specific cards to this page, to see real-time device data as it flows in.
To add a device specific card to the overview dashboard:
1.	In the overview dashboard, click Add New Card.
2.	In the Edit Generic visualization Card box, scroll down to Devices.
3.	Select a visualization type
Tip: Select Generic visualization to proceed with a basic configuration. You have the option to select card type later.
Click Show more to see the full list of card types.
4.	Select card source data
 1.	Give the data set an identifying name.
 2.	Select the Name
 3.	Select a Property
 4.	Set the Type, Unit, Precision, and min and Max values as appropriate.
 5.	Select visualization.
Select the type and size of visualization that you want to use.  

Header 1 | Header 2
------------- | -------------
Generic visualization |	Display the value of one or more data sets.
Realtime chart |	Display one or more data sets in a real time scrolling chart.
Bar chart |	Display and compare data set values in labeled bars.
Donut chart |	Display and compare two or more data sets in a circular representation.
Value	| Display the raw value of one or more data sets.
Gauge	| Display the value of a data set as a gauge. You can configure display thresholds for good, fair, and critical values of the data set.

6.	Format card and provide a title and a description
7.	Finally, place the card on your dashboard by dragging it to a good location. The Dashboard reflows as you move things around.
Great! You can now see the real-time data of your device!
