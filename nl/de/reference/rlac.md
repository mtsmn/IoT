---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-17"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Zugriffssteuerung auf Ressourcenebene konfigurieren
{: #configure_RLAC}

Die Zugriffssteuerung auf Ressourcenebene ermöglicht Ihnen die Steuerung des Zugriffs von Benutzern und API-Schlüsseln zur Verwaltung von Geräten. Sie können Ressourcengruppen verwenden, um die Geräte in einer Organisation zu definieren, die von den einzelnen Benutzern oder API-Schlüsseln verwaltet werden können. Benutzern und API-Schlüsseln kann ein Rolle-zu-Gruppe-Paar zugewiesen werden, in dem definiert ist, dass nur Operationen ausgeführt werden können, die von der angegebenen Rolle auf Geräten abgedeckt sind, die in den entsprechenden Gruppen enthalten sind. Weitere Informationen zur Zugriffssteuerung auf Ressourcenebene erhalten Sie in [Übersicht zur Zugriffssteuerung auf Ressourcenebene](rlac_overview.html) und in der [{{site.data.keyword.iot_short_notm}}-Dokumentation zur Zugriffssteuerungs-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Zugriffssteuerung auf Ressourcenebene konfigurieren - Prozessablauf
{: #RLAC_process}

Der folgende Prozessablauf wird normalerweise verwendet, um die Zugriffssteuerung auf Ressourcenebene zu aktivieren und zu verwenden:
1. [Erstellen Sie eine Organisation](../iotplatform_overview.html#organizations).
2. [Erstellen Sie Benutzer](../add_users.html#adding-new-users) und [API-Schlüssel](../platform_authorization.html#api-key).
3. [Erstellen Sie Ressourcengruppen](rlac.html#create_delete_group).
4. [Weisen Sie den Benutzern und API-Schlüsseln Rolle-zu-Gruppen-Zuordnungen zu](rlac.html#assign_roletogroup).
5. [Fügen Sie Geräte zu den Ressourcengruppen hinzu](rlac.html#add_device).
6. [Aktivieren Sie die Zugriffssteuerung auf Ressourcenebene](rlac.html#RLAC_enable).

Die Zugriffssteuerung auf Ressourcenebene wird in verschiedenen gerätebezogenen APIs erzwungen. Eine Liste der entsprechenden APIs finden Sie in [APIs mit erzwungener Zugriffssteuerung auf Ressourcenebene](rlac_overview.html#RLAC_enforced_APIs).

## Ressourcengruppen erstellen und löschen
{: #create_delete_group}

Ressourcengruppen können unabhängig von den verbindenden Gateways erstellt und gelöscht werden.

Verwenden Sie die folgende API, um eine Ressourcengruppe zu erstellen und die Gruppendetails zurückzugeben:

    POST /api/v0002/groups

    {
        "name": "groupA",
        "description": "Devices in the red group",
        "searchTags": ["red"]
    }

Wenn Sie eine Ressourcengruppe löschen, dann werden die in der Gruppe befindlichen Geräte aus der Gruppe entfernt, andere Auswirkungen für diese Geräte ergeben sich jedoch nicht. Verwenden Sie die folgende API, um eine Ressourcengruppe zu löschen:

    DELETE /api/v0002/groups/{groupUid}


## Benutzern oder API-Schlüsseln Rolle-zu-Gruppen-Paare zuweisen
{: #assign_roletogroup}

Sie müssen einem Benutzer oder einem API-Schlüssel ein Rolle-zu-Gruppe-Paar zuweisen, um den Zugriff des betreffenden Benutzers oder der betreffenden API auf eine bestimmte Gruppe von Geräten zu beschränken. Benutzer und API-Schlüssel ohne Ressourcengruppen können alle Geräte in der eigenen Organisation ohne Einschränkungen verwalten. Einem API-Schlüssel kann nur eine Rolle zugeordnet werden, wenn ein Rolle-zu-Gruppe-Paar verwendet wird. Die Rolle, die in "rolesToGroups" angegeben wird, muss mit der Rolle übereinstimmen, die in "roles" angegeben wurde.

Verwenden Sie die folgende API, um einem Benutzer ein Rolle-zu-Gruppe-Paar zuzuweisen:

    PUT /api/v0002/authorization/users/{userUid}/roles

    {
        "roles": [ "PD_ADMIN_USER" ],
        "rolesToGroups": {
           "PD_ADMIN_USER": [ "groupUID" ]
        }
    }



Verwenden Sie die folgende API, um einem API-Schlüssel ein Rolle-zu-Gruppe-Paar zuzuweisen:

    PUT /api/v0002/authorization/apikeys/{apikeyUid}/role

    {
        "roles": [ "PD_OPERATOR_APP" ],
        "rolesToGroups": {
           "PD_OPERATOR_APP": [ "groupUID" ]
        }
    }

## Geräte zu Ressourcengruppen hinzufügen und aus Ressourcengruppen entfernen
{: #add_device}

Bevor ein Benutzer oder ein API-Schlüssel mit einem Rolle-zu-Gruppe-Paar ein Gerät verwalten kann, muss das Gerät als Mitglied der Ressourcengruppe definiert werden, die dem Benutzer bzw. dem API-Schlüssel zugewiesen ist. Wenn Sie Geräte einer Ressourcengruppe hinzufügen, dann muss die Gruppe, zu der Geräte hinzugefügt werden, im Pfad der Anforderung angegeben werden und die Geräte, die hinzugefügt werden, müssen im Hauptteil der Anforderung angegeben werden. Verwenden Sie die folgende API, um mehrere Geräte gleichzeitig einer Ressourcengruppe hinzuzufügen:

    PUT /api/v0002/bulk/devices/{groupId}/add

    [
    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Wenn Sie Geräte aus einer Ressourcengruppe entfernen, dann werden die Geräte, die im Hauptteil der Anforderung angegeben sind, aus der Gruppe entfernt, die im Pfad der Anforderung angegeben ist. Verwenden Sie die folgende API, um mehrere Geräte aus einer Ressourcengruppe zu entfernen:

    PUT /api/v0002/bulk/devices/{groupUid}/remove

    [
	    {
            "typeId":"{typeUid}",
            "deviceId":"{deviceUid}"
        }
    ]

Weitere Informationen zum Anforderungsschema und zur entsprechenden Antwort finden Sie in der [{{site.data.keyword.iot_short_notm}}-Dokumentation zur Zugriffsteuerungs-API ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002-beta/security-subjects-beta.html){: new_window}.

## Zugriffssteuerung auf Ressourcenebene aktivieren
{: #RLAC_enable}

Das Aktivieren der Zugriffssteuerung auf Ressourcenebene für eine Organisation ermöglicht es Ihnen, den Zugriff von Benutzern und API-Schlüsseln auf die zugewiesenen Ressourcengruppen zu beschränken. Zur Verwendung der Zugriffssteuerung auf Ressourcenebene müssen Sie das Konfigurationsflag auf Organisationsebene aktivieren, indem Sie die folgende API verwenden:

    PUT /api/v0002/accesscontrol

    {
        "enable": true
    }

## Ressourcengruppen suchen
{: #find_group}

Ressourcengruppen können Suchtags zugeordnet werden. Suchtags können verwendet werden, um mithilfe der folgenden API die Details zu einer Ressourcengruppe abzurufen:

    GET /api/v0002/groups

Diese API gibt die Ressourcengruppen zurück, die dem verwendeten Suchtag zugeordnet sind. Wenn kein Suchtag angegeben ist, werden alle Ressourcengruppen zurückgegeben.

Verwenden Sie die folgende API, um die eindeutige ID der Ressourcengruppen zu suchen, die einem Benutzer zugewiesen sind:

    GET /api/v0002/authorization/users/{userUid}

Verwenden Sie die folgende API, um die eindeutige ID der Ressourcengruppen zu suchen, die einem API-Schlüssel zugewiesen sind:

    GET /api/v0002/authorization/apikeys/{apikeyUid}


## Ressourcengruppen abfragen
{: #query_group}

Ressourcengruppen können mithilfe verschiedener Parameter abgefragt werden, um die vollständigen Eigenschaften aller Geräte in der Gruppe, die eindeutigen Kennungen aller Geräte in der Gruppe oder die Eigenschaften der Ressourcengruppe zurückzugeben.

Verwenden Sie die folgende API, um die vollständigen Eigenschaften für alle Geräte in der angegebenen Ressourcengruppe zurückzugeben:

    GET /api/v0002/bulk/devices/{groupUid}

Verwenden Sie die folgende API, um nur die eindeutigen Kennungen der Mitglieder der Ressourcengruppe zurückzugeben:

    GET /api/v0002/bulk/devices/{groupUid}/ids

Verwenden Sie die folgenden API, um die Eigenschaften der Ressourcengruppe einschließlich Name, Beschreibung, Suchtags und eindeutiger Kennung (ID), die im Pfad angegeben sind, zurückzugeben:

    GET /api/v0002/groups/{groupUid}

Diese API gibt nicht die Liste der Mitglieder der Ressourcengruppe zurück.

## Gruppeneigenschaften aktualisieren
{: #update_group}

Verwenden Sie die folgende API, um die Eigenschaften einer Gruppe zu aktualisieren:

    PUT /api/v0002/groups/{groupId}
