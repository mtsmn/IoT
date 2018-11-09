---

copyright:
years: 2017, 2018
lastupdated: "2018-03-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Informations de référence relatives à la gestion des données

Utilisez les informations de référence suivantes pour connaître les restrictions qui s'appliquent lorsque vous utilisez le composant de gestion de données de {{site.data.keyword.iot_full}}. 

## Restrictions de schéma

Les types de schéma suivants sont utilisés dans la gestion des données :
- Schéma de type d'événement
- Schéma d'interface logique
- Schéma de type d'objet

Le tableau suivant répertorie les restrictions qui s'appliquent à chaque type de schéma :

Type de schéma       |        Contrainte
------------------- | -------------
Tous        | Tous les schémas doivent être conformes à un JSON valide. 
Tous        | Tous les schémas doivent être conformes à un schéma JSON valide. 
Tous        | La racine de tous les schémas doit être de type "object". 
Tous        | Tous les schéma ont un maximum de sept niveaux d'imbrication.
Interface logique |  Toutes les propriétés qui sont spécifiées comme obligatoires doivent avoir une valeur par défaut définie. 
Interface logique | Si un objet est défini dans le schéma, au moins une propriété doit être définie pour cet objet. 
Interface logique | Si un tableau est défini dans le schéma, les éléments associés doivent correspondre à un seul type, par exemple au type "string" uniquement. 
Type d'objet      | Seules les propriétés de niveau supérieur sont autorisées. Aucune imbrication n'est autorisée au-delà du premier niveau. 
Type d'objet      | La propriété de niveau supérieur doit être de type "object".
Type d'objet      | La propriété de niveau supérieur doit faire référence à un $logicalInterfaceRef et à un type. La valeur de $logicalInterfaceRef est l'identificateur ou le nom d'alias d'une interface logique. Le type doit être défini sur "object" ou "array". 

## Exemple de schémas valides et non valides

### Exemple 1  - définition d'un schéma d'interface logique valide
L'exemple suivant définit un schéma d'interface logique conforme aux contraintes décrites à la section "Restrictions de schéma" :

  - Le schéma est conforme à un format JSON et à un schéma JSON valide.
  - Le schéma appartient au type "object".
  - Le schéma a un niveau d'imbrication inférieur à sept. 
  - Deux propriétés sont définies pour le schéma. 
  - Les propriétés obligatoires **a** et **b** ont des valeurs par défaut spécifiées.

```
{
 "type": "object",
 "properties":{
    "a":{
       "type":"string"
    },
    "b":{
       "type": "number"
      }
  },
  "default":{"a":"HelloWorld", "b":2},
  "required": ["a", "b"]
}
```


### Exemple 2 - Définition d'un schéma d'interface logique non valide
L'exemple suivant montre un schéma d'interface logique non valide. Le schéma n'est pas valide car le tableau est défini pour contenir des éléments de type "string" ou de type "number". Si un tableau est défini dans le schéma, les éléments associés doivent appartenir à un seul type, par exemple "string".

```
{
 "type": "object",
 "properties":{
    "a":{
      "type":"array",
       "items":{
          "type": ["string", "number"]
       }
    }
  }
}
```
### Exemple 3 - Définition d'un schéma de type d'objet valide
L'exemple suivant définit un schéma de type d'objet conforme aux contraintes décrites dans la section "Restrictions des schémas" :

  - Le schéma est conforme à un format JSON et à un schéma JSON valide.
  - Le schéma appartient au type "object".
  - Le schéma a un niveau d'imbrication inférieur à sept. 
  - Le schéma définit uniquement les propriétés de niveau supérieur. 
  - Les propriétés de niveau supérieur font référence à $logicalInterfaceRef et à un type qui est défini sur "array" ou sur "object". Le type "array" peut être utilisé pour agréger un certain nombre de terminaux ou d'objets, par exemple, un certain nombre de capteurs de température. Le type "object" peut être utilisé pour faire référence à un terminal ou à un objet unique, par exemple, un capteur d'humidité unique.   
  - Les propriétés obligatoires n'ont pas besoin de spécifier une valeur par défaut. Cette contrainte s'applique uniquement aux schémas d'interface logique. Une valeur par défaut ne peut pas être spécifiée dans ce type de schéma. 

```
{
   "type" : "object",
   "properties" : {
       "temperatureSensors": {
           "description": "Aggregated temperature sensors",
           "$logicalInterfaceRef": "IThermometer",
           "type" : "array"
       },
       "humiditySensor": {
           "description": "The humidity sensor device",
           "$logicalInterfaceRef": "5846cd7c6522050001db0e24"
            "type" : "object"
       }
   },
   "required" : [
       "temperatureSensors",
       "humiditySensor"
   ]
  }
```
