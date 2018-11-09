---

copyright:
  years: 2018
lastupdated: "2018-04-06"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Configuration des règles de routages de {{site.data.keyword.iot_short_notm}} Edge (prévisualisation)
{: #edge_rules}

Les règles de routage de {{site.data.keyword.iot_full}} Edge définissent comment les messages sont renvoyés au sein du noeud Edge.

**Important :** la fonction {{site.data.keyword.iot_full}} Edge n'est disponible que dans le cadre d'un programme de prévisualisation limité. Il est possible que des mises à jour ultérieures incluent des modifications incompatibles avec la version en cours de cette fonction. Essayez-la et [dites-nous ce que vous en pensez ![Icône de lien externe](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

Vous pouvez définir plusieurs règles de routage pour un noeud Edge. Les règles sont évaluées pour chaque message qu'un terminal local ou une application publie directement dans le noeud Edge. Les messages qui sont publiés dans le noeud Edge à partir du cloud sont toujours publiés localement et ne sont pas affectés par le routage.

## Format des règles de routage
{: #edge_rules_format}

Une règle de routage est définie en tant qu'objet JSON dans le format suivant :

```
{
  "rule_id": id,
  "matching_filter": "filter",
  "destination": "destination",
  "continue_matching": true,
  "forward_skip_levels": 0,
  "store": true
}
```

Une règle de routage est spécifiée sous forme de tableau d'objets JSON dans le fichier `routing.json` qui est stocké dans le dossier `/etc/wiotp-edge` sur le noeud Edge.

Les propriétés de chaque règle de routage sont défini comme suit :

Propriété      | Type     | Description       
------------- | -----------| -----------
rule_id | nombre entier, obligatoire | Identificateur de la règle. Les règles de routage sont évaluées par ordre croissant de rule_id. Il ne peut pas exister plusieurs règles avec le même rule_id.
matching_filter | chaîne, obligatoire | Utilisé pour évaluer si la règle doit être appliquée à un message publié. Le format doit être celui d'un abonnement MQTT. Les caractères génériques MQTT à un niveau (+) et à plusieurs niveaux (#) sont pris en charge. Ce filtre est évalué par rapport à la rubrique sur laquelle le message est publié. Cette propriété est définie dans le format de la rubrique d'application {{site.data.keyword.iot_short_notm}} et inclut les zones `/type/deviceType/id/deviceId`. Par exemple : `"matching_filter" : "iot-2/type/+/id/+/evt/EvtTypeAlarm/#"`
Destination | chaîne, obligatoire | Définit où le message doit être renvoyé. Définissez l'une des valeurs suivantes : `"CLOUD"`, `"IM"` (pour renvoyer le message au composant Gestion de l'information) ou `"LOCAL"` (pour renvoyer le message localement sur le noeud Edge). Par exemple, `"destination" : "CLOUD"`
continue_matching | booléen, facultatif, valeur par défaut = true | Spécifie si les messages additionnels doivent être évalués à l'aide de cette règle si la règles correspond une fois. La propriété offre un meilleur contrôle sur le routage lorsque plusieurs règles de routage se chevauchent.
forward_skip_levels | entier, facultatif, valeur par défaut = 0) | Spécifie le nombre de niveaux de la rubrique du message d'origine qui doivent être supprimés lorsque le message est à nouveau publié. La valeur par défaut est 0, ce qui signifie que la rubrique d'origine est maintenue. Par exemple, si `forward_skip_levels` est définie sur `1` et un message est publié sur la rubrique `localPrefix/iot-2/type/T1/id/D1/evt/E1/fmt/json`, le message est republié sur la rubrique `iot-2/type/T1/id/D1/evt/E1/fmt/json`.
store | booléen, facultatif, valeur par défaut = true) | Définit si la fonction de stockage et retransmission doit être appliquée aux messages qui sont routés par cette règle. Cette propriété s'applique uniquement lorsque la propriété de destination est définie sur `"CLOUD"`. Si la propriété est définie sur false, les messages qui correspondent à la règle ne sont pas stockés lorsque le noeud Edge est déconnecté du cloud.

## Exemple de fichier routing.json 
{: #edge_rules_example}

Dans l'exemple suivant, trois règles sont configurées :
```
[
  {
    "rule_id": 1,
    "matching_filter": "iot-2/type/+/id/+/evt/AlarmEvent/fmt/+",
    "destination": "CLOUD",
    "continue_matching": false,
  },
  {
    "rule_id": 2,
    "matching_filter": "sendToCloud/iot-2/#",
    "destination": "CLOUD",
    "forward_skip_levels": 1,
    "continue_matching": false
  },
  {
    "rule_id": 3,
    "matching_filter": "#",
    "destination": "LOCAL"
  }
]
```

La première règle route tous les événements de type `AlarmEvent` directement au cloud.

Le second règle route tous les événements qui sont publiés sur une rubrique commençant par `sendToCloud/iot-2` directement au cloud. Le message est transféré dans une rubrique qui commence par `iot-2` et n'inclut pas le préfixe `sendToCloud/`.

La troisième règle est la règle par défaut, qui route tous les autres messages vers courtier MQTT local. Ces messages peuvent ensuite être utilisés par n'importe quel consommateur local disposant d'un abonnement correspondant. L'installation du noeud Edge inclut la règle par défaut de tout router localement dans `/etc/wiotp-edge/routing.json`.

## Edition des règles de routage
{: #edge_rules_edit}

Vous pouvez éditer les règles directement dans le fichier `/etc/wiotp-edge/routing.json`. Après avoir éditer le fichier, sauvegardez vos changements puis exécutez `docker ps` pour déterminer l'ID du conteneur edge-connector. Redémarrez le conteneur Docker à l'aide de cet ID.

## Traitement des incidents de règles de routage
{: #edge_rules_ts}

Si le conteneur edge-connector continue de redémarrer après un changement dans `routing.json`, cela signifie que le fichier des règles de routage contiennent des erreurs de syntaxe. Vérifiez le fichier journal `/var/wiotp-edge/trace/edgeConnector_trace.log` pour voir les messages d'erreur.
