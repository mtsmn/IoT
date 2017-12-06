---

copyright:
  years: 2017
lastupdated: "2017-06-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Guía 3: Supervisión de los datos de dispositivo
Ahora que tiene uno o varios dispositivos conectados, es hora de empezar a supervisar los datos de dispositivo en tiempo real.
{:shortdesc}

## Visión general y objetivo
{: #overview}  

En esta guía, desplegará una aplicación de supervisión en {{site.data.keyword.Bluemix_notm}} para ver los datos de sus dispositivos.

Como en la guía 1, puede seguir una o ambas de las siguientes vías de acceso:
- Vía de acceso A: [Paso 1A: Desplegar y conectar la aplicación web de supervisión](#deploy_app)  
Siguiendo la vía de acceso A, desplegará una app ya preparada que supervisa el dispositivo de cinta transportadora que está ejecutando en su organización.
- Vía de acceso B: [Paso 1B: Crear una interfaz de usuario de supervisión utilizando la biblioteca de widgets](#widget-library)
La vía de acceso B, ligeramente más compleja, introduce la biblioteca de widgets y le guía a través de la creación de algunas interfaces de usuario básicas.

## Requisitos previos
{: #prereqs}  
Antes de empezar, asegúrese de que se cumplen los siguientes requisitos.

### Entorno local
- [Node.js ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://nodejs.org){: new_window} versión 6.x o superior.
- Vía de acceso A: [CLI de Angular ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/angular/angular-cli){: new_window} versión 1.x o superior.  
- [CLI de Cloud Foundry ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilice la CLI de cf para desplegar apps y servicios en {{site.data.keyword.Bluemix_notm}}. Para obtener más información, consulte la [Documentación de la CLI de Cloud Foundry![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
- Opcional: [Git ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://git-scm.com/downloads){: new_window}  
Si elige utilizar Git para descargar los ejemplos de código también debe tener una cuenta de [GitHub.com ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com){: new_window}. También puede descargar el código como archivo comprimido sin una cuenta de GitHub.com.

### Otros requisitos
También debe tener un dispositivo conectado del tipo de dispositivo `iot-conveyor-belt` que envía sucesos con nombre de suceso `sensorData` y con una carga útil de mensaje que incluye las siguientes propiedades:
```
{
	"d": {
		"id": "belt1",
		"ts": 1494946276931,
		"ay": "0.00",
		"running": true,
		"rpm": "1.0"
		}
}
```
Para obtener más información acerca de los sucesos de dispositivo y el formato de mensaje, consulte [Publicación de sucesos](/docs/services/IoT/devices/mqtt.html#publishing_events).  
Si ha completado la [Guía 1: Iniciación a {{site.data.keyword.iot_short_notm}} y una cinta transportadora simulada](getting-started-iot-conveyor.html), está listo.  
{: tip}

## Paso 1A: Desplegar y conectar la aplicación web de supervisión
{: #deploy_app}

La app de muestra Plant Floor Monitoring enumera todos los dispositivos de tipo iot-conveyor-belt conectados con su organización de {{site.data.keyword.iot_short_notm}} junto con un subconjunto de los datos de sucesos como RPM, última actualización e ID de dispositivo.

La app de muestra se compila utilizando las bibliotecas de cliente de Node.js en: [https://github.com/ibm-watson-iot/iot-nodejs ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![App de supervisión basada en Node.js](images/app_monitor.png "App de supervisión basada en Node.js")

Como parte de este paso aprenderá a:
- Desplegar una aplicación web de supervisión de muestra obtenida en GitHub utilizando Cloud Foundry.
- Configurar la app de muestra para conectarse con {{site.data.keyword.iot_short_notm}} utilizando una clave de API y una señal de autenticación.
- Utilizar la aplicación web para supervisar sus dispositivos de cinta transportadora conectados.  

### Pasos detallados
Los siguientes pasos le guían a través de la creación y el despliegue de la app en {{site.data.keyword.Bluemix_notm}}. Para obtener información acerca de la ejecución de la app de forma local, consulte el archivo README en GitHub.
1. Clone el repositorio de GitHub de la aplicación de muestra *Plant Floor Monitoring* de Node.js.  
Utilice su herramienta Git favorita para clonar el siguiente repositorio:  
https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
En Git shell, utilice el siguiente mandato:

```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-angular
  ```
2. Cree una combinación de clave de API y señal de autenticación para su app.  
La necesitará cuando configure la app para conectarla con su organización. Para obtener más información acerca del registro de dispositivos, consulte [Conexión de aplicaciones](/docs/services/IoT/platform_authorization.html).  
 1. Abra el panel de control de {{site.data.keyword.iot_short_notm}}.
 2. Seleccione **Apps**.
 3. Pulse **Generar clave de API**
 4. Copie los valores de la clave de API y la señal de autenticación que se listan.
 5. Seleccione **Aplicación de visualización** como rol de API.  
**Consejo:** Si añade más características a la aplicación, deberá elevarlo a un rol más alto.
 6. Añada un comentario para que pueda identificar fácilmente esta combinación de clave de API y señal de autenticación.
 7. Pulse **Generar**.
3. Configure su app para que se conecte con {{site.data.keyword.Bluemix_notm}}.
Navegue a la raíz del repositorio *guide-conveyor-ui-angular* y cree un archivo `basicConfig.json` que contenga el siguiente contenido:
  ```
{
  "org": "your orgID",
  "apiKey": "your API key",
  "apiToken": "your Authentication Token"
}
  ```
Sustituya los valores del parámetro con los valores correspondientes para su organización de {{site.data.keyword.Bluemix_notm}}: orgID, clave de API y señal de autenticación que acaba de crear.  
Ejemplo:
```
 {   
  "org": "3v5whr",    
  "apiKey": "a-3v5whr-jhkmsghlqv",  
  "apiToken": "-q0MkPN2cNYB6+?ISk"  
}
```
4. Inicie la sesión en la cuenta de {{site.data.keyword.Bluemix_notm}} utilizando la CLI de Cloud Foundry.  
Para obtener más información, consulte la [Documentación de la CLI de Cloud Foundry ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
En la línea de mandatos, especifique el mandato siguiente:  
  ```
cf login
  ```
Si se le solicita, seleccione la organización y el espacio donde desea desplegar la app de muestra de supervisión.
5. Si es necesario, defina su punto final de API ejecutando el mandato cf api.   
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
6. Cambie el directorio por el directorio en el que se encuentra la app de muestra.  
  ```
cd guide-conveyor-ui-angular
  ```
7. Ejecute `npm install -g @angular/cli` para instalar la CLI de Angular.
8. Ejecute `npm install`.
9. Ejecute `npm run push` para crear el proyecto y enviar por push a su organización.  
Su aplicación web de muestra está desplegada en {{site.data.keyword.Bluemix_notm}}.
  
Cuando el despliegue finaliza, se muestra un mensaje para indicar que su aplicación está en ejecución.
   
Ejemplo:
  
  ```
requested state: started
instances: 1/1
usage: 64M x 1 instances
urls: iotmonitoringcontrol-undertided-eng.mybluemix.net
last uploaded: Tue May 16 19:01:13 UTC 2017
stack: cflinuxfs2
buildpack: https://github.com/cloudfoundry/nodejs-buildpack
     state     since                    cpu    memory     disk        details
#0   running   2017-05-16 03:03:05 PM   0.0%   0 of 64M   0 of 256M
  ```
10. Abra la app web.  
En el panel de control de la app de {{site.data.keyword.Bluemix_notm}}, pulse **Visitar URL de app** para abrir la aplicación web.  
Acceda y gestione el URL de aplicación pulsando en **Rutas**.   
El URL predeterminado es similar a:  
`https://iotmonitoringcontrol-RANDOM-STRING-ENG.mybluemix.net`

## Paso 1B: Crear una interfaz de usuario de supervisión utilizando la biblioteca de widgets
{: #widget-library}

La aplicación de muestra basada en la biblioteca de widgets incluye un indicador de la velocidad del motor, un indicador de los datos de acelerómetro y un diagrama de velocidad del motor que muestra los datos de un único dispositivo de tipo iot-conveyor-belt conectado con su organización de {{site.data.keyword.iot_short_notm}}. Puede utilizar el código de ejemplo para crear una aplicación frontal completa para sus dispositivos conectados de {{site.data.keyword.iot_short_notm}}.

![App de supervisión basada en biblioteca de widgets](images/app_monitor_b.png "App de supervisión basada en biblioteca de widgets")

Como parte de este paso aprenderá a:
- Desplegar una aplicación web de muestra obtenida en GitHub utilizando Cloud Foundry.
- Configurar la app de muestra para conectarse con {{site.data.keyword.iot_short_notm}} utilizando una clave de API y una señal de autenticación.
- Configurar tres widgets de interfaz de usuario para visualizar datos de dispositivo como indicadores y gráficos.
- Utilizar la aplicación web para supervisar su dispositivo de cinta transportadora conectado.  

### Pasos detallados
Los siguientes pasos le guían a través de la creación y el despliegue de la app en {{site.data.keyword.Bluemix_notm}}. Para obtener información acerca de la ejecución de la app de forma local, consulte el archivo README en GitHub.
1. Clone el repositorio de GitHub de la aplicación de muestra *Widget Library Monitoring*.  
Utilice su herramienta Git favorita para clonar el siguiente repositorio:  
https://github.com/ibm-watson-iot/guide-conveyor-ui-html
En Git Shell, utilice el siguiente mandato:
```
git clone https://github.com/ibm-watson-iot/guide-conveyor-ui-html
```
2. Instale las dependencias de aplicación.  
Navegue a la raíz del repositorio *guide-conveyor-ui-html* y ejecute el siguiente mandato:
```
npm install
```
3. Construya la interfaz de usuario.  
Para construir la interfaz de usuario de la aplicación debe añadir los widgets como código JavaScript en el archivo index.html de la aplicación para cada componente de la interfaz de usuario.  
Cada widget utiliza los siguientes parámetros JavaScript:  
`WIoTPWidget.CreateWIDGET_TYPE("ELEMENT_ID","EVENT_NAME", "DEVICE_TYPE", "DEVICE_ID", "PROPERTY" , {WIDGET_DEFAULT_OVERRIDE}, [WIDGET_SPECIFIC_SETTINGS])`

La siguiente tabla proporciona las descripciones de los parámetros:

| Parámetro | Descripción |    
| ----- | ---- |   
| WIDGET_TYPE | El tipo de widget a crear. Ejemplo: `Gauge` o `Chart` |
| ELEMENT_ID | El ID de elemento del widget, como aparecerá en la aplicación. Ejemplo: `RPM` |
| EVENT_NAME | El nombre de suceso de dispositivo que incluye la propiedad a visualizar. Ejemplo: `sensorData` |
| DEVICE_TYPE | El tipo de dispositivo. Ejemplo: `iot-conveyor-belt` |
| DEVICE_ID | El ID del dispositivo que proporciona los datos a visualizar. Ejemplo: `belt1` |
| PROPERTY | La propiedad de carga útil de mensaje de dispositivo a visualizar. Ejemplo: `rpm` |
| WIDGET_DEFAULT_OVERRIDE | Los valores de configuración del widget para sustituir los valores predeterminados.|
| WIDGET_SPECIFIC_SETTINGS | Uno o varios parámetros adicionales para el widget, vea ejemplos.|

Para obtener detalles acerca de cada tipo de widget, consulte los ejemplos siguientes y la documentación en [Repositorio GitHub de widgets de IoT ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/iot-widgets){: new_page}.
 1. Añada un indicador de RPM.  
Este indicador muestra las RPM de la cinta transportadora como indicador que tiene un mínimo de 0 y un máximo de 5 rpm.
    1. Abra la siguiente plantilla para editarla: `public/index.html`  
    2. Localice el marcador de indicador de rpm: `<!--- place holder for rpm gauge  -->`
    3. Añada el siguiente elemento div con un ID exclusivo como se muestra:
 ```html
 <div id="rpmgauge" ></div>
 ```  
    3. Localice el marcador de JavaScript: `/// Add your scripts here`
    4. Añada el código JavaScript de rpm.  
Ejemplo:  
 ```javascript
 WIoTPWidget.CreateGauge("rpmgauge","sensorData", "iot-conveyor-belt", "belt1", "rpm" ,{
            label: {
                format: function(value, ratio) {
                    return value;
                },
                show: true // to turn off the min/max labels.
            },
         min: 0.0, // 0 is default, can handle negative min e.g. vacuum / voltage / current flow / rate of change
         max: 5.0, // 100 is default
         units: 'rpm'
       },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 2. Añada un indicador de aceleración.  
Este indicador muestra la lectura del acelerómetro como indicador que tiene lecturas entre -1 y 1.
    1. Localice el marcador de indicador del acelerómetro: `<!--- place holder for accelerometer gauge  -->`
    2. Añada el siguiente elemento div con un ID exclusivo como se muestra:
 ```html
 <div id="aygauge" ></div>
 ```  
    3. Localice el marcador de javascript: `/// Add your scripts here`
    4. Añada el código JavaScript del acelerómetro.  
Ejemplo:   
 ```javascript
 WIoTPWidget.CreateGauge("aygauge","sensorData", "iot-conveyor-belt", "belt1", "ay" ,{
      label: {
          format: function(value, ratio) {
              return value;
          },
          show: true // to turn off the min/max labels.
      },
   min: -1.0, // 0 is default,can handle negative min e.g. vacuum / voltage / current flow / rate of change
   max: 1.0, // 100 is default
   units: 'g'//,
 },['#FF0000', '#F97600', '#F6C600', '#60B044']);
 ```
 3. Añada un gráfico de velocidad de motor.  
Este gráfico muestra la velocidad del motor como un diagrama de barras.
    1. Localice el marcador de indicador de velocidad del motor: `<!--- place holder for motor speed gauge  -->`
    2. Añada el siguiente elemento div con un ID exclusivo como se muestra:
 ```html
 <div id="speedchart" ></div>
 ```  
    3. Localice el marcador de JavaScript: `/// Add your scripts here`
    4. Añada el código JavaScript del gráfico de velocidad.  
Ejemplo:  
 ```javascript
 WIoTPWidget.CreateChart("speedchart ","sensorData", "iot-conveyor-belt", "belt1",
 ["rpm", "ay"], [["line","rpm"],["line","ay"]],['#2ca02c','#d62728']);
 ```
4. Despliegue la aplicación en {{site.data.keyword.Bluemix_notm}}.  
 1. Actualice el archivo manifest.yml con su nombre de servicio de {{site.data.keyword.iot_short_notm}}.  
Por ejemplo, si despliega un servicio de {{site.data.keyword.iot_short_notm}} como parte de la [Guía 1: Conectar un dispositivo de cinta transportadora](getting-started-iot-monitoring.html) entonces YOUR_PLATFORM_NAME es `iotp-for-conveyor`.
<pre><code>
declared-services:
  YOUR_IOT_PLATFORM_NAME:  </br>
    label: iotf-service  </br>
    plan: iotf-service-free  </br>
applications:  </br>
\- path: .  </br>
  memory: 128M  </br>
  instances: 1  </br>
  domain: mybluemix.net  </br>
  disk_quota: 1024M  </br>
  services:  </br>
  \- YOUR_IOT_PLATFORM_NAME  </br>
</pre></code>
 2. Inicie la sesión en la cuenta de {{site.data.keyword.Bluemix_notm}} utilizando la CLI de Cloud Foundry.  
Para obtener más información, consulte la [Documentación de la CLI de Cloud Foundry ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://docs.cloudfoundry.org/cf-cli/){: new_window}  
En la línea de mandatos, especifique el mandato siguiente:  
   ```
 cf login
   ```
Si se le solicita, seleccione la organización y el espacio donde desea desplegar la app de muestra de supervisión.
 5. Si es necesario, defina su punto final de API ejecutando el mandato cf api.   
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
 6. Cambie el directorio por el directorio en el que se encuentra la app de muestra.  
   ```
 cd guide-conveyor-ui-html
   ```
 2. Ejecute el mandato cf push para enviar por push la aplicación a {{site.data.keyword.Bluemix_notm}}:  
Otorgue a su aplicación un nombre exclusivo.
```
cf push YOUR_APP_NAME
```
5. Abra la app web basada en biblioteca de widgets.  
En el panel de control de la app de {{site.data.keyword.Bluemix_notm}}, pulse **Visitar URL de app** para abrir la aplicación web.  
Acceda y gestione el URL de aplicación pulsando en **Rutas**.   
El URL predeterminado es similar a:  
`https://YOUR_APP_NAME.mybluemix.net`

## Paso 2: Ver los dispositivos conectados
{: #view_devices}

Ahora que la consola está activa y en ejecución, puede ver sus dispositivos de cinta transportadora conectados.
1. Desde la sección **Dispositivos** de la consola web, verifique que sus cintas transportadoras están enumeradas y que se muestran las RPM y el estado de ejecución correcto.
2. Cambie el valor de RPM de su dispositivo de cinta transportadora y verifique que el valor está actualizado como se espera en la app de supervisión.


## ¿Qué hacer a continuación?
{: @whats_next}  
Continúe con la guía siguiente o salte a otro tema que le interese:
- Vía de acceso A: Modificar la app de supervisión para que se ajuste a sus necesidades.  
Para obtener los detalles técnicos, consulte:
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-conveyor-ui-angular/blob/master/README.md){: new_window}
 - [Bibliotecas de cliente Node.js ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
- Vía de acceso B: Modificar la app de biblioteca de widgets para que se ajuste a sus necesidades.  
Para obtener los detalles técnicos, consulte:
 - [https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-conveyor-ui-html/blob/master/README.md){: new_window}
- [Guía 4: Simulación de un gran número de dispositivos](getting-started-iot-large-scale-simulation.html)  
Expanda la simulación básica añadiendo una mayor cantidad de simuladores automáticos en su entorno. Esta expansión le permitirá realizar pruebas con las analíticas básicas y la supervisión de las guías anteriores en un entorno más realista y multi-dispositivo.
- [Obtenga más información acerca de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html){:new_window}
- [Obtenga más información acerca de las API de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/reference/api.html){:new_window}
