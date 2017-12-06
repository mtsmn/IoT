---

copyright:
years: 2016, 2017
lastupdated: "2017-10-04"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Paquete de pasarela de IoT (Beta)
{: #gw_package}

El paquete de pasarela de IoT de {{site.data.keyword.iot_full}} habilita una pasarela registrada con {{site.data.keyword.iot_short_notm}} para enviar sucesos a la plataforma para los dispositivos que están en el grupo de recursos asociado con la pasarela.
{:shortdesc}

**Importante:** El paquete de pasarela de IoT solo está disponible como parte de un programa de beta limitado. Las actualizaciones futuras pueden incluir cambios que no son compatibles con la versión actual de esta característica. Pruébela y [denos su opinión ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Visión general

El paquete de IoT incluye las siguientes entidades:

| Entidad| Tipo | Parámetros| Descripción |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | Paquete| org, domain, gatewayTypeId, gatewayId, gatewayToken  | Trabaja con la pasarela de {{site.data.keyword.iot_short_notm}}|
| `/watson-iot/iot-gateway/publishEvent` | Acción| org, domain, gatewayTypeId, gatewayId, gatewayToken, eventType, typeId, deviceId, payload | Envía sucesos desde una pasarela registrada en nombre de sus dispositivos asociados a {{site.data.keyword.iot_short_notm}}   |

## Creación de un enlace de paquete de pasarela de IoT
Para crear un enlace de paquete de pasarela de IoT, debe especificar los siguientes parámetros:

| Parámetro |  Descripción |
| --- | ---  |
| org | El identificador de la organización|
| gatewayTypeId | El identificador de tipo de pasarela de la pasarela registrada|
| gatewayId | El identificador de pasarela de la pasarela registrada|
| gatewayToken | La señal de autorización de la pasarela registrada|


Complete los siguientes pasos para crear un enlace de paquete:  
1. [Inicie sesión en la consola de Bluemix ![Icono de enlace externo](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/).
2. Cree el [Servicio de la Plataforma Internet de las cosas](https://console.bluemix.net/docs/services/IoT/index.html) en Bluemix y [anote la `API Key` y la `API Token`](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications). Esta información es necesaria para crear un tipo de pasarela y para registrar una pasarela.
3. [Cree un tipo de pasarela](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), por ejemplo, *myGWType* en su organización de IoT de Watson y [registre una instancia de la pasarela](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), por ejemplo, *myGWId*. Tome nota de la *Señal de pasarela* para la pasarela registrada.
4. Cree un enlace de paquete con el paquete de pasarela de IoT utilizando el mandato de ejemplo siguiente:
   ```
   wsk package bind /watson/iotgw myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. Verifique que el enlace de paquete existe utilizando el siguiente mandato:  
   ```
   wsk package list
   ```

## Publicación de sucesos de dispositivos

La acción `/watson/iotgw/publishEvent` publica sucesos desde una pasarela de {{site.data.keyword.iot_short_notm}} registrada, en nombre de sus dispositivos asociados. La siguiente tabla describe los parámetros utilizados al publicar sucesos:  

Parámetro |  Descripción | Ejemplo
------------- | ------------- | -------------
org | El identificador de la organización a la que pertenece la pasarela registrada.| `-p org "uguhsp"`
domain | Opcional. El dominio al que pertenece la pasarela registrada. El valor predeterminado apunta a `messaging.internetofthings.ibmcloud.com` | `-p domain "messaging.internetofthings.ibmcloud.com"`
gatewayTypeId | El identificador de tipo de pasarela de la pasarela registrada. | `-p gatewayTypeId "myGatewayType"`
gatewayId | El identificador de pasarela de la pasarela registrada. El identificador debe ser exclusivo en una organización para un tipo de pasarela dado. | `-p gatewayId "00aabbccde03"`
gatewayToken | La señal (contraseña) utilizada por la pasarela registrada para conectarse a Watson IoT Platform. | `-p gatewayToken "ZZZ"`
eventType | El tipo de suceso en el que la pasarela registrada publica sucesos en nombre de sus dispositivos asociados. | `-p eventType "evt"`
typeId | El tipo de dispositivo del dispositivo asociado con la pasarela registrada. La pasarela registrada publica sucesos en nombre del dispositivo asociado. | `-p typeId "myDeviceType"`
deviceId | El identificador de dispositivo del dispositivo asociado con la pasarela registrada. La pasarela registrada publica sucesos en nombre del dispositivo asociado. El identificador de dispositivo debe ser exclusivo en una organización para un tipo de pasarela dado. | `-p deviceId "00aabbccde03_0001"`
payload | La carga útil que publica la pasarela registrada en nombre del dispositivo.| `-p payload "{'d':{'temp':38}}"`


El siguiente ejemplo muestra cómo publicar sucesos desde el paquete *iotgw*:

Publique un suceso de dispositivo utilizando la acción *publishEvent* en el enlace de paquete que ha creado. Debe reemplazar `/myNamespace/myGateway` con el nombre de su paquete.

 ```
  wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## Cómo trabajar con el repositorio

Complete los siguientes pasos para desplegar el paquete utilizando `installCatalog.sh`
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

Donde *AUTH* es su clave de autorización, *APIHOST* es el nombre de host de OpenWhisk y *WSK_CLI* es la ubicación del binario de la CLI de Openwhisk.
