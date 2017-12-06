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

# Package de passerelle IoT (bêta)
{: #gw_package}

Le package de passerelle {{site.data.keyword.iot_full}} IoT permet à une passerelle qui est enregistrée auprès de {{site.data.keyword.iot_short_notm}} d'envoyer des événements à la plateforme pour des terminaux qui se trouvent dans le groupe de ressources associé à la passerelle.
{:shortdesc}

**Important :** Le package de passerelle IoT est disponible uniquement dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Présentation

Le package de passerelle IoT inclut les entités suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | package | org, domain, gatewayTypeId, gatewayId, gatewayToken  | Fonctionne avec {{site.data.keyword.iot_short_notm}} Gateway |
| `/watson-iot/iot-gateway/publishEvent` | action | org, domain, gatewayTypeId, gatewayId, gatewayToken, eventType, typeId, deviceId, payload | Envoie des événements depuis une passerelle enregistrée pour le compte de ses terminaux associés à {{site.data.keyword.iot_short_notm}}   |

## Création d'une liaison de package de passerelle IoT 
Pour créer une liaison de package de passerelle IoT, spécifiez les paramètres suivants :

| Paramètre |  Description |
| --- | ---  |
| org | Identificateur de l'organisation |
| gatewayTypeId | Identificateur du type de passerelle enregistrée |
| gatewayId | Identificateur de la passerelle enregistrée |
| gatewayToken | Jeton d'autorisation de la passerelle enregistrée |


Pour créer une liaison de package, procédez comme suit :  
1. [Connectez-vous à la console Bluemix ![External link icon](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/).
2. Créez le [service Internet of Things Platform](https://console.bluemix.net/docs/services/IoT/index.html) dans Bluemix et [notez la `clé d'API` et le `jeton d'API`](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications). Ces informations sont requises pour créer un type de passerelle et enregistrer une passerelle.
3. [Créez un type de passerelle](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), par exemple, *myGWType* dans votre organisation Watson IoT et[enregistrez une instance de la passerelle](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), par exemple, *myGWId*. Notez le *jeton de passerelle* de la passerelle enregistrée.
4. Créez une liaison de package avec le package de passerelle IoT en utilisant l'exemple de commande suivant : 
   ```
   wsk package bind /watson/iotgw myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. Vérifiez que la liaison de package existe en utilisant la commande suivante :  
   ```
   wsk package list
   ```

## Publication d'événements de terminaux

L'action `/watson/iotgw/publishEvent` publie des événements depuis une passerelle {{site.data.keyword.iot_short_notm}} enregistrée, pour le compte des terminaux qui lui sont associés. Le tableau ci-dessous décrit les paramètres qui sont utilisés lors de la publication d'événements :  

Paramètre |  Description | Exemple
------------- | ------------- | -------------
org | Identificateur de l'organisation à laquelle appartient la passerelle enregistrée.  | `-p org "uguhsp"`
domain | Facultatif. Domaine auquel appartient la passerelle enregistrée. La valeur par défaut est `messaging.internetofthings.ibmcloud.com` | `-p domain "messaging.internetofthings.ibmcloud.com"`
gatewayTypeId | Identificateur du type de passerelle enregistrée. | `-p gatewayTypeId "myGatewayType"`
gatewayId | Identificateur de la passerelle enregistrée. L'identificateur doit être unique au sein d'une organisation pour un certain type de passerelle. | `-p gatewayId "00aabbccde03"`
gatewayToken | Jeton (mot de passe) utilisé par la passerelle enregistrée pour se connecter à Watson IoT Platform.  | `-p gatewayToken "ZZZ"`
eventType | Type d'événement pour lequel la passerelle enregistrée publie des événements pour le compte des terminaux qui lui sont associés. | `-p eventType "evt"`
typeId | Type de terminal du terminal associé à la passerelle enregistrée. La passerelle enregistrée publie des événements pour le compte du terminal qui lui est associé. | `-p typeId "myDeviceType"`
deviceId | Identificateur du terminal associé à la passerelle enregistrée. La passerelle enregistrée publie des événements pour le compte du terminal qui lui est associé. L'identificateur de terminal doit être unique au sein d'une organisation pour un certain type de passerelle. | `-p deviceId "00aabbccde03_0001"`
payload | Contenu publié par la passerelle enregistrée pour le compte du terminal. | `-p payload "{'d':{'temp':38}}"`


L'exemple ci-dessous montre comment publier des événements à partir du package *iotgw* :

Publiez un événement de terminal à l'aide de l'action *publishEvent* dans la liaison de package que vous avez créée précédemment. Prenez soin de remplacer `/myNamespace/myGateway` par votre nom de package.

 ```
  wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## Utilisation du référentiel

Effectuez les étapes suivantes pour déployer le package en utilisant `installCatalog.sh`
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

où *AUTH* désigne votre clé d'autorisation, *APIHOST* le nom d'hôte OpenWhisk et *WSK_CLI* l'emplacement de l'interface de ligne de commande binaire Openwhisk.
