---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-09"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Comprendre le langage d'expression de mappage
{: #mapping_expression}

Vous pouvez utiliser le langage d'expression de mappage pour manipuler et combiner des données, et pour formater les résultats des requêtes que vous pouvez exécuter sur les données ayant été traitées. Le langage d'expression de mappage est un sous-ensemble de [JSONata ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://docs.jsonata.org/index.html){:new_window} et peut être utilisé lors de la définition de [mappages](ga_im_definitions.html#definitions_resources). JSONata est un langage de requête et de transformation simple des données JSON.

Les informations ci-dessous présentent les opérateurs et les fonctions clés actuellement pris en charge, ainsi que des exemples de leur utilisation. 

## Opérateur JSONata pris en charge
{: #operators}

Les opérateurs JSONata du sous-ensemble suivant sont pris en charge : 

Type d'opérateur                   | Opérateurs pris en charge     | Notes   
------------- | ------------- | -------------
Arithmétique | *+* - / * % | L'opérateur % renvoie le reste.
Comparaison | < <= > >= != = | L'opérateur d'égalité est = , tout comme dans JSONata.
Booléen | *in*, *and* | Les constantes booléennes sont *true* ou *false*.
Conditionnel ternaire | ? | L'opérateur ? évalue une des deux expressions alternatives basées sur le résultat d'une condition de test. L'opérateur prend la forme suivante *expression*  ? *value_if_true* : *value_if_false*
Chaîne | & | L'opérateur & joint les valeurs de chaîne des opérandes en une seule chaîne résultante.
Générateur de séquence | .. | Crée un tableau d'entiers croissants, en commençant par le nombre sur le membre de gauche et en finissant par le nombre sur le membre de droite, par exemple, [1..4] -> [1,2,3,4]. Les opérandes doivent avoir pour résultat un entier. Le générateur de séquence peut uniquement être utilisé au sein d'un constructeur de tableau [].
Autre | . | L'opérateur point est utilisé pour l'accès aux objets avec une clé littérale, par exemple, $event.object.hh. *
e:* L'expression de la partie gauche est soumise à une propriété spécifique, soit dans l'événement ($event), ou dans l'état ($state) ou dans les métadonnées ($instance).
**Remarques :** 
- Utilisez des parenthèses ( ) pour regrouper des expressions et pour modifier la priorité de l'opérateur
- Utilisez des guillemets simples pour entourer les noms de propriétés qui contiennent des espaces, par exemple $event.object.'a b' 

## Extension du langage d'expression de mappage

Le langage d'expression de mappage a été étendu à des fins d'utilisation de la fonction de gestion des données grâce à l'introduction des variables $event, $state et $instance. JSON est lié à ces variables avant que l'expression ne soit évaluée. Le tableau ci-dessous offre une vue d'ensemble de ces variables, qui sont définies pour être utilisées dans des expressions :

Variable                   | Exemple d'entrée JSON     | Exemple d'expression    | A utiliser pour...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Sélectionner une propriété d'un événement entrant à utiliser dans une expression. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Sélectionner une propriété sur l'état du terminal à utiliser dans une expression.
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Sélectionner un attribut de terminal ou de type de terminal à utiliser dans une expression. 

Vous pouvez également définir une expression qui combine l'utilisation de ces variables. Dans l'exemple suivant, une propriété *temp_adjustment* est définie dans les métadonnées du terminal et est utilisée pour calibrer le relevé d'événement. La propriété est définie dans un mappage, mais peut être appliquée à plusieurs terminaux. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## Fonctions JSONata prises en charge
{: #functions}
Les fonctions JSONata du sous-ensemble suivant sont prises en charge : 

Type de fonction                   |Fonction                   | Description | Exemple
------------- | ------------- | ------------- 
Chaîne | $substring(string, start_index[, length]) | Sous-chaîne de chaîne, par exemple, *$substring("Hello World", 3, 5) => "lo Wo"*. 
Chaîne | $string(arg) | Transtype l'argument une une valeur de chaîne, par exemple, *$string(2) => "2"*.
Numérique | $number(arg) | Transtype l'argument en une valeur numérique si possible, par exemple, *$number(2) => 2*.
Numérique | $sum(array) | Renvoie la somme arithmétique d'un tableau de nombres, par exemple, *([1,2,3]) = 6*.
Numérique | $average(array) | Renvoie la valeur moyenne d'un tableau de nombre, par exemple, *([1,2,3]) = 2.0*.
Booléen | $exists(expression) | Renvoie *true* si la propriété existe dans l'expression, sinon *false*.
Tableau | $count(array) | Renvoie le nombre d'éléments dans le paramètre de tableau, par exemple, *([1,2,3,4]) = 4*. Si le paramètre de tableau n'est pas un tableau, mais plutôt une valeur d'un autre type JSON, alors le paramètre est considéré comme un tableau singleton qui contient cette valeur, et la fonction renvoie *1*.
Tableau| $append(array1, array2) | Renvoie un tableau qui contient les valeurs de *array1*, suivies par les valeurs de *array2*, par exemple, *([1,2], [3,4]) = [1,2,3,4]*. Si l'un des paramètres n'est pas un tableau, il est considéré comme un tableau singleton contenant cette valeur, par exemple *$append("Hello", "World") => ["Hello", "World"]*.

## Tableaux
Utilisez les tableaux JSON pour organiser une collection de valeurs dans un ordre donné. Les tableaux associent chaque valeur du tableau à un index ou à une position. Pour gérer les valeurs individuelles d'un tableau, placez l'index entre crochets après le nom de zone du tableau. Si les crochets contiennent un nombre ou une expression qui a pour résultat un nombre, ce nombre constitue l'index de la valeur à sélectionner. Un tableau de nombres peut également être utilisé comme index, par exemple *["a","b","c"][[1,2]] -> ["b", "c"]*. Le tableau *[1,2]* est utilisé comme un index identifiant les valeurs à sélectionner dans le tableau *["a","b","c"]*. 

Les index ont un décalage du point zéro, ce qui signifie que la première valeur du tableau *arr* est *arr[0]*, par exemple, *[1,2,3][0] -> 1*. Si le nombre n'est pas un entier, il est arrondi au nombre entier inférieur, par exemple *[1,2,3][0.9] -> [1]*.

Utilisez des index négatifs pour compter à partir de la fin du tableau, par exemple, *[1,2,3][-1] -> 3*. 

Si l'index spécifié dépasse la taille du tableau, aucune valeur n'est sélectionnée.

## Construction de la sortie
Vous pouvez indiquer la façon dont les données traitées sont affichées dans la sortie en utilisant des constructeurs de tableau ou d'objet.

### Constructeurs de tableau pris en charge
Les tableaux peuvent être construits en plaçant entre crochets [] une liste de littéraux ou d'expressions séparés par des virgules. Les virgules sont utilisées pour séparer plusieurs expressions au sein du constructeur de tableau, par exemple, *[1, 3-1] = [1,2]*.

### Constructeurs d'objet pris en charge
{: #constructors}
Vous pouvez construire des objets JSON dans la sortie en utilisant des accolades {} qui contiennent les valeurs clés ou des paires séparées par des virgules, chaque clé et valeur étant séparée par un double point, par exemple, *{key1 : value1, key2:value2}* ou *{"hello" : "world"}*. La clé d'objet doit être une chaîne.  


## Exemple : Utilisation de tableaux pour traiter et communiquer les données de température
Les sections ci-dessous reposent sur l'exemple de la section [Initiation à la gestion de données](ga_im_example.html) afin d'illustrer la façon dont vous pouvez utiliser des tableaux pour conserver une fenêtre dynamique des relevés de température et calculer la somme ou la moyenne actuelle du relevé dans cette fenêtre. 

Une fenêtre dynamique stocke les données dans leur ordre d'arrivée. Au lieu de conserver toutes les données insérées depuis le début, les fenêtres dynamiques peuvent être configurées de manière à supprimer des données de manière incrémentielle. Lorsqu'une fenêtre dynamique est saturée, toutes les prochaines insertions entraînent la suppression des données les plus anciennes de cette fenêtre.

L'exemple ci-dessous illustre une fenêtre dynamique configurée avec une stratégie d'expulsion basée sur la valeur 5 :
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

L'exemple ci-dessous illustre la façon dont la configuration du fichier schéma de l'interface logique décrite à l'étape 7 du [Guide détaillé](ga_im_index_scenario.html#step4), peut être modifiée en ajoutant un tableau appelé *tempReadings*. Le tableau est utilisé pour conserver dans la fenêtre les 5 valeurs les plus récentes envoyées à partir des terminaux pour les 5 derniers événements. Si aucune valeur n'est stockée dans *tempReadings*, le tableau est défini sur [0] et le prochain relevé reçu est ajouté au tableau qui s'agrandit jusqu'à réception de 5 relevés. Une fois les 5 relevés reçus, tout nouveau relevé entraîne la suppression du relevé le plus ancien de la fenêtre. 

**Important :** Vous devez définir *tempReadings* comme étant "obligatoire" et comme tableau par défaut. 

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
**Remarque :** Le tableau *$state.tempReadings* est recalculé avant d'être utilisé dans les fonctions $average et $sum. Cette opération est nécessaire pour garantir que le tableau contient les valeurs à jour lorsque l'expression *tempAverage* ou *tempSum* est évaluée, l'ordre des expressions de mappage n'étant pas contrôlé.
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

