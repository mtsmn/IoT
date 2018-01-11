---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Understanding the mapping expression language
{: #mapping_expression}

You can use the mapping expression language to manipulate and combine data, and to format the results of any queries that you might run on your processed data. The mapping expression language is a subset of [JSONata ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/index.html){:new_window} and can be used when defining [mappings](ga_im_definitions.html#definitions_resources). JSONata is a lightweight query and transformation language for JSON data.

The following information shows the key operators and functions that are currently supported, along with some examples of how you might use them. 

## Supported JSONata operators
{: #operators}

The following subset of JSONata operators are supported: 

Type of operator                   | Supported operators     | Notes   
------------- | ------------- | -------------
Arithmetic | *+* - / * % | The % operator returns the remainder.
Comparison | < <= > >= != = | The equality operator is = , as it is in JSONata.
Boolean | *in*, *and* | The boolean constants are *true* or *false*.
Conditional ternary | ? | The ? operator evaluates one of two alternative expressions based on the result of a test condition. The operator takes the following form *expression*  ? *value_if_true* : *value_if_false*
String | & | The & operator joins the string values of the operands into a single resultant string.
Sequence generator | .. | Creates an array of increasing integers, starting  with the number on the LHS and ending with the number on the RHS, for example, [1..4] -> [1,2,3,4]. The operands must evaluate to an integer. The sequence generator can only be used within an array constructor [].
Other | . | The dot operator is used for object access with a literal key, for example $event.object.hh. *
* Note: The expression on the left-hand side is constrained to a specific property, either in the event ($event) or the state ($state) or the metadata ($instance). 

**Notes:** 
- Use Parenthesis ( ) for expression grouping and to alter operator precedence
- Use single quotes to surround property names that contain spaces, for example $event.object.'a b' 

## Extending the mapping expression language

The mapping expression language has been extended for use with the data management feature through the introduction of $event, $state and $instance variables. JSON is bound to these variables before the expression is evaluated. The following table provides an overview of these variables, which are defined for use in expressions:

Variable                   | Example input JSON     | Example as an expression    | Use to ...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Select a property of an inbound event to use in an expression. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Select a property on the device state to use in an expression.
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Select device or device type attribute to use in an expression. 

You can also define an expression that combines the use of these variables. In the following example, a *temp_adjustment* property is defined in the device metadata and is used to calibrate the event reading. The property is defined in one mapping but can be applied to many devices. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## Supported JSONata functions
{: #functions}
The following subset of JSONata functions are supported: 

Type of function                   |Function                   | Description | Example
------------- | ------------- | ------------- 
String | $substring(string, start_index[, length]) | String substring, for example, *$substring("Hello World", 3, 5) => "lo Wo"*. 
String | $string(arg) | Casts the argument to a string value, for example, *$string(2) => "2"*.
Numeric | $number(arg) | Casts the argument to a numeric value if possible, for example, *$number(2) => 2*.
Numeric | $sum(array) | Returns the arithmetic sum of an array of numbers, for example, *([1,2,3]) = 6*.
Numeric | $average(array) | Returns the mean value of an array of numbers, for example, *([1,2,3]) = 2.0*.
Boolean | $exists(expression) | Returns *true* if the property in the expression exists, *false* otherwise.
Array | $count(array) | Returns the number of items in the array parameter, for example, *([1,2,3,4]) = 4*. If the array parameter is not an array, but rather a value of another JSON type, then the parameter is treated as a singleton array containing that value, and this function returns *1*.
Array | $append(array1, array2) | Returns an array containing the values in *array1*, followed by the values in *array2*, For example, *([1,2], [3,4]) = [1,2,3,4]*. If either parameter is not an array, then it is treated as a singleton array containing that value, for example *$append("Hello", "World") => ["Hello", "World"]*.

## Arrays
Use JSON arrays to put a collection of values in a specified order. Arrays associate each value in the array with an index or position. To address individual values in an array, you must specify the index by using square brackets after the field name of the array. If the square brackets contain a number, or an expression that evaluates to a number, then the number represents the index of the value to select. An array of numbers can also be used as an index, for example *["a","b","c"][[1,2]] -> ["b", "c"]*. The array *[1,2]* is used as an index that identifies which values to select from the array *["a","b","c"]*. 

Indexes are zero offset, so the first value in an array *arr* is *arr[0]*, for example, *[1,2,3][0] -> 1*. If the number is not an integer, then it is rounded down to an integer, for example *[1,2,3][0.9] -> [1]*.

Use negative indexes to count from the end of the array, for example, *[1,2,3][-1] -> 3*. 

If the specified index exceeds the size of the array, then nothing is selected.

## Constructing output
You can specify how processed data is presented in the output by using array constructors or object constructors.

### Supported array constructors
Arrays can be constructed by enclosing a comma-separated list of literals or expressions in a pair of square brackets []. Commas are used to separate multiple expressions within the array constructor, for example, *[1, 3-1] = [1,2]*.

### Supported object constructors
{: #constructors}
You can construct JSON objects in your output by using a pair of braces {} that contain key values or pairs separated
by commas, with each key and value separated by a colon, for example, *{key1 : value1, key2:value2}* or *{"hello" : "world"}*. The object key must be a string.  


## Example: Using arrays to process and report temperature data
The following sections build on the example in [Getting started with data management](ga_im_example.html) to show how you might use arrays to maintain a sliding window of temperature readings, and calculate the current sum or average of the reading contained within that window. 

A sliding window stores data in the order of arrival. Rather than keeping all the data ever inserted, sliding windows can be configured to evict data in an incremental fashion. When a sliding window fills up, any future insertion results in the eviction of the oldest data item in that window.

The following example shows a sliding window that is configured with a count-based eviction policy of size 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

The following example shows how the logical interface schema file configuration that is shown in step 7 of the [Step-by-step guide](ga_im_index_scenario.html#step4), can be modified by adding an array called *tempReadings*. The array is used to maintain a window of the last 5 values that are sent from devices for the last 5 events. If there are no values stored in *tempReadings*, the array is set to [0] and the next received reading is appended on to the array which grows until 5 readings are received. After 5 readings are received, a new reading results in the removal of the oldest reading from the window. 

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

