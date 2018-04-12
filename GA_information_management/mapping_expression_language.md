---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Understanding the mapping expression language
{: #mapping_expression}

You can use the mapping expression language to manipulate and combine data, and to format the results of any queries that you might run on your processed data. The mapping expression language is a subset of [JSONata ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/index.html){:new_window} and can be used when defining [mappings](ga_im_definitions.html#resources) or creating [rules](../information_management/im_rules.html). 

JSONata is a lightweight query and transformation language for JSON data. A [JSONata Exerciser ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://try.jsonata.org/){:new_window} tool is also available, providing a quick and convenient way to try out JSONata.

The following information shows the key operators and functions that are currently supported, along with some examples of how you might use them. 

## JSONata operators
{: #operators}

All [JSONata Operators ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/operators.html){:new_window} are supported except for the following operators:

- ~> (function chaining)
- ^(…) (order-by)
- := (variable binding)

**Note:** 
- Use Parenthesis ( ) for expression grouping and to alter operator precedence
- Use single or double quotes to surround strings and property names, for example $event.object.'ab' 
- Use backticks to surround property names that contain special characters, including spaces, for example 
```
{"x y":22}.`x y`
```
- Use backticks to surround property names that begin with a number, for example
```
`7emperature`
```

## JSONata functions
{: #functions}
The following JSONata functions are supported: 

 - All [String ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/string-functions.html){:new_window} functions
 - All [Numeric ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/numeric-functions.html){:new_window} functions 	
 - All [Numeric Aggregation ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/aggregation-functions.html){:new_window} functions 
 - All [Boolean ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/boolean-functions.html){:new_window} functions  
 - [$count and $append Array ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/array-functions.html){:new_window} functions 	

## Extending the mapping expression language

The mapping expression language has been extended for use with the data management feature through the introduction of $event, $state and $instance variables. JSON is bound to these variables before the expression is evaluated. The following table provides an overview of these variables, which are defined for use in expressions:

Variable                   | Example input JSON     | Example as an expression    | Use to ...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Select a property of an inbound event to use in an expression. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Select a property on the device state to use in an expression.
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Select device or device type attribute to use in an expression. 

The following example uses the following object as the input to an event:  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
The object is converted to an array by using the following expression:

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
The expression results in the generation of the following output: 
```
 [
    35,
    72,
    1024
  ]
```

You can reverse this example, and convert an array from the input to an object. The following example uses the following array as the input to an event:
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
The array is converted to an object by using the following expression: 
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
 The expression results in the generation of the following output: 
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
You can also define an expression that combines the use of these variables. In the following example, a *temp_adjustment* property is defined in the device metadata and is used to calibrate the event reading. The property is defined in one mapping but can be applied to many devices. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


The dot operator "." is used for object access with a literal key, for example $event.object.hh. The expression on the left-hand side is constrained to a specific property, either in the event ($event) or the state ($state) or the metadata ($instance). The expression on the right-hand side contains the information that you might want to access. 

For more information about the dot operator, see the [Operators ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/operators.html){:new_window} section of the JSONata documentation. 


## Language guide

- All [Basic selection ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/basic.html){:new_window} is supported.
- All [Complex selection ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/complex.html){:new_window} is supported, except for wildcards.
- Conditional and parenthesized expressions are supported as part of [Programming constructs ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/programming.html){:new_window}. Variables are supported through the use of the $instance, $state, and $event variables as part of the extended mapping language which is specific to Watson IoT Platform data management. The "$" and "$$" variables that are used in JSONata are not currently supported.

## Constructing output
You can specify how processed data is presented in the output by using array constructors or object constructors.

### Supported array constructors
Arrays can be constructed by enclosing a comma-separated list of literals or expressions in a pair of square brackets []. Commas are used to separate multiple expressions within the array constructor inlcuding sequence generation, for example, *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* and *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### Supported object constructors
{: #constructors}
You can construct JSON objects in your output by using a pair of braces {} that contain key values or pairs separated
by commas, with each key and value separated by a colon, for example, *{key1 : value1, key2:value2}* or *{"hello" : "world"}*. The object key must be a string.  


## Example: Using arrays to process and report temperature data
The following sections build on the example in [Getting started with data management by using REST APIs](ga_im_example.html) to show how you might use arrays to maintain a sliding window of temperature readings, and calculate the current sum or average of the reading contained within that window. 

A sliding window stores data in the order of arrival. Rather than keeping all the data ever inserted, sliding windows can be configured to evict data in an incremental fashion. When a sliding window fills up, any future insertion results in the eviction of the oldest data item in that window.

The following example shows a sliding window that is configured with a count-based eviction policy of size 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

The following example shows how the logical interface schema file configuration that is shown in step 7 of the [Step-by-step guide 1](ga_im_index_scenario.html#step4), can be modified by adding an array called *tempReadings*. The array is used to maintain a window of the last 5 values that are sent from devices for the last 5 events. If there are no values stored in *tempReadings*, the array is set to [0] and the next received reading is appended on to the array which grows until 5 readings are received. After 5 readings are received, a new reading results in the removal of the oldest reading from the window. 

**Important Notes:** You must set *tempReadings* as "required", and as an array by default. 

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature": {
          "description" : "latest temperature reading in degrees Celsius",
          "type" : "number",
          "minimum" : -273.15,
          "default" : 0.0
      },
      "tempAverage": {
          "type": "number"
      },
      "tempReadings": {
          "default": [],
          "items": {
              "type": "number"
          },
         "type": "array"
      },
      "tempSum": {
          "type": "number"
      }
  },
  "required": [
      "tempReadings"
  ],
  "type": "object"
}
```
The following section shows an example of how you can configure the mappings resource to calculate the average temperature reading, and the sum of all temperature readings based on the readings that are contained within the current sliding window:


```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt": {
               "tempAverage": "$average($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))",
               "tempReadings": "$count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t)",
               "tempSum": "$sum($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))"
           }
       },
       "updated": "2017-10-13T10:05:40Z",
       "updatedBy": "a-8x7nmj-9iqt56kfil",
       "version": "active"
   }
]
```
**Note:** The *$state.tempReadings* array is recalculated before it is used in the $average and $sum functions. The recalculation of the array is required to ensure that the array contains the current values when the *tempAverage* or *tempSum* expression is evaluated, because the order of the mapping expressions cannot be controlled.

The following example shows the average temperature and the summed temperature based on a sliding window of 5 temperature readings:
```
{
    "state": {
        "tempAverage": 18557.6,
        "tempReadings": [
            17591,
            10262,
            25621,
            16676,
            22638
        ],
        "tempSum": 92788
    },
    "timestamp": "2017-10-13T11:07:20Z",
    "updated": "2017-10-13T11:05:40Z"
}
```

## Handling mismatches between mapping expression and input data

A state update might fail when one of the mapping expressions contains a reference to input data that is not specified in the published event.

For example, you might configure the following expressions:
```
temperature = $event.t 
humidity = $event.hum
```
where *t* is an optional property for the event. 

If an event that contains only humidity data is received, for example, `{"humidity":22}`, then the expression `temperature = $event.t` fails to evaluate, because the optional *t* property is not specified in the published device event.

The state update fails. The humidity state property is not updated, and an error message is published to the MQTT error topic for the device:
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
To prevent state update failures because of unspecified optional data, you can use the $exists function in combination with a ternary conditional to specify a default value for optional properties. The following example defines a default value of *0* for the *t* property: 

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

By defining a default value for the optional property in this way, the expression evaluates successfully, even when the *t* property is not specified in the published device event. 

Therefore, if the event `{"humidity":22}` is received, the state update completes successfully and the device state is set to `{"humidity":22, "temperature":0}`.
