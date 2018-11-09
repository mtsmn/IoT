---

copyright:
years: 2016, 2018
lastupdated: "2018-03-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Cómo empezar con la gestión de datos utilizando las API REST
{: #im_example}

Utilice los pasos siguientes para ayudarle a configurar los recursos que necesita para empezar a utilizar las características de duplicado de dispositivo y activo del componente de gestión de datos de {{site.data.keyword.iot_full}}.

Para obtener detalles acerca de la API, consulte la documentación de la [API REST HTTP de {{site.data.keyword.iot_short_notm}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html){:new_window}.


## Flujo de trabajo de alto nivel
{: #workflow}

Utilice los pasos siguientes para ayudarle a configurar los recursos que necesita para empezar a correlacionar los datos del dispositivo o de la cosa utilizando la característica de duplicado de dispositivo o de activo.

**Consejo:** para obtener más información sobre cada uno de los pasos, consulte los casos de ejemplo o utilice los enlaces para ir directamente a un paso específico de un caso de ejemplo. [Guía paso a paso 1](ga_im_index_scenario.html#scenario) le guiará a través de los pasos para crear una interfaz lógica de tipo de dispositivo para dispositivos de termómetro heterogéneos y la [Guía paso a paso 2](../information_management/im_index_scenario_thing.html#scenario) aumenta el caso de ejemplo describiendo cómo crear una interfaz lógica que permita que la aplicación consuma datos de dos tipos diferentes de dispositivos de clima combinados en una sola cosa de tipo sala.

El proceso para crear y consumir interfaces lógicas difieren ligeramente en función de si está creando una interfaz lógica asociada a un tipo de dispositivo o a un tipo de cosa.

### Antes de empezar
Para crear una interfaz lógica asociada con un tipo de dispositivo o un tipo de cosa, debe tener [al menos un dispositivo registrado con {{site.data.keyword.iot_short_notm}}](ga_im_index_scenario.html#step14) que envíe sucesos con propiedades de estado.  


### Pasos

1. 	Defina las propiedades de estado de entrada.  
En primer lugar, defina las propiedades de estado de entrada que desea que la interfaz lógica ponga a disposición de las aplicaciones.  
En función de la interfaz lógica que esté creando, realice una de estas dos cosas:
<dl>
<dt>Tipo de dispositivo: cree una interfaz física.</dt>
<dd>
<ol>
<li>[Cree un archivo de esquema de suceso](ga_im_index_scenario.html#step1). El archivo de esquema de suceso es un archivo .JSON local que define la estructura y el formato de un suceso de entrada.
<li>[Cree un recurso de esquema de suceso para el tipo de suceso](ga_im_index_scenario.html#step2). El recurso de esquema de suceso es una construcción programática utilizada por o que se utiliza por {{site.data.keyword.iot_short_notm}}.
<li>[Cree un tipo de suceso que haga referencia al esquema de suceso](ga_im_index_scenario.html#step3). {{site.data.keyword.iot_short_notm}} utiliza el tipo de suceso para correlacionar uno o varios recursos de esquema de suceso con una interfaz física.
<li>[Cree una interfaz física](ga_im_index_scenario.html#step7).
<li>[Añada el tipo de suceso a la interfaz física](ga_im_index_scenario.html#step8).
<li>[Añada la interfaz física al tipo de dispositivo](ga_im_index_scenario.html#step9).
</ol>
</dd>
<dt>Tipo de cosa: Defina un tipo de cosa.</dt>
<dd>
<ol>
<li>[Cree un archivo de esquema de tipo cosa](../information_management/im_index_scenario_thing.html#crt_composition_file).  
Un archivo de esquema de tipo cosa es un archivo .JSON local que define la composición del tipo de cosa apuntando a las interfaces lógicas existentes.
<li>[Cree el recurso de esquema de tipo cosa](../information_management/im_index_scenario_thing.html#crt_composition_resource).  
Cargue el archivo .JSON local en {{site.data.keyword.iot_short_notm}}.
<li>[Cree un tipo cosa](../information_management/im_index_scenario_thing.html#crt_thing_type). Un tipo cosa sirve el mismo propósito que un tipo de dispositivo en el sentido de que representa una clase de cosas.
</ol>
</dd>
</dl>
4. 	Cree la interfaz lógica.
 1. 	Cree un archivo de esquema de interfaz lógica para el [tipo de dispositivo](ga_im_index_scenario.html#step4) o el [tipo de cosa](../information_management/im_index_scenario_thing.html#crt_ai_schema_file).  
Un archivo de esquema de interfaz lógica es un archivo .JSON local que define el estado del dispositivo que pasará a disposición de las aplicaciones.
 2. 	Cree un recurso de esquema de interfaz lógica para el [tipo de dispositivo](ga_im_index_scenario.html#step5) o [tipo de cosa](../information_management/im_index_scenario_thing.html#crt_ai_schema_resource).
 3.	Cree una interfaz lógica para el [tipo de dispositivo](ga_im_index_scenario.html#step6) o [tipo de cosa](../information_management/im_index_scenario_thing.html#crt_thing_ai).
 4.	Añada la interfaz lógica al [tipo de dispositivo](ga_im_index_scenario.html#step10) o [tipo de cosa](../information_management/im_index_scenario_thing.html#add_thing_ai).
5. 	Defina las correlaciones para el [tipo de dispositivo](ga_im_index_scenario.html#step11) o [tipo de cosa](../information_management/im_index_scenario_thing.html#define_Thing_type_mappings).   
Las correlaciones se utilizan para correlacionar propiedades de entrada con propiedades de la interfaz lógica.  
6. 	Valide y active la configuración que está asociada con el [tipo de dispositivo](ga_im_index_scenario.html#step15) borrador o [Thing_type](../information_management/im_index_scenario_thing.html#activate).
7. 	Recupere el estado del [dispositivo](ga_im_index_scenario.html#step13) o [cosa](../information_management/im_index_scenario_thing.html##verify_Thing_state).  
Verifique que las suscripciones muestran los datos de dispositivo actualizados o que los datos de dispositivo actualizados se devuelven utilizando una llamada REST o suscribiéndose a una serie de tema.  



