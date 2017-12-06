---

copyright:
  years: 2017
lastupdated: "2017-09-28"
---

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Simulación de datos de dispositivo (Beta)
{: #sim_device_data}

Utilizando el simulador de dispositivos de {{site.data.keyword.iot_full}}, puede configurar sucesos simulados para dispositivos. Puede utilizar los datos de suceso de dispositivo para aprender, probar y demostrar la funcionalidad completa de las características de {{site.data.keyword.iot_short_notm}}.
{: shortdesc}

Puede utilizar tipos de dispositivo existentes y dispositivos y el simulador le permite generar nuevos dispositivos de tipos existentes. Puede configurar los detalles de suceso de cada dispositivo o definir una configuración predeterminada que se aplique a todos los dispositivos. Puede exportar una configuración de suceso de simulado para poder volver a utilizarla o compartirla para configurar otras simulaciones.

**Importante:** La característica de simulador de dispositivo de {{site.data.keyword.iot_full}} solo está disponible como parte de un programa beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Para simular datos de dispositivo: 

1. Inicie una sesión en {{site.data.keyword.iot_short_notm}}.
2. En el panel de navegación principal, seleccione **Valores**.
3. En la sección **Características experimentales** active el simulador de dispositivo.
4. En el panel de navegación principal, seleccione **Dispositivos**. Un mensaje en la esquina inferior derecha de la pantalla indica que no se están ejecutando simulaciones.
5. Pulse en el mensaje "Se están ejecutando 0 simulaciones".
6. En la ventana emergente **Simulaciones**, pulse **Añadir primera simulación**.
7. Seleccione el tipo de dispositivo para el que desea simular datos.
8. Seleccione una de las siguientes opciones:
  - Para añadir uno o varios dispositivos, seleccione la cantidad y pulse **Dispositivo nuevo**. Los nuevos dispositivos se enumeran.
  - Para añadir un dispositivo existente, pulse **Utilizar dispositivo registrado** y seleccione un dispositivo de la lista.
  - Para importar una simulación existente, pulse **Importación y exportación**, seleccione el separador **Importación** e importe un archivo JSON o copie una configuración previamente exportada desde el portapapeles.
9. Pulse ![Icono Valores](images/settings_icon.png) y configure los detalles de la simulación para un tipo de dispositivo:
   1. Seleccione el tipo de dispositivo en la lista y pulse **+ Tipo de suceso** para añadir un tipo de suceso al tipo de dispositivo.
   2. Seleccione un tipo de suceso en la lista y pulse **Planificación** para definir la frecuencia del suceso.
   3. Edite los detalles del tipo de suceso en formato JSON y guarde el tipo de suceso actualizado.
   
   <p> Puede utilizar las dos variables especiales siguientes al configurar sucesos:  
        - `range`: Esta variable se sustituye con un número aleatorio entre los dos valores proporcionados. Por ejemplo: `{ "random": range(300, 100) }`  
        - `$counter`: Esta variable se sustituye con el número de dispositivos añadidos en el simulador para el tipo actual. Por ejemplo: `{"total": $counter}`</p>
   {: tip}
   
   4. Elija si desea utilizar estos valores como valores predeterminados para todos los dispositivos de este tipo de dispositivo o si desea utilizar valores personalizados para dispositivos individuales. 
   5. Repita los pasos de configuración para cada dispositivo al que desee aplicar valores personalizados. El mensaje en la parte inferior derecha de la pantalla muestra el número de simulaciones activas.
10. **Opcional:** Después de configurar simulaciones para dispositivos, exporte los detalles de la simulación para que se puedan reutilizar o compartir:
    1. En la ventana emergente **Simulaciones**, pulse **Importación y exportación**.
    2. Seleccione el separador **Exportar**.
    3. Copie los detalles de la simulación en el portapapeles o descárguelos en un archivo JSON.
11. En la página **Dispositivos**, navegue hasta uno de los dispositivos simulados y seleccione el separador **Sucesos recientes** para verificar que los sucesos simulados están funcionando.
