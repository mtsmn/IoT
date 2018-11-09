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

# Comprensión del lenguaje de expresión de correspondencia
{: #mapping_expression}

Puede utilizar el lenguaje de expresión de correspondencia para manipular y combinar datos y para dar formato a los resultados de cualquier consulta que pueda ejecutar en los datos procesados. El lenguaje de expresión de correspondencia es un subconjunto de [JSONata ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/index.html){:new_window} y se puede utilizar al definir [correlaciones](ga_im_definitions.html#resources) o al crear [reglas](../information_management/im_rules.html). 

JSONata es un lenguaje de transformaciones y una consulta ligera para datos JSON. También está disponible una herramienta [JSONata Exerciser ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://try.jsonata.org/){:new_window}, que proporciona una forma rápida y conveniente de probar JSONata.

La siguiente información muestra las funciones y operadores clave que se soportan actualmente, junto con algunos ejemplos de cómo puede utilizarlos. 

## Operadores de JSONata
{: #operators}

Todos los [Operadores de JSONata ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/operators.html){:new_window} están soportados, excepto los operadores siguientes:

- ~> (encadenamiento de funciones)
- ^(…) (ordenar por)
- := (enlace de variables)

**Nota:** 
- Utilice paréntesis ( ) para agrupar expresiones y para alterar la prioridad de operador.
- Utilice comillas simples o dobles para rodear las series y los nombres de propiedades, por ejemplo $event.object.'ab' 
- Utilice las tildes graves para rodear los nombres de propiedad que contienen caracteres especiales, incluidos espacios, por ejemplo 
```
{"x y":22}.`x y`
```
- Utilice las tildes graves para rodear los nombres de propiedad que empiezan con un número, por ejemplo
```
``7emperature`
```

## Funciones de JSONata
{: #functions}
Se da soporte a las siguientes funciones de JSONata: 

 - Todas las funciones de [Serie ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/string-functions.html){:new_window}
 - Todas las funciones de [Numéricas ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/numeric-functions.html){:new_window} 	
 - Todas las funciones de [Agregación numérica ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 
 - Todas las funciones de [Booleanas ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/boolean-functions.html){:new_window}  
 - Funciones de [Matriz $count y $append ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/array-functions.html){:new_window} 	

## Ampliación del lenguaje de expresión de correspondencia

El lenguaje de expresión de correlación se ha ampliado para utilizarlo con la característica de gestión de datos a través de la introducción de las variables $event, $state e $instance. El JSON se limita a estas variables antes de evaluar la expresión. La siguiente tabla proporciona una visión general de estas variables, definidas para utilizarlas en expresiones:

Variable                   | JSON de ejemplo de entrada     | Ejemplo como expresión    | Utilícela para...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Seleccionar una propiedad de un suceso de entrada para utilizar en una expresión. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Seleccionar una propiedad en el estado de dispositivo para utilizar en una expresión.
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Seleccionar dispositivo o atributo de tipo de dispositivo para utilizar en una expresión. 

En el ejemplo siguiente se utiliza el objeto siguiente como entrada a un suceso:  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
El objeto se convierte en una matriz utilizando la expresión siguiente:

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
La expresión da como resultado la generación de la salida siguiente: 
```
 [
    35,
    72,
    1024
  ]
```

Puede invertir este ejemplo y convertir una matriz de la entrada en un objeto. En el ejemplo siguiente se utiliza la matriz siguiente como entrada a un suceso:
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
La matriz se convierte en un objeto utilizando la expresión siguiente: 
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
 La expresión da como resultado la generación de la salida siguiente: 
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
También puede definir una expresión que combine el uso de estas variables. En el ejemplo siguiente, se ha definido una propiedad *temp_adjustment* en los metadatos de dispositivo y se utiliza para calibrar la lectura de sucesos. La propiedad se define en una correlación pero se puede asignar a muchos dispositivos. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


El operador punto "." se utiliza para el acceso de objeto con una clave literal, por ejemplo, $event.object.hh. La expresión en el lado izquierdo está restringida a una propiedad específica, ya sea en el suceso ($event) o en el estado ($state) o en los metadatos ($instance). La expresión en la parte derecha contiene la información a la que es posible acceder. 

Para obtener más información sobre el operador punto, consulte la sección [Operators ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/operators.html){:new_window} de la documentación de JSONata. 


## Guía del lenguaje

- All [Selección básica ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/basic.html){:new_window} está soportado.
- All [Selección compleja ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/complex.html){:new_window} está soportado, excepto para los comodines.
- Las expresiones condicionales y entre paréntesis están soportadas como parte de [Programming constructs ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/programming.html){:new_window}. Las variables están soportadas a través del uso de las variables $instance, $state y $event como parte del lenguaje de correlación ampliado que es específico de la gestión de datos de Watson IoT Platform. Las variables "$" y "$$" que se utilizan en JSONata no están soportadas en este momento.

## Construcción de la salida
Puede especificar cómo se presentan los datos procesados en la salida utilizando los constructores de matrices o los constructores de objetos.

### Constructores de matrices soportados
Las matrices se pueden construir mediante la inclusión de una lista separada por comas de literales o expresiones en un par de corchetes []. Las comas se utilizan para separar varias expresiones dentro del constructor de matriz, incluida la generación de secuencia, por ejemplo, *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* y *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### Constructores de objetos soportados
{: #constructors}
Puede construir objetos JSON en la salida utilizando un par de llaves {} que contienen valores de claves o pares separados
por comas, con cada clave y valor separado por dos puntos, por ejemplo, *{key1 : value1, key2:value2}* o *{"hello" : "world"}*. La clave de objeto debe ser una serie.  


## Ejemplo: Utilización de matrices para procesar e informar datos de temperatura
Las siguientes secciones se basan en el ejemplo de [Cómo empezar con la Gestión de datos utilizando API REST](ga_im_example.html) para mostrar cómo podría utilizar las matrices para mantener una ventana deslizante de lecturas de temperatura, y calcular la suma actual o el promedio de las lecturas contenidas en esa ventana. 

Una ventana deslizante almacena datos en orden de llegada. En lugar de mantener todos los datos siempre insertados, las ventanas deslizantes pueden configurarse para expulsar datos de forma incremental. Cuando una ventana deslizante se llena, cualquier futura inserción da como resultado el desalojo del elemento de datos más antiguo de la ventana.

El siguiente ejemplo muestra una ventana deslizante configurada con una política de desalojo basada en recuento de un tamaño de 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

El siguiente ejemplo muestra cómo se puede modificar la configuración de archivo de esquema de interfaz lógica que se muestra en el paso 7 de la [Guía paso a paso 1](ga_im_index_scenario.html#step4) añadiendo una matriz llamada *tempReadings*. La matriz se utiliza para mantener una ventana de los 5 últimos valores que envían los dispositivos para los 5 últimos sucesos. Si no hay valores almacenados en *tempReadings*, la matriz se establece en [0] y la siguiente lectura que se reciba se añadirá a la matriz que se ampliará hasta las 5 lecturas recibidas. Cuando se reciban 5 lecturas, una nueva lectura provocará la eliminación del resultado más antiguo de la ventana. 

**Notas importantes:** Debe establecer *tempReadings* como "Obligatorio" y como matriz de forma predeterminada. 

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature" : {
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
   "required" : [
      "tempReadings"
  ],
  "type": "object"
}
```
La siguiente sección muestra un ejemplo de cómo puede configurar el recurso de correlación para calcular la lectura de temperatura promedio y la suma de todas las temperaturas basándose en las lecturas contenidas en la ventana deslizante actual:


```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt" : {
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
**Nota:** La matriz *$state.tempReadings* se vuelve a calcular antes de utilizarla en las funciones $average y $sum. El recálculo de la matriz es necesario para asegurarse de que la matriz contiene los valores actuales cuando se evalúa la expresión *tempAverage* o *tempSum*, porque el orden de las expresiones de correlación no se puede controlar.

El siguiente ejemplo muestra la temperatura promedio y la suma de temperaturas basadas en las 5 lecturas de temperatura de la ventana deslizante:
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

## Manejo de desajustes entre la expresión de correlación y los datos de entrada

Es posible que una actualización de estado falle cuando una de las expresiones de correlación contiene una referencia a los datos de entrada que no se especifican en el suceso publicado.

Por ejemplo, puede configurar las expresiones siguientes:
```
temperature = $event.t 
humidity = $event.hum
```
donde *t* es una propiedad opcional para el suceso. 

Si se recibe un suceso que contiene solo datos de humedad, por ejemplo, `{"humidity":22}`, la expresión `temperature = $event.t` no se evalúa, porque la propiedad opcional *t* no se especifica en el suceso de dispositivo publicado.

La actualización de estado falla. La propiedad de estado de humedad no se actualiza, y se publica un mensaje de error en el tema de error de MQTT para el dispositivo:
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
Para evitar anomalías de actualización de estado debido a datos opcionales no especificados, puede utilizar la función $exists en combinación con un condicional ternario para especificar un valor predeterminado para las propiedades opcionales. En el ejemplo siguiente se define un valor predeterminado de *0* para la propiedad *t*: 

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

Al definir un valor predeterminado para la propiedad opcional de esta forma, la expresión se evalúa correctamente, incluso cuando la propiedad *t* no se especifica en el suceso de dispositivo publicado. 

Por lo tanto, si se recibe el suceso `{"humidity":22}`, la actualización de estado se completa correctamente y el estado de dispositivo se establece en `{"humidity":22, "temperature":0}`.
