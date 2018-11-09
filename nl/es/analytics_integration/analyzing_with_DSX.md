---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-01"
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
){: new_window}, acceso al servicio de [Apache Spark ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/apache-spark){:new_window} y acceso a una [Cuenta de DSX ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono enlace externo")](https://datascience.ibm.com/docs/content/getting-started/get-started-wdp.html){: new_window}.


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
2. Espere a que se complete el despliegue y luego navegue al panel de control de IBM Cloud.
3. Inicie el servicio {{site.data.keyword.iot_short_notm}} "wiotp-for-weather-sensors-simulator" creado por el proceso de despliegue.
4. Continúe con el [Paso 2. Configurar el conector de base de datos](#DSX_config_db).


## Paso 2. Configurar el conector de base de datos
{: #DSX_config_db}

Los datos de dispositivo se pueden almacenar en Cloudant en bases de datos diarias, semanales o mensuales en función de la opción de intervalo de receptáculo seleccionada. Los datos recopilados de todos los dispositivos en el mismo intervalo de receptáculo (día, semana o mes) se almacenan en la misma base de datos.

Indiferentemente de la configuración del intervalo de receptáculo, el conector crea tres bases de datos automáticamente. Se crea una base de datos para el intervalo de receptáculo actual, otra para el próximo intervalo, y una para la base de datos de configuración. Cuando se alcanza el final de un intervalo, los datos de dispositivo se almacenan en la base de datos de receptáculo para el nuevo intervalo y se crea una nueva base de datos para el receptáculo siguiente.

Para utilizar {{site.data.keyword.cloudant_short_notm}} con DSX, debe configurar el almacenamiento de datos de plataforma para que se utilice Cloudant NoSQL DB como servicio histórico.

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
<!--3. [Configure alerts on sensor anomalies](#config_alerts).-->


### 1. Configurar un cuaderno Jupyter preconfigurado
{: #setup_jupyter_notebook}

El cuaderno Jupyter es una aplicación web que permite crear y compartir documentos que contienen código ejecutable, fórmulas matemáticas, gráficos/visualización (matplotlib) y texto explicativo.

Para configurar un cuaderno Jupyter preconfigurado para obtener información sobre sus datos y para detectar anomalías:
1. Utilice un navegador soportado para iniciar sesión en [DSX ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://datascience.ibm.com/){: new_window} con su identificador de IBM Cloud.
2. Pulse **+ Proyecto nuevo** para crear un nuevo proyecto y seleccione el mosaico **Cuadernos Jupyter**. Los proyectos crean un espacio para recopilar y compartir cuadernos, conectarse con orígenes de datos, crear conductos y añadir conjuntos de datos.
3. Pulse el desplegable **+ Añadir a proyecto** y seleccione **Conexión**. En la lista de servicios, seleccione **Cloudant** y, a continuación, especifique el URL de Cloudant, el nombre de usuario y la contraseña que se encuentran en el separador Credenciales de servicio de la página de servicio de {{site.data.keyword.cloudant_short_notm}} y especifíquelos en los campos que se muestran. Especifique un nombre para la conexión y pulse **Crear**.
4. Compruebe que la conexión esté disponible en la sección Activos de datos del panel de control.
5. Pulse **+ Nuevo cuaderno** para crear un nuevo cuaderno Jupyter.
6. En el desplegable Seleccionar tiempo de ejecución bajo Servicios, seleccione **spark-iw**. Si esto no está presente, significa que el servicio Apache Spark no se ha suministrado correctamente. Compruebe en el panel de control de IBM Cloud que está presente y, si no, visite la página de servicio para suministrarla.
7. Establezca el lenguaje como **Python 2** y la versión de Spark como **2.1**.
8. Seleccione **Desde URL** para cargar un cuaderno existente, especifique un nombre descriptivo para el cuaderno y especifique el siguiente URL para abrir el cuaderno de muestra:
```
https://github.com/ibm-watson-iot/analytics-integration-samples/blob/master/dsx/notebooks/witop_dsx_weather_sensors_sim_notebook.ipynb
```

9. Pulse **Crear cuaderno**. Compruebe que el cuaderno se ha creado con metadatos y código.
10. Seleccione la celda que empieza por '!pip install --upgrade pixiedust, ,' y, a continuación, pulse **Ejecutar** para ejecutar el código.
11. Cuando la instalación se haya completado, reinicie el kernel de Spark pulsando el icono **Reiniciar Kernel** o seleccionando **Reiniciar** en el menú Kernel.
12. Espere unos 10 segundos para que el kernel se reinicie y, a continuación, pulse en la celda de código vacía con el comentario para seleccionarlo.
13. Importe sus credenciales de Cloudant para esa celda completando los siguientes pasos:

    1. Pulse ![Encontrar y añadir datos](images/find_add_data_icon.png).
    2. Seleccione el separador **Conexiones**.
    3. Pulse **Insertar para codificar**.
Se crea un diccionario llamado "credentials_1" con sus credenciales de Cloudant. Si no se especifica el nombre como "credentials_1", cambie el nombre del diccionario por "credentials_1" puesto que es el nombre necesario para que se ejecute el código del cuaderno.
14. En la celda con el nombre de base de datos (dbname) especifique el nombre de la base de datos Cloudant que es el origen de datos, por ejemplo, `iotp_yourWIoTPorgId_default_Year-month-day`.
15. Guarde el cuaderno. El cuaderno está listo para ejecutarse.


### 2. Ejecutar el análisis
{: #run_analysis}

1.	Seleccione la celda que contiene sus credenciales de Cloudant.
2.	Pulse **Reproducir** para ejecutar el código de la celda.
3.	Compruebe los resultados de la ejecución y analice el código python que se utiliza en cada celda.
4.	Repita los pasos 2 y 3 para cada celda. Para las celdas con **Entrada de usuario necesaria** especificada, debe proporcionar nuevos valores de entrada para la variable definida en la siguiente celda de código antes de su ejecución.

**Nota:** Algunas celdas ejecutan trabajos Spark en segundo plano y es posible que tarden más en finalizar. Cuando finaliza la ejecución de código dentro de una celda, el asterisco `*` se convierte en un número, por ejemplo, En `[*]` pasa a ser En `[1]`. Tras finalizar los pasos, puede crear reglas de nube en {{site.data.keyword.iot_short_notm}} para generar automáticamente alertas cuando se detecten anomalías.


<!-- ### 3. Configure alerts on sensor anomalies
{: #config_alerts}


You can create cloud rules in the {{site.data.keyword.iot_short_notm}}. These rules can generate alerts if anomalies are detected when published events cross the threshold values that you derived in the notebook.

In this example, we use Nitrogen Dioxide (NO2) and the upper/lower thresholds for one specific device. We are creating an email action, so that an email is sent to a specified email address whenever the NO2 value crosses the threshold values that we set.

To create cloud rules:

1. Create a schema:
    1. In the **Devices** tab of your {{site.data.keyword.iot_short_notm}} dashboard, select the **Manage Schemas** tab.
    2. Click **Add Schema**.
    3. Select the DeviceType WS for which the schema is created and click **Next**.
    4. Click **Add a property** to add the data point from the connected devices.
    5. From the **Manual** tab, set the data type field to `Float` and the property field to `NO2`.
    6. Click **OK**.
    7. Click **Finish**.

2. Create an action:
    1. Select the **Rules** tab in the {{site.data.keyword.iot_short_notm}} dashboard.
    2. Select the **Actions** tab.
    3. Click **+Create an Action**.
    4. In the **Create New Action** screen, enter a name and select "Send email" as the action type.
    5. Click **Next**.
    6. In the **Edit Action** screen, turn on the **Include Data** toggle.
    7. Click **Finish**.
    8. From the **Rules** tab, select the **Browse** tab.
    9. Click **+Create Cloud Rule**.
    10. In the **Add New Cloud Rule** screen, enter a name for your rule and select your schema name in the **Applies to** field. In this example, the schema name is "WS".
    11. Click **Next**.

3. Set the condition - The threshold values that you use in these steps are found in the last code chunk that is executed in the notebook, next to the NO2 chart:
    1. Click New Condition and set the first condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select greater than icon `>`.
    - In the **Value** field enter the upper threshold value.
    - Click **OK**.
    2. Select OR and then add the second condition:
    - In the **Property** field enter `Nitrogen Dioxide`.
    - In the **Operator** field select less than icon `<`.
    - In the **Value** field enter the lower threshold value.
    - Click **OK**. The conditions to trigger the rule are now set.
4. Set the action to "Send email" and click **OK** to activate the rule. An email alert is generated whenever the value of the Nitrogen Dioxide reading that is published by a device crosses either of the threshold values. You can run the simulator to see the alert emails. -->


## ¿Qué hacer a continuación?

Para obtener más información acerca de DSX, consulte los siguientes recursos:

<!-- - [WIoTP Cloud Rules ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/IoT/cloud_analytics.html#cloud_analytics){: new_window} -->
 - [Cuadernos de la comunidad y guías de aprendizaje de DSX ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://idaas.iam.ibm.com/idaas/mtfim/sps/authsvc?PolicyId=urn:ibm:security:authentication:asf:basicldapuser){: new_window} siga los enlaces para aprender más acerca de los cuadernos Jupyter.
 - [Recetas de análisis en el cookbook de Watson IoT Platform ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/iotplatform/resources/watson-iot-analytics-cookbook/)
