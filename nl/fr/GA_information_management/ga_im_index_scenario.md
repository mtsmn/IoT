---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-22"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Guide détaillé 1 : Exemple détaillé d'utilisation des terminaux via une interface commune
{: #scenario}

Utilisez les informations suivantes pour créer un scénario dans lequel deux terminaux de température publient des événements dans {{site.data.keyword.iot_full}}. Un terminal mesure la température en degrés Celsius. L'autre mesure la température en degrés Fahrenheit. Ces mesures
sont mappées à un unique relevé de température exprimé en degrés Celsius. Lorsqu'une nouvelle mesure de température est publiée par ces terminaux, la valeur de la
propriété associée à l'état du terminal concerné change.
{: shortdesc}

## Avant de commencer

Lorsque vous créez une [ressource](ga_im_definitions.html#definitions_resources), cette ressource est créée sous forme de version provisoire. La version provisoire est une copie de travail que vous pouvez interroger, mettre à jour et supprimer directement par le biais des API. Lorsque vous utilisez les API REST, le préfixe **draft/** est utilisé pour identifier les ressources qui se trouvent à l'état provisoire. 

Pour plus d'informations sur les versions provisoires et sur les versions actives des ressources, voir [Connaissance de la gestion des données](ga_im_definitions.html).

## Conditions prérequises

Vous devez avoir une [instance d'organisation](../iotplatform_overview.html#organizations) {{site.data.keyword.iot_short_notm}} ainsi qu'une clé d'API et un jeton d'authentification pour que cette organisation authentifie les demandes. 

Pour obtenir des informations sur la génération d'une API, voir la documentation des [API](../reference/api.html). Pour en savoir plus sur la génération d'un jeton d'API, voir la documentation [Tutoriel d'initiation](../getting-started.html).


## A propos de cette tâche

Dans ce scénario, deux terminaux sont créés.

Un terminal est appelé *tSensor*. Ce terminal publie des événements de température qui sont mesurés en degrés Celsius. L'événement de température est publié sur la rubrique
`iot-2/evt/tevt/fmt/json` et comporte l'exemple de contenu suivant :
```
{
  "t" : 34.5
}
```

**Remarque :** l'identificateur d'événement est *tevt*. Cet identificateur est requis lorsque vous ajoutez un événement de température de ce type à l'interface physique et lorsque vous définissez des mappages dans le but de mapper une propriété associée à un événement entrant de ce type à une propriété définie dans votre interface logique. Dans ce scénario, la propriété définie dans l'interface logique se nomme **temperature**.

L'autre terminal est appelé *tempSensor*. Ce terminal publie des événements de température qui sont mesurés en degrés Fahrenheit. L'événement de température est publié sur la rubrique `iot-2/evt/tempevt/fmt/json` et comporte l'exemple de contenu suivant :
```
{
  "temp" : 72.55
}
```

**Remarque :** l'identificateur d'événement est *tempevt*. Cet identificateur est requis lorsque vous ajoutez un événement de température de ce type à l'interface physique et lorsque vous définissez des mappages dans le but de mapper une propriété associée à un événement entrant de ce type à une propriété définie dans votre interface logique. Dans ce scénario, la propriété définie dans l'interface logique se nomme **temperature**.

Une interface logique est également configurée. Cette interface logique représente l'état des terminaux de ce type dans la structure suivante :
```
{
  "temperature" : <current temperature value in Celsius>
  }
```
Cette configuration signifie que vous pouvez configurer votre application pour traiter la
valeur associée à **temperature** au lieu de la configurer
pour traiter d'une part la valeur associée à **t**, d'autre
part la valeur associée à **temp** après sa conversion
en degrés Celsius.

![Mappage entre les terminaux de capteur de température et une application sur {{site.data.keyword.iot_short_notm}}.](../information_management/images/Information Management Device example.svg "Mappage entre les terminaux de capteur de température et une application sur {{site.data.keyword.iot_short_notm}}")

Utilisez l'exemple suivant pour mettre en place votre propre environnement d'interfaces.

**Remarque importante :** vous devez sauvegarder les ID qui sont générés dans les réponses curl car ils seront requis pour effectuer d'autres tâches.
Un tableau répertoriant les noms, les valeurs et les identificateurs des propriétés de ressources utilisées dans ce guide est documenté dans [Informations supplémentaires pour les guides détaillés 1 et 2 - Noms et identificateurs de ressources](../information_management/im_id_reference.html).

## Si nécessaire, ajoutez un type de terminal et un terminal
{: #step14}

Dans ce scénario, on utilise deux types de terminal et deux instances de terminal. L'instance de terminal *tSensor* est associée au type de terminal *TSensor*. L'instance de terminal *tempSensor* est associée au type de terminal *TempSensor*.  

Vous pouvez créer des types de terminal et des terminaux à l'aide du [tableau de bord {{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://internetofthings.ibmcloud.com){: new_window} ou à l'aide des API REST. Pour en savoir plus sur l'utilisation du tableau de bord {{site.data.keyword.iot_short_notm}} pour ajouter des types de terminal et des terminaux, reportez-vous à la documentation [Initiation à la gestion des données via l'interface Web](im_ui_flow.html).

L'exemple suivant montre comment créer un type de terminal appelé *TSensor* à l'aide de l'API REST :

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TSensor", "description" : "The Celsius sensor device type", "metadata": {"tempThresholdMax": 44,
    "tempThresholdMin": 10}}' \
 ```
 
 L'exemple suivant montre comment créer un type de terminal appelé *TempSensor* à l'aide de l'API REST :

