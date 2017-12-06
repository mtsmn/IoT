---

copyright:
  years: 2017
lastupdated: "2017-09-18"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Análisis de datos mediante Data Science Experience
{: #DSX_integration}

Puede utilizar {{site.data.keyword.iot_full}} con Data Science Experience (DSX) para visualizar y aprender acerca de los datos enviados desde dispositivos conectados con la plataforma.
{: shortdesc}

## Visión general y objetivos

Esta guía le muestra, paso a paso, el proceso de visualización de datos de suceso de dispositivo {{site.data.keyword.iot_short_notm}} utilizando Data Science Experience como herramienta de analíticas.

DSX proporciona un entorno interactivo, colaborativo, basado en la nube donde puede utilizar varias herramientas para activar sus conocimientos. Utilice los cuadernos Jupyter, disponibles en IBM DSX, para cargar datos de {{site.data.keyword.iot_short_notm}} históricos y utilice los datos para crear visualizaciones para ayudar al análisis de datos y la identificación de anomalías. Puede derivar los valores de umbral de los datos históricos y utilizarlos para crear reglas de nube en {{site.data.keyword.iot_short_notm}}. Las reglas de nube alertan a los usuarios cuando un dispositivo de IoT asociado con una regla publica una lectura que está fuera de los límites del umbral configurado.

Los datos de dispositivo enviados a {{site.data.keyword.iot_short_notm}} se pueden recopilar y almacenar en {{site.data.keyword.Bluemix}} utilizando el servicio de {{site.data.keyword.cloudantfull}} NoSQL DB. Para recopilar los datos, primero debe conectar {{site.data.keyword.iot_short_notm}} con el servicio {{site.data.keyword.cloudant_short_notm}}. Los datos de dispositivo se almacenan en bases de datos diarias, semanales o mensuales de {{site.data.keyword.cloudant_short_notm}} en función del intervalo de receptáculo configurado. 


![Visión general del uso de DSX para analizar datos](images/DSX_overview.png)

Como parte de esta guía aprenderá:

 - Cómo configurar el almacenamiento de datos de plataforma para que se utilice Cloudant NoSQL DB como servicio histórico.
 - Cómo utilizar el simulador Weather Sensors para generar datos para que los utilice la plataforma.
 - Cómo exportar los datos e importarlos a DSX para analizarlos.
 - Cómo visualizar los datos de IoT, detectar anomalías y configurar alertas.


## Requisitos previos

Para completar estos pasos debe tener acceso a [{{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/internet-of-things-platform){: new_window} con [Cloudant NoSQL DB ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/cloudant-nosql-db
){: new_window} y acceso a [Cuenta de DSX ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://datascience.ibm.com/docs/content/getting-started/get-started.html){: new_window}.


## Paso 1. Configurar el simulador
{:#DSX_sensor_data}

Para realizar un análisis significativo, debe tener datos significativos. Puede simular los datos de sensor reales para aprender acerca de cómo se pueden analizar los datos de dispositivo de Watson IoT Platform mediante DSX. Este paso proporciona instrucciones para:
 - [Configurar el simulador con una **instancia existente de {{site.data.keyword.iot_short_notm}}**](#sim_existing_platorm)
 - [Configurar el simulador con una **nueva instancia de {{site.data.keyword.iot_short_notm}}**](#sim_new_platform)


### Configuración del simulador Weather Sensors con una instancia de {{site.data.keyword.iot_short_notm}} existente
{: #sim_existing_platform}

Para simular sucesos de datos de sensor reales en sus organizaciones utilizando el simulador Weather Sensors, primero debe configurar el simulador. Estos pasos presuponen que ya tiene una instancia de {{site.data.keyword.iot_short_notm}} en ejecución.

1. [Genere la clave de API y la señal necesarias para ejecutar el simulador. ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#api-key){: new_window}
2. [Despliegue la app web del simulador Weather Sensors ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window} y siga las instrucciones detalladas.

   Para obtener más información acerca de Weather Sensors, consulte la [guía del simulador Weather Sensors ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
3. Continúe con el [Paso 2. Configurar el conector de base de datos](#DSX_config_db).


### Configuración del simulador Weather Sensors con una nueva instancia de {{site.data.keyword.iot_short_notm}}
{: #sim_new_platform}

Para simular sucesos de datos de sensor reales en sus organizaciones utilizando el simulador Weather Sensors, primero debe configurar el simulador. Estos pasos incluyen las instrucciones para la creación de una instancia de {{site.data.keyword.iot_short_notm}} junto con el simulador.

1. [Despliegue la app web del simulador Weather Sensors con una instancia de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/guide-weathersensors-simulator&branch=bindwiotp){: new_window} y siga los pasos detallados.

   Para obtener más información acerca de Weather Sensors, consulte la [guía del simulador Weather Sensors ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-weathersensors-simulator){: new_window}.
2. Espere a que se complete el despliegue y luego navegue al panel de control de Bluemix.
3. Inicie el servicio {{site.data.keyword.iot_short_notm}} "wiotp-for-weather-sensors-simulator" creado por el proceso de despliegue.
4. Continúe con el [Paso 2. Configurar el conector de base de datos](#DSX_config_db).


## Paso 2. Configurar el conector de base de datos
{: #DSX_config_db}

Los datos de dispositivo se pueden almacenar en Cloudant en bases de datos diarias, semanales o mensuales en función de la opción de intervalo de receptáculo seleccionada. Los datos recopilados de todos los dispositivos en el mismo intervalo de receptáculo (día, semana o mes) se almacenan en la misma base de datos.

Indiferentemente de la configuración del intervalo de receptáculo, el conector crea tres bases de datos automáticamente. Se crea una base de datos para el intervalo de receptáculo actual, otra para el próximo intervalo, y una para la base de datos de configuración. Cuando se alcanza el final de un intervalo, los datos de dispositivo se almacenan en la base de datos de receptáculo para el nuevo intervalo y se crea una nueva base de datos para el receptáculo siguiente.

Para utilizar {{site.data.keyword.cloudant_short_notm}} con DSX, debe configurar el almacenamiento de datos de plataforma para que se utilice Cloudant NoSQL DB como servicio histórico.

1. En el panel de control de {{site.data.keyword.cloudant_short_notm}}, pulse **Extensiones** en la barra de navegación.
2. En **Almacenamiento de datos históricos**, pulse **Configuración**. La sección **Configurar el almacenamiento de datos históricos** lista todos los servicios de Cloudant NoSQL DB disponibles en el mismo espacio Bluemix que {{site.data.keyword.cloudant_short_notm}}.
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
2. Si ha desplegado el simulador con una instancia de {{site.data.keyword.iot_short_notm}} limitada, continúe con el paso 3. Si ha desplegado una versión autónoma del simulador, introduzca los siguientes detalles:
   - Organización de Watson IoT Platform
   - Clave de API
   - Señal de autenticación

3. Pulse **Ejecutar simulador**. Los datos tardarán unos minutos en generarse.
4. Vaya a Watson IoT Platform mientras el simulador se está ejecutando y verifique que se han creado los dispositivos y que los sucesos llegan a estos dispositivos.
5. Continúe con el [Paso 4. Configurar DSX y visualizar datos](#DSX_visualize_data).


## Paso 4. Configurar DSX y visualizar datos
{: #DSX_visualize_data}

Para configurar DSX y empezar a visualizar datos:

1. [Configure un cuaderno Jupyter preconfigurado](#setup_jupyter_notebook) para obtener información sobre sus datos y para detectar anomalías.
2. [Ejecute el análisis.](#run_analysis)
3. [Configure alertas en las anomalías del sensor](#config_alerts).


### 1. Configurar un cuaderno Jupyter preconfigurado
{: #setup_jupyter_notebook}

El cuaderno Jupyter es una aplicación web que permite crear y compartir documentos que contienen código ejecutable, fórmulas matemáticas, gráficos/visualización (matplotlib) y texto explicativo.

Para configurar un cuaderno Jupyter preconfigurado para obtener información sobre sus datos y para detectar anomalías:
1. Utilice un navegador soportado para iniciar sesión en [DSX ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://datascience.ibm.com/){: new_window} con su identificador de Bluemix.
2. Pulse "+" y seleccione **Crear proyecto** para crear un proyecto nuevo. Los proyectos crean un espacio para recopilar y compartir cuadernos, conectarse con orígenes de datos, crear conductos y añadir conjuntos de datos.
3. Especifique el nombre del proyecto y pulse **Crear**. Durante la configuración de la cuenta de DSX, se crean automáticamente el servicio Spark y la instancia de almacenamiento de objetos. Como alternativa, puede crearlos utilizando la interfaz de Bluemix y asociándolos con su proyecto de DSX en una etapa posterior.
4. Importe sus credenciales de Cloudant seleccionando su proyecto en el menú **DSX** y pulsando ![Icono encontrar y añadir datos](images/find_add_data_icon.png).
5. Pulse el separador **Conexiones**.
6. Pulse **Crear conexión** para importar sus credenciales de Cloudant para hacer que estén disponibles en cualquier cuaderno de su proyecto.
7. Complete la pantalla **Nuevas conexiones** especificando la siguiente información:
   - Nombre de conexión
   - Establezca **Categoría de servicio** en `Data Service`.
   - Seleccione su servicio Cloudant en el menú desplegable **Instancia de servicio de destino**.
   - Seleccione la base de datos Cloudant correspondiente a la fecha actual.
8. Pulse **Crear**.
9. Pulse **Añadir cuadernos** para crear un nuevo cuaderno Jupyter.
10. Seleccione **Desde URL** para cargar un cuaderno existente, especifique un nombre descriptivo para el cuaderno y especifique el siguiente URL para abrir el cuaderno de muestra:
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```
11. Pulse **Crear cuaderno**. Compruebe que el cuaderno se ha creado con metadatos y código.
12. Seleccione la celda que empieza por '!pip install --upgrade pixiedust, ,' y pulse **Reproducir** para ejecutar el código.
13. Cuando finalice la instalación, reinicie el kernel Spark pulsando el icono **Reiniciar Kernel**.
14. Espere unos 10 segundos a que se reinicie el kernel y luego pulse en la celda con el comentario para seleccionarla.
15. Importe sus credenciales de Cloudant para esa celda completando los siguientes pasos:

    1. Pulse ![Icono encontrar y añadir datos](images/find_add_data_icon.png).
    2. Seleccione el separador **Conexiones**.
    3. Pulse **Insertar para codificar**.
Se crea un diccionario llamado "credentials_1" con sus credenciales de Cloudant. Si no se especifica el nombre como "credentials_1", cambie el nombre del diccionario por "credentials_1" puesto que es el nombre necesario para que se ejecute el código del cuaderno.
16. En la celda con el nombre de base de datos (dbname) especifique el nombre de la base de datos Cloudant que es el origen de datos, por ejemplo, `iotp_yourWIoTPorgId_default_Year-month-day`.
17. Guarde el cuaderno. El cuaderno está listo para ejecutarse.


### 2. Ejecutar el análisis
{: #run_analysis}

1.	Seleccione la celda que contiene sus credenciales de Cloudant.
2.	Pulse **Reproducir** para ejecutar el código de la celda.
3.	Compruebe los resultados de la ejecución y analice el código python que se utiliza en cada celda.
4.	Repita los pasos 2 y 3 para cada celda. Para las celdas con **Entrada de usuario necesaria** especificada, debe proporcionar nuevos valores de entrada para la variable definida en la siguiente celda de código antes de su ejecución.

**Nota:** Algunas celdas ejecutan trabajos Spark en segundo plano y es posible que tarden más en finalizar. Cuando finaliza la ejecución de código dentro de una celda, el asterisco `*` se convierte en un número, por ejemplo, En `[*]` pasa a ser En `[1]`. Tras finalizar los pasos, puede crear reglas de nube en {{site.data.keyword.iot_short_notm}} para generar automáticamente alertas cuando se detecten anomalías.


### 3. Configurar alertas en anomalías de sensor
{: #config_alerts}


Puede crear reglas de nube en {{site.data.keyword.iot_short_notm}}. Estas reglas pueden generar alertas si se detectan anomalías al publicar sucesos en los valores umbral que ha derivado en el cuaderno.

En este ejemplo, utilizamos dióxido de nitrógeno (NO2) y los umbrales inferior y superior para un dispositivo específico. Creamos una acción de correo electrónico, para que se envíe un correo electrónico a una dirección de correo electrónico especificada si el valor de NO2 cruza los valores de umbral definidos.

Para crear reglas de nube:

1. Cree un esquema:
    1. En el separador **Dispositivos** de su panel de control de {{site.data.keyword.iot_short_notm}} seleccione el separador **Gestionar esquemas**.
    2. Pulse **Añadir esquema**.
    3. Seleccione el WS de DeviceType para el que se crea el esquema y pulse **Siguiente**.
    4. Pulse **Añadir una propiedad** para añadir el punto de datos de los dispositivos conectados.
    5. En el separador **Manual**, defina el campo de tipo de datos en `Float` y el campo de propiedad en `NO2`.
    6. Pulse **Aceptar**.
    7. Pulse **Finalizar**.

2. Cree una acción:
    1. Seleccione el separador **Reglas** en el panel de control de {{site.data.keyword.iot_short_notm}}.
    2. Seleccione el separador **Acciones**.
    3. Pulse **+ Crear una acción**.
    4. En la pantalla **Crear nueva acción**, especifique un nombre y seleccione "Enviar correo electrónico" como el tipo de acción.
    5. Pulse **Siguiente**.
    6. En la pantalla **Editar acción**, active el conmutador **Incluir datos**.
    7. Pulse **Finalizar**.
    8. En el separador **Reglas**, seleccione el separador **Examinar**.
    9. Pulse **+ Crear regla de nube**.
    10. En la pantalla **Añadir nueva regla de nube**, especifique un nombre para su regla y seleccione su nombre de esquema en el campo **Se aplica a**. En este ejemplo, el nombre de esquema es "WS".
    11. Pulse **Siguiente**.

3. Establezca la condición. Los valores de umbral que utilice en estos pasos se encuentran en el último trozo de código que se ejecuta en el cuaderno, junto al gráfico de NO2:
    1. Pulse Nueva condición y defina la nueva condición:
    - En el campo **Propiedad** especifique `Nitrogen Dioxide`.
    - En el campo **Operador** seleccione el icono mayor que `>`.
    - En el campo **Valor** especifique el valor del umbral superior.
    - Pulse **Aceptar**.
    2. Seleccione OR y añada la segunda condición:
    - En el campo **Propiedad** especifique `Nitrogen Dioxide`.
    - En el campo **Operador** seleccione el icono menor que `<`.
    - En el campo **Valor** especifique el valor del umbral inferior.
    - Pulse **Aceptar**. Las condiciones para desencadenar la regla están ahora definidas.
4. Establezca la acción en "Enviar correo electrónico" y pulse **Aceptar** para activar la regla. Se genera una alerta por correo electrónico cuando el valor de la lectura de dióxido de nitrógeno publicado por un dispositivo cruce cualquiera de los valores de umbral. Puede ejecutar el simulador para ver los correos electrónicos de alerta.


## ¿Qué hacer a continuación?

Para obtener más información acerca de DSX, consulte los siguientes recursos:

 - [Reglas de nube de WIoTP ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window}
 - [Cuadernos de la comunidad y guías de aprendizaje de DSX ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} siga los enlaces para aprender más acerca de los cuadernos Jupyter.
 - [Recetas de análisis en el cookbook de Watson IoT Platform ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
