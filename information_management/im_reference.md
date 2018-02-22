---

copyright:
years: 2017, 2018
lastupdated: "2018-02-21"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Data management reference information

Use the following reference information to learn about the restrictions that apply when using the data management component of Watson IoT Platform. 

## Schema restrictions

The following types of schema are used in data management:
- Event type schema
- Logical interface schema
- Thing type schema

The following table lists the restrictions that apply to each schema type:

Schema type       |        Constraint
------------------- | -------------
All        | All schemas must conform to valid JSON. 
All        | All schemas must conform to valid JSON schema. 
All        | The root of all schemas must be of type "object". 
All        | All schemas have a maximum of seven levels of nesting.
Logical interface        |  All properties that are specified as required must have a default value defined. 
Logical interface     | If an object is defined in the schema, at least one property must be defined for that object. 
Logical interface | If an array is defined in the schema, associated items must be of only one type, for example only of type "string". 
Thing type        | Only top level properties are allowed. No nesting is allowed beyond the first level. 
Thing type        | The top level property must be of type "object".
Thing type        | The top level property must reference a $logicalInterfaceRef and a type. The value of $logicalInterfaceRef is the ID or alias name of a logical interface. The type must be set to "object" or "array". 

## Examples of valid and invalid schemas

### Example 1  - a valid logical interface schema defintion
The following example defines a logical interface schema that conforms to the constraints that are outlined in the "Schema restrictions" section:

  - The schema conforms to valid JSON and valid JSON schema.
  - The schema is of type "object".
  - The schema has a nesting level of less than seven. 
  - Two properties are defined for the schema. 
  - The required properties **a** and **b** have default values specified.

```
{
 "type": "object",
 "properties":{
    "a":{
       "type":"string"
    },
    "b":{
       "type":"number"
    }
  },
  "default":{"a":"HelloWorld", "b":2},
  "required": ["a", "b"]
}
```


### Example 2 - an invalid logical interface schema defintion
The following example shows an invalid logical interface schema. The schema is invalid because the array is defined to contain items of type "string" or type "number". If an array is defined in the schema, the associated items must be of only one type, for example "string".

```
{
 "type": "object",
 "properties":{
    "a":{
      "type":"array",
       "items":{
          "type": ["string", "number"]
       }
    }
  }
}
```
### Example 3 - a valid thing type schema defintion
The following example defines a thing type schema that conforms to the constraints that are outlined in the "Schema restrictions" section:

  - The schema conforms to valid JSON and valid JSON schema.
  - The schema is of type "object".
  - The schema has a nesting level of less than seven. 
  - The schema only defines top-level properties. 
  - The top-level properties reference a $logicalInterfaceRef and a type that is set to either "array" or "object". The type "array" can be used to aggregate a number of devices, for example, a number of temperature sensors. The type "object" can be used to reference a single device, for example, a single humidity sensor.   
  - The required properties do not need a default value to be specified. This constraint applies only to logical interface schemas. A default value cannot be specified in this schema type. 

```
{
   "type" : "object",
   "properties" : {
       "temperatureSensors": {
           "description": "Aggregated temperature sensors",
           "$logicalInterfaceRef": "temperatureSensorsLogicalInterface",
           "type" : "array"
       },
       "humiditySensor": {
           "description": "The humidity sensor device",
           "$logicalInterfaceRef": "58c135ea52faff0001678f06"
            "type" : "object"
       }
   },
      "required" : [
       "temperatureSensors",
       "humiditySensor"
   ]
  }
```
