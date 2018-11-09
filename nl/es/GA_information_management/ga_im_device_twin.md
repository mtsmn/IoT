---

copyright:
years: 2017, 2018
lastupdated: "2018-08-28"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introducción a la gestión de datos
{: #device_twins}

<!--An unprecedented number of devices and sensors exist in the modern world. Connected devices generate vast amounts of digital data at extraordinary speeds. Such volumes of data represent great opportunities but also challenges, in terms of how big data can be processed, analyzed and presented to help to deliver insights and drive transformation.-->

Los dispositivos pueden proporcionar salidas de datos similares, pero varían en función de la marca, el modelo y la versión, y pueden producir datos en distintos formatos. Por ejemplo, un dispositivo con un sensor de temperatura en una oficina puede notificar la temperatura en grados Fahrenheit o en grados Celsius. No es eficiente configurar las aplicaciones para poder consumir datos en todos estos formatos; en lugar de ello, los datos debe recopilarse, transformarse y normalizarse para crear un único modelo lógico, de modo que una aplicación pueda interactuar con los distintos dispositivos de la misma forma. 

El componente de gestión de datos de {{site.data.keyword.iot_short_notm}} incluye una característica de duplicado de dispositivo y una característica de duplicado de activo. La característica de duplicado de dispositivo le permite aprovechar la recogida, la transformación y la normalización de diferentes formatos de datos de dispositivo en un único modelo lógico. La característica de duplicado de activo le permite agrupar distintos dispositivos para crear una cosa, que es una estructura de datos basada en activos de valor más alto. Incluso puede agrupar las cosas para crear cosas nuevas. Una aplicación puede interactuar con el modelo lógico, independientemente del formato de datos utilizado por los dispositivos o las cosas individuales. 

Por ejemplo, un grupo de dispositivos de informe sobre temperatura, humedad y luz ambiente se pueden agregar en una cosa "Sala" para representar el nivel de comodidad dentro de una oficina específica. Un número de cosas "Sala" se pueden agregar en una cosa "Planta" para representar a todas las oficinas en un nivel específico, y un número de cosas "Planta" se puede agregar en una cosa "Edificio". Al utilizar una abstracción de cosa, la aplicación se desacopla de los detalles de cómo están conectados los dispositivos, el formato en el que los dispositivos publican datos de sucesos, y cómo se combinan los datos.
{: shortdesc}

## Duplicados de dispositivo

Un duplicado de dispositivo es una representación digital basada en la nube de un dispositivo físico que está conectado a {{site.data.keyword.iot_short_notm}}. Un duplicado de dispositivo representa un modelo lógico de los sucesos que se publican mediante un dispositivo. Una vez definido y con una instancia creada, el duplicado de dispositivo proporciona medidas coherentes de la interacción con un dispositivo al estilo REST, independientemente de si el dispositivo está en línea o fuera de línea. Las propiedades de un dispositivo, incluida la información sobre el estado actual del dispositivo (estado de dispositivo), se pueden recuperar con una solicitud HTTP, o mediante la suscripción a un tema IoT.

Los duplicados de dispositivo pueden ayudarle a:
- Proporcionar a sus desarrolladores de aplicaciones interfaces coherentes para acceder a datos de dispositivos controlados por sucesos al estilo REST.
- Acceder al estado de un dispositivo.
- Normalizar datos procedentes de dispositivos de diferentes modelos que publican datos en distintos formatos.
- Filtrar los datos innecesarios.


Para crear un duplicado de dispositivo, debe definir los recursos siguientes en {{site.data.keyword.iot_short_notm}}:
- La estructura de los sucesos que envía su dispositivo.  
La estructura de un suceso de entrada se define en los recursos de la interfaz física, el tipo de suceso y el esquema de suceso. 
- Las propiedades que desea registrar.  
Estas propiedades definen la estructura lógica del estado de dispositivo que sus aplicaciones pueden consumir. Las propiedades se definen en los recursos de la interfaz lógica y el esquema lógico.  
- La correlación de sucesos de interfaz física en las propiedades de la interfaz lógica.  
Utilice el recurso de correlaciones para correlacionar sucesos con propiedades.

En el diagrama siguiente se muestran dos dispositivos de temperatura distintos en ubicaciones separadas. Un dispositivo informa de los datos de dispositivo en grados Celsius y de los demás datos de informes en grados Fahrenheit. Los datos se envían a {{site.data.keyword.iot_short_notm}} en los formatos de temperatura "t" y "temp". {{site.data.keyword.iot_short_notm}} transforma automáticamente los grados Fahrenheit en grados Celsius. Los formatos de temperatura "t" y "temp" se normalizan en el formato lógico "temperatura". La aplicación puede consultar el estado de cualquiera de los dispositivos accediendo al valor para el parámetro "temperatura". 

![Visión general de la gestión de datos en {{site.data.keyword.iot_short_notm}}.](../information_management/images/ga_im_resources_overview.svg "Visión general de la gestión de datos en {{site.data.keyword.iot_short_notm}}")


## Duplicados de activo (cosas)

Los duplicados de activo le permiten llevar el concepto de duplicados de dispositivo un paso más allá. Un duplicado de activo habilita la agregación de dispositivos en una única entidad denominada una cosa.  Una cosa, o duplicado de activo, es un concepto similar al duplicado de dispositivo, pero el duplicado de activo representa un grupo de dispositivos como un único modelo lógico. Incluso puede agregar cosas para formar niveles más altos de abstracción. Por ejemplo, una cosa "Sala" puede agregar los dispositivos siguientes:

- Un dispositivo con un sensor de temperatura (termómetro)
- Un dispositivo con un sensor de humedad (higrómetro)

Una cosa "Planta" podría entonces agregar varias cosas "Sala". 

La estructura de una cosa se define utilizando un JSON-Schema. El esquema hace referencia a las interfaces lógicas de los dispositivos agregados o las cosas. Las propiedades de una cosa, incluida la información sobre el estado actual de la cosa, se pueden recuperar utilizando una solicitud HTTP, o suscribiéndose a un tema IoT.

Los duplicados de activo pueden ayudarle a:
 
- Agregar varios duplicados de dispositivos o cosas para definir cosas nuevas.
- Acceder al estado de una cosa.
- Gestionar activos sin estar expuesto a la instrumentación individual de los mismos.
- Filtrar los datos innecesarios.
- Normalizar las interfaces de cosa para desacoplar las aplicaciones de las complejidades de la forma en que se construyen las cosas específicas.


Para crear un duplicado de activo, debe definir los recursos siguientes en {{site.data.keyword.iot_short_notm}}:

- La estructura de la cosa.  
La estructura de la cosa se define mediante el esquema de la cosa, que especifica los dispositivos o las cosas agregadas.
- La estructura del estado de la cosa deseado, que está formada por las propiedades que desea registrar.  
Estas propiedades definen la estructura lógica del estado de la cosa que puede ser consumida por las aplicaciones. Las propiedades se definen en los recursos de la interfaz lógica y el esquema lógico.  
- Cómo se correlaciona la interfaz de cosa en las propiedades de interfaz lógica.  
Utilice el recurso de correlaciones para correlacionar sucesos con propiedades.


El diagrama siguiente muestra los sensores de temperatura y humedad en diferentes dispositivos que están publicando datos de sucesos de temperatura y humedad en {{site.data.keyword.iot_short_notm}}. Dos duplicados de dispositivo, cada uno de los cuales representando un dispositivo físico, tienen interfaces lógicas asociadas y se crean en {{site.data.keyword.iot_short_notm}}. Los datos que se publican desde el dispositivo de temperatura se correlacionan con la interfaz lógica de "IThermometer". Los datos que se publican desde el dispositivo de humedad se correlacionan con la interfaz lógica de "IHygrometer". Las interfaces lógicas se agregan en un tipo de cosa *Sala* con una interfaz lógica "IRoom". La interfaz lógica "IRoom" define las propiedades de temperatura y humedad y le permite crear su propio modelo lógico mediante la agregación de dispositivos en una sola cosa con la que la aplicación puede interactuar.  

![Visión general de la gestión de datos en {{site.data.keyword.iot_short_notm}}.](../information_management/images/Thing)

**Importante:** La característica de cosa de {{site.data.keyword.iot_short_notm}} solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.


Para obtener más información acerca de la definición y configuración de recursos e información clave, consulte [Visión general de la gestión de datos](ga_im_definitions.html). 

## Pasos siguientes

- Cree su propio duplicado de dispositivo en {{site.data.keyword.iot_short_notm}}. Para obtener más información, consulte la documentación [Cómo empezar con la Gestión de datos utilizando la interfaz web](im_ui_flow.html). 
- Cree un duplicado de dispositivo y un duplicado de activo utilizando las API REST. Para obtener más información, consulte la documentación [Cómo empezar con la gestión de datos](../information_management/getting_started_things.html).  
- Cree reglas que se desencadenan cuando los datos de suceso que coinciden con una condición especificada, o un conjunto de condiciones, se reciben mediante {{site.data.keyword.iot_short_notm}}. Para obtener más información, consulte la documentación beta [Reglas incorporadas](../information_management/im_rules.html).

Para obtener información más detallada sobre cada uno de los pasos descritos en la documentación de *Cómo empezar con la Gestión de datos*, consulte los casos de ejemplo que se describen en los temas siguientes: 

- [Guía paso a paso 1: Un ejemplo detallado sobre cómo trabajar con dispositivos a través de una interfaz común](ga_im_index_scenario.html#scenario) 
- [Guía paso a paso 2: Un ejemplo detallado sobre cómo trabajar con las cosas a través de una interfaz común](../information_management/im_index_scenario_thing.html#scenario) 


