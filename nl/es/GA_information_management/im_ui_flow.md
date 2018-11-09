---

copyright:
years: 2016, 2018
lastupdated: "2018-05-14"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Cómo empezar con la Gestión de datos utilizando la interfaz web
{: #gs_web}

{{site.data.keyword.iot_full}} proporciona herramientas en línea para ayudarle a normalizar y transformar datos como parte de la característica de Gestión de datos.

## Visión general
{: #web_overview}

Además de la [API de gestión de datos de Watson IoT Platform ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} que se proporciona, puede utilizar el Creador de interfaz simple o el Creador de interfaz avanzado para configurar recursos y empezar a asignar sus datos de dispositivo. Los creadores de interfaz en línea le guían a través de los pasos en la interfaz de usuario.

Una interfaz física se utiliza para modelar la interfaz entre un dispositivo físico y Watson IoT Platform. Los tipos de suceso se pueden asociar con una interfaz física. Una interfaz lógica se utiliza para definir la vista normalizada en el estado de dispositivo en Watson IoT Platform y normalmente la consumen las aplicaciones. Una interfaz lógica debe asociarse con un esquema de interfaz lógica. El estado se actualiza como respuesta a sucesos de dispositivo de entrada.

Para obtener más información sobre la característica de Gestión de datos y sus ventajas, consulte la publicación [Introducción a la Gestión de datos](../GA_information_management/ga_im_device_twin.html#device_twins).

Para obtener información acerca de los recursos necesarios para la configuración, consulte [Visión general de la Gestión de datos](../GA_information_management/ga_im_definitions.html#definitions_resource).

Con el Creador de interfaz simple, se añade una interfaz física y, a continuación, se crea automáticamente una interfaz lógica y las dos se correlacionan conjuntamente. Con el creador de interfaz avanzado, tiene más control sobre la configuración. Puede añadir una interfaz física y una o varias interfaces lógicas y luego asignarlas entre ellas.

Las interfaces y las asignaciones se añaden en formato de borrador. Después de haberlas añadido y validado, debe activarlas. Para obtener más información acerca de los recursos borrador y los recursos activos, consulte [Creación, actualización, activación y desactivación de recursos](../GA_information_management/ga_im_definitions.html#draft_active_resources).

Las bibliotecas de la interfaz contienen interfaces lógicas y físicas de plantilla que puede reutilizar.

## Flujo de alto nivel
{: #interface_flow}

Siga los pasos siguientes como ayuda para configurar los recursos que necesita para empezar a utilizar la característica de gestión de datos.

### Antes de empezar

En este procedimiento se supone que tiene un tipo de dispositivo con al menos un dispositivo registrado y conectado. Para obtener más información sobre estos requisitos previos y para obtener instrucciones sobre cómo crear un tipo de dispositivo y registrar un dispositivo, consulte [Conexión de dispositivos](../iotplatform_task.html#iotplatform_task).

### Pasos

1. Seleccione el Creador de interfaz simple o el Creador de interfaz avanzada:
  - **Flujo de Creador de interfaz simple**
    1. [Cree una interfaz física borrador](#create_physical_interface_simple).
  - **Flujo de Creador de interfaz avanzado**
    1. [Cree una interfaz física borrador](#create_physical_interface_advanced).
    2. [Cree una o varias interfaces lógicas borrador](#create_logical_interface). (Cuando utiliza el flujo simple este paso se completa automáticamente).
    3. [Asigne la interfaz física con las interfaces lógicas](#create_interface_mappings).
2. [Defina las preferencias de notificación](#set_notifications).
3. [Active la configuración](#validate_activate).

También puede configurar la característica de Gestión de datos utilizando las API. Para obtener información detallada sobre la configuración de la característica de Gestión de datos para normalizar y transformar los datos de dispositivo utilizando las API, consulte [Cómo empezar con la Gestión de datos](ga_im_example.html#im_example). [Guía paso a paso: Un ejemplo detallado sobre cómo trabajar con dispositivos a través de una interfaz común](../GA_information_management/ga_im_index_scenario.html#scenario) le guía a través de los pasos para crear una interfaz lógica de tipo de dispositivo para dispositivos de termómetro heterogéneo. Para obtener detalles acerca de la API, consulte la documentación de la [API REST HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.
{: tip}


## Creación de una interfaz física borrador (Flujo simple)
{: #create_physical_interface_simple}

1. En el menú de navegación principal, pulse **Dispositivos**.
2. Pulse **Tipos de dispositivos** y seleccione el tipo de dispositivo para el que desea crear una interfaz. También puede crear un nuevo tipo de dispositivo. Consulte [Conexión de dispositivos](../iotplatform_task.html#iotplatform_task) para obtener más información.
3. Vea la información del tipo de dispositivo y pulse **Interfaz**.
4. Pulse **Flujo simple** para abrir el Creador de interfaz simple.
5. Pulse **Crear interfaz**.
6. Pulse **Añadir propiedad** para iniciar la adición de sucesos y propiedades a la interfaz física.
   a. En la ventana **Añadir propiedades a la interfaz**, seleccione los sucesos que desea añadir a la interfaz. El sistema escucha los sucesos activos de los dispositivos conectados del tipo de dispositivo seleccionado. Puede seleccionar el ID de dispositivo del último suceso en memoria caché o seleccionar otro suceso de la lista.
   b. Seleccione las propiedades del suceso y pulse **Añadir**.
   c. Añada más propiedades si es necesario.
7. Pulse **Listo**. Se ha creado la interfaz física. Si desea añadir más detalles, puede [actualizar a una interfaz avanzada](#create_physical_interface_advanced) seleccionando el enlace **Utilizar el creador de interfaz avanzado**.


## Creación de una interfaz física borrador (Flujo avanzado)
{: #create_physical_interface_advanced}

1. En el menú de navegación principal, pulse **Dispositivos**.
2. Pulse **Tipos de dispositivos** y seleccione el tipo de dispositivo para el que desea crear una interfaz. También puede crear un nuevo tipo de dispositivo. Consulte [Conexión de dispositivos](../iotplatform_task.html#iotplatform_task) para obtener más información.
2. Vea la información del tipo de dispositivo y pulse **Interfaz**.
3. Pulse **Flujo avanzado** para abrir el Creador de interfaz avanzado.
4. Elija una de las siguientes opciones:
 - Para crear una nueva interfaz física, pulse **Crear interfaz**.
 - Para utilizar una plantilla de interfaz de la biblioteca, pulse **Añadir desde biblioteca**, seleccione la interfaz y, a continuación, vaya a [Creación de una interfaz lógica borrador](#create_logic_interface).
5. En el separador **Identidad** de la página **Crear interfaz física**, escriba un nombre y una descripción para la interfaz física.
6. Opcional: Encienda el interruptor de Edge si utiliza dispositivos con la interfaz habilitada para Edge. Para obtener más información sobre Edge, consulte [Analítica de Edge](../edge_analytics.html#edge_analytics).
7. Pulse **Siguiente**.
8. Defina la interfaz física añadiendo y modificando los tipos de sucesos y las cargas útiles:
   a. Seleccione un tipo de suceso de la lista para modificar el tipo de suceso existente, o pulse **Crear tipo de suceso**.
   b. Seleccione el último suceso almacenado en memoria caché, seleccione un suceso de la lista o añada un suceso manualmente y, a continuación, pulse **Añadir**.
9. Pulse **Listo**. Se ha creado la interfaz física en formato borrador. Puede continuar añadiendo una o varias interfaces lógicas.

## Creación de interfaces lógicas de borrador (flujo avanzado)
{: #create_logical_interface}

1. Después de haber creado la interfaz física de borrador, elija una de las opciones siguientes:
 - Pulse **Añadir interfaz lógica** para crear una nueva interfaz lógica.
 - Pulse **Añadir desde biblioteca** para utilizar una plantilla de interfaz de la biblioteca de interfaz. Seleccione la interfaz y, a continuación, vaya al paso 7.
2. En el separador **Identidad** de la página **Crear interfaz lógica**, escriba un nombre y una descripción para la interfaz lógica.
3. Pulse **Siguiente**.
4. Pulse **Añadir propiedad** para empezar a definir las propiedades de la interfaz lógica.
5. En la ventana **Crear propiedad** seleccione las propiedades de la interfaz física que desea añadir a la interfaz lógica y guarde los cambios.
6. Pulse **Añadir objeto**.
7. Añada interfaces lógicas según sea necesario y luego pulse **Siguiente**.
8. {: #set_notifications}Configure las preferencias de notificación para la interfaz. Seleccione una de las siguientes opciones para determinar cuando desea que se envíen notificaciones de estado de dispositivo:
 - Sin notificadores de suceso: No se envían notificaciones. Puede utilizar la API REST para recuperar la información de cambio de estado de dispositivo.
 - En cambio de estado: Se le notifica cuando el estado del dispositivo cambia.
 - En cada suceso: Se le notifica cada vez que la plataforma procesa un suceso para el dispositivo, incluso si no da como resultado un cambio de estado.
8. Pulse **Listo**. Las interfaces se crean e ![Icono de estado Borrador](images/draft_icon.png) indica el estado Borrador. Puede continuar modificando o activando las interfaces.

## Activación de la configuración
{: #validate_activate}

Después de haber creado las interfaces físicas y lógicas de borrador, debe activar la configuración.

- Pulse **Activar** para activar la configuración. El estado de configuración se establece en activado.
![Despliegue activo](images/active_deployment.png) Si realiza cambios en las interfaces, las interfaces se vuelven a convertir en borradores y debe volver a activarlas.
