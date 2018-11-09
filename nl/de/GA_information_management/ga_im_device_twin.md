---

copyright:
years: 2017, 2018
lastupdated: "2018-08-28"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung zum Datenmanagement
{: #device_twins}

<!--An unprecedented number of devices and sensors exist in the modern world. Connected devices generate vast amounts of digital data at extraordinary speeds. Such volumes of data represent great opportunities but also challenges, in terms of how big data can be processed, analyzed and presented to help to deliver insights and drive transformation.-->

Geräte stellen möglicherweise ähnliche Datenausgaben bereit, unterscheiden sich jedoch in Marke, Modell und Version und können Daten in unterschiedlichen Formaten ausgeben. So kann z. B. ein Gerät mit einem Temperatursensor in einem Büro die Temperatur in Grad Fahrenheit oder in Grad Celsius melden. Es ist nicht effizient, Anwendungen so zu konfigurieren, dass Daten in all diesen Formaten verarbeitet werden können. Stattdessen müssen die Daten erfasst, transformiert und normalisiert werden, um ein einzelnes logisches Modell zu erstellen, damit eine Anwendung mit den verschiedenen Geräten in derselben Weise interagieren kann. 

Die Datenverwaltungskomponente von {{site.data.keyword.iot_short_notm}} weist eine Funktion für Gerätezwillinge und eine Funktion für Assetzwillinge auf. Mit dem Funktion für Gerätezwillinge können Sie die Erfassung, Transformation und Normalisierung verschiedener Formate von Gerätedaten in ein einzelnes logisches Modell nutzen. Mit der Funktion für Assetzwillinge können Sie verschiedene Geräte zusammenfassen, um ein Ding zu erstellen, bei dem es sich um eine assetbasierte Datenstruktur mit höheren Werten handelt. Sie können Dinge sogar gruppieren, um neue Dinge zu erstellen. Eine Anwendung kann unabhängig vom Datenformat, das von den einzelnen Geräten oder Dingen verwendet wird, mit dem logischen Modell interagieren. 

Beispielsweise kann eine Gruppe von Meldegeräten für Temperatur, Feuchtigkeit und Raumbeleuchtung zu einem Ding des Typs 'Raum' zusammengefasst werden, um das Komfortniveau in einem bestimmten Büro darzustellen. Eine Reihe von 'Raum'-Dingen kann in einem 'Stockwerk'-Ding zusammengefasst werden, um alle Büros auf einer bestimmten Ebene darzustellen, und eine Reihe von 'Stockwerk'-Dingen kann zu einem 'Gebäude'-Ding zusammengefasst werden. Durch die Verwendung einer 'Ding'-Abstraktion wird Ihre Anwendung von den Besonderheiten, wie die Geräte miteinander verbunden sind, vom Format, in dem Geräte Ereignisdaten publiziert, und wie die Daten kombiniert werden, gelöst.
{: shortdesc}

## Gerätezwillinge

Ein Gerätezwilling ist eine cloudbasierte digitale Darstellung eines physischen Geräts, das mit {{site.data.keyword.iot_short_notm}} verbunden ist. Ein Gerätezwilling stellt ein logisches Modell der Ereignisse dar, die von einem Gerät publiziert werden. Nachdem der Gerätezwilling definiert und instanziiert wurde, stellt er eine konsistente Möglichkeit zur Interaktion mit einem Gerät im REST-Stil bereit, und zwar unabhängig davon, ob das Gerät online oder offline ist. Die Eigenschaften eines Geräts, einschließlich der Informationen zum aktuellen Status des Geräts (Gerätestatus), können mit einer HTTP-Anforderung oder durch Subskribieren eines IoT-Topics abgerufen werden.

Gerätezwillinge bieten Ihnen Unterstützung in folgenden Bereichen:
- Bereitstellung konsistenter Schnittstellen für Ihre Anwendungsentwickler für den Zugriff auf ereignisgesteuerte Gerätedaten in einer REST-ähnlichen Form.
- Zugriff auf den Status eines Geräts.
- Normalisierung von Daten von Geräten unterschiedlicher Marken oder Modelle, die Daten in unterschiedlichen Formaten publizieren.
- Herausfiltern nicht benötigter Daten.


Um einen Gerätezwillings zu erstellen, müssen Sie die folgenden Ressourcen in {{site.data.keyword.iot_short_notm}} definieren:
- Die Struktur der Ereignisse, die von Ihrem Gerät gesendet werden.  
Die Struktur eines eingehenden Ereignisses wird in der physischen Schnittstelle, im Ereignistyp und in den Ereignisschemaressourcen definiert. 
- Die Eigenschaften, die Sie aufzeichnen wollen.  
Diese Eigenschaften definieren die logische Struktur des Gerätestatus, die von Ihren Anwendungen genutzt werden kann. Die Eigenschaften werden in der logischen Schnittstelle und in den logischen Schemaressourcen definiert.  
- Die Zuordnung von physischen Schnittstellenereignissen zu den Eigenschaften der logischen Schnittstelle.  
Verwenden Sie die Zuordnungsressource, um Ereignisse bestimmten Eigenschaften zuzuordnen.

Das folgende Diagramm zeigt zwei verschiedene Temperaturgeräte an separaten Standorten. Ein Gerät meldet Gerätedaten in Grad Celsius und das andere Berichtsdaten in Grad Fahrenheit. Die Daten werden an {{site.data.keyword.iot_short_notm}} in den Temperaturformaten 't' und 'temp' gesendet. {{site.data.keyword.iot_short_notm}} wandelt Grad Fahrenheit automatisch in Grad Celsius um. Die Temperaturformate 't' und 'temp' werden in das logische 'Temperatur'-Format normalisiert. Die Anwendung kann den Status eines der Geräte abfragen, indem sie auf den Wert für den Parameter 'temperature' zugreift. 

![Übersicht zum Datenmanagement in {{site.data.keyword.iot_short_notm}}.](../information_management/images/ga_im_resources_overview.svg "Übersicht zum Datenmanagement in {{site.data.keyword.iot_short_notm}}")


## Assetzwillinge (Dinge)

Mit Assetzwillingen können Sie das Konzept der Gerätezwillinge noch ausweiten. Mit einem Assetzwilling können Sie Geräte in einer einzelnen Entität namens 'Ding' zusammenfassen. Ein Ding, oder Assetzwilling, ist als Konzept ähnlich wie ein Gerätezwilling, stellt jedoch eine Gruppe von Geräten als ein einzelnes logisches Modell dar. Sie können sogar Dinge zusammenfassen, um höhere Abstraktionsebenen zu erreichen. Ein 'Raum'-Ding kann beispielsweise die folgenden Geräte zusammenfassen:

- Ein Gerät mit Temperatursensor (Thermometer)
- Ein Gerät mit einem Feuchtigkeitssensor (Hygrometer)

Ein 'Stockwerk'-Ding kann dann mehrere 'Raum'-Dinge zusammenfassen. 

Die 'Ding'-Struktur wird mithilfe eines JSON-Schemas definiert. Das Schema referenziert die logischen Schnittstellen der zusammengefassten Geräte oder Dinge. Die Eigenschaften einer 'Ding'-Instanz, einschließlich Informationen zum aktuellen 'Ding'-Status, können mithilfe einer HTTP-Anforderung oder durch Subskribieren eines IoT-Topics abgerufen werden.

Assetzwillinge bieten Ihnen Unterstützung in folgenden Bereichen:
 
- Zusammenfassen von mehreren Gerätezwillingen oder Dingen zum Definieren neuer Dinge.
- Zugriff auf den 'Ding'-Status.
- Verwalten von Assets, ohne der einzelnen Instrumentierung dieser Geräte ausgesetzt zu sein.
- Herausfiltern nicht benötigter Daten.
- Normalisieren von 'Ding'-Schnittstellen, um Anwendungen von der Komplexität der Erstellung bestimmter Dinge zu lösen.


Um einen Assetzwilling zu erstellen, müssen Sie die folgenden Ressourcen in {{site.data.keyword.iot_short_notm}} definieren:

- Die 'Ding'-Struktur.  
Die 'Ding'-Struktur wird durch das 'Ding'-Schema definiert, die die zusammengefassten Geräte oder Dinge angibt.
- Die Struktur des gewünschten 'Ding'-Status, der aus den Eigenschaften besteht, die aufgezeichnet werden sollen.  
Diese Eigenschaften definieren die logische Struktur des 'Ding'-Status, die von Ihren Anwendungen verarbeitet werden kann. Die Eigenschaften werden in der logischen Schnittstelle und in den logischen Schemaressourcen definiert.  
- Die Zuordnung der 'Ding'-Schnittstelle zu den Eigenschaften der logischen Schnittstelle.  
Verwenden Sie die Zuordnungsressource, um Ereignisse bestimmten Eigenschaften zuzuordnen.


Das folgende Diagramm zeigt Temperatur- und Feuchtigkeitssensoren auf verschiedenen Geräten, die Temperatur- und Feuchtigkeitsereignisdaten in {{site.data.keyword.iot_short_notm}} publizieren. Zwei Gerätezwillinge, die jeweils ein physisches Gerät darstellen, verfügen über zugeordnete logische Schnittstellen und werden in {{site.data.keyword.iot_short_notm}} erstellt. Die Daten, die vom Temperaturgerät veröffentlicht werden, werden der logischen Schnittstelle 'IThermometer' zugeordnet. Die Daten, die vom Feuchtigkeitsgerät publiziert werden, werden der logischen Schnittstelle 'IHygrometer' zugeordnet. Die logischen Schnittstellen werden in einem Ding des Typs *Raum* mit einer logischen 'IRoom'-Schnittstelle zusammengefasst. Die logische 'IRoom'-Schnittstelle definiert Temperatur- und Feuchtigkeitseigenschaften und ermöglicht durch das Zusammenfassen von Geräte in einem einzelnen Ding, mit dem die Anwendung interagieren kann, das Erstellen eines eigenen logischen Modells.  

![Übersicht zum Datenmanagement in {{site.data.keyword.iot_short_notm}}.](../information_management/images/Thing overview.svg "Übersicht zum Datenmanagement in {{site.data.keyword.iot_short_notm}}")

**Wichtig:** Die Funktion für Dinge in {{site.data.keyword.iot_short_notm}} steht nur im Rahmen eines eingeschränkten Betaprogramms zur Verfügung. Zukünftige Aktualisierungen enthalten möglicherweise Änderungen, die mit der aktuellen Version dieser Funktion nicht kompatibel sind. Starten Sie einen Versuch und [senden Sie uns Ihren Erfahrungsbericht ![Symbol für externen Link](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.


Weitere Informationen zum Definieren und Konfigurieren von Schlüsselinformationen und Ressourcen finden Sie in [Erklärung des Datenmanagements](ga_im_definitions.html). 

## Nächste Schritte

- Erstellen Sie Ihren eigenen Gerätezwilling in {{site.data.keyword.iot_short_notm}}. Weitere Informationen finden Sie in der Dokumentation [Einführung zum Datenmanagement mithilfe der Webschnittstelle](im_ui_flow.html). 
- Erstellen Sie mithilfe von REST-APIs einen Geräte- und einen Assetzwilling. Weitere Informationen finden Sie in der Dokumentation [Einführung zum Datenmanagement](../information_management/getting_started_things.html).  
- Erstellen Sie Regeln, die ausgelöst werden, wenn Ereignisdaten, die mit einer angegebenen Bedingung oder einer Gruppe von Bedingungen übereinstimmen, von {{site.data.keyword.iot_short_notm}} empfangen werden. Weitere Informationen finden Sie in der Dokumentation [Eingebettete Regeln erstellen (Beta)](../information_management/im_rules.html).

Detaillierte Informationen zu den einzelnen Schritten, die in der Dokumentation *Einführung zum Datenmanagement* beschrieben sind, finden Sie in den Beispielszenarien, die in den folgenden Abschnitten dokumentiert sind: 

- [Schrittweise Anleitung 1: Detailliertes Beispiel zur Vorgehensweise beim Arbeiten mit Geräten über eine allgemeine Schnittstelle](ga_im_index_scenario.html#scenario) 
- [Schrittweise Anleitung 2: Ausführliches Beispiel zur Arbeit mit Dingen über eine gemeinsame Schnittstelle](../information_management/im_index_scenario_thing.html#scenario) 