```
curl --request POST \
    --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types \
    --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
    --header 'content-type: application/json' \
    --data '{"id" : "TempSensor", "description" : "The Fahrenheit sensor device type"}' \
 ```

**Remarque :** vous pouvez ajouter des métadonnées lorsque vous créez un type de terminal et un terminal. Dans ce scénario, les métadonnées suivantes sont ajoutées au type de terminal *TSensor* :
```
{
    "tempThresholdMax": 44,
    "tempThresholdMin": 10 
}
```
Ces métadonnées sont utilisées lorsque vous créez des [règles](../information_management/im_rules.html) qui se déclenchent lorsqu'un événement de température est reçu par {{site.data.keyword.iot_short_notm}} du terminal *tSensor* qui fait dépasser de 44 degrés Celsius la propriété *temperature*. 


Vous devez ensuite enregistrer une instance de terminal associée à un type de terminal. L'exemple suivant montre comment enregistrer une instance de terminal appelée *tSensor* qui est associée au type de terminal *TSensor* à l'aide de l'API REST :
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tSensor", "authToken": "password"}' \
```

L'exemple suivant montre comment enregistrer une instance de terminal appelée *tempSensor* qui est associée au type de terminal *TempSensor* à l'aide de l'API REST :
```
    curl --request POST \
        --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices \
        --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
        --header 'content-type: application/json' \
        --data '{"deviceId": "tempSensor", "authToken": "password"}' \
```

Pour en savoir plus sur l'utilisation des API REST pour ajouter des types de terminal et des terminaux, voir la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html){: new_window}.

**Remarque :** lorsqu'un terminal envoie une demande HTTP via l'API REST HTTP de Watson IoT Platform, un nom d'utilisateur et un mot de passe sont requis. Le mot de passe est la valeur du jeton d'authentification qui est généré automatiquement ou spécifié manuellement lorsqu'un terminal est enregistré. Si vous utilisez un client MQTT, vous devez annoter le jeton d'authentification de votre terminal car vous aurez besoin du jeton pour extraire l'état de l'objet et du terminal en vous abonnant à une chaîne de rubrique. 

