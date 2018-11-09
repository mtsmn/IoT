---

copyright:
years: 2016, 2018
lastupdated: "2018-06-07"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Guide détaillé 2 : Exemple détaillé d'utilisation des objets via une interface commune (bêta)
{: #scenario}

Ce scénario s'appuie sur le [guide détaillé 1 : Exemple détaillé d'utilisation des terminaux via une interface commune](../GA_information_management/ga_im_index_scenario.html).

**Important :** la fonction Objets de {{site.data.keyword.iot_full}} pour la gestion des données n'est disponible que dans le cadre d'un programme bêta limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Dans ce scénario, les terminaux de température et d'humidité publient des données d'environnement qui sont collectées dans deux salles de réunion : la salle de réunion 1 et la salle de réunion 2. Pour chaque salle de réunion, les données des terminaux de température et d'humidité sont mappées séparément à deux interfaces logiques de type de terminal. 

Dans la salle de réunion 1, les données du terminal de température associées au type de terminal *TSensor* sont mappées à l'interface logique *Thermometer Interface* et les données du terminal d'humidité associées au type de terminal *HumiditySensor* sont mappées à l'interface logique *Hygrometer Interface*. 

Dans la salle de réunion 2, les données du terminal de température associées au type de terminal TempSensor sont mappées à l'interface logique *Thermometer Interface* et les données du terminal d'humidité associées au type de terminal *HumiditySensor* sont mappées à l'interface logique *Hygrometer Interface*. 

Un type d'objet appelé *RoomType* est ensuite créé, ainsi que deux instances d'objet de salle : *meetingroom1* et *meetingroom2*.

Dans ce scénario, nous mettons en place une composition qui inclut les interfaces logiques Thermometer et Hygrometer puis nous mappons le terminal environnemental correct à chaque instance de salle. Par exemple, *tSensor* et *humiditySensor1* sont mappés à *meetingroom1*.

