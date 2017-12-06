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

# IoT-Gateway-Paket (Beta)
{: #gw_package}

Das {{site.data.keyword.iot_full}}-IoT-Gateway-Paket ermöglicht einem Gateway, der bei {{site.data.keyword.iot_short_notm}} registriert ist, das Senden von Ereignissen an die Plattform für Geräte, die sich in der Ressourcengruppe befinden, die dem Gateway zugeordnet ist.
{:shortdesc}

**Wichtig:** Das IoT-Gateway-Paket steht nur als Teil eines eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Übersicht

Das IoT-Gateway-Paket umfasst die folgenden Entitäten:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | Paket | org, domain, gatewayTypeId, gatewayId, gatewayToken  | Mit {{site.data.keyword.iot_short_notm}}-Gateway arbeiten |
| `/watson-iot/iot-gateway/publishEvent` | Aktion | org, domain, gatewayTypeId, gatewayId, gatewayToken, eventType, typeId, deviceId, payload | Ereignisse von einem registrierten Gateway im Namen der zugeordneten Geräte an {{site.data.keyword.iot_short_notm}} senden  |

## Bindung für IoT-Gateway-Paket erstellen
Zum Erstellen einer Bindung für ein IoT-Gateway-Paket müssen Sie die folgenden Parameter angeben:

| Parameter |  Beschreibung |
| --- | ---  |
| org | Die Organisations-ID |
| gatewayTypeId | Die ID des Gateway-Typs des registrierten Gateways |
| gatewayId | Die Gateway-ID des registrierten Gateways |
| gatewayToken | Das Berechtigungstoken des registrierten Gateways |


Führen Sie die folgenden Schritte aus, um eine Paketbindung zu erstellen:  
1. [Melden Sie sich bei der Bluemix-Konsole an ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/).
2. Erstellen Sie den [Internet of Things Platform-Service](https://console.bluemix.net/docs/services/IoT/index.html) in Bluemix und [notieren Sie den `API-Schlüssel` und das `API-Token`](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications). Diese Informationen sind erforderlich, um einen Gateway-Typ zu erstellen und ein Gateway zu registrieren.
3. [Erstellen Sie einen Gateway-Typ](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), z. B. *myGWType*, in Ihrer Watson IoT-Organisation und [registrieren Sie eine Instanz des Gateways](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), z. B. *myGWId*. Notieren Sie das *Gateway-Token* des registrierten Gateways.
4. Erstellen Sie eine Paketbindung mit dem IoT-Gateway-Paket, indem Sie den folgenden Beispielbefehl eingeben:
   ```
   wsk package bind /watson/iotgw myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. Überprüfen Sie, ob die Paketbindung vorhanden ist, indem Sie den folgenden Befehl eingeben:  
   ```
   wsk package list
   ```

## Geräteereignisse publizieren

Die Aktion `/watson/iotgw/publishEvent` dient zum Publizieren von Ereignissen eines registrierten {{site.data.keyword.iot_short_notm}}-Gateways im Namen der zugeordneten Geräte. In der folgenden Tabelle werden die Parameter beschrieben, die beim Publizieren von Ereignissen verwendet werden:  

Parameter |  Beschreibung | Beispiel
------------- | ------------- | -------------
org | Die ID der Organisation, zu der das registrierte Gateway gehört. | `-p org "uguhsp"`
domain | Optional. Die Domäne, zu der das registrierte Gateway gehört. Standardmäßig wird auf `messaging.internetofthings.ibmcloud.com` verwiesen. | `-p domain "messaging.internetofthings.ibmcloud.com"`
gatewayTypeId | Die ID des Gateway-Typs des registrierten Gateways. | `-p gatewayTypeId "myGatewayType"`
gatewayId | Die Gateway-ID des registrierten Gateways. Die ID muss innerhalb einer Organisation für einen bestimmten Gateway-Typ eindeutig sein. | `-p gatewayId "00aabbccde03"`
gatewayToken | Das Token (Kennwort), das vom registrierten Gateway zur Herstellung der Verbindung zu Watson IoT Platform verwendet wird.  | `-p gatewayToken "ZZZ"`
eventType | Der Ereignistyp, für den das registrierte Gateway Ereignisse im Namen seiner zugeordneten Geräte publiziert. | `-p eventType "evt"`
typeId | Der Gerätetyp des Geräts, das dem registrierten Gateway zugeordnet ist. Das registrierte Gateway publiziert Ereignisse im Namen des zugeordneten Geräts. | `-p typeId "myDeviceType"`
deviceId | Die Geräte-ID des Geräts, das dem registrierten Gateway zugeordnet ist. Das registrierte Gateway publiziert Ereignisse im Namen des zugeordneten Geräts. Die Geräte-ID muss innerhalb einer Organisation für einen bestimmten Gateway-Typ eindeutig sein. | `-p deviceId "00aabbccde03_0001"`
payload | Die Nutzdaten, die das registrierte Gateway im Namen des Geräts publiziert. | `-p payload "{'d':{'temp':38}}"`


Das folgende Beispiel zeigt, wie Ereignisse des Pakets *iotgw* publiziert werden:

Publizieren Sie ein Geräteereignis mithilfe der Aktion *publishEvent* in der Paketbindung, die Sie erstellt haben. Sie müssen `/myNamespace/myGateway` durch den Paketnamen ersetzen.

 ```
  wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## Mit dem Repository arbeiten

Führen Sie die folgenden Schritte aus, um das Paket mithilfe von `installCatalog.sh` bereitzustellen:
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

Hierbei steht *AUTH* für den Autorisierungsschlüssel, *APIHOST* für den OpenWhisk-Hostnamen und *WSK_CLI* für die Position der Openwhisk-CLI-Binärdatei.