## Etape 1 : créez un fichier schéma d'événement
{: #step1}

Pour les besoins de ce scénario, créez deux fichiers schéma d'événement afin de définir la structure de chaque événement de température entrant.

L'exemple suivant montre comment créer un fichier schéma nommé *tEventSchema.json*. Ce fichier définit la structure d'un événement entrant en provenance d'un terminal de température qui mesure la température en degrés Celsius :

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tEventSchema",
  "description" : "defines the structure of a temperature event in degrees Celsius",
  "properties" : {
    "t" : {
      "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
  },
  "required" : ["t"]
}
  ```
**Astuce :** utilisez le paramètre **required** pour marquer une ou plusieurs propriétés comme obligatoires. Les propriétés obligatoires doivent être incluses dans un message de terminal afin que {{site.data.keyword.iot_short_notm}} mette à jour l'état du terminal avec les données de ce terminal. Un message qui n'inclut pas de propriété obligatoire n'est pas traité.   

Le nom de fichier schéma *tEventSchema* est utilisé lorsque vous créez une ressource de schéma d'événement pour votre type d'événement.

L'exemple suivant montre comment créer un fichier schéma nommé *tempEventSchema.json*. Ce fichier définit la structure d'un événement entrant en provenance d'un terminal de température qui mesure la température en degrés Fahrenheit :

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type" : "object",
  "title" : "tempEventSchema",
  "description" : "defines the structure of a temperature event in degrees Fahrenheit",
  "properties" : {
    "temp" : {
      "description" : "temperature in degrees Fahrenheit",
      "type" : "number",
      "minimum" : -459.67,
      "default" : 0.0
    }
  },
  "required" : ["temp"]
}
  ```
Le nom de fichier schéma *tempEventSchema* est utilisé lorsque vous créez une ressource de schéma d'événement pour votre type d'événement.   

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

L'exemple suivant montre comment utiliser cURL pour créer la ressource de schéma d'événement *tEventSchema.json* :

```curl
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tEventSchema.json"'
```

**Astuce :** l'exemple de valeur d'autorisation `MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=` comporte les informations suivantes :
`{API Key}:{authorization token}`,  qui sont ensuite codées en Base64.

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "name" : "tEventSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "tEventSchema.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5846cd7c6522050001db0e0d",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de schéma *5846cd7c6522050001db0e0d* qui est renvoyé en réponse à la méthode POST est requis lorsque vous ajoutez un schéma d'événement à votre type d'événement.

L'exemple suivant montre comment utiliser cURL pour créer la ressource de schéma d'événement *tempEventSchema.json* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=tempEventSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/tempEventSchema.json"'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "schemaType" : "json-schema",
  "schemaFileName" : "tempEventSchema.json",
  "updated" : "2016-12-06T14:44:51Z",
  "name" : "tempEventSchema",
  "version" : "draft",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "created" : "2016-12-06T14:44:51Z",
  "id" : "5846cee36522050001db0e0e",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e/content"
  },
  "contentType" : "application/octet-stream",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de schéma *5846cee36522050001db0e0e* qui est renvoyé en réponse à la méthode POST est requis lorsque vous ajoutez un schéma d'événement à votre type d'événement.

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


L'exemple suivant montre comment utiliser cURL afin de créer un type d'événement pour un événement de température mesuré en degrés Celsius :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tEvent", "schemaId" : "5846cd7c6522050001db0e0d"}'
```

L'identificateur de schéma *5846cd7c6522050001db0e0d* est utilisé pour ajouter le schéma d'événement au type d'événement. Cet identificateur a été renvoyé en réponse à la méthode POST qui a été utilisée pour créer la ressource de schéma d'événement *tEventSchema.json*.

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "updated" : "2016-12-06T14:53:49Z",
  "schemaId" : "5846cd7c6522050001db0e0d",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cd7c6522050001db0e0d"
  },
  "name" : "tEvent",
  "version" : "draft",
  "created" : "2016-12-06T14:53:49Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846d0fd6522050001db0e0f",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

L'identificateur de type d'événement *5846d0fd6522050001db0e0f* qui est renvoyé en réponse à la méthode POST est utilisé pour ajouter un type d'événement à l'interface physique.

L'exemple suivant montre comment utiliser cURL afin de créer un type d'événement pour un événement de température mesuré en degrés Fahrenheit :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/event/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "tempEvent", "schemaId" : "5846cee36522050001db0e0e"}'
```
L'identificateur de schéma *5846cee36522050001db0e0e* est utilisé pour ajouter le schéma d'événement au type d'événement. Cet identificateur a été renvoyé en réponse à la méthode POST qui a été utilisée pour créer la ressource de schéma d'événement *tempEventSchema.json*.

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "schemaId" : "5846cee36522050001db0e0e",
  "created" : "2016-12-06T15:00:20Z",
  "id" : "5846d2846522050001db0e10",
  "updated" : "2016-12-06T15:00:20Z",
  "name" : "tempEvent",
  "version" : "draft",
  "refs" : {
    "schema" : "/api/v0002/draft/schemas/5846cee36522050001db0e0e"
  },
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de type d'événement *5846d2846522050001db0e10* qui est renvoyé en réponse à la méthode POST est utilisé pour ajouter un type d'événement à l'interface physique.

