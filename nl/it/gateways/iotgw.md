---

copyright:
years: 2016, 2018
lastupdated: "2018-07-11"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Pacchetto gateway IoT 
{: #gw_package}

Il pacchetto gateway IoT {{site.data.keyword.iot_full}} abilita un gateway registrato con {{site.data.keyword.iot_short_notm}} ad inviare gli eventi alla piattaforma per i dispositivi nel gruppo di risorse associato al gateway. 
{:shortdesc}


## Panoramica

Il pacchetto gateway IoT include le seguenti entità:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | pacchetto | org, domain, gatewayTypeId, gatewayId, gatewayToken  | Utilizza con gateway {{site.data.keyword.iot_short_notm}} |
| `/watson-iot/iot-gateway/publishEvent` | azione | org, domain, gatewayTypeId, gatewayId, gatewayToken, eventType, typeId, deviceId, payload | Invia gli eventi da un gateway registrato al posto dei relativi dispositivi associati a {{site.data.keyword.iot_short_notm}}   |

## Creazione di un bind al pacchetto gateway IoT
Per creare un bind al pacchetto gateway IoT, devi specificare i seguenti parametri:

| Parametro |  Descrizione |
| --- | ---  |
| org | L'identificativo dell'organizzazione |
| gatewayTypeId | L'identificativo del tipo di gateway del gateway registrato |
| gatewayId | L'identificativo del gateway del gateway registrato |
| gatewayToken | Il token di autorizzazione del gateway registrato |


Completa la seguente procedura per creare un bind al pacchetto:  
1. [Accedi alla console IBM Cloud ![Icona link esterno](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/).
2. Crea il [Servizio della piattaforma Internet of Things](https://console.bluemix.net/docs/services/IoT/index.html) in IBM Cloud e [prendi nota del valori `API Key` e `API Token`](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications). Queste informazioni sono necessarie per creare un tipo di gateway e per registrarlo.
3. [Crea un tipo di gateway](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), ad esempio, *myGWType* nella tua organizzazione Watson IoT e [registra un'istanza del gateway](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), ad esempio, *myGWId*. Prendi nota del *Token gateway* del gateway registrato.
4. Crea un bind al pacchetto con un pacchetto gateway IoT utilizzando il seguente comando di esempio:
   ```
   bx wsk package bind /watson-iot/iot-gateway myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. Verifica che il bind al pacchetto esista utilizzando il seguente comando:  
   ```
   bx wsk package list
   ```

## Pubblicazione eventi dispositivo

L'azione `/watson-iot/iot-gateway/publishEvent` pubblica gli eventi da un gateway {{site.data.keyword.iot_short_notm}} registrato, per conto dei suoi dispositivi associati. La seguente tabella illustra i parametri utilizzati durante la pubblicazione degli eventi:  

Parametro |  Descrizione | Esempio
------------- | ------------- | -------------
org | L'identificativo dell'organizzazione a cui appartiene il gateway registrato.  | `-p org "uguhsp"`
domain | Facoltativo. Il dominio a cui appartiene il gateway registrato. Il valore predefinito punta a `messaging.internetofthings.ibmcloud.com` | `-p domain "messaging.internetofthings.ibmcloud.com"`
gatewayTypeId | L'identificativo del tipo di gateway del gateway registrato. | `-p gatewayTypeId "myGatewayType"`
gatewayId | L'identificativo del gateway del gateway registrato. L'identificativo deve essere all'interno di un'organizzazione di un tipo di gateway selezionato. | `-p gatewayId "00aabbccde03"`
gatewayToken | Il token (password) utilizzato dal gateway registrato per il collegamento a Watson IoT Platform.  | `-p gatewayToken "ZZZ"`
eventType | Il tipo di evento in cui il gateway registrato pubblica gli eventi al posto dei relativi dispositivi associati. | `-p eventType "evt"`
typeId | Il tipo di dispositivo del dispositivo associato al gateway registrato. Il gateway registrato pubblica gli eventi al posto del dispositivo associato. | `-p typeId "myDeviceType"`
deviceId | L'identificativo del dispositivo del dispositivo associato al gateway registrato. Il gateway registrato pubblica gli eventi al posto del dispositivo associato. L'identificativo del dispositivo deve essere all'interno di un'organizzazione di un tipo di gateway selezionato. | `-p deviceId "00aabbccde03_0001"`
payload | Il payload che il gateway registrato pubblica al posto del dispositivo. | `-p payload "{'d':{'temp':38}}"`


Il seguente esempio mostra come pubblicare gli eventi dal pacchetto *iotgw*:

Pubblica un evento utilizzando l'azione *publishEvent* nel bind al pacchetto che hai creato. Devi sostituire `/myNamespace/myGateway` con il tuo nome del pacchetto.

 ``` 
 bx wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## Utilizzo del repository

Completa la seguente procedura per distribuire il pacchetto utilizzando `installCatalog.sh`
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

dove *AUTH* è la tua chiave di autorizzazione, *APIHOST* è il nome host di IBM Cloud Functions e *WSK_CLI* è l'ubicazione del binario della CLI IBM Cloud Functions.
