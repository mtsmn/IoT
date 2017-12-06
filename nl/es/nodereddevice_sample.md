---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Creación y conexión de un simulador de dispositivos Node-RED
Utilice Node-RED para crear un simulador de dispositivos para enviar datos de dispositivos simulados a la organización de {{site.data.keyword.iot_full}}.  
{:shortdesc}

## Visión general

Node-RED es una herramienta utilizada para la conexión conjunta de dispositivos de hardware, API y servicios en línea de una interesante nueva forma. Para obtener más información, consulte el sitio web de [Node-RED ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://nodered.org/){: new_window}.  

Puede ejecutar su instancia de Node-RED en su propio entorno o utilizarla como una aplicación de {{site.data.keyword.Bluemix_notm}}. Los siguientes pasos incluyen instrucciones para {{site.data.keyword.Bluemix_notm}}.

Cree y conecte el simulador de dispositivos Node-RED para enviar mensajes de dispositivo MQTT a {{site.data.keyword.iot_short_notm}}. En el siguiente ejemplo, el simulador de dispositivos simula el envío de datos para un contenedor de flete a un intermediario MQTT, por ejemplo {{site.data.keyword.iot_short_notm}}.

## Pasos

1. Cree el simulador de dispositivos Node-RED completando los siguientes pasos:   
    1. Inicie la sesión en {{site.data.keyword.Bluemix_notm}} en: https://console.ng.bluemix.net
    2. Seleccione el separador **Catálogo**.
    3. Localice la sección Contenedores modelo del catálogo de servicio y pulse **Iniciador de plataforma Internet de las cosas**. **Consejo:** Pulse [aquí ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.ng.bluemix.net/catalog/starters/internet-of-things-platform-starter){: new_window} para ir directamente a la página Iniciador de plataforma Internet de las cosas.
    4. En la página Iniciador de plataforma Internet de las cosas, seleccione el espacio donde desea desplegar Node-RED, verifique las selecciones Crear una app y pulse **Crear** para añadir Node-RED a la organización de Bluemix. Por ejemplo:
    <ul>
     <li> Espacio: dev
     <li> Nombre de app: myDevice
     <li> Nombre de host: myDevice  
    </ul>  
Deje las otras opciones como valores predeterminados. Una vez que se despliegue la aplicación, se mostrará la página Comenzar la codificación con Node-RED. **Nota:** El proceso de transferencia puede tardar algunos minutos.  

2. Pulse el enlace Rutas para abrir Node-RED o Visitar URL de app, por ejemplo `http://myDevice.mybluemix.net`  
3. Complete los siguientes pasos para **Proteger el editor de Node-RED**:
    1. Pulse **Siguiente**.
    2. Añada un nombre de usuario y una contraseña.  
    **Nota:** Debe cumplir las reglas de complejidad de la contraseña, de lo contrario, el botón **Siguiente** seguirá en gris.  
    3. Opcional. Marque **Permitir a cualquiera ver el editor pero no realizar cambios** o **No recomendado: Permitir a cualquiera acceder al editor y realizar cambios**.
    4. Pulse **Finalizar** para completar la configuración.
4. Pulse **Ir al editor de flujos de Node-RED**.
5. Especifique el nombre de usuario y la contraseña que ha creado anteriormente.  
El flujo de simulador de dispositivos se vuelve disponible en el editor de flujo. El flujo simula un **Termostato** que envía sus datos de ubicación, temperatura medida y humedad a {{site.data.keyword.iot_short_notm}}.  
6. Confirme que el dispositivo está conectado comprobando que se muestra un punto con el mensaje conectado para el nodo **Enviar a plataforma IBM IoT**. La comprobación también es válida para el nodo **IBM IoT App In** en el subflujo **Supervisor de temperatura**.  
7. Envíe y reciba mensajes de dispositivo MQTT completando los siguientes pasos:  
    1. Pulse el nodo de inyección **Enviar datos** para desencadenar el envío de datos a {{site.data.keyword.iot_short_notm}}.
       **Nota:** Puede activar el nodo de depuración **Depurar carga útil de salida** para ver qué datos se envían y para comprobar el nodo de función **Carga útil de dispositivo** para ver el código que compila la carga útil. 
    2. Después de pulsar **Enviar datos**, los mensajes MQTT se envían a {{site.data.keyword.iot_short_notm}} y los recibe el nodo **IBM IoT App In**. El subflujo **Supervisor de temperatura** establece si la temperatura está dentro de los límites establecidos y muestra un mensaje en el separador Depuración.
       **Nota:** Pulse el nodo de conmutador **umbral de temperatura** para comprobar y cambiar los valores de umbral.
    3. Compruebe el separador Depuración. Se muestra un mensaje, por ejemplo **Temperatura (17) dentro de los límites seguros**.
    
Ahora ha conectado el dispositivo de IoT de ejemplo a {{site.data.keyword.iot_short_notm}} y puede ver datos de dispositivo.