## Etape 4 : créez une interface physique
{: #step7}

Pour créer une interface physique, utilisez l'API suivante :

```
POST /draft/physicalinterfaces
```

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Dans ce scénario, nous avons besoin de deux interfaces physiques, une pour chaque type d'événement.

L'exemple suivant montre comment utiliser cURL pour créer la première interface physique :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TSensor Physical Interface"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

L'identificateur d'interface physique *5847d1df6522050001db0e1a* qui est renvoyé dans la réponse est utilisé dans l'URL de la méthode POST appelée pour ajouter un événement de température mesuré en degrés Celsius à l'interface physique.

L'exemple suivant montre comment utiliser cURL pour créer la seconde interface physique :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "TempSensor Physical Interface"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```

L'identificateur d'interface physique *5847d1df6522050001db0e1b* qui est renvoyé dans la réponse est utilisé dans l'URL de la méthode POST appelée pour ajouter un événement de température mesuré en degrés Fahrenheit à l'interface physique.   

## Etape 5 : ajoutez le type d'événement à l'interface physique
{: #step8}

Pour ajouter un type d'événement à votre interface physique, utilisez l'API suivante :

```
POST /draft/physicalinterfaces/{physicalInterfaceId}/events
```

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Physical_Interfaces).

Dans ce scénario, les types d'événement suivants sont ajoutés aux interfaces physiques spécifiées :
- L'événement de température en degrés Celsius, *tevt*, est ajouté à l'interface physique dont
l'identificateur est *5847d1df6522050001db0e1a* en utilisant l'*eventId* issu de la rubrique
et l'*eventTypeId* résultant de la création de la ressource de schéma d'événement.
- L'événement de température en degrés Fahrenheit, *tempevt*,
est ajouté à l'interface physique dont l'identificateur
est *5847d1df6522050001db0e1b* en utilisant l'*eventId* issu de la rubrique et
l'*eventTypeId* résultant de la création de la ressource de schéma d'événement.


L'exemple suivant montre comment utiliser cURL pour ajouter l'événement de
température *tevt* à l'interface physique dont l'identificateur
est *5847d1df6522050001db0e1a* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tevt", "eventTypeId" : "5846d0fd6522050001db0e0f"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "eventTypeId" : "5846d0fd6522050001db0e0f",
  "eventId" : "tevt"
}
```

L'exemple suivant montre comment utiliser cURL pour ajouter l'événement de
température *tempevt* à l'interface physique dont l'identificateur
est *5847d1df6522050001db0e1b* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"eventId" : "tempevt", "eventTypeId" : "5846d2846522050001db0e10"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "eventTypeId" : "5846d2846522050001db0e10",
  "eventId" : "tempevt"
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

Dans ce scénario, le type de terminal *TSensor* est mis à jour pour se connecter à l'interface physique *5847d1df6522050001db0e1b* et le type de terminal *TempSensor* est mis à jour pour se connecter à l'interface physique *5847d1df6522050001db0e1a*.


L'exemple suivant montre comment utiliser cURL pour mettre à jour le type de terminal *TSensor* :

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1b"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1b/events"
  },
  "id" : "5847d1df6522050001db0e1b",
  "name" : "TSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:19:51Z",
  "updated" : "2016-12-07T09:19:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de terminal *TSensor* est requis lorsque vous ajoutez votre interface physique et votre interface logique.

L'exemple suivant montre comment utiliser cURL pour mettre à jour le type de terminal *TempSensor* :

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/physicalinterface \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "5847d1df6522050001db0e1a"}'
```

L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
    "events" : "/api/v0002/draft/physicalinterfaces/5847d1df6522050001db0e1a/events"
  },
  "id" : "5847d1df6522050001db0e1a",
  "name" : "TempSensor Physical Interface",
  "version" : "draft",
  "created" : "2016-12-07T09:09:51Z",
  "updated" : "2016-12-07T09:09:51Z",
  "createdBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de terminal *TempSensor* est requis lorsque vous ajoutez votre interface physique et votre interface logique.


