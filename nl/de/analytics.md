---

copyright:
  years: 2016, 2018
lastupdated: "2017-03-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# IoT-Datenanalyse in Echtzeit
{: #analytics}  

**Wichtig:** Eine Betaversion, mit der eine neue Methode zur Definition von Regeln für IoT-Gerätedaten zur Verfügung steht,
wird im Rahmen eines umfassenderen Programms mit Änderungen gestartet, das die Bereitstellung von Regeln und Aktionen in {{site.data.keyword.iot_full}} verbessern soll.

Weitere Informationen finden Sie im Blogbeitrag [Alternative Methode zur Definition von Regeln für IoT-Daten ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/iotplatform/2018/03/01/alternative-approach-defining-rules-iot-data/){: new_window}.

Lesen Sie als ersten Schritt bei der Definition eigener Regeln die Informationen in der Dokumentation [Eingebettete Regeln erstellen (Beta)](information_management/im_rules.html).

## Informationen zur Echtzeitdatenanalyse in IoT

Verwenden Sie {{site.data.keyword.iot_short_notm}} Analytics, um aus den von Ihrem Gerät erzeugten Rohdaten die für Sie erforderlichen Analyseinformationen in Echtzeit abzurufen.  
{: shortdesc}

Mithilfe von [Boards und Karten](data_visualization.html) können Sie Grafiken anzeigen, die die Datasetwerte eines oder mehrerer Geräte darstellen, sodass Sie eine schnelle Übersicht erhalten und Gerätedaten leicht verstehen können.

Mit Analyseregeln geben Sie die Bedingungen an, durch die Aktionen ausgelöst werden. Beispielsweise können Sie eine Regel erstellen, um sicherzustellen, dass ein Alert an das Dashboard auf dem Gerät eines Benutzers und eine E-Mail an den Administrator gesendet wird, wenn die Verbindung zum Gerät verloren geht oder wenn die Temperatur des Geräts einen Höchststand erreicht.

Verwenden Sie [Cloud-Regeln](cloud_analytics.html), um Regeln für Geräte auszulösen, die in der Cloud direkt mit {{site.data.keyword.iot_short_notm}} verbunden sind, und verwenden Sie [Edge-Regeln](edge_analytics.html), um Regeln für Geräte auszulösen, die mit für Edge Analytics aktivierten Gateways verbunden sind.
