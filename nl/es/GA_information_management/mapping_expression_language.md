---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-09"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Visión general del lenguaje de expresión de correlación
{: #mapping_expression}

Puede utilizar el lenguaje de expresión de correlación para manipular y combinar datos y para dar formato a los resultados de cualquier consulta que ejecute en sus datos procesados. El lenguaje de expresión de correlación es un subconjunto de [JSONata ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.jsonata.org/index.html){:new_window} y se puede utilizar al definir [correlaciones](ga_im_definitions.html#definitions_resources). JSONata es un lenguaje de transformaciones y una consulta ligera para datos JSON.

La siguiente información muestra las funciones y operadores clave que se soportan actualmente, junto con algunos ejemplos de cómo puede utilizarlos. 

## Operadores JSONata soportados
{: #operators}

Está soportado el siguiente subconjunto de operadores JSONata: 

Tipo de operador| Operadores soportados| Notas   
------------- | ------------- | -------------
Aritmético | *+* - / * % | El operador % devuelve el resto.
Comparación| < <= > >= != = | El operador de igualdad es = , como en JSONata.
Booleano| *in*, *and* | Las constantes booleanas son *true* o *false*.
Ternario condicional| ? | El operador ? evalúa una de dos expresiones alternativas en función del resultado de una condición de prueba. El operador toma la siguiente forma *expresión*  ? *valor_si_verdad* : *valor_si_falso*
Serie | & | El operador & une los valores de serie de los operandos en una única serie resultante.
Generador de secuencia | .. | Crea una matriz de enteros crecientes, empezando con el número en LHS y acabando con el número en RHS, por ejemplo, [1..4] -> [1,2,3,4]. Los operandos deben evaluar un entero. El generador de secuencias solo se puede utilizar en un constructor de matrices [].
Otro| . | El operador punto se utiliza para el acceso de objeto con una clave literal, por ejemplo, $event.object.hh. *
e:* La expresión de la izquierda se restringe a una propiedad específica, tanto en el suceso ($event), el estado ($state) o los metadatos ($instance).

**Notas:** 
- Utilice paréntesis ( ) para agrupar expresiones y para alterar la prioridad de operador.
- Utilice las comillas simples para rodear nombres de propiedad que contienen espacios, por ejemplo, $event.object.'a b' 

## Ampliar el lenguaje de expresión de correlación

El lenguaje de expresión de correlación se ha ampliado para utilizarlo con la característica de gestión de datos a través de la introducción de las variables $event, $state e $instance. El JSON se limita a estas variables antes de evaluar la expresión. La siguiente tabla proporciona una visión general de estas variables, definidas para utilizarlas en expresiones:

Variable| JSON de ejemplo de entrada| Ejemplo como expresión| Utilícela para...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Seleccionar una propiedad de un suceso de entrada para utilizar en una expresión. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Seleccionar una propiedad en el estado de dispositivo para utilizar en una expresión. 
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Seleccionar dispositivo o atributo de tipo de dispositivo para utilizar en una expresión.

También puede definir una expresión que combine el uso de estas variables. En el siguiente ejemplo, se define una propiedad *temp_adjustment* en los metadatos de dispositivo y se utiliza para calibrar la lectura de suceso. La propiedad se define en una correlación pero se puede asignar a muchos dispositivos. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## Funciones JSONata soportadas
{: #functions}
Está soportado el siguiente subconjunto de funciones JSONata: 

Tipo de función|Función | Descripción | Ejemplo
------------- | ------------- | ------------- 
Serie | $substring(string, start_index[, length]) | Subserie de serie, por ejemplo, *$substring("Hello World", 3, 5) => "lo Wo"*. 
Serie | $string(arg) | Emite el argumento en un valor de serie, por ejemplo, *$string(2) => "2"*.
Numérico| $number(arg) | Emite el argumento en un valor numérico si es posible, por ejemplo, *$number(2) => "2"*.
Numérico| $sum(array) | Devuelve la suma aritmética de una matriz de números, por ejemplo, *([1,2,3]) = 6*.
Numérico| $average(array) | Devuelve el valor medio de una matriz de números, por ejemplo, *([1,2,3]) = 2,0*.
Booleano| $exists(expression) | Devuelve *true* si existe la propiedad en la expresión, de lo contrario devuelve *false*.
Matriz| $count(array) | Devuelve el número de elementos en el parámetro de matriz, por ejemplo, *([1,2,3,4]) = 4*. Si el parámetro de matriz no es una matriz sino un valor de otro tipo JSON, el parámetro se trata como una matriz de singleton que contiene ese valor y esta función devuelve *1*.
Matriz| $append(array1, array2) | Devuelve una matriz que contiene los valores de *array1*, seguida por los valores de *array2*. Por ejemplo, *([1,2], [3,4]) = [1,2,3,4]*. Si el parámetro no es una matriz, se trata como una matriz singleton que contiene ese valor, por ejemplo *$append("Hello", "World") => ["Hello", "World"]*.

## Matrices
Utilice matrices JSON para poner una colección de valores en un orden especificado. Las matrices asocian cada valor en la matriz con un índice o posición. Para direccionar valores individuales en una matriz, debe especificar el índice utilizando corchetes después del nombre de campo de la matriz. Si los corchetes contienen un número o una expresión que evalúa a un número, el número representa el índice del valor a seleccionar. Una matriz de números también se puede utilizar como índice, por ejemplo *["a","b","c"][[1,2]] -> ["b", "c"]*. La matriz *[1,2]* se utiliza como índice que identifica qué valores seleccionar de la matriz *["a","b","c"]*. 

Los índices son de desplazamiento cero, de modo que el primer valor en una matriz *arr* es *arr[0]*, por ejemplo, *[1,2,3][0] -> 1*. Si el número no es un entero, se redondea a la baja, por ejemplo *[1,2,3][0.9] -> [1]*.

Utilice índices negativos para el recuento desde el final de la matriz, por ejemplo *[1,2,3][-1] -> 3*. 

Si el índice especificado supera el tamaño de la matriz, no se selecciona nada.

## Construcción de la salida
Puede especificar cómo se presentan los datos procesados en la salida utilizando constructores de matrices o constructores de objetos.

### Constructores de matrices soportados
Las matrices se pueden construir delimitando una lista de literales o expresiones separados por comas en un par de corchetes []. Las comas se utilizan para separar varias expresiones dentro del constructor de matrices, por ejemplo, *[1, 3-1] = [1,2]*.

### Constructores de objetos soportados
{: #constructors}
Puede construir objetos JSON en su salida utilizando un par de llaves {} que contengan valores clave o pares separados por comas, con cada clave y valor separado por dos puntos, por ejemplo, *{key1 : value1, key2:value2}* o *{"hello" : "world"}*. La clave de objeto debe ser una serie.  


## Ejemplo: Utilización de matrices para procesar e informar datos de temperatura
Las siguientes secciones se basan en el ejemplo de [Cómo empezar con la Gestión de datos](ga_im_example.html) para mostrar cómo podría utilizar las matrices para mantener una ventana deslizante de lecturas de temperatura, y calcular la suma actual o el promedio de las lecturas contenidas en esa ventana. 

Una ventana deslizante almacena datos en orden de llegada. En lugar de mantener todos los datos siempre insertados, las ventanas deslizantes pueden configurarse para expulsar datos de forma incremental. Cuando una ventana deslizante se llena, cualquier futura inserción da como resultado el desalojo del elemento de datos más antiguo de la ventana.

El siguiente ejemplo muestra una ventana deslizante configurada con una política de desalojo basada en recuento de un tamaño de 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

El siguiente ejemplo muestra cómo se puede modificar la configuración de archivo de esquema de interfaz lógica que se muestra en el paso 7 de la [Guía paso a paso](ga_im_index_scenario.html#step4) añadiendo una matriz llamada *tempReadings*. La matriz se utiliza para mantener una ventana de los 5 últimos valores que envían los dispositivos para los 5 últimos sucesos. Si no hay valores almacenados en *tempReadings*, la matriz se establece en [0] y la siguiente lectura que se reciba se añadirá a la matriz que se ampliará hasta las 5 lecturas recibidas. Cuando se reciban 5 lecturas, una nueva lectura provocará la eliminación del resultado más antiguo de la ventana. 

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
**Nota:** La matriz *$state.tempReadings* se vuelve a calcular antes de utilizarla en las funciones $average y $sum. Es necesario volver a calcular la matriz para asegurarse de que la matriz contiene los valores actuales cuando se evalúan las expresiones *tempAverage* o *tempSum*, porque el orden de las expresiones de correlación no se puede controlar.
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

