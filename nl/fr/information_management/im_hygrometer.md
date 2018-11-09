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

# Informations suplémentaires concernant le guide détaillé 2 - Configuration d'une interface logique pour un terminal d'humidité

Utilisez les informations suivantes pour créer un scénario dans lequel des capteurs sur deux terminaux d'humidité publient des événements dans IBM Watson™ IoT Platform. Les relevés des terminaux d'humidité sont mappés à un seul relevé d'humidité. Un terminal est situé dans la salle de réunion 1 et l'autre dans la salle de réunion 2.


## A propos de cette tâche

Utilisez la même instance d'organisation {{site.data.keyword.iot_short_notm}} et la même clé d'API ou le même jeton pour cette organisation que ceux qui sont utilisés dans le [guide détaillé 1](../GA_information_management/ga_im_index_scenario.html). 

Cette configuration est requise pour le tutoriel décrit dans la documentation du [guide détaillé 2](im_index_scenario_thing.html).

Dans ce scénario, un type de terminal et deux instances de terminal sont créés.

Les instances de terminal sont appelées *humiditySensor1* et *humiditySensor2*. Les terminaux publient les événements d'humidité. L'événement d'humidité a par exemple le contenu suivant :
```
{
  "hum" : 35
}
```
Les événements d'humidité sont publiés sur la rubrique `iot-2/evt/humevt/fmt/json`. 

**Remarque :** l'identificateur d'événement est *humevt*. Cet identificateur est requis lorsque vous ajoutez un événement d'humidité de ces types à l'interface physique et lorsque vous définissez des mappages pour mapper une propriété associée à un événement entrant de ce type à une propriété dans votre interface logique. Dans ce scénario, la propriété définie dans l'interface logique est appelée **humidity**. Cette interface logique représente l'état des terminaux de ce type dans la structure suivante :
```
{
  "humidity" : <current humidity value in Celsius>
}
```

Utilisez l'exemple suivant pour mettre en place votre propre environnement d'interfaces.

**Remarque importante :** vous devez sauvegarder les ID qui sont générés dans les réponses curl car ils seront requis pour effectuer d'autres tâches.


## Si nécessaire, ajoutez un type de terminal et un terminal
{: #step14}

Dans ce scénario, un type de terminal et deux instances de terminal sont supposés. Les instances de terminal *humiditySensor1* et *humiditySensor2* sont associées au type de terminal *HumiditySensor*. 

Vous pouvez créer des types de terminal et des terminaux à l'aide du [tableau de bord {{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://internetofthings.ibmcloud.com){: new_window} ou à l'aide des API REST. Pour en savoir plus sur l'utilisation du tableau de bord IoT Platform de {{site.data.keyword.iot_short_notm}} pour ajouter des types de terminal et des terminaux, reportez-vous à la documentation [Initiation à la gestion des données via l'interface Web](../GA_information_management/im_ui_flow.html).

L'exemple suivant montre comment créer un type de terminal appelé *HumiditySensor* à l'aide de l'API REST :

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "HumiditySensor", "description" : "The humidity sensor device type", "metadata": {"maxHumidityThreshold": 70}' \
 ```
**Remarque importante :** vous pouvez ajouter des métadonnées lorsque vous créez un type de terminal et un terminal. Dans ce scénario, les métadonnées suivantes sont ajoutées au type de terminal *HumiditySensor* :
```
{
    "maxHumidityThreshold": 70
}
```
Ces métadonnées peuvent être utilisées lorsque vous [créez des règles](im_rules.html) qui se déclenchent si un événement d'humidité faisant dépasser la propriété *humidity* de l'état du terminal de 70% est reçu par {{site.data.keyword.iot_short_notm}} de *humiditySensor1* ou *humiditySensor2*. 

## Enregistrer les terminaux 

L'exemple suivant montre comment enregistrer une instance de terminal appelée *humiditySensor1* :
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor1", "authToken": "password"}' \
```
L'exemple suivant montre comment enregistrer une instance de terminal appelée *humiditySensor2* :
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/HumiditySensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "humiditySensor2", "authToken": "password"}' \
```

**Remarque :** lorsqu'un terminal envoie une demande HTTP via l'API REST HTTP de Watson IoT Platform, un nom d'utilisateur et un mot de passe sont requis. Le mot de passe est la valeur du jeton d'authentification qui est généré automatiquement ou spécifié manuellement lorsqu'un terminal est enregistré. Si vous utilisez un client MQTT, vous devez noter le jeton d'authentification de votre terminal car vous aurez besoin du jeton pour extraire l'état de l'objet et du terminal en vous abonnant à une chaîne de rubrique IoT. 

