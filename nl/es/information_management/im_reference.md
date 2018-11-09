---

copyright:
years: 2017, 2018
lastupdated: "2018-03-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Información de referencia de gestión de datos

Utilice la siguiente información de referencia para obtener información sobre las restricciones que se aplican al utilizar el componente de gestión de datos de {{site.data.keyword.iot_full}}. 

## Restricciones de esquema

Se utilizan los siguientes tipos de esquema en la gestión de datos:
- Esquema de tipo de suceso
- Esquema de interfaz lógica
- Esquema de tipo de cosa

En la tabla siguiente se listan las restricciones que se aplican a cada tipo de esquema:

Tipo de esquema       |        Restricción
------------------- | -------------
All        | Todos los esquemas deben ajustarse a JSON válido. 
All        | Todos los esquemas deben ajustarse al esquema JSON válido. 
All        | La raíz de todos los esquemas debe ser de tipo "objeto". 
All        | Todos los esquemas tienen un máximo de siete niveles de anidamiento.
Interfaz lógica        |  Todas las propiedades que se especifican como es necesario deben tener un valor predeterminado definido. 
Interfaz lógica     | Si se ha definido un objeto en el esquema, debe definirse al menos una propiedad para ese objeto. 
Interfaz lógica | Si se ha definido una matriz en el esquema, los elementos asociados deben ser de un solo tipo, por ejemplo solo de tipo "serie". 
Tipo de cosa        | Solo se permiten propiedades de nivel superior. No se permite ningún anidamiento más allá del primer nivel. 
Tipo de cosa        | La propiedad de nivel superior debe ser de tipo "objeto".
Tipo de cosa        | La propiedad de nivel superior debe hacer referencia a un $logicalInterfaceRef y a un tipo. El valor de $logicalInterfaceRef es el identificador o el nombre de alias de una interfaz lógica. El tipo debe establecerse en "objeto" o "matriz". 

## Ejemplos de esquemas válidos y no válidos

### Ejemplo 1: Una definición de esquema de interfaz lógica válida
El ejemplo siguiente define un esquema de interfaz lógica que se ajusta a las restricciones que se describen en la sección "Restricciones de esquema":

  - El esquema se ajusta a JSON válido y al esquema de JSON válido.
  - El esquema es de tipo "objeto".
  - El esquema tiene un nivel de anidamiento de menos de siete. 
  - Se definen dos propiedades para el esquema. 
  - Las propiedades necesarias **a** y **b** tienen valores predeterminados especificados.

```
{
 "type": "object",
 "properties":{
    "a":{
       "type":"string"
    },
    "b":{
       "type": "number"
      }
  },
  "default":{"a":"HelloWorld", "b":2},
  "required": ["a", "b"]
}
```


### Ejemplo 2: Una definición de esquema de interfaz lógica no válida
En el ejemplo siguiente se muestra un esquema de interfaz lógica no válido. El esquema no es válido porque la matriz se ha definido para que contenga elementos de tipo "serie" o de tipo "número". Si se ha definido una matriz en el esquema, los elementos asociados deben ser de un solo tipo, por ejemplo "serie".

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
### Ejemplo 3: Una definición de esquema de tipo de cosa válida
En el ejemplo siguiente se define un esquema de tipo de cosa que se ajusta a las restricciones que se describen en la sección "Restricciones de esquema":

  - El esquema se ajusta a JSON válido y al esquema de JSON válido.
  - El esquema es de tipo "objeto".
  - El esquema tiene un nivel de anidamiento de menos de siete. 
  - El esquema solo define las propiedades de nivel superior. 
  - Las propiedades de nivel superior hacen referencia a un $logicalInterfaceRef y a un tipo que se establece en "matriz" u "objeto". El tipo "matriz" se puede utilizar para agregar un número de dispositivos o cosas, por ejemplo, un número de sensores de temperatura. El tipo "objeto" se puede utilizar para hacer referencia a un solo dispositivo o cosa, por ejemplo, un solo sensor de humedad.   
  - Las propiedades necesarias no necesitan que se especifique un valor predeterminado. Esta restricción se aplica únicamente a los esquemas de interfaz lógica. No se puede especificar un valor predeterminado en este tipo de esquema. 

```
{
   "type" : "object",
   "properties" : {
       "temperatureSensors": {
           "description": "Aggregated temperature sensors",
           "$logicalInterfaceRef": "IThermometer",
           "type" : "array"
       },
       "humiditySensor": {
           "description": "The humidity sensor device",
           "$logicalInterfaceRef": "5846cd7c6522050001db0e24"
            "type" : "object"
       }
   },
   "required" : [
       "temperatureSensors",
       "humiditySensor"
   ]
  }
```
