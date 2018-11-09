---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Guía 1: Conectar un dispositivo de cinta transportadora  
Crear una cinta transportadora básica con un dispositivo IoT que envíe datos de supervisión a {{site.data.keyword.iot_full}} en {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Visión general y objetivo
{: #overview}  
Esta guía es la primera de una serie que le guiará, paso a paso, a través del proceso de conexión de dispositivos a {{site.data.keyword.iot_short_notm}}, supervisión y actuación en datos de dispositivo y más. Cada guía se basa en la guía anterior y el código de ejemplo de cada guía sirve como entrada para la siguiente guía. También puede utilizar estas guías de forma autónoma modificando el código de ejemplo para que se ajuste a su propósito.

En esta primera guía, configuramos una cinta transportadora conectada y la utilizamos para enviar datos de IoT a {{site.data.keyword.iot_short_notm}}.
Dependiendo de su nivel de habilidad, puede seguir una o ambas de las siguientes vías de acceso para configurar su cinta transportadora:
- Vía de acceso A  
Esta vía de acceso le permite iniciar rápidamente instalando una app de simulador de cinta transportadora en {{site.data.keyword.Bluemix_notm}}. La app registra por sí sola un dispositivo con {{site.data.keyword.iot_short_notm}} y envía automáticamente datos bien formateados a la plataforma. Las instrucciones para esta vía de acceso están en el [Paso 2A: Uso de la app de muestra de simulador de GitHub](#deploy_app).  
- Vía de acceso B  
Esta vía de acceso es más exigente técnicamente hablando y requiere hardware adicional, habilidades de programación en Python y registro manual de su dispositivo en {{site.data.keyword.iot_short_notm}}. Las instrucciones para esta vía de acceso están en el [Paso 2B: Construcción de una cinta transportadora física con una Raspberry Pi y un motor eléctrico](#raspberry).

Como parte de esta guía aprenderá a:
- Crear y desplegar una organización de {{site.data.keyword.iot_short_notm}} utilizando la CLI de Cloud Foundry.
- Construir y desplegar un dispositivo de cinta transportadora de muestra.
- Conectar el dispositivo de cinta transportadora simulado a {{site.data.keyword.iot_short_notm}}.

Para empezar con {{site.data.keyword.iot_short_notm}} utilizando un dispositivo IoT diferente, consulte [Guía de aprendizaje de iniciación](/docs/services/IoT/getting-started.html).
{: tip}

## Requisitos previos
{: #prereqs}

Necesitará las siguientes cuentas y herramientas:
* [Cuenta de {{site.data.keyword.Bluemix_notm}} ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://console.ng.bluemix.net/registration/){: new_window}
* [Interfaz de línea de mandatos de Cloud Foundry (cf CLI) ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}  
Utilice la CLI de cf para desplegar apps y servicios en {{site.data.keyword.Bluemix_notm}}. Para obtener más información, consulte la [Documentación de la CLI de Cloud Foundry![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://docs.cloudfoundry.org/cf-cli/){: new_window}
* Opcional: [Git ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://git-scm.com/downloads){: new_window}  
Si elige utilizar Git para descargar los ejemplos de código también debe tener una cuenta de [GitHub.com ![icono de enlace externo](../../../icons/launch-glyph.svg "icono de enlace externo")](https://github.com){: new_window}. También puede descargar el código como archivo comprimido sin una cuenta de GitHub.com.
* Opcional: Un teléfono móvil en el que ejecutar la aplicación web de muestra de *Cinta transportadora* para enviar los datos del acelerómetro.

## Paso 1: Desplegar {{site.data.keyword.iot_short_notm}}  
{: #deploy_watson_iot_platform_service}
Los pasos siguientes desplegarán una instancia del servicio {{site.data.keyword.iot_short_notm}} con el nombre `iotp-for-conveyor` en su entorno de {{site.data.keyword.Bluemix_notm}}. Si ya tiene una instancia de servicio en ejecución, puede utilizarla con la guía y saltarse este primer paso. Asegúrese de que está utilizando el nombre de servicio y espacio de {{site.data.keyword.Bluemix_notm}} correcto cuando continúe con las guías.
{: tip}
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
3. Despliegue el servicio de {{site.data.keyword.iot_short_notm}} en {{site.data.keyword.Bluemix_notm}}.  
   ```
cf create-service iotf-service iotf-service-free YOUR_IOT_PLATFORM_NAME
   ```
Para YOUR_IOT_PLATFORM_NAME, utilice *iotp-for-conveyor*.  
Ejemplo: `cf create-service iotf-service iotf-service-free iotp-for-conveyor`
3. Cree su dispositivo de cinta transportadora de ejemplo.  
 - Vía de acceso A: [Paso 2A - Utilizar la app de ejemplo de simulador de GitHub](#deploy_app).  
 - Vía de acceso B: [Paso 2B - Construir una cinta transportadora física con un Raspberry Pi y un motor eléctrico](#raspberry).  

## Paso 2A - Desplegar la aplicación web de cinta transportadora de ejemplo  
{: #deploy_app}

La app de ejemplo le permite simular una cinta transportadora industrial conectada de {{site.data.keyword.Bluemix_notm}}. Puede iniciar y detener la cinta y ayustar la velocidad. Cada cambio que realice en la cinta se envía a {{site.data.keyword.Bluemix_notm}} en forma de mensaje de MQTT que se muestra en la app. Puede supervisar el comportamiento de la cinta utilizando las tarjetas predeterminadas del panel de control.
La app de muestra se compila utilizando las bibliotecas de cliente de Node.js en: [https://github.com/ibm-watson-iot/iot-nodejs ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}

![App de cinta transportadora](images/app_conveyor_belt.png "App de cinta transportadora")

1. Utilice su herramienta Git favorita para clonar el siguiente repositorio:  
https://github.com/ibm-watson-iot/guide-conveyor-simulator
En Git shell, utilice el siguiente mandato:
  ```bash
git clone https://github.com/ibm-watson-iot/guide-conveyor-simulator
  ```
2. En la línea de mandatos, cambie el directorio por el directorio en el que se encuentra la app de ejemplo.
  ```bash
cd guide-conveyor-simulator
  ```
3. Desde el directorio *guide-conveyor-simulator*, envíe por push su app a {{site.data.keyword.Bluemix_notm}} y póngale un nombre nuevo reemplazando *YOUR_APP_NAME* en el mandato cf push. Utilice la opción `--no-start` puesto que iniciará la app en la siguiente etapa después de limitarla a {{site.data.keyword.iot_short_notm}}.
**Nota:** El despliegue de la aplicación puede tardar varios minutos.
   ```bash
cf push YOUR_APP_NAME --no-start
  ```  
1. En el directorio *guide-conveyor-simulator*, enlace su app a su instancia de {{site.data.keyword.iot_short_notm}} utilizando los nombres que ha proporcionado.
  ```bash
cf bind-service YOUR_APP_NAME iotp-for-conveyor
  ```
Para obtener más información acerca de enlazar aplicaciones, consulte [Conexión de aplicaciones](/docs/services/IoT/platform_authorization.html#bluemix-binding).
2. Inicie su aplicación para que el enlace surta efecto.
  ```bash
cf start YOUR_APP_NAME
  ```
En esta etapa, su aplicación web de muestra se despliega en {{site.data.keyword.Bluemix_notm}}.
Cuando el despliegue finaliza, se muestra un mensaje para indicar que su app está en ejecución.   
Ejemplo:
  ```bash
name:              YOUR_APP_NAME
requested state:   started
instances:         1/1
usage:             64M x 1 instances
routes:            YOUR_APP_NAM.mybluemix.net
last uploaded:     Tue 09 May 09:29:49 EDT 2017
stack:             cflinuxfs2
buildpack:         SDK for Node.js(TM) (ibm-node.js-4.8.0,
                   buildpack-v3.11-20170303-1144)
start command:     ./vendor/initial_startup.rb

     state     since                  cpu    memory         disk            details
#0   running   2017-05-09T13:35:08Z   0.0%   19.6M of 64M   66.2M of 256M
  ```
Para ver tanto el estado del despliegue de la app como el URL de la app, puede ejecutar el siguiente mandato:
  ```bash
cf apps
  ```
Resolver errores en el proceso de despliegue utilizando el mandato `cf logs YOUR_APP_NAME --recent`.
{: tip}
1. En un navegador, acceda a la app.  
Abra el siguiente URL: `https://YOUR_APP_NAME.mybluemix.net`    
Ejemplo: `https://conveyorbelt.mybluemix.net/`.
2. Introduzca un ID de dispositivo y una señal para su dispositivo.  
La app de muestra registra automáticamente un dispositivo de tipo `iot-conveyor-belt` con el ID de dispositivo y señal que ha proporcionado. Para obtener más información acerca del registro de dispositivos, consulte [Conexión de dispositivos](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
4. Continúe con [Paso 3: Ver datos en bruto en {{site.data.keyword.iot_short_notm}}](#see_live_data).

## Paso 2B: Construir una cinta transportadora basada en una Raspberry Pi
{: #raspberry}

La solución de la Raspberry Pi es construir utilizando las bibliotecas de cliente de Python en: [https://github.com/ibm-watson-iot/iot-python ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/iot-python){: new_window}

### Diagrama esquemático del circuito

![Diagrama del circuito](images/raspi_circuit.png)

### Conexiones necesarias

Las siguientes conexiones entre la Raspberry Pi y el módulo de controlador de motor Dual H Bridge L298N son necesarias. Conecte:

- Pin 2 de la Raspberry Pi en +5v del módulo L298N
- Pin 6 de la Raspberry Pi en GND del módulo L298N
- Pin 13 de la Raspberry Pi en IN1 del módulo L298N
- Pin 15 de la Raspberry Pi en IN2 del módulo L298N
- +9v de la batería en +12v del módulo L298N
- -9v de la batería en GND del módulo L298N
- El nodo +ve del motor en OUT1 del módulo L298N
- El nodo -ve del motor en OUT2 del módulo L298N

### Requisitos de hardware

1. [Raspberry Pi(2/3) ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.raspberrypi.org/){: new_window} con Raspbian Jessie (8.x).
2. [Módulo de controlador de motor Dual H Bridge L298N ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://tronixlabs.com.au/robotics/motor-controllers/l298n-dual-motor-controller-module-2a-australia/){: new_window}
3. Motor DC de 9V
4. Batería de 9V

### Requisitos de software para Raspberry Pi
- Python 2.7.9 y superior, instalado con Raspbian Jessie (8.x).
- [Biblioteca Python de Watson IoT ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/iot-python){: new_window}
- Opcional: Git.  
Si no tiene git instalado en su Raspberry Pi, puede instalarlo utilizando el siguiente mandato:  
```bash
$ sudo apt-get install git
```

### Pasos detallados

1. Abra el terminal o SSH en su Raspberry Pi.
2. Utilice su herramienta Git favorita para clonar el siguiente repositorio:
https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
En Git Shell, utilice el siguiente mandato:
```bash
$ git clone https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi
```
3. Registre el dispositivo con {{site.data.keyword.iot_short_notm}}.  
Para obtener más información acerca del registro de dispositivos, consulte [Conexión de dispositivos](/docs/services/IoT/iotplatform_task.html#iotplatform_subtask1).
	 1. En la consola de {{site.data.keyword.Bluemix_notm}}, pulse en **Iniciar** en la página de detalles del servicio de {{site.data.keyword.iot_short_notm}}.
	     Se abre la consola web de {{site.data.keyword.iot_short_notm}} en un nuevo separador del navegador con el siguiente URL:
	     ```
	     https://ORG_ID.internetofthings.ibmcloud.com/dashboard/#/overview
	     ```
	     Donde ORG_ID es el ID de seis caracteres exclusivo de [su organización de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html#organizations){: new_window}.
	 2. En el panel de instrumentos Visión general, desde el panel del menú, seleccione **Dispositivos** y, a continuación, pulse **Añadir dispositivo**.
	 3. Cree un tipo de dispositivo para el dispositivo que está añadiendo.
	     1. Pulse **Crear tipo de dispositivo**.
	     2. Especifique el nombre de tipo de dispositivo `iotp-for-conveyor` y una descripción para el tipo de dispositivo.  
	**Sugerencia:** Puede introducir cualquier nombre de tipo de dispositivo, pero las otras guías de la serie esperan dispositivos del tipo `iotp-for-conveyor`. Si utiliza un tipo de dispositivo diferente, debe modificar los valores de las otras guías según convenga.
	     3. Opcional: Especifique los atributos y metadatos de tipo de dispositivo.
	 4. Pulse **Siguiente** para empezar el proceso de adición de su dispositivo con el tipo de dispositivo seleccionado.
	 5. Especifique un ID de dispositivo, por ejemplo, `conveyor_belt`.
	 5. Pulse **Siguiente** para completar el proceso.
	 6. Proporcione una señal de autenticación o acepte un token generado automáticamente.
	 7. Verifique que la información de resumen sea correcta y, a continuación, pulse **Añadir** para añadir la conexión.
	 8. En la página de información de dispositivos, copie y guarde los siguientes detalles:
	     * ID de organización
	     * Tipo de dispositivo
	     * ID de dispositivo
	     * Método de autenticación
	     * Señal de autenticación
	     Necesitará los valores para el ID de organización, el Tipo de dispositivo, el ID de dispositivo y la Señal de autenticación para configurar su dispositivo para conectarse a {{site.data.keyword.iot_short_notm}}.
4. Navegue a la raíz de *guide-conveyor-rasp-pi* del repositorio clonado.
5. Haga que el script de shell sea ejecutable utilizando este mandato `sudo chmod +x setup.sh`
6. Ejecute el archivo *setup.sh* y especifique los detalles que ha copiado de la página de información de dispositivo cuando se le solicite.
```bash  
./setup.sh
```  

7. Ejecute el programa deviceClient.  
Cuando ejecuta el programa, su Raspberry Pi enciende el motor y estará en ejecución durante un máximo de 2 minutos en función de los valores de parámetro.
```bash
python deviceClient.py -t 2
```

Mientras el motor se está ejecutando, el programa publica sucesos de tipo de suceso `sensorData` que tienen la siguiente estructura de carga útil de ejemplo:  
```
{
"d": {
  "elapsed": 1,
  "running": true,
  "temperature": 35.00,
  "ay": "0.00",
  "rpm": "0.6"
}
}
```

8. Continúe con el [Paso 3: Ver los datos en bruto en {{site.data.keyword.iot_short_notm}}](#see_live_data).

## Paso 3: Ver los datos en bruto en {{site.data.keyword.iot_short_notm}}
{: #see_live_data}
1. Verifique que el dispositivo está registrado con {{site.data.keyword.iot_short_notm}}.
 1. Inicie sesión en su panel de control de {{site.data.keyword.Bluemix_notm}} en: https://bluemix.net
 2. En la [lista de servicios ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://bluemix.net/dashboard/services){: new_window}, pulse el servicio *iotp-for-conveyor* {{site.data.keyword.iot_short_notm}}.
 3. Pulse *Iniciar* para abrir el panel de control de {{site.data.keyword.iot_short_notm}} en un nuevo separador del navegador.  
Puede marcar el URL para facilitar el acceso posterior.   
Ejemplo: `https://*iot-org-id*.internetofthings.ibmcloud.com`.
 4. En el menú, seleccione **Dispositivos** y verifique que se visualiza el nuevo dispositivo.
2. Enviar datos de sensor a la plataforma.   
El dispositivo envía datos a {{site.data.keyword.iot_short_notm}} cuando las lecturas del sensor cambian. Puede simular este envío de datos deteniendo, iniciando o cambiando la velocidad de la cinta transportadora.   
**Vía de acceso A:** Si está accediendo a la app en un navegador móvil, intente agitar el teléfono inteligente para activar los datos del acelerómetro para la cinta transportadora.
{: tip}

## Qué hacer a continuación
{: @whats_next}  
Continúe con la guía siguiente o salte a otro tema que le interese:
- Vía de acceso A: Modifique la app de la cinta transportadora para que se adapte a sus necesidades.  
Para obtener los detalles técnicos, consulte:
 - [https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-conveyor-simulator/blob/master/README.md){: new_window}
 - [Bibliotecas de clienmt de Node.js ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
 
 **Nota:** Bluemix es ahora IBM Cloud. Consulte el [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo") para obtener más detalles.
 
- Vía de acceso B: Modificar la configuración de la Raspberry Pi para que se ajuste a sus necesidades.  
Para obtener los detalles técnicos, consulte:
 - [https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-watson-iot/guide-conveyor-rasp-pi/blob/master/README.md){: new_window}


**Nota:** Bluemix es ahora IBM Cloud. Consulte el [IBM Cloud Blog](https://www.ibm.com/blogs/bluemix/2017/10/bluemix-is-now-ibm-cloud/){: new_window} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo") para obtener más detalles.

- [Guía 2: Supervisión de los datos de dispositivo](getting-started-iot-monitoring.html)  
Ahora que ha conectado uno o varios dispositivos y ha empezado a realizar un buen uso de los datos de dispositivo, es hora de empezar a supervisar una colección de dispositivos.
- [Guía 3: Simulación de un gran número de dispositivos](getting-started-iot-large-scale-simulation.html)  
La app de muestra de cinta transportadora en la vía de acceso A le permite simular de forma manual uno o varios dispositivos de cinta transportadora. Esta guía le permite configurar un entorno simulado que tiene un gran número de dispositivos.
- [Obtenga más información acerca de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/iotplatform_overview.html)
- [Obtenga más información acerca de las API de {{site.data.keyword.iot_short_notm}}](/docs/services/IoT/reference/api.html)