## Etape 1 : créez un fichier schéma d'événement
{: #step1}

Dans ce scénario, créez un fichier de schéma d'événement pour définir la structure des événements d'humidité entrants.

L'exemple suivant montre comment créer un fichier schéma nommé *humEventSchema.json*. Ce fichier définit la structure d'un événement entrant en provenance d'un terminal d'humidité :

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "humEventSchema",
    "description" : "defines the structure of a humidity event",
    "properties" : {
        "humidity" : {
            "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
      "humidity"
    ]
}
```
**Astuce :** utilisez le paramètre **required** pour marquer une ou plusieurs propriétés comme obligatoires. Les propriétés obligatoires doivent être incluses dans un message de terminal afin que {{site.data.keyword.iot_short_notm}} mette à jour l'état du terminal avec les données de ce terminal. Un message qui n'inclut pas de propriété obligatoire n'est pas traité.   


## Etape 2 : créez une ressource de schéma d'événement pour votre type d'événement
{: #step2}

Pour créer une ressource de schéma d'événement, utilisez l'API suivante :

```
POST /draft/schemas
```

Le fichier de définition de schéma est transmis à Watson IoT Platform dans un POST à plusieurs parties (données de formulaire/multiple). Le corps du POST doit contenir au moins deux parties :

- Une appelée **schemaFile** qui contient le contenu actuel du fichier comme le corps de la partie.
- Une appelée **name** qui contient une chaîne qui définit le nom de la ressource de schéma dans le corps de la partie.

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

L'exemple suivant montre comment utiliser cURL pour créer la ressource de schéma d'événement *humEventSchema.json* :

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=humEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/humEventSchema.json"'
```

**Remarque :** L'exemple de valeur d'autorisation `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` comporte les informations suivantes :
`{API Key}:{authorization token}`, qui sont ensuite codées en Base64.

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "name" : "humEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "humEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e20",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de schéma *5846cd7c6522050001db0e20* qui est renvoyé en réponse à la méthode POST est requis lorsque vous ajoutez un schéma d'événement à votre type d'événement.



## Etape 3 : créez un type d'événement qui fasse référence au schéma d'événement
{: #step3}

Chaque type d'événement fait référence à un schéma d'événement adéquat parmi ceux qui ont été
créés dans l'exemple précédent. Il utilise à cet effet l'identificateur de schéma figurant dans la
réponse à la méthode POST utilisée pour créer la ressource de schéma d'événement.

Pour créer un type d'événement, utilisez l'API suivante :

```
POST /draft/event/types
```
Le type d'événement provisoire doit faire référence à la définition de schéma qui définit la structure de l'événement MQTT entrant.


Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Event_Types).


L'exemple suivant montre comment utiliser cURL pour créer un type d'événement pour un événement d'humidité :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "humEvent", "schemaId" : "5846cd7c6522050001db0e20"}'
```

L'identificateur de schéma *5846cd7c6522050001db0e20* est utilisé pour ajouter le schéma d'événement au type d'événement. Cet identificateur a été renvoyé en réponse à la méthode POST qui a été utilisée pour créer la ressource de schéma d'événement *humEventSchema.json*.

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e20",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e20"
  },
  "name" : "humEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e21",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

L'identificateur de type d'événement *5846cd7c6522050001db0e21* qui est renvoyé en réponse à la méthode POST est utilisé pour ajouter un type d'événement à l'interface physique.


## Etape 4 : créez une interface physique
{: #step7}

Pour créer une interface physique, utilisez l'API suivante :

```
POST /draft/physicalinterfaces
```

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Dans ce scénario, nous avons besoin d'une interface physique pour le type d'événement que nous avons créé plus tôt.

L'exemple suivant montre comment utiliser cURL pour créer l'interface physique :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json’ \
  --data '{"name" : "Hygrometer Physical Interface"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

L'identificateur d'interface physique *5846cd7c6522050001db0e22* qui est renvoyé dans la réponse est utilisé dans l'URL de la méthode POST appelée pour ajouter un événement de température mesuré en degrés Celsius à l'interface physique.


## Etape 5 : ajoutez le type d'événement à l'interface physique
{: #step8}

Pour ajouter un type d'événement à votre interface physique, utilisez l'API suivante :

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Dans ce scénario, le type d'événement suivant est ajouté aux interfaces physiques spécifiées :
- l'événement d'humidité *humevt* est ajouté à l'interface physique avec l'identificateur *5846cd7c6522050001db0e22* en utilisant l'*eventId* issu de la rubrique et l'*eventTypeId* issu de la création de la ressource de schéma d'événement.

L'exemple suivant montre comment utiliser cURL pour ajouter l'événement d'humidité *humevt* à l'interface physique avec l'identificateur *5846cd7c6522050001db0e22* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "humevt", "eventTypeId" : "5846cd7c6522050001db0e21"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "eventTypeId" : "5846cd7c6522050001db0e21",
  "eventId" : "humevt"
}
```

## Etape 6 : mettez à jour le type de terminal pour connecter l'interface physique
{: #step9}

Pour mettre à jour un type de terminal, utilisez l'API suivante :

```
POST /draft/device/types/{typeId}/physicalinterface
```

où *typeId* est l'identificateur du type de terminal.


Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Dans ce scénario, le type de terminal *HumiditySensor* est mis à jour pour se connecter à l'interface physique *5846cd7c6522050001db0e22*.

L'exemple suivant montre comment utiliser cURL pour mettre à jour le type de terminal *HumiditySensor* :

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5846cd7c6522050001db0e22"}'
```

L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5846cd7c6522050001db0e22/events"
  },
  "id" : "5846cd7c6522050001db0e22",
  "name" : "Hygrometer Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de terminal *HumiditySensor* est requis lorsque vous ajoutez votre interface physique et votre interface logique.
  


## Etape 7 : créez un fichier de schéma d'interface logique
{: #step4}

L'exemple suivant montre comment créer un fichier de schéma d'interface logique appelé *hygrometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "hygrometerSchema",
    "description" : "Schema that defines the canonical interface for a hygrometer",
    "properties" : {
        "humidity" : {
            "description" : "percentage humidity",
            "type" : "number",
            "minimum" : 25,
            "default" : 0.0
        }
    },
    "required" : ["humidity"]
}
```

## Etape 8 : créez une ressource de schéma d'interface logique 
{: #step5}
L'exemple suivant montre comment utiliser cURL pour créer une interface logique :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```
L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometer",
  "version" : "draft"
}
```

## Etape 9 : créez une interface logique faisant référence à un schéma d'interface logique
{: #step6}

Pour créer une interface logique, utilisez l'API suivante :

```
POST /draft/logicalinterfaces
```

Si vous le souhaitez, vous pouvez spécifier un alias significatif pour votre interface logique. L'alias peut être référencé dans l'appel API ou dans l'abonnement à la chaîne de rubrique utilisée pour extraire l'état d'un terminal, au lieu d'utiliser l'identificateur d'interface logique auto-généré.

**Remarque :** l'alias doit avoir une longueur comprise entre 1 et 36 caractères et peut inclure des caractères alphanumériques, des traits d'union, des points et des traits de soulignement. L'alias ne peut pas être une chaîne hexadécimale de 24 caractères. 

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

Dans ce scénario, utilisez l'identificateur de schéma *5846cd7c6522050001db0e23* qui a été renvoyé dans la réponse précédente pour ajouter le schéma d'interface d'application à l'interface logique.

L'exemple suivant montre comment utiliser cURL pour créer une interface logique :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Hygrometer Interface", "alias" : "IHygrometer", "schemaId" : "5846cd7c6522050001db0e23"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "schemaId" : "5846cd7c6522050001db0e23",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846cd7c6522050001db0e24",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Hygrometer Interface",
  "alias" : "IHygrometermometer",
  "version" : "draft"
}
```
Dans ce scénario, utilisez l'identificateur d'interface logique *5846cd7c6522050001db0e24* retourné dans la réponse à la méthode POST pour ajouter votre interface logique à votre type de terminal. Vous pouvez également utiliser cet identificateur pour mapper un événement de terminal entrant à une propriété définie par l'interface logique. Vous pouvez utiliser l'alias de l'interface logique *IHygrometer* pour extraire l'état du terminal soit à l'aide des API REST HTTP, soit en vous abonnant à une chaîne de rubrique.


## Etape 10 : ajoutez l'interface logique au type de terminal

L'exemple suivant montre comment utiliser cURL pour ajouter l'interface logique 5846cd7c6522050001db0e24 qui fait référence à l'identificateur de schéma de l'interface logique 5846cd7c6522050001db0e23 au type de terminal HumiditySensor :
```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846cd7c6522050001db0e24"}'
```
L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e23"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Hygrometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846cd7c6522050001db0e24",
  "schemaId" : "5846cd7c6522050001db0e23"
}
```

## Etape 11 : ajoutez les mappages au type de terminal

    
L'exemple suivant montre comment utiliser cURL pour ajouter un mappage au type de terminal *HumiditySensor* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846cd7c6522050001db0e24","notificationStrategy": "on-state-change","propertyMappings" :  : {              "humevt" : {"humidity" : "$event.hum"}}}' 
```

L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "logicalInterfaceId": "5846cd7c6522050001db0e24",
  "propertyMappings": {
    "humevt": {
      "humidity": "$event.hum"
    }
  },
  "notificationStrategy": "on-state-change",
  "version": "draft",
  "created": "2017-06-16T15:41:49Z",
  "createdBy": "a-8x7nmj-9iqt56kfil",
  "updated": "2017-06-16T15:41:49Z",
  "updatedBy": "a-8x7nmj-9iqt56kfil"
}
```

## Etape 12 : activez la configuration pour le type de terminal


    
L'exemple suivant montre comment utiliser cURL pour valider et activer la configuration de votre type de terminal *HumiditySensor* :
```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/HumiditySensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
L'exemple suivant illustre une réponse à la méthode PATCH :
```
{
 "message": "CUDRS0520I: State update configuration for device type 'HumiditySensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["HumiditySensor"]
  },
 "failures": []
}
```