## Etape 7 : créez un fichier de schéma d'interface logique
{: #step4}

L'exemple suivant montre comment créer un fichier de schéma d'interface logique appelé *thermometer.json*.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "thermometerSchema",
    "description" : "Schema that defines the canonical interface for a thermometer",
    "properties" : {
        "temperature" : {
            "description" : "temperature in degrees Celsius",
            "type" : "number",
            "minimum" : -273.15,
            "default" : 0.0
        }
    },
    "required" : ["temperature"]
}
```

## Etape 8 : créez une ressource de schéma d'interface logique 
{: #step5}

Pour créer une ressource de schéma d'interface logique, utilisez l'API suivante :

```
POST /draft/schemas
```

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).

L'exemple suivant montre comment utiliser cURL pour créer le schéma d'interface logique :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=thermometerSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/deviceStateDemo/setup/schemas/thermometer.json"'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "thermometerSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "thermometer.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5846ec826522050001db0e11/content"
  },
  "id" : "5846ec826522050001db0e11"
}
```
Utilisez l'identificateur de schéma *5846ec826522050001db0e11* retourné dans la réponse à la méthode POST pour ajouter le schéma d'interface logique à l'interface logique.

## Etape 9 : créez une interface logique faisant référence à un schéma d'interface logique
{: #step6}

Pour créer une interface logique, utilisez l'API suivante :

```
POST /draft/logicalinterfaces
```

Si vous le souhaitez, vous pouvez spécifier un alias significatif pour votre interface logique. L'alias peut être référencé dans l'appel API ou dans l'abonnement à la chaîne de rubrique utilisée pour extraire l'état d'un terminal, au lieu d'utiliser l'identificateur d'interface logique auto-généré.

**Remarque :** L'alias doit avoir une longueur comprise entre 1 et 36 caractères et peut inclure des caractères alphanumériques, des traits d'union, des points et des traits de soulignement. L'alias ne peut pas être une chaîne hexadécimale de 24 caractères. 

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).

Dans ce scénario, utilisez l'identificateur de schéma *5846ec826522050001db0e11* qui a été renvoyé dans la réponse précédente pour ajouter le schéma d'interface d'application à l'interface logique.

L'exemple suivant montre comment utiliser cURL pour créer une interface logique :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "Thermometer Interface", "alias" : "IThermometer", "schemaId" : "5846ec826522050001db0e11"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "schemaId" : "5846ec826522050001db0e11",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5846ed076522050001db0e12",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "Thermometer Interface",
  "alias" : "IThermometer",
  "version" : "draft"
}
```
Dans ce scénario, utilisez l'identificateur d'interface logique *5846ed076522050001db0e12* retourné dans la réponse à la méthode POST pour ajouter votre interface logique à votre type de terminal. Vous pouvez également utiliser cet identificateur pour mapper un événement de terminal entrant à une propriété définie par l'interface logique. Vous pouvez utiliser l'alias d'interface logique *IThermometer* pour [extraire l'état du terminal](##step13), soit à l'aide des API REST HTTP, soit en vous abonnant à une chaîne de rubrique.

## Etape 10 : ajoutez l'interface logique à un type de terminal
{: #step10}

Pour ajouter une interface logique à un type de terminal, utilisez l'API suivante :

```
POST /draft/device/types/{typeId}/logicalinterfaces
```

où *typeId* est le nom du type de terminal. 

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).  
**Remarque :** Dans ce scénario, la même interface logique *5846ed076522050001db0e12* est associée à *TSensor* et à *TempSensor*.

L'exemple suivant montre comment utiliser cURL pour ajouter l'interface logique *5846ed076522050001db0e12* associée à l'identificateur de schéma logique *5846ec826522050001db0e11* au type de terminal *TSensor* :

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

L'exemple suivant montre comment utiliser cURL pour ajouter l'interface logique *5846ed076522050001db0e12* qui fait référence à l'identificateur de schéma d'interface logique *5846ec826522050001db0e11* au type de terminal *TempSensor* :

```
curl --request POST \
--url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/logicalinterfaces \
--header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
--header 'content-type: application/json' \
--data '{"id": "5846ed076522050001db0e12"}'
```

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5846ec826522050001db0e11"
  },
  "updated" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "name" : "Thermometer Interface",
  "version" : "draft",
  "created" : "2016-12-06T16:53:27Z",
  "id" : "5846ed076522050001db0e12",
  "schemaId" : "5846ec826522050001db0e11"
}
```

