---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Développement de passerelles sur {{site.data.keyword.iot_short_notm}}
{: #gw_dev_index}

Si vos terminaux ne peuvent pas se connecter directement à Internet, vous pouvez générer un terminal de passerelle pour extraire et envoyer des données aux applications de votre organisation {{site.data.keyword.iot_full}}. Des bibliothèques client, des exemples et des informations vous sont fournis afin de vous aider à connecter des passerelles de terminaux à votre organisation et à vos applications {{site.data.keyword.iot_short_notm}}.
{:shortdesc}

## Protocoles de connexion
Gateways se connecte à {{site.data.keyword.iot_short_notm}} à l'aide du protocole de messagerie MQTT ou HTTP. 

## Bibliothèques client
Les bibliothèques client pour le développement de passerelles pouvant se connecter à {{site.data.keyword.iot_short_notm}} sont disponibles dans les langues suivantes :

|Bibliothèque client |Lien vers la bibliothèque et autre documentation
|:---|:---
|C++|[https://github.com/ibm-watson-iot/iot-cpp ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-cpp){: new_window}
|C#|[https://github.com/ibm-watson-iot/iot-csharp ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-csharp){: new_window}
|Embedded C| [https://github.com/ibm-watson-iot/iot-embeddedc ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-embeddedc){: new_window}
|Java|[https://github.com/ibm-watson-iot/iot-java ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-java){: new_window}
|Mbed C++|[https://os.mbed.com/teams/IBM_IoT/code/IBMIoTF/ ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://os.mbed.com/teams/IBM_IoT/code/IBMIoTF/){: new_window}
|Node.js|[https://github.com/ibm-watson-iot/iot-nodejs ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-nodejs){: new_window}
|Node-RED|[https://github.com/ibm-watson-iot/iot-nodered ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-nodered){: new_window}
|Python|[https://github.com/ibm-watson-iot/iot-python ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-watson-iot/iot-python){: new_window}

Pour plus d'informations et pour accéder à des liens vers les bibliothèques client disponibles, voir aussi [Bibliothèques client pour le développement dans {{site.data.keyword.iot_short_notm}}](../iot_platform_client_lib.html).

## Edge Analytics
{: #eaa_community}

La fonction {{site.data.keyword.iot_short_notm}} Edge Analytics vous permet d'exécuter {{site.data.keyword.iot_short_notm}} Analytics sur un terminal de passerelle. Utilisez Edge Analytics pour amener Analytics à analyser les données collectées au bord du réseau et à répondre à ces analyses. Vous pouvez également envoyer des données de terminal à {{site.data.keyword.iot_short_notm}} en vue d'un traitement d'analyse supplémentaire, d'une visualisation dans le tableau de bord, en tant que données d'entrée pour d'autres analyses ou afin d'être stockées dans un référentiel historique basé sur le cloud.

Vous pouvez télécharger le logiciel SDK Edge Analytics à partir de la [page de la communauté IBM Edge Analytics ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/developerworks/community/groups/service/html/communitystart?communityUuid=3df173af-0c21-4b9c-9fd1-e8e5561ef460&ftHelpTip=true){: new_window}. Le logiciel SDK inclut le fichier JAR SDK, le javadoc, l'exemple de code, les liens de recette et les fichiers ReadMe. Dans la communauté, vous pouvez également regarder des vidéos pour devenir opérationnel avec Edge Analytics, et vous pouvez utiliser le forum de communauté pour poser des questions.
