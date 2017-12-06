---

copyright:
years: 2017
lastupdated: "2017-08-10"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Cómo empezar con la Gestión de datos utilizando la interfaz web (Beta)
{: #gs_web}

**Importante:** La característica de Gestión de datos de {{site.data.keyword.iot_full}} solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

{{site.data.keyword.iot_short_notm}} proporciona herramientas en línea para ayudarle a normalizar y transformar datos como parte de la característica de Gestión de datos. Además de la [API de gestión de datos de Watson IoT Platform ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window} que se proporciona, puede utilizar el Creador de interfaz simple o el Creador de interfaz avanzado para configurar recursos y empezar a asignar sus datos de dispositivo. Los creadores de interfaz en línea le guían a través de los pasos en la interfaz de usuario.

Para obtener información acerca de la característica de Gestión de datos y sus beneficios, consulte la [Introducción a la gestión de datos](../GA_information_management/ga_im_device_twin.html#device_twins).

Para obtener información acerca de los recursos necesarios para la configuración, consulte [Visión general de la Gestión de datos](../GA_information_management/ga_im_definitions.html#definitions_resource).

Con el creador de interfaz simple, puede añadir una interfaz física y se crea automáticamente una interfaz lógica y ambas se asignan juntas. Con el creador de interfaz avanzado, tiene más control sobre la configuración. Puede añadir una interfaz física y una o varias interfaces lógicas y luego asignarlas entre ellas.

Las interfaces y las asignaciones se añaden en formato de borrador. Después de haberlas añadido y validado, debe activarlas. Para obtener más información acerca de los recursos borrador y los recursos activos, consulte [Creación, actualización, activación y desactivación de recursos](../GA_information_management/ga_im_definitions.html#draft_active_resources).



## Flujo de alto nivel
{: #interface_flow}

Siga los pasos siguientes como ayuda para configurar los recursos que necesita para empezar a utilizar la característica de gestión de datos.

**Antes de empezar**
Este procedimiento da por supuesto que tiene un tipo de dispositivo y, por lo menos, un dispositivo conectado y registrado. Para obtener más información acerca de estos requisitos previos y para obtener instrucciones sobre cómo crear un tipo de dispositivo y registrar un dispositivo, consulte [Configuración de dispositivos para utilizarlos con la característica de gestión de datos](im_config_devices.html).

1. Seleccione el tipo de dispositivo para el que desea normalizar y transformar datos.
2. Seleccione el Creador de interfaz simple o avanzado:

a. Flujo simple  
   i. [Cree una interfaz física borrador](#create_physical_interface_simple).  
   
b. Flujo avanzado  
   i. [Cree una interfaz física borrador](#create_physical_interface_advanced).  
   ii. [Cree una o varias interfaces lógicas borrador](#create_logical_interface). (Cuando utiliza el flujo simple este paso se completa automáticamente).  
   iii. [Asigne la interfaz física con las interfaces lógicas](#create_interface_mappings).  
     
     
3. [Defina las preferencias de notificación](#set_notifications).
4. [Active la configuración](#validate_activate).

*Nota:* También puede configurar la característica de Gestión de datos utilizando la API. Para obtener información detallada acerca de la configuración de la característica de Gestión de datos para normalizar y transformar los datos de su dispositivo utilizando la API, consulte [Cómo empezar con la Gestión de datos]{ga_im_example.html#im_example}. [Guía paso a paso: Un ejemplo detallado sobre cómo trabajar con dispositivos a través de una interfaz común](../GA_information_management/ga_im_index_scenario.html#scenario) le guía a través de los pasos para crear una interfaz lógica de tipo de dispositivo para dispositivos de termómetro heterogéneo.

Para obtener detalles acerca de la API, consulte la documentación de la [API REST HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Creación de una interfaz física borrador (Flujo simple)
{: #create_physical_interface_simple}

1. En el menú de navegación principal, seleccione **Dispositivos**. 
2. Seleccione **Tipos de dispositivo** y seleccione el tipo de dispositivo para el que desea crear una interfaz. También puede [crear un nuevo tipo de dispositivo].
3. Vea y actualice la información de tipo de dispositivo como sea necesario y guarde los cambios.
4. Pulse **Interfaz**.
5. Pulse **Flujo simple** para abrir el Creador de interfaz simple.
6. Pulse **Crear interfaz**.
7. Pulse **Añadir propiedad** para empezar a añadir sucesos y propiedades a la interfaz física.  
 a. En la ventana **Añadir propiedades a la interfaz**, seleccione los sucesos que desea añadir a la interfaz. El sistema escucha los sucesos activos de los dispositivos conectados del tipo de dispositivo seleccionado. Puede seleccionar el ID de dispositivo del último suceso en memoria caché o seleccionar otro suceso de la lista.  
  b. Seleccione las propiedades del suceso y pulse **Añadir**.  
  c. Añada más propiedades si es necesario.
8. Pulse **Listo**. Se ha creado la interfaz física. Si desea añadir más detalles puede actualizar la interfaz seleccionando el enlace **Utilizar el creador de interfaz avanzado**.


## Creación de una interfaz física borrador (Flujo avanzado)
{: #create_physical_interface_advanced}

1. En el menú de navegación principal, seleccione **Dispositivos**. 
2. Seleccione **Tipos de dispositivo** y seleccione el tipo de dispositivo para el que desea crear una interfaz. También puede [crear un nuevo tipo de dispositivo].
2. Vea la información del tipo de dispositivo y pulse **Interfaz**.
4. Pulse **Flujo avanzado** para abrir el Creador de interfaz avanzado.
5. Pulse **Crear interfaz**.
7. En el separador **Identidad** de la página **Crear interfaz física**, escriba un nombre y una descripción para la interfaz física.
7. Opcional: Active el conmutador Edge Analytics si está utilizando dispositivos habilitados para Edge con la interfaz.
8. Pulse **Siguiente**.
9. Defina la interfaz física añadiendo tipos de suceso y cargas útiles:
 a. Pulse **Crear tipo de suceso**. 
 b. Seleccione el último suceso en memoria caché, seleccione un suceso de la lista o añada un suceso de forma manual.
10. Pulse **Listo**. Se ha creado la interfaz física en formato borrador. Puede continuar añadiendo una o varias interfaces lógicas.

## Creación de una interfaz lógica borrador
{: #create_logical_interface}

1. Después de crear la interfaz física borrador, pulse **Añadir interfaz lógica**.
2. En el separador **Identidad** de la página **Crear interfaz lógica**, escriba un nombre y una descripción para la interfaz lógica.
3. Pulse **Siguiente**.
4. Pulse **Añadir propiedad** para empezar a definir las propiedades de la interfaz lógica.
5. En la ventana **Crear propiedad** seleccione las propiedades de la interfaz física que desea añadir a la interfaz lógica y guarde los cambios.
6. Añada interfaces lógicas según sea necesario y luego pulse **Siguiente**.
7. {: #set_notifications}Configure las preferencias de notificación para la interfaz. Seleccione una de las siguientes opciones para determinar cuando desea que se envíen notificaciones de estado de dispositivo:
 - Sin notificadores de suceso: No se envían notificaciones. Puede utilizar la API REST para recuperar la información de cambio de estado de dispositivo.
 - En cambio de estado: Se le notifica cuando el estado del dispositivo cambia.
 - En cada suceso: Se le notifica cada vez que la plataforma procesa un suceso para el dispositivo, incluso si no da como resultado un cambio de estado.
8. Pulse **Listo**. Se han creado las interfaces en formato de borrador.
9. {: #validate_activate} Pulse **Desplegar** para validar la configuración.
10. Pulse **Desplegar** para activar la configuración. El estado de la configuración se establece en desplegada. 