## Etape 11 : définissez des mappages entre les propriétés de l'événement entrant et celles de l'interface logique
{: #step11}  
**Important :** si vous utilisez une application client MQTT et que vous voulez vous abonner pour recevoir des notifications sur les mises à jour de l'état du terminal, vous devez configurer les paramètres de notifications lorsque vous définissez les mappages. En configurant ainsi votre stratégie de notification, vous pouvez réutiliser l'interface logique dans plusieurs types de terminal, chaque type de terminal étant configuré de manière à recevoir les notifications de modification d'état comme vous l'entendez. Vous pouvez définir la stratégie de notification, qu'un client MQTT s'abonne ou non à la chaîne de rubrique spécifiée.  
  
Les paramètres de notification pouvant être configurés sont les suivants :  

Paramètre	|	Description
------	|	-----
never	|	Aucun envoi de notification - Valeur par défaut
on-every-event	|	Envoi de notifications à chaque événement, que l'état du terminal ait changé ou non
on-state-change	|	Envoi de notifications uniquement lorsqu'un événement engendre un changement d'état du terminal  

**Remarque :** Si vous sélectionnez un paramètre de notification *never*, votre application ne reçoit jamais de notification de changement d'état d'un terminal. Par conséquent, vous pouvez préférer une stratégie de notification *on-every-event* ou *on-state-change*.  

Pour mapper des événements, utilisez l'API suivante :

```
POST /draft/device/types/{typeId}/mappings
```

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Dans ce scénario, nous définissons des mappages pour le type de terminal *TSensor* afin de mapper la propriété **t** de l'événement entrant *tevt* à la propriété **temperature** de l'interface logique. Nous définissons également des mappages pour le type de terminal *TempSensor* afin de mapper la propriété **temp** de l'événement entrant *tempevt* à la propriété **temperature** de l'interface logique. Les paramètres de notification sont définis sur la valeur "on-state-change". 

L'exemple suivant montre comment utiliser cURL pour ajouter un mappage au type de terminal *TSensor* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tevt" : {"temperature" : "$event.t"}}}'
```

**Important :** pour les charges utiles à
structure [JSON ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://json-schema.org/){:new_window},
assurez-vous que l'entrée $event.*propriété* est bien adaptée à la structure JSON de vos propriétés. Par exemple, si la charge utile est `{"d":{"t":<temp>}}`, utilisez `$event.d.t`

Spécifiez l'identificateur de l'interface logique *5846ed076522050001db0e12* obtenu plus tôt dans la réponse à la méthode POST ayant servi à créer l'interface logique et le type de terminal *TSensor*.

L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
    "tevt" : {
      "temperature" : "$event.t"
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
L'exemple suivant montre comment utiliser cURL pour ajouter un mappage au type de terminal *TempSensor* :

```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor/mappings \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"logicalInterfaceId" : "5846ed076522050001db0e12","notificationStrategy": "on-state-change","propertyMappings" : {              "tempevt" : {"temperature" : "($event.temp - 32) / 1.8"}}}'
```

Spécifiez l'identificateur de l'interface logique *5846ed076522050001db0e12* obtenu plus tôt dans la réponse à la méthode POST ayant servi à créer l'interface logique et le type de terminal *TempSensor*. Une conversion est appliquée pour changer la valeur mesurée en degrés Fahrenheit en son équivalent en degrés Celsius.


L'exemple suivant illustre une réponse à la méthode POST :

```
{
  "logicalInterfaceId": "5846ed076522050001db0e12",
  "propertyMappings": {
     "tempevt" : {
      "temperature" : "($event.temp - 32) / 1.8"
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

## Etape 12 : validez et activez la configuration
{: #step15}

Validez et activez la configuration de mise à jour de l'état du terminal pour chaque type de terminal. Cette configuration inclut vos schémas, types d'événements, interfaces physiques, interfaces logiques et mappages.

Pour valider et activer votre configuration de type de terminal, utilisez l'API suivante :

```
PATCH /draft/device/types/{typeId}
```

où *typeId* est l'identificateur du type de terminal.

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

Dans ce scénario, nous devons valider et activer la configuration pour deux types de terminal.

L'exemple suivant montre comment utiliser cURL pour valider et activer la configuration de votre type de terminal *TSensor* :

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```
L'exemple suivant illustre une réponse à la méthode PATCH :

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TSensor"]
  },
 "failures": []
}
```

L'exemple suivant montre comment utiliser cURL pour valider et activer la configuration de votre type de terminal *TempSensor* :

```
curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/TempSensor \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

L'exemple suivant illustre une réponse à la méthode PATCH :

```
{
 "message": "CUDRS0520I: State update configuration for device type 'TempSensor' has been successfully submitted for activation",
  "details": {
    "id": "CUDRS0520I",
    "properties": ["TempSensor"]
  },
 "failures": []
}
```

## Etape 13 : publiez un événement de terminal entrant
{: #step12}

Publiez un événement de température provenant de *tSensor* sur la rubrique `iot-2/evt/tevt/fmt/json` avec l'exemple de contenu suivant :
```
{
  "t" : 34.5
}
```
Publiez un événement de température provenant de *tempSensor* sur la rubrique `iot-2/evt/tempevt/fmt/json` avec l'exemple de contenu suivant :
```
{
  "temp" : 72.55
}
```

Pour plus d'informations sur la publication d'un événement entrant provenant d'un terminal,
consultez [Connectivité MQTT pour les applications](../applications/mqtt.html#publishing_device_events).


## Etape 14 : extrayez l'état du terminal
{: #step13}

Vous pouvez extraire l'état du terminal soit en utilisant les API REST HTTP, soit en vous abonnant à une chaîne de rubrique. Si vous avez spécifié un alias lors de la création de votre interface logique, vous pouvez utiliser cet alias au lieu du paramètre *logicalInterfaceId* pour extraire l'état du terminal. 

- Si vous possédez une application client MQTT, vous pouvez vous abonner à la chaîne de rubrique suivante :  
```  
iot-2/type/${typeId}/id/${deviceId}/intf/${logicalInterfaceId}/evt/state  
```  

- Vous pouvez également obtenir l'état du terminal le plus récent en utilisant l'API REST HTTP suivante :  
```
GET /device/types/{typeId}/devices/{deviceId}/state/{logicalInterfaceId}
```

Les paramètres suivants sont requis :  

Paramètre	|	Description
------	|	-----
typeId	|	Identificateur du type de terminal.
deviceId	|	Identificateur du terminal.
logicalInterfaceId ou alias|	Identificateur créé pour l'interface logique ou alias spécifié par l'utilisateur. 


Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Device_Types).

L'exemple suivant montre comment utiliser cURL pour extraire l'état en cours de *tSensor* en référençant l'identificateur de l'interface logique qui a été créée :
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/tSensor/devices/tSensor/state/5846ed076522050001db0e12 \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

L'identificateur d'interface logique *5846ed076522050001db0e12* est utilisé dans la méthode GET. Il s'agit
de l'identificateur obtenu dans la réponse à la méthode POST que nous avons utilisée précédemment pour créer l'interface logique.
L'exemple suivant illustre une réponse à la méthode GET :
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":22.5
  }
}
```

L'exemple suivant montre comment utiliser cURL pour extraire l'état en cours de *tempSensor* en référençant l'alias de l'interface logique qui a été créée :
```
curl --request GET \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/device/types/TempSensor/devices/tempSensor/state/IThermometer \
  --header 'authorization: Basic TGS04NXg5dHotKNBzbGZ5eWdiaToxX543S0lKOmE3Tk5Mc0xMu6n='
```

L'alias de l'interface logique *IThermometer* est utilisée dans la méthode GET. Il s'agit
de l'identificateur obtenu dans la réponse à la méthode POST que nous avons utilisée précédemment pour créer l'interface logique.
L'exemple suivant illustre une réponse à la méthode GET :
```
{
  "timestamp": "2017-07-03T12:15:50Z",
  "updated": "2017-07-03T12:15:50Z",
  "state": {
    "temperature":34.5
  }
}
```

Notez que la mesure de température renvoyée est exprimée en degrés Celsius et non en degrés Fahrenheit.

Votre application peut ainsi traiter ces données normalisées sans qu'aucune configuration
ne soit nécessaire pour interpréter ou convertir les différentes échelles de température.

## Etapes suivantes

Vous voudrez peut-être vous appuyer sur ce scénario pour utiliser la fonction actif jumeau de la gestion des données en configurant un type d'objet et une instance d'objet. Pour obtenir des informations sur la configuration des objets, voir [Guide détaillé 2 : Exemple détaillé d'utilisation des objets via une interface commune (bêta)](../information_management/im_index_scenario_thing.html).

Vous pouvez également créer des règles que vous pouvez utiliser pour déclencher une action lorsqu'un événement qui est reçu par {{site.data.keyword.iot_short_notm}} cause un changement d'état de l'objet ou du terminal. Pour obtenir des informations sur la création de règles, voir [Création de règles imbriquées (bêta)](../information_management/im_rules.html).

