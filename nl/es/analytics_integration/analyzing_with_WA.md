---

copyright:
  years: 2017, 2018
lastupdated: "2017-09-18"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Análisis de datos mediante Watson Analytics
{: #WA_integration}  

Puede utilizar {{site.data.keyword.iot_full}} con Watson Analytics (WA) para visualizar y aprender acerca de los datos enviados desde dispositivos conectados con la plataforma.
{: shortdesc}

## Visión general y objetivos

Esta guía le muestra, paso a paso, el proceso de visualización de datos de suceso de dispositivo {{site.data.keyword.iot_short_notm}} utilizando Watson Analytics (WA) como herramienta de analíticas.

Los datos de dispositivo enviados a {{site.data.keyword.iot_short_notm}} se pueden recopilar y almacenar en {{site.data.keyword.Bluemix}} utilizando el servicio de {{site.data.keyword.cloudantfull}} NoSQL DB. Para recopilar los datos, primero debe conectar {{site.data.keyword.iot_short_notm}} con el servicio {{site.data.keyword.cloudant_short_notm}}. Después de recopilar los datos, exporte los datos a un archivo CSV. Puede subir este archivo a WA, donde puede visualizar y analizar los datos de dispositivo. Los datos de dispositivo se almacenan en bases de datos diarias, semanales o mensuales de {{site.data.keyword.cloudant_short_notm}} en función del intervalo de receptáculo configurado.

![Visión general del uso de WA para analizar datos](images/WA_overview.png)

Como parte de esta guía aprenderá:

 - Cómo configurar el almacenamiento de datos de plataforma para que se utilice Cloudant NoSQL DB como servicio histórico.
 - Cómo utilizar el simulador Weather Sensors para generar datos para que los utilice la plataforma.
 - Cómo exportar los datos e importarlos a WA para analizarlos.


## Requisitos previos

Para completar estos pasos debe tener acceso a [{{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} con [Cloudant NoSQL DB ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window}y acceso a [Watson Analytics ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson-analytics){: new_window}.


## Paso 1. Configurar el simulador
{: #WA_sensor_data}


Para realizar un análisis significativo, debe tener datos significativos. Puede simular los datos de sensor reales para aprender acerca de cómo se pueden analizar los datos de dispositivo de Watson IoT Platform mediante Wastson Analytic. Este paso proporciona instrucciones para:
 - [Configurar el simulador con una **instancia existente de {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Configurar el simulador con una **nueva instancia de {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)
 - [Descargar un archivo CSV de ejemplo con datos](#WA_sensor_premade), si no desea utilizar el simulador.


### Configuración del simulador Weather Sensors con una instancia de {{site.data.keyword.iot_short_notm}} existente
{: #sim_existing_platform}

Para simular sucesos de datos de sensor reales en sus organizaciones utilizando el simulador Weather Sensors, primero debe configurar el simulador. Estos pasos presuponen que ya tiene una instancia de {{site.data.keyword.iot_short_notm}} en ejecución.

1. [Genere la clave de API y la señal necesarias para ejecutar el simulador. ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Despliegue la app web del simulador Weather Sensors ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} y siga las instrucciones detalladas.

   Para obtener más información acerca de Weather Sensors, consulte la [guía del simulador Weather Sensors ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Continúe con el [Paso 2. Configurar el conector de base de datos](#WA_config_db).


### Configuración del simulador Weather Sensors con una nueva instancia de {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Para simular sucesos de datos de sensor reales en sus organizaciones utilizando el simulador Weather Sensors, primero debe configurar el simulador. Estos pasos incluyen las instrucciones para la creación de una instancia de {{site.data.keyword.iot_short_notm}} junto con el simulador.

1. [Despliegue la app web del simulador Weather Sensors con una instancia de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} y siga los pasos detallados.

   Para obtener más información acerca de Weather Sensors, consulte la [guía del simulador Weather Sensors ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Espere a que se complete el despliegue y luego navegue al panel de control de IBM Cloud.
3. Inicie el servicio {{site.data.keyword.iot_short_notm}} "wiotp-for-weather-sensors-simulator" creado por el proceso de despliegue.
4. Continúe con el [Paso 2. Configurar el conector de base de datos](#WA_config_db).


### Uso de datos de sensor desde un archivo CSV de ejemplo
{: #WA_sensor_premade}

Para simular sucesos de datos de sensor reales en sus organizaciones utilizando un archivo CSV pre-hecho:

1. [Descargue el archivo CSV de Cloudant ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator/releases/download/v1.0/cloudant.csv){: new_window}.
2. Continúe con el [Paso 5. Configurar WA y visualizar datos](#WA_import_data).


## Paso 2. Configurar el conector de base de datos
{: #WA_config_db}

Para utilizar {{site.data.keyword.cloudant_short_notm}} con Watson Analytics, debe configurar el almacenamiento de datos de plataforma para que se utilice Cloudant NoSQL DB como servicio histórico.

1. En el panel de control de {{site.data.keyword.cloudant_short_notm}}, pulse **Extensiones** en la barra de navegación.
2. En **Almacenamiento de datos históricos**, pulse **Configuración**. La sección **Configurar Almacenamiento de datos históricos** lista todos los servicios de base de datos NoSQL de Cloudant disponibles en el mismo espacio de IBM Cloud que {{site.data.keyword.cloudant_short_notm}}.
3. Seleccione el servicio Cloudant NoSQL DB al que se desea conectar.
4. Especifique las siguientes opciones de configuración de Cloudant NoSQL DB:
  - Intervalo de receptáculo = Día
  - Huso horario = UTC
  - Nombre de base de datos = predeterminado
5. Pulse **Listo** y confirme la autorización para la conexión con el servicio Cloudant. Asegúrese de que las ventanas emergentes están habilitadas en su navegador para tener acceso a la ventana de configuración. Cuando haya configurado correctamente Cloudant NoSQL DB, el estado del Almacenamiento de datos históricos cambia a configurado y los datos de dispositivo se almacenan en {{site.data.keyword.cloudant_short_notm}} NoSQL DB.
6. Continúe con el [Paso 3. Ejecutar el simulador](#run_simulator).


## Paso 3. Ejecutar el simulador
{: #run_simulator}

El simulador publica los datos de sensores meteorológicos reales, de 17 estaciones meteorológicas ubicadas en el área de Haifa, en su organización de {{site.data.keyword.iot_short_notm}}.

1. Navegue al simulador.
2. Introduzca los siguientes detalles:
   - Organización de Watson IoT Platform
   - Clave de API
   - Señal de autenticación

3. Pulse **Ejecutar simulador**. Los datos tardarán unos minutos en generarse.
4. Vaya a Watson IoT Platform mientras el simulador se está ejecutando y verifique que se han creado los dispositivos y que los sucesos llegan a estos dispositivos. 
5. Continúe con el [Paso 4. Exportar una base de datos Cloudant](#WA_export_csv).


## Paso 4. Exportar una base de datos Cloudant
{: #WA_export_csv}

Cuando configura un {{site.data.keyword.cloudant_short_notm}} NoSQL DB para almacenar datos de dispositivo, el conector crea tres bases de datos automáticamente. Se crea una base de datos para el intervalo de receptáculo actual, otra para el próximo intervalo, y una para la base de datos de configuración. Cuando se alcanza el final de un intervalo, los datos de dispositivo se almacenan en la base de datos de receptáculo para el nuevo intervalo y se crea una nueva base de datos para el receptáculo siguiente.

La característica de Almacenamiento de datos históricos en {{site.data.keyword.cloudant_short_notm}} crea un documento de diseño en Cloudant llamado “iotp”. Este documento tiene una función de "lista" llamada "csv" que puede utilizarse para exportar sucesos de dispositivo, almacenados como documentos en Cloudant, a formato CSV. Solo los sucesos en formato JSON se envían al archivo CSV. Este documento de diseño se propaga automáticamente en cada base de datos nueva para los intervalos de receptáculo siguientes.

El archivo CSV contiene información acerca de los metadatos de suceso de dispositivo y su carga útil. La siguiente lista muestra ejemplos de metadatos de suceso:
 -	DeviceId
 -	DeviceType
 - 	EventType
 - 	Indicación de fecha y hora en formato ISO 8601

La función de lista CSV divide la indicación de fecha y hora original en dos campos separados Fecha y Hora. Además de los metadatos, la función de lista CSV incluye los atributos de datos de la carga útil de dispositivo. Esta carga útil se visualiza en el documento Cloudant en la clave "data". Los documentos generados por el simulador Weather Sensors tienen una estructura similar a la del siguiente ejemplo:

```
{"deviceType": "WS",
  "deviceId": "Old-Market",
  "eventType": "sensorData",
  "format": "json",
  "timestamp": "2017-08-09T16:28:14.666Z",
  "data": { "NO2": 3.2, … }}
```

En el archivo CSV resultante, todos los atributos de carga útil están representados como columnas y se agregan al principio con:

```
<deviceType>_<eventType>_  
```

En el ejemplo superior, se añade una columna llamada WS_sensorData_NO2 al archivo CSV.

Para exportar la base de datos de Cloudant a formato CSV:  

1. Inicie sesión en Cloudant NoSQL DB.
2. Seleccione una base de datos a exportar.
3. Abra la base de datos seleccionada.
4. Abra un nuevo separador en el navegador y escriba el siguiente URL:
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-date?include_docs=true
```
   El identificador de servicio y el nombre de base de datos de Cloudant deben cambiarse de acuerdo con su identificador de servicio de Cloudant y el nombre de la base de datos seleccionada. El identificador del servicio de Cloudant se puede copiar del URL del panel de control de gestión de Cloudant.

   **Ejemplo:**
   ```
   https://ccf73725-b617-4f3e-8a7e-f5fb09569af4-bluemix.cloudant.com/iotp_115ccv_default_2017-08-23/_design/iotp/_list/csv/by-date?include_docs=true
   ```

   En este ejemplo, los datos se ordenarán por fecha y hora puesto que se utiliza la vista por fecha para invocar la función de lista. También puede filtrar los datos utilizando la característica de filtrado nativa de las vistas de Cloudant cambiando la vista utilizada en el URL y aplicando los atributos startkey y endkey.

   **Ejemplo:**
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-deviceType?include_docs=true&startkey='WS'&endkey='WS'
   ```
   En este ejemplo, la vista de tipo de dispositivo (deviceType) se utiliza para generar el archivo CSV y solo se incluyen documentos con deviceType=WS en el archivo descargado. Para seleccionar documentos dentro de un marco de tiempo específico, utilice la vista por fecha y utilice el siguiente URL de consulta (sustituyendo las indicaciones de fecha y hora por el marco deseado):
   ```
   https://{cloudant service id}-bluemix.cloudant.com/{dbName}/_design/iotp/_list/csv/by-date?statkey="2017-08-29T12:25:50.995Z"&endkey="2017-08-29T12:25:51.514Z"
   ```
5. Proporcione credenciales de Cloudant si es necesario y descargue el archivo CSV. El nombre del archivo se genera automáticamente según la vista definida en el URL. Por ejemplo, el nombre de archivo podría ser by-date.csv o by-deviceType.csv.
6. Continúe con el [Paso 5. Configurar WA y visualizar datos](#WA_import_data).


## Paso 5. Configurar WA y visualizar datos
{: #WA_import_data}

Para configurar WA y empezar a visualizar datos:

1. Inicie sesión en WA en: https://watson.analytics.ibmcloud.com.
2. En la página de inicio de WA, seleccione **Datos**.
3. Pulse **Archivo local** para importar su archivo CSV local. El nombre del archivo CSV depende de la vista que utilizó para exportar los datos (por ejemplo, by-deviceType o by-date).
4. Seleccione los activos de datos CSV que ha cargado.
5. En el campo **Formular una pregunta sobre los datos**, formule una pregunta utilizando el lenguaje natural.
5. Abra la sugerencia de visualización que mejor coincida con su pregunta. Puede revisar manualmente la sugerencia.
7. Guarde la visualización.


## Ejemplos de visualización de datos utilizando WA
{: #WA_visualize}

Esta sección muestra ejemplos de análisis de datos utilizando WA como herramienta de análisis.

**Nota:** Estos ejemplos están diseñados para proporcionarle una idea de qué esperar cuando realice sus propias visualizaciones. Los resultados en los ejemplos mostrados pueden diferir de los resultados que verá cuando realice estas visualizaciones con los datos de ejemplo, debido a, por ejemplo, los datos recopilados en diferentes fechas y horas.

### Visualización del estado de dispositivo

En esta sección aprenderemos acerca de la población de dispositivos de IoT y responder preguntas como:

1. ¿Cuántos dispositivos han informado?
2. ¿Cuál es el desglose de los dispositivos por tipo de dispositivo?
3. ¿Cuántos informes tiene un dispositivo?
4. ¿Cuántos informes ha enviado cada dispositivo?

**¿Cuántos dispositivos han informado?**

En este ejemplo, contamos en número de dispositivos que informan durante el intervalo especificado para detectar si los dispositivos han informado según lo esperado. Para completar este análisis, copie y pegue o escriba la siguiente pregunta en WA:

*"¿Cuántos deviceId hay?"*

Aquí está el resultado, mostrando que hay 17 dispositivos:

![Resultado del recuento de dispositivos](images/device_count.png)

**¿Cuál es el desglose de los dispositivos por tipo de dispositivo?**

En este ejemplo, comparamos el número de dispositivos por tipo de dispositivo que han informado durante el intervalo para determinar si los dispositivos de todos los tipos de dispositivo han informado según lo esperado. Para completar este análisis, copie y pegue o escriba la siguiente pregunta en WA:

*"¿Cómo se compara el número de deviceId por deviceType?"*

Este es el resultado, mostrando el desglose de dispositivos por tipo de dispositivo:

![Resultado del desglose de dispositivos por tipo de dispositivo](images/deviceID_deviceType.png)

Para ver estos datos en un gráfico circular, pulse **Visualización** a la izquierda y seleccione **Circular**.

![Gráfico circular mostrando el desglose de dispositivos por tipo de dispositivo](images/deviceID_deviceType_pie.png)


**¿Cuántos informes tiene un dispositivo?**

En este ejemplo, contamos el número de informes realizados por un dispositivo para detectar las condiciones de red y otros problemas relacionados con el dispositivo. Para completar este análisis, copie y pegue o escriba la siguiente pregunta en WA:

*"¿Cuántas filas hay? Filtrado por deviceId: Ahuza"*

**Nota:** No es necesario que escriba los nombres de campo completos. WA intenta adivinar el nombre de campo completo, pero los valores del filtro (p.ej. "Ahuza") si deben estar escritos completos y correctamente. Si no ve una sugerencia correcta con el filtro, pulse el enlace **Mostrar siguiente** o pruebe con la pregunta *"¿Cuántas filas hay aquí?".*. A continuación, abra el diagrama, pulse el recuadro **Multiplicador** bajo el diagrama y seleccione el parámetro deviceId de la lista. Deseleccione todos los deviceId irrelevantes.

Este es el resultado que muestra que hay 25 filas o informes realizados por el dispositivo Ahuza:

![Resultado del número de informes](images/25_rows.png)


**¿Cuántos informes tiene cada uno de los diferentes dispositivos?**

En este ejemplo, comparamos el nivel de actividad de los dispositivos en función del número de informes que ha enviado cada dispositivo durante el intervalo inspeccionado. Para completar este análisis, copie y pegue o escriba la siguiente pregunta en WA:

*"¿Cómo se compara el número de filas por deviceId?"*

Este es el resultado que muestra un gráfico de barras con la actividad de dispositivo para los diferentes dispositivos:

![Resultados de la comparación de la actividad de dispositivo](images/compare_activity.png)


### Visualización de los datos de sensor de tipo de dispositivo

En esta sección aprenderemos sobre los datos del sensor resumidos informados por todos los dispositivos de un tipo de dispositivo, respondiendo a preguntas como:

1. ¿Cuál es el promedio/mínimo/máximo de todos los valores de sensor informados?
2. ¿Puedo ver un histograma de una salida de sensor?  
3. ¿Cuál es la correlación entre dos sensores?


**¿Cuál es el promedio/mínimo/máximo de todos los valores de sensor informados?**

En este ejemplo, resumimos los parámetros numéricos informados por todos los dispositivos en un tipo de dispositivo en una tabla. De esta tabla, podemos aprender acerca del intervalo de valores detectados en el entorno y ganar una perspectiva amplia de los datos detectados.

Esta visualización debe compilarse manualmente, utilizando los siguientes pasos:

1.	En la sección **Crear su propia visualización**, seleccione **Tabla**.
2.	Pulse el botón con el signo más "crear nueva columna" y seleccione **Cálculo**.
3.	Nombre la nueva columna, seleccione la columna para este cálculo en la lista desplegable **Columnas** y pulse **Listo** para duplicar la columna. La nueva columna se añade en el extremo derecho de la bandeja de datos.
4.	Pulse el botón derecho en el título de la columna, seleccione un tipo de agregación (mín, máx, o promedio) y luego cierre la ventana de propiedades.
6.	Repita el proceso para añadir más columnas y luego oculte la bandeja de datos.
7.	Pulse **Columnas** y seleccione **Medidas** en la parte inferior de la lista.
8.	Pulse **Agregado por** y seleccione todos los cálculos que ha agregado.
9.	Pulse **Listo**.
10.	Guarde la visualización.

Este es el resultado que muestra el intervalo de valores:

![Resultado de valores de intervalo de sensor](images/sensor_range_values.png)


**¿Puedo ver un histograma de la salida de sensor de un dispositivo?**

En este ejemplo, evaluamos el comportamiento de un sensor en todos los dispositivos y tipos de dispositivo, identificando la distribución de valores detectados en el entorno. Podemos utilizar esta visualización para aprender acerca del entorno en el que detectan los sensores así como acerca de las limitaciones de los sensores. Para completar este análisis, copie y pegue o escriba la siguiente pregunta en WA:

*"¿Cómo se compara el número de filas por TEMP?"*

Este es el resultado que muestra la comparación del número de filas:

![Resultado de la comparación del recuento de filas](images/number_rows.png)


**¿Cuál es la correlación entre dos sensores?**

En este ejemplo, aprenderemos acerca de las correlaciones en el entorno comparando las medidas de dos sensores de dispositivo en todos los dispositivos y tipos de dispositivo. Para completar este análisis, copie y pegue o escriba una de las siguientes preguntas en WA:

*"¿Cuál es la relación entre NO2 y NOX?"* o *"¿Cómo se asocian los valores de NO2 y NOX?"*

Este es el resultado que muestra la relación entre los dos sensores:

![Resultado de la relación de sensores](images/sensor_relationship.png)

También puede ver los datos de sensor utilizando puntos de colores por ID de dispositivo. Para ello, seleccione el deviceID en el recuadro **Color**.

Este es el resultado que muestra un subconjunto de dispositivos limitado:

![Relación de sensores utilizando puntos de colores](images/sensor_color_dots.png)


### Visualización de los detalles de sensor (análisis detallado)

En esta sección, estudiamos los parámetros específicos que informa un dispositivo específico, respondiendo las siguientes preguntas:

1.	¿Cuál es el valor promedio/mínimo/máximo informado?
2.	¿Puedo ver un histograma de la salida de sensor de un dispositivo?
3.	¿Cómo cambia el valor de sensor de dispositivo específico a lo largo del tiempo?
4.	¿Cómo se comparan los valores de sensor de dos dispositivos a lo largo del tiempo?
5.	¿Cómo se comparan los valores de sensor del mismo dispositivo a lo largo del tiempo?
6.	¿Cuál es la correlación entre dos sensores de un dispositivo?


**¿Cuál es el valor promedio/mínimo/máximo informado?**

En este ejemplo, resumimos los parámetros numéricos informados por un dispositivo específico en una tabla, para aprender, por ejemplo, acerca del intervalo de valores observados en el entorno o acerca del funcionamiento incorrecto del sensor.

Esta visualización debe compilarse manualmente utilizando los siguientes pasos:

1)	En la sección **Crear su propia visualización**, seleccione **Tabla**.
2)	Pulse el botón con el signo más "crear nueva columna" y seleccione **Cálculo**.
3)	Nombre la nueva columna, seleccione la columna para este cálculo en la lista desplegable **Columnas** y pulse **Listo** para duplicar la columna. La nueva columna se añade en el extremo derecho de la bandeja de datos.
4)	Pulse el botón derecho en el título de la columna, seleccione un tipo de agregación (mín, máx, o promedio) y luego cierre la ventana de **Propiedades**.
6)	Repita el proceso para añadir más columnas y luego oculte la bandeja de datos.
7)	Pulse **Columnas** y seleccione **Medidas**.
8)	Pulse **Agregado por** y seleccione todos los cálculos que ha agregado.
9)	Pulse **Listo**.
10)	En el recuadro de multiplicador, seleccione el parámetro deviceId y seleccione los dispositivos relevantes a visualizar.
11)	Guarde la visualización.

Este es el resultado que muestra los valores especificados:

![Resultado del análisis detallado de los valores de informe](images/deep_avg_min_max.png)


**¿Puedo ver un histograma de la salida de sensor de un dispositivo?**

En este ejemplo, evaluamos el comportamiento de un sensor de dispositivo específico, identificando la distribución de valores observados en el entorno. Podemos utilizar esta visualización para aprender acerca del entorno en el que observa el sensor así como el posible funcionamiento incorrecto del sensor. Para completar este análisis, copie y pegue o escriba una de las siguientes preguntas en WA:

*"¿Cuál es la distribución de TEMP? Filtrado por deviceId: Ahuza"* o *"¿Cómo se compara el número de filas por TEMP? Filtrado por deviceId: Ahuza"*

Este es el resultado que muestra los datos de salida del sensor de dispositivo en un histograma:

![Resultado de histograma de la salida del sensor de dispositivo](images/deep_histogram.png)


**¿Cómo cambia el valor de sensor de dispositivo específico a lo largo del tiempo?**

En este ejemplo, aprendemos cómo cambian las lecturas de un sensor específico de un dispositivo específico, reflejando los cambios en el entorno a lo largo del tiempo. Esto puede ayudar con la planificación y la detección de problemas. Para completar este análisis, copie y pegue o escriba la siguiente pregunta en WA:

*"¿Cuál es la tendencia de TEMP a lo largo del tiempo? Filtrado por deviceId: Ahuza".*

Este es el resultado que muestra la tendencia de los datos del sensor a lo largo del tiempo:

![Resultado de la tendencia de los datos de sensor de dispositivo a lo largo del tiempo](images/deep_sensor_trend.png)


**¿Cómo se comparan los valores de sensor de dos dispositivos a lo largo del tiempo?**

En este ejemplo, comparamos las tendencias de las lecturas de sensor de diferentes dispositivos, identificando relaciones entre los dispositivos para detectar anomalías, funcionamiento incorrecto, etc. Para completar este análisis, copie y pegue o escriba una de las siguientes preguntas en WA:

*"¿Cuál es la tendencia de TEMP a lo largo del tiempo por deviceId?"* o *"¿Cuál es la tendencia de TEMP a lo largo del tiempo por deviceId?  Filtrado por deviceId: Ahuza, Igud"*

Este es el resultado que muestra la comparación de los valores del sensor a lo largo del tiempo:

![Comparación de datos de sensor entre dispositivos a lo largo del tiempo](images/deep_sensor_compare.png)

También puede ver esta información pulsando en el nombre del parámetro en la parte inferior del gráfico. Se dibujan varias líneas (una por deviceId). Se pueden seleccionar los deviceId relevantes en la lista.

![Comparación de datos de sensor entre dispositivos a lo largo del tiempo mostrado con líneas](images/deep_sensor_compare_lines.png)

Puede utilizar el recuadro **Multiplicador** bajo el gráfico y elegir deviceId para presentar los gráficos en paralelo.

![Comparación de datos de sensor en paralelo](images/deep_sensor_compare_side.png)


**¿Cómo se comparan los valores de sensor del mismo dispositivo a lo largo del tiempo?**

En este ejemplo, visualizamos mutuamente la tendencia de dos sensores de dispositivo para obtener más conocimiento sobre los cambios en el entorno a lo largo del tiempo. Para completar este análisis, copie y pegue o escriba la siguiente pregunta en WA:

*"¿Cuál es la tendencia de NO2 y NOX a lo largo del tiempo por deviceId?  Filtrado por deviceId: Ahuza"*

Este es el resultado que muestra la tendencia de los sensores de dispositivo a lo largo del tiempo:

![Comparación de la tendencia de dos sensores de dispositivo a lo largo del tiempo](images/deep_two_devices.png)


**¿Cuál es la correlación entre dos sensores de un dispositivo?**

En este ejemplo, aprenderemos acerca de las correlaciones en el entorno comparando las medidas de dos sensores de dispositivo. Para completar este análisis, copie y pegue o escriba una de las siguientes preguntas en WA:

*"¿Cuál es la relación entre NO2 NOX?" Filtrado por deviceId: Ahuza"* o *"¿Cómo se asocian los valores de NO2 y NOX? Filtrado por deviceId: Ahuza"*

Este es el resultado que muestra la correlación entre dos sensores de un dispositivo:

![Comparación de dos sensores de un dispositivo](images/deep_two_sensors.png)


## ¿Qué hacer a continuación?

Para obtener más información acerca de WA, consulte los siguientes recursos:
- [Developer Center de Watson Analytics ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/watson-analytics/){: new_window}
- [Comunidad de Watson Analytics ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/communities/analytics/watson-analytics/){: new_window}
- [Foro de Watson Analytics ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://community.watsonanalytics.com/discussions/spaces/15/view.html){: new_window}