## Conditions prérequises
{: #pre_req}

Avant de continuer, vérifiez les points suivants :
- Utilisez la même instance d'organisation {{site.data.keyword.iot_full}} et la même clé d'API ou le même jeton pour cette organisation que ceux utilisés dans le guide détaillé 1. Pour en savoir plus sur les clés d'API et les jetons, consultez la section Authentification de la documentation [API REST HTTP pour les applications](../applications/api.html#authentication).
- Ayez deux interfaces logiques configurées, une pour un terminal de température et une pour un terminal d'humidité. Pour obtenir des informations sur la configuration d'une interface logique pour un terminal de température, consultez le [guide détaillé 1 : Exemple détaillé d'utilisation des terminaux via une interface commune](../GA_information_management/ga_im_index_scenario.html#step4). Pour obtenir des informations sur la configuration d'une interface logique pour un terminal d'humidité, voir [Informations supplémentaires concernant le guide 2 - Configuration d'une interface logique pour un terminal d'humidité](im_hygrometer.html).

## A propos de cette tâche
{: #about}

Dans {{site.data.keyword.iot_short_notm}}, un objet peut comprendre plusieurs terminaux et objets. Un type d'objet définit comment les instances d'un objet sont composées. 

Une interface logique est associée à un type d'objet. Cette association définit la structure de l'état généré pour une instance de type d'objet. Les mappages sont utilisés pour définir comment les propriétés des terminaux et des objets agrégés sont mappés aux propriétés sur un état d'objet. 

L'interface logique est utilisée pour supprimer la nécessité pour l'application de comprendre comment un terminal ou un objet est configuré. Par exemple, vous pourriez mesurer la température d'une pièce à l'aide d'un seul terminal. Vous pourriez
aussi la calculer en faisant la moyenne des mesures de plusieurs terminaux. L'application a seulement besoin de connaître
l'état (en l'occurrence, la propriété de température) d'une ou de plusieurs pièces. Peu importe comment la valeur de température reçue par l'application est calculée.

Dans ce scénario, deux terminaux de température et deux terminaux d'humidité publient des événements dans {{site.data.keyword.iot_short_notm}}. Un terminal de température et un terminal d'humidité sont dans la salle de réunion 1 d'un immeuble de bureaux. Les autres terminaux de température et d'humidité sont dans la salle de réunion 2. Le diagramme ci-dessous illustre la configuration de la salle de réunion 1 :

![Mappage entre l'objet de température et d'humidité et une application sur {{site.data.keyword.iot_short_notm}}.](images/Information Management Thing example scenario.svg "Mappage entre plusieurs capteurs environnementaux dans une salle et une application sur {{site.data.keyword.iot_short_notm}}")

Un type d'objet appelé *RoomType* est utilisé pour définir comment les instances de salles sont composées. Une interface logique est associée à *RoomType* et définit que les événements entrants sont mappés à un seul relevé qui montre à la fois la température et l'humidité. Ce relevé unique est l'état d'objet. Les mappages sont utilisés pour définir comment les propriétés des terminaux de température et d'humidité sont mappées à cet état d'objet. Lorsqu'un nouveau relevé est publié par ces terminaux, la valeur de la propriété qui est associée avec l'état d'objet est changée.

Le tableau suivant montre les quatre terminaux qui sont utilisés dans notre exemple, la rubrique sur laquelle chaque terminal publie et un exemple de contenu pour chaque terminal.

Terminal/Type | Evénement | Charge utile événement/Propriété
------------- |  ------------- | -------------
*tSensor*/TSensor (meetingroom1) | `iot-2/evt/`*`tevt`*`/fmt/json` | `{ "t" : 34.5 }`/ **temperature1**
*tempSensor*/TempSensor (meetingroom2) | `iot-2/evt/`*`tempevt`*`/fmt/json` | `{ "temp" : 34.5 }`/ **temperature2**  
*humiditySensor1*/HumiditySensor (meetingroom1) | `iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75 }`/ **humidity1**
*humiditySensor2*/HumiditySensor (meetingroom2) |`iot-2/evt/`*`humevt`*`/fmt/json` | `{  "hum" : 75}`/ **humidity2**

**Remarque :** les identificateurs d'événement *tevt*, *tempevt*, *humevt* sont requis lorsque vous mappez une propriété associée à un événement entrant de ce type à une propriété dans l'interface logique. Dans ce scénario, deux propriétés sont définies dans l'interface logique : *temperature* et *humidity*.

Une interface logique est également configurée. L'interface logique représente l'état de l'objet dans la structure suivante :
```
{
  "temperature" : <current temperature value in Celsius>
  "humidity" : <current humidity value>
}
```

Utilisez l'exemple suivant pour mettre en place votre propre environnement d'interfaces. 

**Remarque :** Un tableau répertoriant les noms, les valeurs et les identificateurs des propriétés de ressources utilisées dans ce guide est fourni dans la documentation [Propriétés de ressources et identificateurs référencés dans les guides détaillés 1 et 2](im_id_reference.html).

## Si nécessaire, ajoutez un type de terminal et un terminal  
{: #add_device}  
Dans ce scénario, trois types de terminal et quatre instances de terminal sont utilisés. L'instance de terminal *tSensor* est associée au type de terminal *TSensor*. L'instance de terminal *tempSensor* est associée au type de terminal *TempSensor*. Les instances de terminal *humiditySensor1* et *humiditySensor2* sont associées au type de terminal *HumiditySensor*.

Vous pouvez créer des types de terminal et des terminaux à l'aide du [tableau de bord {{site.data.keyword.iot_short_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://internetofthings.ibmcloud.com){: new_window} ou à l'aide des API REST.  

Pour en savoir plus sur l'utilisation du tableau de bord {{site.data.keyword.iot_short_notm}} pour ajouter des types de terminal et des terminaux, reportez-vous à la documentation [Initiation à la gestion des données via l'interface Web](../GA_information_management/im_ui_flow.html).

Pour obtenir des informations sur l'utilisation des API REST pour ajouter des types de terminal et des terminaux, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/org-admin.html#!/Device_Configuration){: new_window}.



## Etape 1 : créez un fichier schéma de type d'objet  
{: #crt_composition_file}  
Créez un fichier schéma de type d'objet faisant référence aux identificateurs d'interfaces logiques des terminaux pour les types de terminal de température et d'humidité.  

L'exemple suivant montre comment créer un fichier schéma de type d'objet appelé *roomTypeSchema*.   
```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "Room Thing Type Schema",
    "description" : "JSON Schema that defines the structure of the Room Thing Type.",
   "properties" : {
        "thermometer": {
            "type": "object",
            "description": "The thermometer device",
            "$logicalInterfaceRef": "IThermometer"
        },
        "hygrometer": {
            "type": "object",
            "description": "The hygrometer device",
            "$logicalInterfaceRef": "IHygrometer"
        }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```
**Astuce :** utilisez le paramètre **required** pour marquer une ou plusieurs propriétés comme obligatoires. Les propriétés obligatoires doivent être incluses dans un message de terminal afin que
Watson IoT Platform mette à jour l'état d'un terminal avec les données de ce terminal. Un message qui n'inclut pas de propriété obligatoire n'est pas traité.   
**Remarque :** lorsque vous créez votre type d'objet, vous devez spécifier l'identificateur de schéma qui a été généré
lorsque vous avez créé le fichier schéma de type d'objet.  

## Etape 2 : créez une ressource de schéma d'objet  
{: #crt_composition_resource}  

Créez la ressource de schéma en téléchargeant le fichier schéma du type d'objet généré à l'étape précédente.  
Téléchargez le fichier schéma du type d'objet à l'aide de l'API suivante :  
```
POST /draft/schemas
```  
Le fichier de définition de schéma est transmis à Watson IoT Platform dans un POST à plusieurs parties (données de formulaire/multiple). Le corps du POST doit contenir au moins deux parties :

  - Une appelée **schemaFile** qui contient le contenu actuel du fichier comme le corps de la partie.
  - Une appelée **name** qui contient une chaîne qui définit le nom de la ressource de schéma dans le corps de la partie.


Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Schemas).  

L'exemple suivant montre comment utiliser cURL pour créer la ressource de schéma de type d'objet :  
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/schemas \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: multipart/form-data' \
  --form name=roomTypeSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/DeviceState/thingStateDemo/setup/schemas/roomTypeSchema
```
L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "name" : "roomTypeSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil", 
  "contentType" : "application/octet-stream",
  "updated" : "2016-12-06T14:38:52Z",
  "schemaFileName" : "roomType.json",
  "version" : "draft",
  "created" : "2016-12-06T14:38:52Z",
  "id" : "5a72ea48d60180002c4f5e58",
  "refs" : {
      "content" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58/content"
  },
  "schemaType" : "json-schema",
  "updatedBy" : "a-8x7nmj-9iqt56kfil"
}
```
L'identificateur de schéma *5a72ea48d60180002c4f5e58* qui est renvoyé en réponse à la méthode POST est requis lorsque vous créez un type d'objet.


## Etape 3 : créez un type d'objet  
{: #crt_thing_type}  

Les types d'objet sont utilisés pour modéliser les instances d'objets. Un type d'objet doit être créé dans une organisation avant qu'une instance d'objet puisse être créée. Dans ce scénario, créez un seul type d'objet.  
C'est le schéma associé à un type d'objet qui définit quels types d'objets sont agrégés pour composer une instance d'un objet. Le type d'objet doit contenir une référence à la ressource de schéma de type d'objet que vous avez créée à l'étape précédente.  
Pour créer un type d'objet, utilisez l'API suivante :
```
POST /draft/thing/types
```
Les paramètres suivants sont requis dans le corps de la méthode POST :  
<table>
<tr><th>Paramètre</th><th>Description</th></tr>
<tr><td>id</td><td>Fournissez un identificateur pour le type d'objet que vous créez.</td></tr>
<tr><td>name</td><td>Fournissez un nom pour le type d'objet que vous créez.</td></tr>
<tr><td>schemaId</td><td>Identificateur créé pour la ressource du schéma de composition.</td></tr>
</table>

Pour plus de détails, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  

L'exemple suivant montre comment utiliser cURL pour créer un type d'objet appelé *RoomType*.
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/thing/types \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id" : "RoomType", "name" : "Room Thing Type", "description" : "Room Thing Type", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```

Réponse : 
```
{
 "name": "RoomType",
 "description": "Room Thing Type",
 "id": "RoomType",
 "schemaId": "5a72ea48d60180002c4f5e58",
 "metadata": {},
  "refs": {
    "schema": "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58",
    "mappings": "/api/v0002/draft/thing/types/RoomType/mappings",
    "logicalInterfaces": "/api/v0002/draft/thing/types/RoomType/logicalinterfaces"
   },
 "version": "draft",
 "created": "2018-02-01T10:22:43Z",
 "createdBy": "ANOther",
 "updated": "2018-02-01T10:22:43Z",
 "updatedBy": "ANOther"
}
```

## Etape 4 : créez un fichier schéma d'interface logique  
{: #crt_ai_schema_file}
Dans votre interface logique, vous pouvez définir la structure des données stockées comme l'état d'objet. Dans ce scénario, créez une interface logique qui définit les propriétés temperature et humidity. Associez l'interface logique au type d'objet *RoomType* en référençant l'identificateur d'interface logique qui est généré lorsque vous créez la ressource d'interface logique.  
L'exemple suivant montre comment créer un fichier schéma d'interface logique appelé *roomSchema*.

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type" : "object",
    "title" : "roomSchema",
    "description" : "JSON Schema that defines the canonical room state structure",
    "properties" : {
        "temperature" : {
            "description" : "Temperature in degrees celsius",
           "type" : "number",
           "minimum" : -273.15,
           "default" : 0.0
        },
       "humidity" : {
            "description" : "Percentage humidity",
           "type" : "number",
           "minimum" : 0,
           "maximum" : 100,
           "default" : 0.0
       }
    },
   "required" : [
        "temperature",
       "humidity"
    ]
}
```  
## Etape 5 : créez une ressource de schéma d'interface logique  
{: #crt_ai_schema_resource}  
Téléchargez le fichier schéma d'interface logique que vous avez créé à l'étape précédente pour créer une ressource de schéma d'interface logique pour votre type d'objet à l'aide de l'API suivante :  
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
  --form name=roomSchema \
  --form 'schemaFile=@"/Users/ANOther/Documents/IoT/ThingState/thingStateDemo/setup/schemas/room.json"'
```
L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "created" : "2016-12-06T16:51:14Z",
  "name" : "roomSchema",
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "updated" : "2016-12-06T16:51:14Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "schemaType" : "json-schema",
  "contentType" : "application/octet-stream",
  "schemaFileName" : "room.json",
  "version" : "draft",
  "refs" : {
    "content" : "/api/v0002/draft/schemas/5a4b9847d60180002efce645/content"
  },
  "id" : "5a4b9847d60180002efce645"
}
```
Utilisez l'identificateur de schéma *5a4b9847d60180002efce645* qui est renvoyé en réponse à la méthode POST pour ajouter la ressource de schéma d'interface logique à l'interface logique de votre type d'objet.  


## Etape 6 : créez une interface logique pour le type d'objet  
{: #crt_thing_ai}  
L'interface logique doit faire référence à la ressource de schéma d'interface logique que vous avez créée à l'étape précédente.  
Pour créer une interface logique, utilisez l'API suivante :  
```
POST draft/logicalinterfaces
```  
Si vous le souhaitez, vous pouvez spécifier un alias significatif pour votre interface logique. L'alias peut être référencé dans l'appel API ou dans l'abonnement à la chaîne de rubrique utilisée pour extraire l'état d'un objet, au lieu d'utiliser l'identificateur d'interface logique auto-généré.

**Remarque :** l'alias doit avoir une longueur comprise entre 1 et 36 caractères et peut inclure des caractères alphanumériques, des traits d'union, des points et des traits de soulignement. L'alias ne peut pas être une chaîne hexadécimale de 24 caractères.

Dans ce scénario, utilisez l'identificateur de schéma *5a4b9847d60180002efce645* qui a été renvoyé dans la réponse précédente pour ajouter le schéma d'interface logique à l'interface logique.

L'exemple suivant montre comment utiliser cURL pour créer une interface logique avec l'alias *IRoom* :
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"name" : "IRoom", "alias" : "IRoom", "schemaId" : "5a72ea48d60180002c4f5e58"}'
```
L'exemple suivant illustre une réponse à la méthode POST :
```
{
  "createdBy" : "a-8x7nmj-9iqt56kfil",
  "refs" : {
      "schema" : "/api/v0002/draft/schemas/5a72ea48d60180002c4f5e58"
  },
  "schemaId" : "5a72ea48d60180002c4f5e58",
  "created" : "2016-12-06T16:53:27Z",
  "updatedBy" : "a-8x7nmj-9iqt56kfil",
  "id" : "5a4b9847d60180002efce645",
  "updated" : "2016-12-06T16:53:27Z",
  "name" : "IRoom",
  "alias" : "IRoom",
  "version" : "draft"
}
```
Dans ce scénario, utilisez l'identificateur d'interface logique *5a4b9847d60180002efce645* qui est renvoyé dans la réponse à la méthode POST  pour ajouter votre interface logique à votre type de terminal. Vous pouvez également utiliser cet identificateur pour mapper un événement de terminal entrant à une propriété définie par l'interface logique.

Pour plus d'informations, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Logical_Interfaces).  


## Etape 7 : ajoutez l'interface logique au type d'objet  
{: #add_thing_ai}  
Pour ajouter une interface logique à un type d'objet, utilisez l'API suivante :  
```
POST draft/thing/types/{thingtypeId}/logicalinterfaces
```  

Pour plus de détails, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  
Dans ce scénario, l'interface logique est associée au type d'objet *RoomType*.

L'exemple suivant montre comment utiliser cURL pour ajouter l'interface logique d'objet *IRoom* au type d'objet *RoomType* :  
```
{   
  "id": "5a4b9847d60180002efce645"
}
```
```
curl --request POST \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/logicalinterfaces \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{"id": "5a4b9847d60180002efce645"}'
```

L'exemple suivant illustre une réponse à la méthode POST :
```
{
 "name": "Room Logical Interface",
 "description": "This is a Room logical interface",
 "id": "5a4b9847d60180002efce645",
 "schemaId": "5a4b9817d60180002efce644",
 "refs": {
   "schema": "/api/v0002/draft/schemas/5a4b9817d60180002efce644"
 },
 "version": "draft",
 "created": "2018-01-02T14:33:43Z",
 "createdBy": "ANOther",
 "updated": "2018-01-02T14:33:43Z",
 "updatedBy": "ANOther"
}
```

## Etape 8 : définissez les mappages
{: #define_Thing_type_mappings}

Définissez les mappages pour le type d'objet qui décrivent comment mapper des propriétés de l'état des terminaux ou des objets agrégés aux propriétés qui sont définies sur l'interface logique du type d'objet.

Pour mapper des événements, utilisez l'API suivante :  
```
POST draft/thing/types/{thingtypeId}/mappings
```  

où *thingtypeId* est l'identificateur qui est renvoyé en réponse à la demande POST lorsque le type d'objet est créé. 

Les paramètres suivants sont requis dans le corps de la demande POST :  
<table>
<tr>
<th>	Paramètre	</th><th>	Description	</th>
</tr>
<tr>
<td>	propertyMappings	</td><td>	 Structure JSON valide qui mappe les propriétés définies pour l'interface logique aux propriétés du contenu de l'événement de terminal.	</td>
</tr>
<tr>
<td>	logicalInterfaceId	</td><td>	Identificateur d'interface logique requis dans le corps du contenu.	</td>
</tr>
</table>  

Pour plus de détails, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).

L'exemple suivant montre comment utiliser cURL pour ajouter un mappage au type d'objet *RoomType* :

```
{
  "logicalInterfaceId": "5a4b9847d60180002efce645",
  "notificationStrategy": "on-state-change",
  "propertyMappings": {
       "thermometer": {
         "temperature": "$event.temperature"
       },
       "hygrometer": {
         "humidity": "$event.humidity"
       }
   },
}
```  
Le terminal *thermometer* est défini dans [roomTypeSchema](#crt_composition_file). La propriété *$event.temperature* est définie dans le schéma d'interface logique avec l'identificateur *5846ed076522050001db0e12* et l'alias *IThermometer*.  
Le terminal *hygrometer* est défini dans [roomTypeSchema](#crt_composition_file). La propriété *$event.humidity* est définie dans le schéma d'interface logique avec l'identificateur *5846cd7c6522050001db0e24* et l'alias *IHygrometer*.


## Etape 9 : validez et activez la configuration
{: #activate}

Validez et activez la configuration en relation avec la mise à jour de l'état d'objet de chaque type d'objet. Cette configuration inclut vos schémas, interfaces logiques et mappages. 

Pour valider et activer votre configuration de type d'objet, utilisez l'API suivante :
```
PATCH /draft/thing/types/{thingTypeId}
```
où *thingTypeId* est l'identificateur de type d'objet. 

L'exemple suivant montre comment utiliser cURL pour valider et activer la configuration associée au type d'objet *RoomType* :
```

curl --request PATCH \
  --url https://yourOrgID.internetofthings.ibmcloud.com/api/v0002/draft/device/types/RoomType \
  --header 'authorization: Basic MK2fdJpobP6tOWlhgTR2a4Hklss2eXC7AZIxZWxPL9B8XlVwSZL=' \
  --header 'content-type: application/json' \
  --data '{
            "operation" : "activate-configuration"
          }'
```

L'exemple suivant illustre une réponse à la méthode PATCH :
```
{
 "message": "CUDIM0300I: State update configuration for Thing Type 'Room Thing Type' has been successfully submitted for  activation.",
"details": {
   "id": "CUDIM0300I",
   "properties": [
     "Thing Type",
     "Room Thing Type"
   ]
 },
 "failures": []
}
```

## Etape 10 : créez une instance d'un type d'objet
{: #create_Thing_instances}

Un objet est une instance d'une type d'objet. Un objet vous permet d'agréger une ou plusieurs instances d'un terminal ou d'un objet ensemble dans un seul objet.

Pour créer un objet, utilisez l'API suivante :

```
POST /thing/types/{thingTypeId}/things
```
où *thingtypeId* est l'identificateur qui est renvoyé en réponse à la demande POST lorsque le type d'objet est créé. 

Pour plus de détails, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Things).

Dans ce scénario, nous devons créer des instances d'objet ayant pour type d'objet *RoomType*. Une instance d'objet est appelée *meetingroom1* et l'autre *meetingroom2*.

L'exemple suivant montre comment utiliser cURL pour créer une instance d'objet appelée *meetingroom1*. L'instance d'objet *meetingroom1* est associée aux instances de terminal *tSensor* et *humiditySensor1*.

```
thingId = "meetingroom1"
 meetingroom1AggregatedObjects = {
   "thermometer": {
     "type": "device",
     "typeId": "TSensor",
     "id": "tSensor"
   },
   "hygrometer": {
     "type": "device",
     "typeId": "HumiditySensor",
     "id": "humiditySensor1"
   }
 }
``` 

L'identificateur d'objet qui est créé est utilisé dans l'URL de la méthode POST qui est appelée pour ajouter un événement de température à l'interface logique d'objet.

L'exemple suivant montre comment utiliser cURL pour créer une instance d'objet appelée *meetingroom2*. *meetingroom2* est associé aux instances de terminal *tempSensor* et *humiditySensor2*.

```
thingId = "meetingroom2"
   meetingroom2AggregatedObjects = {
    "thermometer": {
      "type": "device",
      "typeId": "TempSensor",
      "id": "tempSensor"
    },
    "hygrometer": {
      "type": "device",
      "typeId": "HumiditySensor",
      "id": "humiditySensor2"
    }
  }

``` 

## Etape 11 : vérifiez que les événements de terminal mappés sont publiés dans l'interface logique  
{: #publish_event}  
Publiez les événements suivants pour les terminaux qui sont agrégés dans l'objet *meetingroom1* :  
 - un événement de température de *tSensor* sur la rubrique `iot-2/evt/tevt/fmt/json`  
 - un événement d'humidité de *humiditySensor1* sur la rubrique `iot-2/evt/humevt/fmt/json`  
 
Publiez les événements suivants pour les terminaux qui sont agrégés dans l'objet *meetingroom2* :  
 - un événement de température de *tempSensor* sur la rubrique `iot-2/evt/tempevt/fmt/json`  
 - un événement d'humidité de *humiditySensor2* sur la rubrique `iot-2/evt/humevt/fmt/json`  
 
Pour plus d'informations sur la publication d'un événement entrant provenant d'un terminal,
consultez [Connectivité MQTT pour les applications](../applications/mqtt.html#publishing_device_events).  

## Etape 12 : vérifiez que l'état de l'objet a changé  
{: #verify_Thing_state}  

Vous pouvez extraire l'état de l'objet à l'aide des API REST HTTP ou en vous abonnant à une rubrique.

Si vous possédez une application client MQTT, vous pouvez vous abonner à la chaîne de rubrique suivante :
```
iot-2/type/${thingTypeId}/id/$thingId/intf/${logicalInterfaceId}/evt/state
``` 

Vous pouvez également extraire le dernier état d'objet à l'aide de l'API REST HTTP suivante :

```
GET /thing/types/{thingTypeId}/things/{thingId}/state/{logicalInterfaceId}
```  

Les paramètres suivants sont requis :  
<table>
<tr>
<th>Paramètre	</th><th>	Description</th>
</tr>
<tr>
<td>thingTypeId	</td><td>Identificateur du type d'objet.</td>
</tr>
<tr>
<td>thingId	</td><td>	Identificateur d'objet.</td>
</tr>
<tr>
<td>logicalInterfaceId</td><td>Identificateur créé pour l'interface logique.</td>
</tr>
</table>  

Pour plus de détails, consultez la documentation [{{site.data.keyword.iot_short_notm}} HTTP REST API](https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/state-mgmt.html#!/Thing_Types).  


## Etapes suivantes

Créez des règles que vous pouvez utiliser pour initier une action lorsqu'un événement reçu par {{site.data.keyword.iot_short_notm}} entraîne un changement dans l'état du terminal ou de l'objet. Pour obtenir des informations sur la création de règles, voir [Création de règles imbriquées (bêta)](im_rules.html).
