---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gateway-Zugriffssteuerung
{: #gateway-access-control}

Gateway-Geräte stellen eine spezialisierte Klasse von Geräten dar, die im Namen anderer Geräte handeln können. Gateway-Ressourcengruppen definieren, im Namen welcher Geräte innerhalb einer Organisation jedes Gateway handeln kann. Gateway-Rollen bestimmen die Fähigkeit des Gateways, Geräte für die Watson IoT Platform zu registrieren.
{: #shortdesc}

Sie verwenden Ressourcengruppen, um Gateways zu aktivieren, die im Namen einer Untergruppe der Geräte einer Organisation handeln. Gateways können nur Nachrichten für sich selbst und im Namen der Geräte, die sich in ihrer Ressourcengruppe befinden, publizieren oder subskribieren. 

Wenn ein neues Gateway erstellt wird, wird auch eine Standardressourcengruppe erstellt, die dem Gateway zugeordnet wird. Die Standardressourcengruppe kann nicht aus dem Gateway entfernt werden. Vorhandene Gateways ohne Ressourcengruppen können weiterhin im Namen aller Geräte in der Organisation handeln.

Informationen zum Publizieren von Ereignissen von Gateway-Geräten mithilfe von APIs finden Sie in [HTTP-APIs für Gateway-Geräte](gw_api.html).

## Rolle einem Gateway zuordnen
{: #gw_roles}

Um ein Gateway einer Ressourcengruppe zuordnen zu können, müssen Sie das Gateway zuerst einer Rolle zuweisen. Neu erstellten Gateways wird standardmäßig die Rolle *Privilegiertes Gateway* zugewiesen und sie werden der Standardressourcengruppe zugeordnet. Gateways mit der privilegierten Rolle können Geräte automatisch registrieren, die zur Standardressourcengruppe hinzugefügt werden.

Gateways mit der Rolle *Standard-Gateway* können Geräte nicht automatisch registrieren. 

Ein Gateway, dem eine Ressourcengruppe zugeordnet wurde, kann nur im Namen der Geräte in dieser Ressourcengruppe und im eigenen Namen handeln, selbst wenn die Rolle des Gateways geändert wird.

Weitere Informationen zu Gateway-Rollen finden Sie unter [Gateway-Rollen](../roles_index.html#gateway_roles).

Um einem Gateway eine Rolle zuzuordnen, verwenden Sie die folgende API. Hierbei steht *${clientID}* für die URL-codierte Client-ID im Format *d:${orgId}:${typeId}:${deviceId}* für Geräte oder *g:${orgId}:${typeId}:${deviceId}* für Gateways:

```
PUT /authorization/devices/${clientID}/roles

Request Body:
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ]
}

Request Response: 200
{
    "roles": [
        {
            "roleId": "PD_STANDARD_GW_DEVICE",
            "roleStatus": 1
        }
    ],
    "rolesToGroups": {
        "PD_STANDARD_GW_DEVICE": ["gw_def_res_grp:abcdef:gatewayTypeId:gatewayDeviceId"]
    }
}
```

Details zu dem Anforderungsschema finden Sie in der Dokumentation für die [{{site.data.keyword.iot_full}} Limited Gateway-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_authorization_devices_deviceId_roles){: new_window}.

## Geräte in einer Ressourcengruppe hinzufügen und entfernen
{: #devices_groups}

Damit ein Gateway mit der Rolle *Standardgateway* im Namen eines Geräts handeln kann, muss das betreffende Gerät Mitglied der Ressourcengruppe sein, die dem Gateway zugeordnet ist. Verwenden Sie die folgende API, um mehrere Geräte gleichzeitig einer Ressourcengruppe hinzuzufügen:

```
 PUT /bulk/devices/{groupId}/add
```

Die Gruppe, der Geräte hinzugefügt werden sollen, muss im Pfad der Anforderung angegeben werden, und die Geräte, die hinzugefügt werden sollen, müssen im Hauptteil der Anforderung angegeben werden. Weitere Informationen zu dem Anforderungsschema und den Antworten finden Sie in der Dokumentation für die [{{site.data.keyword.iot_short_notm}} Limited Gateway-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_add){: new_window}.

Verwenden Sie die folgende API, um mehrere Geräte aus einer Ressourcengruppe zu entfernen:

```
PUT /bulk/devices/{groupId}/remove
```

Die im Hauptteil der Anforderung angegebenen Geräte werden aus der Gruppe entfernt, die im Pfad der Anforderung angegeben ist. Weitere Informationen zu dem Anforderungsschema und der Antwort finden Sie in der Dokumentation für die [{{site.data.keyword.iot_short_notm}} Limited Gateway-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/put_bulk_devices_groupId_remove){: new_window}.

## Ressourcengruppe suchen
{: #finding_groups}

Ressourcengruppen können Suchtags zugeordnet werden. Suchtags können verwendet werden, um mithilfe der folgenden API die Details zu einer Ressourcengruppe abzurufen:

```
GET /groups
```

Diese API gibt die Ressourcengruppen zurück, die dem verwendeten Suchtag zugeordnet sind. Wenn kein Suchtag angegeben ist, werden alle Ressourcengruppen zurückgegeben.<!-- For more information about the request schema, response, and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Die ID einer Ressourcengruppe, die einem Gateway zugeordnet wurde, kann mithilfe der folgenden API gefunden werden. Hierbei steht *${clientID}* für die URL-codierte Client-ID im Format *d:${orgId}:${typeId}:${deviceId}* für Geräte oder *g:${orgId}:${typeId}:${deviceId}* für Gateways:

```
GET /authorization/devices/${clientId}
```

Diese API gibt die eindeutige Kennung der Ressourcengruppen zurück, die diesem Gerät zugeordnet wurden. Weitere Informationen zu dieser API finden Sie in der [Dokumentation für die {{site.data.keyword.iot_short_notm}} Limited Gateway-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_authorization_devices_deviceId){: new_window}.


## Ressourcengruppe abfragen
{: #querying_groups}

Beim Abfragen von Ressourcengruppen können verschiedene Parameter verwendet werden, um die vollständigen Eigenschaften aller Geräte in der Gruppe, die eindeutigen Kennungen aller Geräte in der Gruppe oder die Eigenschaften der Ressourcengruppe zurückzugeben.

Verwenden Sie die folgende API, um die vollständigen Eigenschaften für alle Geräte in der angegebenen Ressourcengruppe zurückzugeben:

```
GET /bulk/devices/{groupId}
```

Diese API gibt die vollständige Liste der Eigenschaften für alle Mitglieder der angegebenen Ressourcengruppe zurück. Weitere Informationen zum Anforderungsschema, zu den Antworten und zum Durchblättern der Ergebnisse finden Sie in der Dokumentation für die [{{site.data.keyword.iot_short_notm}} Limited Gateway-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-gateway-beta.html#!/Limited_Gateway/get_bulk_devices_groupId){: new_window}.

Verwenden Sie die folgende API, um nur die eindeutigen Kennungen der Mitglieder der Ressourcengruppe zurückzugeben:

```
GET /bulk/devices/{groupId}/ids
```

Diese API gibt die eindeutigen Kennungen aller Geräte zurück, die Mitglieder der Ressourcengruppe sind. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Verwenden Sie die folgende API, um die Eigenschaften der Ressourcengruppe zurückzugeben:

```
GET /groups/{groupId}
```

Diese API gibt die Eigenschaften der Ressourcengruppe (Name, Beschreibung, Suchtags und eindeutige Kennung) zurück, die im Pfad angegeben ist, jedoch keine Liste der Mitglieder der Ressourcengruppe. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Ressourcengruppe erstellen und löschen
{: #crt_del_groups}

Ressourcengruppen können unabhängig von den verbindenden Gateways erstellt und gelöscht werden. Verwenden Sie die folgende API, um eine Ressourcengruppe zu erstellen:

```
POST /groups
```

Diese API erstellt eine Ressourcengruppe und gibt die Gruppendetails zurück. <!-- For details on the request schema and the responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Verwenden Sie die folgende API, um eine Ressourcengruppe zu löschen:

```
DELETE /groups/{groupId}
```

Diese API löscht die angegebene Ressourcengruppe. Geräte, die Mitglieder der Gruppe waren, werden aus der Gruppe gelöscht, aber die Geräte an sich bleiben unverändert erhalten. <!-- For more information, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

## Gruppeneigenschaften aktualisieren
{: #update_groups}

  - /groups/{groupId}
    - PUT: Aktualisiert die Eigenschaften der angegebenen Gruppe.

## Geräteeigenschaften abrufen und aktualisieren
{: #fdevice_group_props}

Mit der API können Geräteeigenschaften auf verschiedene Arten abgerufen werden. Dabei gibt jede API andere Informationen zurück. Verwenden Sie die folgende API, um die Geräteeigenschaften für alle vorhandenen Geräte abzurufen, die in Ihrer {{site.data.keyword.iot_short_notm}}-Organisation vorhanden sind:

```
GET /authorization/devices

```

Diese API gibt die Eigenschaften aller vorhandenen Geräte in der Organisation einschließlich der zugehörigen Eigenschaften für die Zugriffssteuerung wie Rolle, Status und Ablaufdatum zurück. <!-- For more information on responses and how to page through results, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Um Geräteeigenschaften eines einzelnen Geräts in der Organisation abzurufen, verwenden Sie die folgende API. Hierbei steht *${clientID}* für die URL-codierte Client-ID im Format *d:${orgId}:${typeId}:${deviceId}* für Geräte oder *g:${orgId}:${typeId}:${deviceId}* für Gateways:

```
GET /authorization/devices/${clientId}
```

Diese API gibt alle Geräteeigenschaften für das angegebene Geräte zurück. <!-- For more information, see the [{{site.data.keyword.iot_short_notm}} device model documentation](LINK TO DEVICE MODEL) and [API documentation](LINK TO CORRECT API). -->

Um ausschließlich die Zugriffssteuerungsinformationen eines bestimmten Geräts abzurufen, verwenden Sie die folgende API. Hierbei steht *${clientID}* für die URL-codierte Client-ID im Format *d:${orgId}:${typeId}:${deviceId}* für Geräte oder *g:${orgId}:${typeId}:${deviceId}* für Gateways:

```
GET /authorization/devices/${clientId}/roles
```

Diese API gibt die zugehörigen Informationen für die Zugriffssteuerung des angegebenen Geräts ohne die übrigen Geräteeigenschaften zurück. <!-- For more information on the request schema and responses, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Die Geräteeigenschaften können mit zwei Methoden aktualisiert werden, entweder durch Aktualisieren ohne Ändern der Zugriffssteuerungseigenschaften oder durch direktes Aktualisieren der Zugriffsteuerungseigenschaften.

Um Geräteeigenschaften zu aktualisieren, ohne dass dies Auswirkungen auf die Zugriffssteuerungseigenschaften hat, verwenden Sie die folgende API. Hierbei steht *${clientID}* für die URL-codierte Client-ID im Format *d:${orgId}:${typeId}:${deviceId}* für Geräte oder *g:${orgId}:${typeId}:${deviceId}* für Gateways:

```
PUT /authorization/devices/${clientId}
```

Diese API aktualisiert nur Geräteeigenschaften, die nicht der Zugriffssteuerung zugeordnet sind. <!-- For more information on request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->

Um ausschließlich die Zugriffssteuerungseigenschaften des angegebenen Geräts zu aktualisieren, verwenden Sie die folgende API. Hierbei steht *${clientID}* für die URL-codierte Client-ID im Format *d:${orgId}:${typeId}:${deviceId}* für Geräte oder *g:${orgId}:${typeId}:${deviceId}* für Gateways:

```
PUT /authorization/devices/${clientId}/withroles
```

Diese API aktualisiert nur die Zugriffssteuerungseigenschaften für das angegebene Gerät. <!-- For more information on the request schema, see the [{{site.data.keyword.iot_short_notm}} API documentation](LINK TO CORRECT API). -->
