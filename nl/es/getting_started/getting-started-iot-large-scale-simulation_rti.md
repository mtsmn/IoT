---

copyright:
  years: 2018
lastupdated: "2018-05-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guía 4: Simulación de un gran número de dispositivos
En la primera guía, ha configurado un simulador de dispositivo básico para simular manualmente una o varias cintas transportadoras. En esta guía, expandiremos esta simulación añadiendo una mayor cantidad de simuladores automáticos a su entorno para probar análisis y supervisar en un entorno más realista y multi-dispositivo.
{:shortdesc}

**Importante:** La aplicación requiere 512 MB de memoria, que es más de lo asignado por defecto y que también excede la cantidad disponible en las cuentas de prueba gratuita, incluyendo la cuenta de prueba y la cuenta estándar de {{site.data.keyword.Bluemix}}. Los titulares de una cuenta de Subscripción o de Pago según uso pueden aumentar la memoria asignada a 512 MB. Los titulares de una cuenta de prueba gratuita deben actualizar a una cuenta de Subscripción o de Pago según uso. Para obtener más información acerca de los tipos de cuenta de {{site.data.keyword.Bluemix_notm}}, consulte [Tipos de cuentas](/docs/pricing/index.html#pricing).

**Nota:** Bluemix es ahora IBM Cloud. Consulte el [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo") para obtener más detalles.

## Visión general y objetivo
{: #overview}

En esta guía, configurará una aplicación de {{site.data.keyword.Bluemix_notm}} para simular varios dispositivos de cinta transportadora utilizando un "programa de fondo" [Node-RED ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://nodered.org){: new_window}.

La aplicación contiene tres flujos que:   
1. Crean varios dispositivos.   
2. Simulan la publicación de sucesos simultánea.
3. Suprimen los dispositivos.   

![Creación y supresión de dispositivos](images/device_creation.png "Creación y supresión de dispositivos")

![Simulación de suceso de dispositivo](images/device_event_simulation.png "Simulación de suceso de dispositivo")

Al completar las instrucciones de esta guía:
- Utilizará Cloud Foundry para desplegar una aplicación de simulador de dispositivo basado en Node-RED y habilitada para webhook.
- Utilizará llamadas API para crear y registrar dispositivos, publicar sucesos de dispositivo y suprimir dispositivos.

**Importante:** Es posible que simular un gran número de dispositivos que envían datos de dispositivo de forma simultánea a {{site.data.keyword.iot_short_notm}} utilice una gran cantidad de datos. Puede utilizar el panel de control {{site.data.keyword.iot_short_notm}} *Uso* para supervisar cuantos datos están utilizando sus dispositivos y aplicaciones. Las medidas se actualizan en intervalos de 2 horas.

## Requisitos previos
{: #prereqs}  
Necesitará las siguientes cuentas y herramientas:

* [Cuenta {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/registration/) con:    
 - Más de 512 MB de RAM   
 - Más de 1 GB de cuota de disco  
 - Dos servicios disponibles
* [Interfaz de línea de mandatos de Cloud Foundry (cf CLI) ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilice la CLI de cf para desplegar y gestionar sus aplicaciones de {{site.data.keyword.Bluemix_notm}}.
* Opcional: [Git ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://git-scm.com/downloads){: new_window}  
Si elige utilizar Git para descargar los ejemplos de código también debe tener una cuenta de [GitHub.com ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com){: new_window}. También puede descargar el código como archivo comprimido sin una cuenta de GitHub.com.

Para cualquiera de las llamadas REST en los pasos siguientes, puede utilizar tanto cURL como el plugin complementario de cliente REST disponible en Mozilla.  
{: tip}

## Paso 1: Desplegar la aplicación en {{site.data.keyword.Bluemix_notm}}
{: #step1}

Siga los pasos siguientes para crear y desplegar su app de forma manual.   

1. Clone el repositorio de GitHub de la app de muestra *Lesson4*.  
Utilice su herramienta Git favorita para clonar el siguiente repositorio:  
https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
En Git Shell, utilice el siguiente mandato:
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-multi-simulator
```
3. Configure la aplicación para su entorno editando el archivo manifest.yml.  
Qué editar:
 - Para utilizar un servicio de {{site.data.keyword.iot_short_notm}} existente, actualice todas las instancias de `lesson4-simulate-iotf-service` para reflejar ese nombre de servicio. Por ejemplo, si está utilizando el servicio {{site.data.keyword.iot_short_notm}} de la guía 1, utilice `iotp-for-conveyor` para el nombre del servicio.    
 - Establezca el nombre del dispositivo y el host.   
En la sección de aplicaciones, cambie las entradas `name` y `host` a algo exclusivo, como `YOUR_NAME-lesson4-simulate`.   
**Consejo:** El URL ROUTES que utiliza para acceder a la app se crea a partir de la entrada `host`, por ejemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
<pre><code>
declared-services:
    lesson4-simulate-cloudantNoSQLDB:
     label: cloudantNoSQLDB
     plan: Lite
    lesson4-simulate-iotf-service:
     label: iotf-service
     plan: iotf-service-free
applications:  </br>
  -  path: .
      memory: 512M
      instances: 1
      domain: mybluemix.net
      name: lesson4-simulate
      host: lesson4-simulate
      disk_quota: 1024M</br>
  services:</br>
     - lesson4-simulate-cloudantNoSQLDB
     - lesson4-simulate-iotf-service
</code></pre>  
1. Desde la línea de mandatos, defina su punto final de API ejecutando el mandato cf api.   
Sustituya el valor `API-ENDPOINT` con el punto final de API de su región.
   ```
cf api API-ENDPOINT
   ```
Ejemplo: `cf api https://api.ng.bluemix.net`  
<table>
<tr>
<th>Región</th>
<th>Punto final de API</th>
</tr>
<tr>
<td>EE.UU. sur</td>
<td>https://api.ng.bluemix.net</td>
</tr>
<tr>
<td>Reino Unido</td>
<td>https://api.eu-gb.bluemix.net</td>
</tr>
 <!--<tr>
 <td>Germany</td>
 <td>https://api.eu-de.bluemix.net</td>
 </tr>-->
</table>
2. Inicie la sesión en la cuenta de {{site.data.keyword.Bluemix_notm}}.
  ```
cf login
  ```
Si se le solicita, seleccione la organización y el espacio donde desea desplegar {{site.data.keyword.iot_short_notm}} y la app de muestra.
6. Cree los servicios necesarios en {{site.data.keyword.Bluemix_notm}}.   
 1. Utilice el siguiente mandato para crear el servicio cloudantNoSQLDB Lite:
<pre><code>$ cf create-service cloudantNoSQLDB Lite lesson4-simulate-cloudantNoSQLDB</code></pre>    
 2. Utilice el siguiente mandato para crear el servicio de {{site.data.keyword.iot_short_notm}}.
<pre><code>$ cf create-service iotf-service iotf-service-free lesson4-simulate-iotf-service </code></pre>   
**Importante:** Si está utilizando un servicio de {{site.data.keyword.iot_short_notm}} existente y ha actualizado el archivo manifest.yml, asegúrese de que utiliza el nombre de servicio existente. Por ejemplo, de la guía 1, utilice `iotp-for-conveyor` en lugar de `lesson4-simulate-iotf-service` en la llamada cf.
7. Ejecute el mandato `cf push` para crear el proyecto y enviar por push su organización.  
Su aplicación de muestra está desplegada en {{site.data.keyword.Bluemix_notm}}.  
Cuando el despliegue finaliza, se muestra un mensaje para indicar que su app está en ejecución.   
Ejemplo:  
  ```
requested state: started
instances: 1/1
usage: 512M x 1 instances
urls: lesson4-simulate.mybluemix.net
last uploaded: Tue May 30 13:22:17 UTC 2017
stack: cflinuxfs2
buildpack: SDK for Node.js(TM) (ibm-node.js-4.8.2, buildpack-v3.12-20170505-0656
)

     state     since                    cpu     memory           disk
details
#0   running   2017-05-30 09:25:41 AM   23.9%   194.9M of 512M   378.4M of 1G
  ```
8. Obtenga las credenciales de servicio para su app.
 1. Inicie sesión en {{site.data.keyword.Bluemix_notm}} en:  
 [https://bluemix.net ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://bluemix.net){: new_window}.
 2. Seleccione la cuenta y el espacio donde ha desplegado la app.
 3. En el menú, seleccione **Apps** y seleccione **Panel de control**.
 4. En las apps de Cloud Foundry, pulse el nombre de la aplicación que acaba de desplegar.
 4. Seleccione **Conexiones**.
 5. Localice el iotf-service que está utilizando con su app y pulse **Ver credenciales**.  
 6. En la página de credenciales de servicio, localice `apiKey` y `apiToken`.   
La clave de API y la señal de autenticación se utilizan como credenciales cuando utiliza la API de app para crear y ejecutar sus dispositivos simulados.

## Paso 2: Proteger su flujo Node-RED
{: #step2}

Aunque este paso es opcional, se recomienda proteger su flujo Node-RED.

1. Proteja su editor para que solo los usuarios autorizados puedan acceder.
2. Para proteger el flujo Node-RED, vaya al panel de control de {{site.data.keyword.Bluemix_notm}} y seleccione la aplicación que desplegó anteriormente.
3. Pulse **Tiempo de ejecución** y pulse **Variables de entorno**.
4. Añada las siguientes variables de entorno definidas por el usuario y sus valores respectivos:
    a. NODE_RED_USERNAME
    b. NODE_RED_PASSWORD
5. Guarde el flujo.

Cuando su flujo Node-RED esté protegido, si planea utilizar mandatos de API REST para crear, suprimir o simular varios dispositivos, debe proporcionar las credenciales de nombre de usuario y contraseña de Node-RED durante la autenticación básica.

## Paso 3: Crear y conectar dispositivos
{: #step3}

Puede utilizar la interfaz del flujo Node-RED o la API REST de aplicación para completar las siguientes tareas.

### Creación y conexión de dispositivos mediante Node-RED  
Para registrar varios dispositivos:  
1. Inicie sesión en {{site.data.keyword.Bluemix_notm}} en:  
[https://bluemix.net ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://bluemix.net){: new_window}.
2. Seleccione la cuenta y el espacio donde ha desplegado la app.
3. En el menú, seleccione **Apps** y seleccione **Panel de control**.
4. En las apps de Cloud Foundry, pulse el URL **ROUTE** de la aplicación que acaba de desplegar.  
El ROUTE_URL se construye a partir de la entrada `host` que utilizó en el archivo manifest.yml: `HOST.mybluemix.net`  
Ejemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net`.  
Se abre la interfaz de Node-RED.
5. Pulse **Ir al editor de flujos de Node-RED**.
6. En el editor de flujos, seleccione el separador **Tipo de dispositivo e instancia**.
7. Para registrar cinco dispositivos, pulse el nodo inyectar etiquetado como **Registrar 5 dispositivos motorController**.
8. Verifique que sus dispositivos están registrados.
 1. En el panel de control de {{site.data.keyword.iot_short_notm}}, en el menú, seleccione **Paneles**.
 3. Seleccione el panel **Analítica centrada en dispositivo**.
 4. Localice la tarjeta **Dispositivos que me interesan**.  
Se visualizan los nombres de dispositivo.

### Creación y conexión de dispositivos mediante la API REST  
Para registrar varios dispositivos:  

1. Realice una solicitud HTTP POST sobre el siguiente URL: `ROUTE_URL/rest/devices`  
Ejemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/devices`  
 - Establezca 'Content-Type' y 'Accept' en 'application/json'  
 - Utilice la siguiente carga útil JSON:  
<pre><code>{  </br>
"numberDevices":5, </br>
"typeId":"iotp-for-conveyor",  </br>
"authToken":"mypassword",  </br>
"chunkSize":5,  </br>
"deviceName":"belt"  </br>
}
</code></pre>  
  Donde:  
    - numberDevices es el número de dispositivos a crear y registrar.
    - Opcional: typeId es el tipo de dispositivo con el que se registrarán los dispositivos. Si no se proporciona typeId el valor predeterminado es "iotp-for-conveyor". **Consejo:** Puede introducir cualquier nombre de tipo de dispositivo, pero las otras guías de la serie esperan dispositivos del tipo `iotp-for-conveyor`. Si utiliza un tipo de dispositivo diferente, debe modificar los valores de la guía según convenga.
    - Opcional: authToken es la señal de autorización con la que se registrarán los dispositivos.
    - Opcional: Si no se proporciona chunkSize, se establece en 500 de forma predeterminada. El 'chunksize' debe ser menor que y un factor de numberDevices.
    - deviceName es el patrón para el deviceID de los dispositivos creados.
2. Verifique que sus dispositivos están registrados.
 1. En el panel de control de {{site.data.keyword.iot_short_notm}}, en el menú, seleccione **Paneles**.
 3. Seleccione el panel **Analítica centrada en dispositivo**.
 4. Localice la tarjeta **Dispositivos que me interesan**.  
Se visualizan los nombres de dispositivo.

## Paso 4: Simular sucesos de dispositivo
{: #step4}

Puesto que los dispositivos simulados se registran con {{site.data.keyword.iot_short_notm}}, puede ejecutar ahora el simulador para empezar a enviar sucesos de dispositivo.

### Simulación de sucesos de dispositivo utilizando Node-RED  
Para enviar sucesos de dispositivo:  
1. En el editor de flujos de Node-RED, seleccione el separador **Simular varios dispositivos**.
7. Para simular cinco dispositivos, pulse el nodo inyectar etiquetado como **Simular 5 dispositivos**.
8. Verifique que sus dispositivos están enviando datos.
 1. En el panel de control de {{site.data.keyword.iot_short_notm}}, en el menú, seleccione **Paneles**.
 3. Seleccione el panel **Analítica centrada en dispositivo**.
 4. Localice la tarjeta **Dispositivos que me interesan**.  
 5. Seleccione uno de los dispositivos y verifique que los puntos de datos de dispositivo actualizados que corresponden con el mensaje publicado se visualizan en la tarjeta **Propiedades de dispositivo**.  


### Simulación de sucesos de dispositivo utilizando la API REST  
Para enviar sucesos de dispositivo:

1. Realice una solicitud HTTP POST sobre el siguiente URL: `ROUTE_URL/rest/runtest`  
Ejemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/runtest`  
 - Establezca 'Content-Type' y 'Accept' en 'application/json'  
 - Utilice la siguiente carga útil JSON:   
<pre><code>{  </br>
"numberDevices":5,  </br>
"numberEvents":10,  </br>
"timeInterval":1000,  </br>
"deviceType":"iotp-for-conveyor",  </br>
"deviceName":"belt"  </br>
}</code></pre>
Donde:
    - numberDevices es el número de dispositivos para los que desea simular datos.
    - numberEvents es el nombre de sucesos que envía cada dispositivo simulado.
    - timeInterval es el espaciado entre sucesos en milisegundos.
    - deviceType es el tipo de dispositivo para el que ha creado dispositivos simulados.
    - deviceName es el patrón para el deviceID de los dispositivos creados.
8. Verifique que sus dispositivos están enviando datos.
 1. En el panel de control de {{site.data.keyword.iot_short_notm}}, en el menú, seleccione **Paneles**.
 3. Seleccione el panel **Analítica centrada en dispositivo**.
 4. Localice la tarjeta **Dispositivos que me interesan**.  
 5. Seleccione uno de los dispositivos y verifique que los puntos de datos de dispositivo actualizados que corresponden con el mensaje publicado se visualizan en la tarjeta **Propiedades de dispositivo**.   

## Paso 5: Supresión de dispositivos
{: #deleting}

### Node-RED  
Para suprimir dispositivos:  
1. En el editor de flujos de Node-RED, seleccione el separador **Tipo de dispositivo e instancia**.
2. Para suprimir cinco dispositivos, pulse el nodo inyectar etiquetado como **Suprimir 5 dispositivos**.
3. Verifique que sus dispositivos están suprimidos.
 1. En el panel de control de {{site.data.keyword.iot_short_notm}}, en el menú, seleccione **Paneles**.
 3. Seleccione el panel **Analítica centrada en dispositivo**.
 4. Localice la tarjeta **Dispositivos que me interesan**.  
 5. Verifique que los dispositivos ya no se enumeran.  


### API Rest  

1. Realice una solicitud HTTP POST sobre el siguiente URL: `ROUTE_URL/rest/deleteDevices`  
Ejemplo: `https://YOUR_APP_NAME-lesson4-simulate.mybluemix.net/rest/deleteDevices`
 - Establezca 'Content-Type' y 'Accept' en 'application/json'  
 - Utilice la siguiente carga útil JSON:      
<pre><code>{
"numberDevices":500,   
"deviceType":"iot-conveyor-belt",  
"deviceName": "belt"  
}</code></pre>
2. Verifique que sus dispositivos están suprimidos.
 1. En el panel de control de {{site.data.keyword.iot_short_notm}}, en el menú, seleccione **Paneles**.
 3. Seleccione el panel **Analítica centrada en dispositivo**.
 4. Localice la tarjeta **Dispositivos que me interesan**.  
 5. Verifique que los dispositivos ya no se enumeran.  


## ¿Qué hacer a continuación?
{: @whats_next}  
Salte a otro tema que le interese:
- [Guía 2: Utilización de reglas y acciones básicas en tiempo real](getting-started-iot-rules.html)  
Ahora que ha configurado correctamente su cinta transportadora, la ha conectado con {{site.data.keyword.iot_short_notm}} y ha enviado algunos datos, es hora de hacer que esos datos trabajen para usted utilizando reglas y acciones.
- [Guía 3: Supervisión de los datos de dispositivo](getting-started-iot-monitoring.html)  
Ahora que ha conectado uno o varios dispositivos y ha empezado a realizar un buen uso de los datos de dispositivo, es hora de empezar a supervisar una colección de dispositivos.
- [Obtenga más información acerca de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Obtenga más información acerca de las API de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/reference/api.html){:new_window}
