---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connaissance du langage d'expression de mappage
{: #mapping_expression}

Vous pouvez utiliser le langage d'expression de mappage pour manipuler et combiner des données, ainsi que pour formater les résultats des requêtes que vous exécutez sur les données que vous avez traitées. Le langage d'expression de mappage est un sous-ensemble de [JSONata ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/index.html){:new_window} et peut être utilisé lorsque vous définissez des [mappages](ga_im_definitions.html#resources) ou lorsque vous créez des [règles](../information_management/im_rules.html). 

JSONata est un langage de requête et de transformation simple des données JSON. L'outil [JSONata Exerciser ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://try.jsonata.org/){:new_window} offre également un moyen rapide et pratique d'essayer JSONata.

Les informations ci-dessous présentent les opérateurs et les fonctions clés actuellement pris en charge, ainsi que des exemples de leur utilisation. 

## Opérateurs JSONata
{: #operators}

Tous les [opérateurs JSONata![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/operators.html){:new_window} sont pris en charge à l'exception des opérateurs suivants :

- ~> (chaînage de fonctions)
- ^(…) (trier par)
- := (liaison de variables)

**Remarque :** 
- Utilisez des parenthèses ( ) pour regrouper des expressions et pour modifier la priorité de l'opérateur
- Utilisez les guillemets simples ou doubles pour entourer les chaînes et les noms de propriétés, par exemple $event.object.'ab' 
- Utilisez les apostrophes obliques pour entourer les noms de propriétés qui contiennent des caractères spéciaux, y compris les espaces, par exemple  
```
{"x y":22}.`x y`
```
- Utilisez les apostrophes obliques pour entourer les noms de propriétés qui commencent par un chiffre, par exemple
```
`7emperature`
```

## Fonctions JSONata 
{: #functions}
Les fonctions JSONata suivantes sont prises en charge : 

 - Toutes les fonctions [de chaînes ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/string-functions.html){:new_window} 
 - Toutes les fonctions [numériques ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/numeric-functions.html){:new_window} 	
 - Toutes les fonctions [de regroupement numérique ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 
 - Toutes les fonctions [booléennes ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/boolean-functions.html){:new_window}   
 - Les fonctions [des tableaux $count et $append![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/array-functions.html){:new_window} 	

## Extension du langage d'expression de mappage

Le langage d'expression de mappage a été étendu à des fins d'utilisation de la fonction de gestion des données grâce à l'introduction des variables $event, $state et $instance. JSON est lié à ces variables avant que l'expression ne soit évaluée. Le tableau ci-dessous offre une vue d'ensemble de ces variables, qui sont définies pour être utilisées dans des expressions :

Variable                   | Exemple d'entrée JSON     | Exemple d'expression    | A utiliser pour...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Sélectionner une propriété d'un événement entrant à utiliser dans une expression. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Sélectionner une propriété sur l'état du terminal à utiliser dans une expression.
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Sélectionner un attribut de terminal ou de type de terminal à utiliser dans une expression. 

L'exemple suivant utilise l'objet suivant comme entrée dans un événement :  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
L'objet est converti en un tableau à l'aide de l'expression suivante :

```
  [$event.temperature, $event.humidity, $event.pressure]
```  
L'expression a pour résultat la génération de la sortie suivante :
```
 [
    35,
    72,
    1024
  ]
```

Vous pouvez inverser cet exemple et convertir un tableau à partir de l'entrée à un objet. L'exemple suivant utilise le tableau suivant comme entrée dans un événement :
```
{
    "readings": [
      35,
      72,
      1024
    ]
  }
```
Le tableau est converti en objet à l'aide de l'expression suivante :
```
 {"temperature": $event.readings[0], "humidity": $event.readings[1], "pressure": $event.readings[2]}
```
L'expression a pour résultat la génération de la sortie suivante :
```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
Vous pouvez également définir une expression qui combine l'utilisation de ces variables. Dans l'exemple suivant, une propriété *temp_adjustment* est définie dans les métadonnées du terminal et est utilisée pour calibrer la lecture d'événement. La propriété est définie dans un mappage, mais peut être appliquée à plusieurs terminaux. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


L'opérateur point "." est utilisé pour l'accès aux objets avec une clé littérale, par exemple $event.object.hh. L'expression sur le côté gauche est soumise à une propriété spécifique, soit dans l'événement ($event), soit dans l'état ($state), soit dans les métadonnées ($instance). L'expression sur la droite contient les informations auxquels vous souhaitez éventuellement accéder.

Pour en savoir plus sur l'opérateur point, voir la section [Operateurs ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/operators.html){:new_window} de la documentation JSONata.


## Guide des langages

- Toutes les [sélections de base ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/basic.html){:new_window} sont prises en charge.
- Toutes les [sélections complexes ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/complex.html){:new_window} sont prises en charge à l'exception des caractères génériques.
- Les expressions conditionnelles et entre parenthèses sont prises en charge dans le cadre de [constructions de programmation ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.jsonata.org/programming.html){:new_window}. Les variables sont prises en charge à travers l'utilisation des variables $instance, $state et $event dans le cadre du langage de mappage étendu spécifique à la gestion de données de Watson IoT Platform. Les variables "$" et "$$" qui sont utilisées dans JSONata ne sont pas prises en charge actuellement.
## Sortie de construction
Vous pouvez spécifier comment les données traitées sont présentées dans la sortie à l'aide de constructeurs de tableau et de constructeurs d'objet.

### Constructeurs de tableau pris en charge
Pour construire des tableaux, vous pouvez joindre une liste de littéraux ou expressions séparés par des virgules dans une paire de crochets []. Les virgules sont utilisées pour séparer les multiples expressions au sein du constructeur de tableau incluant la génération de séquence, par exemple, *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* et *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### Constructeurs d'objet pris en charge
{: #constructors}
Vous pouvez construire des objets JSON dans votre sortie en utilisant une paire d'accolades {} contenant des valeurs clé ou des paires séparées par des virgules, avec chaque clé et valeur séparée par deux points, par exemple, *{key1 : value1, key2:value2}* ou *{"hello" : "world"}*. La clé d'objet doit être une chaîne.  


## Exemple : Utilisation de tableaux pour traiter et générer des rapports sur les données de température
Les sections suivantes s'appuient sur l'exemple fourni dans [Initiation à la gestion des données via les API REST](ga_im_example.html) pour montrer comment vous pouvez utiliser des tableaux pour maintenir une fenêtre dynamique des relevés de température et calculer la somme ou la moyenne actuelle du relevé contenu dans cette fenêtre.

Une fenêtre dynamique stocke les données dans leur ordre d'arrivée. Au lieu de conserver toutes les données insérées depuis le début, les fenêtres dynamiques peuvent être configurées de manière à supprimer des données de manière incrémentielle. Lorsqu'une fenêtre dynamique est saturée, toutes les prochaines insertions entraînent la suppression des données les plus anciennes de cette fenêtre.

L'exemple ci-dessous illustre une fenêtre dynamique configurée avec une stratégie d'expulsion basée sur la valeur 5 :
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

L'exemple suivant montre comment la configuration du fichier schéma de l'interface logique montré à l'étape 7 du [guide détaillé 1](ga_im_index_scenario.html#step4), peut être modifié en ajoutant un tableau appelé *tempReadings*. Le tableau est utilisé pour conserver dans la fenêtre les 5 valeurs les plus récentes envoyées à partir des terminaux pour les 5 derniers événements. Si aucune valeur n'est stockée dans *tempReadings*, le tableau est défini sur [0] et le prochain relevé reçu est ajouté au tableau qui s'agrandit jusqu'à ce que 5 relevés soient reçus. Une fois les 5 relevés reçus, tout nouveau relevé entraîne la suppression du relevé le plus ancien de la fenêtre. 

**Remarques importantes :** vous devez définir *tempReadings* comme étant "obligatoire" et comme tableau par défaut.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature" : {
          "description" : "latest temperature reading in degrees Celsius",
          "type" : "number",
          "minimum" : -273.15,
          "default" : 0.0
      },
      "tempAverage": {
          "type": "number"
      },
      "tempReadings": {
          "default": [],
          "items": {
              "type": "number"
          },
         "type": "array"
      },
      "tempSum": {
          "type": "number"
      }
  },
   "required" : [
      "tempReadings"
  ],
  "type": "object"
}
```
La section ci-dessous illustre un exemple de la façon dont vous pouvez configurer la ressource de mappages pour calculer le relevé de température moyenne et la somme de tous les relevés de température basés sur les relevés contenus dans la fenêtre dynamique en cours :


```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt" : {
               "tempAverage": "$average($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))",
               "tempReadings": "$count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t)",
               "tempSum": "$sum($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))"
           }
       },
       "updated": "2017-10-13T10:05:40Z",
       "updatedBy": "a-8x7nmj-9iqt56kfil",
       "version": "active"
   }
]
```
**Remarque :** Le tableau *$state.tempReadings* est recalculé avant d'être utilisé dans les fonctions $average et $sum. Cette opération est nécessaire pour garantir que le tableau contient les valeurs en cours lorsque l'expression *tempAverage* ou *tempSum* est évaluée, car l'ordre des expressions de mappage ne peut pas être contrôlé.

L'exemple suivant illustre la moyenne et la somme des températures d'une fenêtre dynamique qui comporte 5 relevés de température :
```
{
    "state": {
        "tempAverage": 18557.6,
        "tempReadings": [
            17591,
            10262,
            25621,
            16676,
            22638
        ],
        "tempSum": 92788
    },
    "timestamp": "2017-10-13T11:07:20Z",
    "updated": "2017-10-13T11:05:40Z"
}
```

## Gestion des non concordances entre l'expression de mappage et les données d'entrée

Une mise à jour d'état peut échouer lorsque l'une des expressions de mappage contient une référence aux données d'entrée qui n'est pas spécifiée dans l'événement publié.

Par exemple, vous pouvez configurer les expressions suivantes :
```
temperature = $event.t 
humidity = $event.hum
```
où *t* est une propriété facultative de l'événement.

Si un événement contenant uniquement des données d'humidité est reçu, par exemple, `{"humidity":22}`, l'évaluation de l'expression `temperature = $event.t` échoue car la propriété facultative *t* n'est pas spécifiée dans l'événement du terminal publié.

La mise à jour de l'état échoue. La propriété de l'état d'humidité n'est pas mise à jour et un message d'erreur est publié dans la rubrique d'erreur MQTT du terminal :
```
iot-2/type/${typeId}/id/${deviceId}/err/data
```
Pour éviter les erreurs de mise à jour d'état lorsque les données facultatives ne sont pas spécifiées, vous pouvez utiliser la fonction $exists en combinaison avec un conditionnel ternaire pour spécifier une valeur par défaut pour les propriétés facultatives. L'exemple suivant définit une valeur par défaut de *0* pour la propriété *t* :

```
"tempEvent:
    {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

En définissant une valeur par défaut pour la propriété optionnelle de cette façon, l'expression est évaluée correctement, même lorsque la propriété *t* n'est pas spécifiée dans l'événement de terminal publié.

Par conséquent, si l'événement `{"humidity":22}` est reçu, la mise à jour de l'état s'effectue correctement et l'état du terminal est défini sur `{"humidity":22, "temperature":0}`.
